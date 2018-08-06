---
comments: true
date: 2015-09-16T00:00:00Z
tags:
  - nodejs
  - golang
title: Porovnání implementace service v NodeJS a Go lang
url: /2015/09/16/porovnani-implementace-microservice-v-nodejs-and-go-lang/
---

## Datadog a log parsing service

Pro používání [Datadog](https://www.datadoghq.com/) na [Heroku](https://heroku.com) je potřeba několik věcí.
Za prvé, pro datadog agenta potřebujete [custom buildpack](https://github.com/miketheman/heroku-buildpack-datadog.git), který v kombinaci s vaším buildpackem vám umožní mít vše pohromadě. Pokud to nechcete můžete udělat samostatnou service přes kterou se dají parsovat logy pomocí této [knihovny v NodeJS](https://github.com/ozinc/heroku-datadog-drain).
Pokud chcete do Datadogu zapisovat deploy na Heroku použijte [emailový post deploy hook](https://devcenter.heroku.com/articles/deploy-hooks#email).
Aplikaci a její metriky můžete posílat přes Datadog API.

<!--more-->

## Začátek

Jako první jsem použil výchozí aplikaci od tvůrců a pustil tam jednu malou aplikaci, kde počet req/min dosahoval několika desítek a vše bylo bez problémů.

Tak jsem zapojil produkční aplikace. Počet requestů stoupl na 3000 req/min a aplikace začala mít značné problémy i když běžela na Performace-M dynu.

<a href="{{ root_url }}/images/drain/01.png"><img class="center" src="{{ root_url }}/images/drain/01.png" alt="Heroku monitoring - NodeJS before optimalization" /></a>

## Řešení

Po diagnostikování těchto problémů jsme se dali do [hledání memory leaks v NodeJS aplikaci](https://github.com/apiaryio/heroku-datadog-drain) a současně jsme zkusili tuto malou service přepsat do [Go](https://github.com/apiaryio/heroku-datadog-drain-golang).

Obě řešení zafungovala a za pár hodin práce jsme měli už přijatelné výsledky v NodeJS. Mohli jsme snížit používaná dyna na běžné `1X` a tam provádět další srovnání.

<a href="{{ root_url }}/images/drain/02.png"><img class="center" src="{{ root_url }}/images/drain/02.png" alt="Heroku monitoring - NodeJS" /></a>

Verze v go langu je na tom ještě trochu lépe hlavně s ohledem na stabilitu a pamět. Tuto verzi jsme nechali potom trvale v běhu na nejmenším dynu k dispozici s monitoringem.

<a href="{{ root_url }}/images/drain/03.png"><img class="center" src="{{ root_url }}/images/drain/03.png" alt="Heroku monitoring - Go lang" /></a>

## Závěr

Pokud vás toto zaujalo pojďte si popovídat o Go langu na první [Go lang meetup v Praze](https://srazy.info/golang-meetup/5676). Budeme mít lighting talk o tomto příkladu s dalšími detaily a zúčastní se i další firmy, které řeknou o svých zkušenostech. Pokud vás zajímají nějaké detaily o používaní Datadogu na Heroku tak se ozvěte v komentářích.


