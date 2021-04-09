---
categories:
- docker
comments: true
date: 2017-04-21T00:00:00Z
title: Docker - multi stage build
url: /2017/04/21/docker-multi-stage-build/
---

Tato novinka je dostupná v poslední verzi Dockeru 17.05, musíte mít [Edge edici](https://docs.docker.com/docker-for-mac/install/#download-docker-for-mac) a se zapnutými experimentálním funkcemi, ale objeví se to v další stabilní verzi. Je pro jistotu tady je výpis z mého `docker version`.

```
$ docker version
Client:
 Version:      17.05.0-ce-rc1
 API version:  1.29
 Go version:   go1.7.5
 Git commit:   2878a85
 Built:        Tue Apr 11 20:55:05 2017
 OS/Arch:      darwin/amd64

Server:
 Version:      17.05.0-ce-rc1
 API version:  1.29 (minimum version 1.12)
 Go version:   go1.7.5
 Git commit:   2878a85
 Built:        Tue Apr 11 20:55:05 2017
 OS/Arch:      linux/amd64
 Experimental: true
```

<!--more-->

## Multi stage

O co vlastně jde v tom multi stage. Pokud potřebujete vyrobit image pro aplikaci, která má artefakty (C/C++, Go, Java, etc.) tak potřebujete jen malou část těch závislostí pro běh v produkci, ale při výrobě těchto artefaktů musíte nainstalovat hodně a většinou ne malých závislostí. Řešilo se to patternem builder za pomocí dvou Dockerfile souborů, ale teď existuje jednodušší řešení.

Řešení v jednom souboru kde máte dvě sekce, každá začíná deklarací `FROM`. Tady je ukázka pro Go.

```
FROM golang:1.8 as builder

ENV GLIDE_VERSION 0.10.2
ENV APP_VERSION 1.0.2

ADD https://github.com/Masterminds/glide/releases/download/${GLIDE_VERSION}/glide-${GLIDE_VERSION}-linux-amd64.tar.gz /tmp/glide-${GLIDE_VERSION}-linux-amd64.tar.gz

RUN cd /tmp && \
    tar -zxvf /tmp/glide-${GLIDE_VERSION}-linux-amd64.tar.gz && \
    cp /tmp/linux-amd64/glide /usr/local/bin/glide && \
    chmod 755 /usr/local/bin/glide && \
    rm /tmp/glide-${GLIDE_VERSION}-linux-amd64.tar.gz && rm -rf /tmp/linux-amd64/

COPY . /go/src/github.com/apiaryio/heroku-datadog-drain-go

RUN cd /go/src/github.com/apiaryio/heroku-datadog-drain-go && \
    glide install && \
    go install

FROM scratch
COPY --from=builder /go/bin/heroku-datadog-drain-go .
CMD ["./heroku-datadog-drain-go"]
```

V první části se použije image golang a nainstaluje se glide (package manager) a vyrobí se binární soubor, který potom v druhé část (`FROM scratch`) se použije a pomocí `COPY --from=builder` se překopíruje artefakt z jednoho image do druhého. Pokud si image v první sekci nepojmenujete tak použijte číselné označení `COPY --from=0`.

Výsledné image potom vypadají takto:

```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
drain               latest              7a7abced7a9d        6 seconds ago       10MB
<none>              <none>              edabce509d75        7 seconds ago       752MB
golang              1.8                 c0ccf5f2c036        13 days ago         703MB
```

Máte dva image jeden pro produkci a druhý můžete klidne smazat.

## Závěr

Tato inovace zjednodušší život mnoha lidem a je to super. Pokud si to chcete vyzkoušet můžete na [webu v bez nutnosti instalovat si poslední Docker](https://training.play-with-docker.com/multi-stage/).
