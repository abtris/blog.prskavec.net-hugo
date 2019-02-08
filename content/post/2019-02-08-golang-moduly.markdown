---
title: "Moduly v jazyku Go"
date: 2019-02-08T12:08:30+01:00
tags:
  - golang
---


Jedna z věcí co mi přišli na na Go ze začátku těžké byla [GOPATH](https://github.com/golang/go/wiki/GOPATH). Byl jsem zvyklý udělat `git clone` kamkoliv jsem chtěl a potom to pustit, ale v Go to jednoduše nešlo. Až ve verzi 1.11 přišli [go modules](https://github.com/golang/go/wiki/Modules) a tuto nevýhodu odstranili. Hodně vývojářů začala tuto funkci hned používat a ja jsem taky migroval všechny svoje projekty.

<!--more-->

Nemá cenu vysvětlovat jak co funguje, protože to za mě udělal už ve skvělém videocastu [JustForFunc](https://www.youtube.com/c/justforfunc/).

<iframe width="560" height="315" src="https://www.youtube.com/embed/aeF3l-zmPsY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/H_4eRD8aegk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Jako další krok bude vytvořit [module index](https://blog.golang.org/modules2019), kde bude centrální repository. Jako první se otevřelo [JFrog central registry](https://gocenter.jfrog.com/) a určitě nebude jediné. Samozřejmě tam není všechno, co chybí se stáhne z Githubu jako dnes.


