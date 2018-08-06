---
title: "Monitorama Konference 2018 v Portlandu"
date: 2018-06-09T19:44:25-07:00
tags:
 - conference
---

Tento týden jsem se zúčastnil konference [Monitorama](https://monitorama.com) v Portlandu, která je zaměřená na monitoring. Je to třídenní konference s jedním sálem, kde nemusíte rozhodovat na co půjdete. Je to všechno o open source monitorovacích nástrojích, ale hojně podporované ze stran sponzorů. Normální přednášky nejsou na sponsory přímo napojené a ti mají vymezené 5 min sloty a ja na nich zda jich využijí a jak. Každý hlavně opakoval, že všichni nabírají, to už na IT konferencích je samozřejmé.

Přednášky jsou kvalitní, ale ne všechno je pro každého. Osobně mě zaujalo hlavně case study z velkých firem jako Slack, Uber o tom jak dělají monitoring ve velkém.

Další skupina byla z oblasti best practices o tom jak pracovat s různými oblasti kolem monitoringu. Poslední oblast byla zaměřená na soft skill od učení až po diverzita v týmech.

Konference je steamovaná živě a odkazy na [videa](https://gist.github.com/irabinovitch/9768289082f269a5174bee49a13f46ca) jsou v odkazovaném gistu.

Zavisí co vás aktuálně pálí za problémy ve vašem týmu, ale podle mě všude se využijí poznatky o tom jak nejlépe zaškolit, trénovat oncally, jak mít flexibilní Slack bot, který pomáhá, jak mít oncall, kde se nemusíte stresovat. Pěkně shrnuli děvčata z Mapboxu (3 různé přednášky).

- [The on call simulator](https://youtu.be/QZf60dYMxKY?t=20417) - Franka Schmidt (Mapbox)
- [Incident Response as Code](https://youtu.be/1YITF2_Yba8?t=20029) - Papasweni Pathak (Mapbox)_
- [slack-bots: coordination through community](https://youtu.be/1YITF2_Yba8?t=6460) - Aruna Sankaranarayanan

Pokud vaše monitorovací řešení nestíhá tak se podívejte na Slack, Uber a Demonware přednášky.

- [Slack in the age of Prometheus](https://youtu.be/1YITF2_Yba8?t=17263) - George Leong (Slack)
- [Metrics at Uber: Putting billions of timeseries to work at Uber with autonomous monitoring](https://youtu.be/M0CLU4Onko4?t=7243)
- [Assisted Remediation: By trying to build an autoremediation system, we realized we never actually wanted one](https://youtu.be/M0CLU4Onko4?t=19938) - Kale Stedman (Demonware)

[Přednáška Achieving Google-levels of Observability into your Application with OpenCensus](https://youtu.be/M0CLU4Onko4?t=1748) mě velmi zaujala a mohla by do budoucnosti ušetřit hodně práce. Google opět open source další svůj projekt, který se jmenuje Census interně a existuje více jak 10 let. Jde o to vytvořit framework pro metriky, tracing a debugging, který vám pomůže s instrumentací a bude podporovaný napříč nástroji. Zatím je podporovány tyto jazyky CPP, Go, Erlang, Java, Ruby, PHP a Python. Jako další bude přídán [.Net a Node.js](https://opensource.googleblog.com/2018/05/opencensus-journey-ahead-part-1.html). Mělo by to snad být už v tomto roce.

Podle mě nejlepší přednáška z mého pohledu o tom jak automatizovat monitorování compliance od Dave Cadwallader. V přednášce [ukazoval](https://github.com/geekdave/monitorama) jak použít projekt [inspec.io](https://www.inspec.io) k monitorování pomocí [Prometheus exporteru](https://github.com/geekdave/prometheus_inspec_exporter) a integrace s PagerDuty včetně přesných runbooků, které se odkazují na každém incidentu.


## Závěr

Pokud vás to zaujalo, tak se můžete 4-5. září zůčastnit evropské verze [Monitorami](https://monitorama.eu) v Amstrodamu. Bude to menší akce než v Portlandu, ale určitě to bude zajímavé také. Stálé jsou [otevřené CFP a můžete přihlásit svůj příspěvek](https://monitorama.eu/#cfp). Komunita kolem Monitoramy je velmi dobrá a lístky do Portlandu se vyprodali za necelých 24 hodin. Monitoringu se budeme také věnovat v jedné z [Hive talks](https://meetup.com/apiaryio) na podzim, tak pokud máte co řící tak mi pošlete návrh vaší přednášky.
