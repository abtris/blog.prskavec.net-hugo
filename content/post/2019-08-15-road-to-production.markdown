---
title: "Jaká je cesta k produkčnímu kódu?"
date: 2019-08-15T09:39:48+02:00
tags:
  - reliability
  - sre
---

V poslední době jsem četl několik dobrých článků jak [Elixir + gRPC: the road to production](https://code.tubitv.com/elixir-grpc-the-road-to-production-5d7daad4945b) nebo [Don’t use Go’s default HTTP client (in production)](https://medium.com/@nate510/don-t-use-go-s-default-http-client-4804cb19f779) a zkoušel jsem hledat zda dokumentace v projektech vlastně učí vývojáře se zamyslet nad tím co poskytují jazyky jako základ a jak ve skutečnosti je potřeba potom aplikaci nastavit, aby šla provozovat dostatečně robustně a spolehlivě.

Klasický příběh je zmíněný v tom článku "Don’t use Go’s default HTTP client (in production)" a to jsou defaultní hodnoty timeoutu pro HTTP klienty. Například v tom článku, je zmíněný default 0 což znamená žádný timeout a to je asi ta nejhorší varianta. V Node.JS je default 120s a dalších jazycích je to například 60s (Ruby, PHP) apod. Je to opravdu zajímavé obzvláště pokud to porovnám se svým světem, kde Heroku [zabije každý request za 30s](https://devcenter.heroku.com/articles/request-timeout) a vy musíte aktivně pracovat na tom, aby jste těch 30s nedosáhli.

Všechno to jsou známé věci, ale nemyslím, že to všichni systematicky dělají. Skvělá dokumentace od Microsoft Azure o [Transient fault handling](https://docs.microsoft.com/en-us/azure/architecture/best-practices/transient-faults) to krásně popisuje a je jen škoda, že něco takového není pro každý cloud (Azure service-specific retry guidelines) a chybí někde pěkně dohromady například pro GRPC. Pokud někdo o něčem podobném týkajícího se GRPC víte, určitě mi dejte vědět. Za léta jsem slyšel hodně best practice většinou od lidí s Googlu, ale nikde v dokumentu detailně popsané konfigurace pro deadlocky a timeouty jsem nenašel.

Na závěr bych se zeptal, jak to děláte vy? Jsou věci jako [circuit-breaker](https://martinfowler.com/bliki/CircuitBreaker.html), [retry](https://docs.microsoft.com/en-us/azure/architecture/patterns/retry) a timeouts cizí věci nebo se jimi aktivně zabýváte? Jak to testujete? Jak to monitorujete? Rád si o tom popovídám a pokud vás zajíma Golang můžeme se nepříklad potkat na [Golang Prague #4](https://www.meetup.com/Prague-Golang-Meetup/events/263611817/), kde budu mluvit o [TinyGO](https://tinygo.org) což je poslední věc se kterou si hraju.
