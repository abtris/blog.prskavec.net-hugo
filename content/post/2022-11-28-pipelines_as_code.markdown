---
title: "Pipelines as Code"
date: 2022-11-28T13:44:04+01:00
---

"Pipelines as code" není uplně nový koncept, já jsem o něm slyšel před pár lety s uvedením [Tektonu](https://tekton.dev/). Napsal jsem tehdy design dokument na vytvoření nového CI, které bude akceptovat různé předpisy (Jenkins, CircleCI, TravisCI a Github Actions) a nebudete se muset učit novou syntaxi. Bohužel se projekt nikdy nerealizoval, tak jsem se tímto moc déle nezabýval.

Solomon Hykes před pár lety odešel z Dockeru a založil [Dagger.io](https://dagger.io/), kde začali adoptovat[Cue lang](https://cuelang.org/) a pracovat na zajímavém novém projektu. Když jsem dostal přístup do early accessu tak jsem z toho moc nadšený nebyl, protože psaní pipelines v Cue není něco co bych vyvojářům doporučil. Po zkušenostech jak bylo těžké prosadit v některých týmech [Jsonnet](https://jsonnet.org/) pro Grafana dashboardy, tak jsem opatrný.

Před pár týdny, ale uvedli v technical preview [Dagger Go SDK](https://docs.dagger.io/sdk/go/) a když jsem si vyzkoušel [základní demo](https://youtu.be/GgMskf-znh4) podle videa, tak mi to přišlo super. Rozhodl jsem se předělat jeden malý project `ga-badge`, který generuje badge pro Github Actions. Tady je příslušný [PR](https://github.com/abtris/ga-badge/pull/49/files) se změnami, kde je vidět jak jednoduchá ta změna je. Potom tady mám záznam když se to pustí. Používám [Mage](https://magefile.org/) na pouštění podle doporučení lidí z Daggeru pro multi repository. Určitě to půjde udělat i jinak, ale na začátek mi to přišlo v pohodě.

[![asciicast](https://asciinema.org/a/4DwhBANFV53kW7QgsJRMsUZ5E.svg)](https://asciinema.org/a/4DwhBANFV53kW7QgsJRMsUZ5E)

O Dagger Go SDK budu mluvit na [Go Meetupu \#9](https://www.meetup.com/prague-golang-meetup/events/289247920/) 30.11.2022 a pokud vás to zaujmulo tak se zastavte na diskuzi.



