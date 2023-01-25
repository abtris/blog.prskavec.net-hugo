---
title: "Pozor na základní nastavení http klienta v Go (a nejen tam)"
date: 2023-01-25T08:25:52+01:00
---

O tomto problému se napsalo mnoho článků, ale stále vidím ten problém, který se vrací dokola a dokola. Většina programovacích jazyků nemá základní nastavení pro HTTP dělané pro běh v produkci. Budeme si to demonstrovat na příkladu Go, ale ostatní jazyky jsou na tom často podobně, někdy lépe někdy hůře.

Pokud vezmete standardní knihovnu a budete chtít udělat request tak vám vyjde něco takového.

```go
func main() {
 url := "http://localhost:3000"
 var httpClient = &http.Client{}
 response, _ := httpClient.Get(url)
 fmt.Println(response)
}
```

V tomto případě veškeré nastavení HTTP klienta je dáno pomocí [default transport](https://go.dev/src/net/http/transport.go).

```go
var DefaultTransport RoundTripper = &Transport{
 Proxy: ProxyFromEnvironment,
 DialContext: defaultTransportDialContext(&net.Dialer{
  Timeout:   30 * time.Second,
  KeepAlive: 30 * time.Second,
 }),
 ForceAttemptHTTP2:     true,
 MaxIdleConns:          100,
 IdleConnTimeout:       90 * time.Second,
 TLSHandshakeTimeout:   10 * time.Second,
 ExpectContinueTimeout: 1 * time.Second,
}
```

Podle toho základního nastavení se váš HTTP klient bude chovat, všechny nastavené timeouty jsou definovány a vy nemůžete jednoduše to klienta později zasáhnout.

Pokud to chcete vylepšit, tak pokud vám stačí sáhnout na základní timeout můžete to nastavit takto:

```go
func main() {
 url := "http://localhost:3000"
 var httpClient = &http.Client{
  Timeout: time.Second * 10,
 }
 response, _ := httpClient.Get(url)
 fmt.Println(response)
}
```

Tím přepíšete 30s timeout na 10s což je tak maximální hodnota co bych pro synchronní POST request asi akceptoval, i když je to hodně za tím co jsem ochotný čekat jako uživatel.

Cestou jak si držet vše pod kontrolou je vlastní klient, kde transport přetížíme a použijeme vlastní nastavení.

```go
type HTTPClientSettings struct {
 Connect          time.Duration
 ConnKeepAlive    time.Duration
 ExpectContinue   time.Duration
 IdleConn         time.Duration
 MaxAllIdleConns  int
 MaxHostIdleConns int
 ResponseHeader   time.Duration
 TLSHandshake     time.Duration
}

func NewHTTPClientWithSettings(httpSettings HTTPClientSettings) (*http.Client, error) {
 var client http.Client
 tr := &http.Transport{
  ResponseHeaderTimeout: httpSettings.ResponseHeader,
  Proxy:                 http.ProxyFromEnvironment,
  DialContext: (&net.Dialer{
   KeepAlive: httpSettings.ConnKeepAlive,
   DualStack: true,
   Timeout:   httpSettings.Connect,
  }).DialContext,
  MaxIdleConns:          httpSettings.MaxAllIdleConns,
  IdleConnTimeout:       httpSettings.IdleConn,
  TLSHandshakeTimeout:   httpSettings.TLSHandshake,
  MaxIdleConnsPerHost:   httpSettings.MaxHostIdleConns,
  ExpectContinueTimeout: httpSettings.ExpectContinue,
 }

 // So client makes HTTP/2 requests
 err := http2.ConfigureTransport(tr)
 if err != nil {
  return &client, err
 }

 return &http.Client{
  Transport: tr,
 }, nil
}
```

Pokud máme vlastní nastavení tak použití je podobné jen přímo specifikujeme jednotlivé parametry.

```go
func main() {
 url := "http://localhost:3000"

 httpClient, err := NewHTTPClientWithSettings(HTTPClientSettings{
  Connect:          5 * time.Second,
  ExpectContinue:   1 * time.Second,
  IdleConn:         90 * time.Second,
  ConnKeepAlive:    30 * time.Second,
  MaxAllIdleConns:  100,
  MaxHostIdleConns: 10,
  ResponseHeader:   5 * time.Second,
  TLSHandshake:     5 * time.Second,
 })
 if err != nil {
  fmt.Println("Got an error creating custom HTTP client:")
  fmt.Println(err)
  return
 }

 response, _ := httpClient.Get(url)
 fmt.Println(response)
}
```

Tohle je dobré řešení, ale stále to neřeší všechno. Někdy bychom chtěli pro specifický případ (PUT, POST) jiný timeout nebo deadline a nechceme kvůli tomu modifikovat HTTP klienta, to se dá docílit pomocí contextu `context.WithTimeout` nebo `context.WithCancel`. Použití je vidět potom v dalším případě.

```go
func main() {
 url := "http://localhost:3000"

 req, err := http.NewRequest(http.MethodGet, url, nil)
 if err != nil {
  log.Fatal(err)
 }

 ctx, cancel := context.WithCancel(context.Background())
 _ = time.AfterFunc(1*time.Second, func() { cancel() })

 httpClient, err := NewHTTPClientWithSettings(HTTPClientSettings{
  Connect:          5 * time.Second,
  ExpectContinue:   1 * time.Second,
  IdleConn:         90 * time.Second,
  ConnKeepAlive:    30 * time.Second,
  MaxAllIdleConns:  100,
  MaxHostIdleConns: 10,
  ResponseHeader:   5 * time.Second,
  TLSHandshake:     5 * time.Second,
 })
 if err != nil {
  fmt.Println("Got an error creating custom HTTP client:")
  fmt.Println(err)
  log.Fatal(err)
 }

 response, _ := httpClient.Do(req.WithContext(ctx))
 fmt.Println(response)
}
```

Stále se, ale může stát, že nejste na straně klienta, ale serveru a tam se to úplně dobře vyřešit nedá a proto v Go 1.20 přidali [`ResponseController`](https://pkg.go.dev/net/http@go1.20rc2#ResponseController), kde získáte přístup k těmto metodám:

```go
Flush()
FlushError() error // alternative Flush returning an error
Hijack() (net.Conn, *bufio.ReadWriter, error)
SetReadDeadline(deadline time.Time) error
SetWriteDeadline(deadline time.Time) error
```

Tady se dá například prodloužit deadline requestu, pokud to je potřeba. Celou [diskuzi o manipulaci timeoutu v Handleru najde na Githubu](https://github.com/golang/go/issues/16100).

Proto nezapomínejte nastavit HTTP klienty pro produkci, testuje performance, měřte latency a podle toho si upravte nastavení, je dobře vědět kde máte limity a podle toho si to nastavit. Nenechávejte to základním nastavení, v produkci to může způsobovat nepříjemné chyby, které se špatně odhalují, protože často působí jako náhodné chyby.

Pokud vás tohle třeba trochu zaujalo tak si přijďte popovídat o Go and release 1.20 na [další Pražský Go meetup 21.února](https://www.meetup.com/prague-golang-meetup/events/291042846/).
