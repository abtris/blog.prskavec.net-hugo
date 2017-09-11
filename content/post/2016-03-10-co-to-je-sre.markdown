---
categories: [sre]
comments: true
date: 2016-03-10T00:00:00Z
title: Co to je SRE?
url: /2016/03/10/co-to-je-sre/
---

Včera jsem měl [přednášku v Brně o dockeru](http://srazy.info/jak-nejlepe-nasadit-docker-kontejnery-do-cloudu/6197) a ptal jsem se lidí kolem na meetup a v hospodě potom zda znají Site Reliability Engineering (SRE) ze svého okolí. Tento koncept od Googlu rozšiřuje klasické pojetí DevOps a myslím, že je to jedna z nejlepších věcí co Google vymyslel.

Můžete to slyšet přímo od Bena Treynora. Poslechněte jeho skvělou přednášku [Keys to SRE z SRECon14](https://www.usenix.org/conference/srecon14/technical-sessions/presentation/keys-sre).

<!--more-->

Díky, že jste přímo neutekli na přednášku Bena Treynora, podívejte se na ní aspoň někdy později, protože to opravdu stojí za to.

Jestli jste se snažili někdy ve firmě zavést DevOps tak jste možná narazili, případně říkáte, že máte DevOps, ale často to úplně ve všem nepomohlo.

Přišel jsem před 7 lety do [LMC](http://www.lmc.eu). Měli jsme všecho rozdělené na týmy podle toho co kdo dělal a přišlo nám to tehdy logické. Produkt měl svůj tým, QA měl svůj tým, Support, Ops, Sales, Marketing a Development také. Development byl ještě rozdělný na tři týmy podle specializace (interní systémy, B2C). A ještě jsme měli externí partnery. Také jsme měli release 3-4 krát do roka a celkem to fungovalo.

Potom, ale přišla revoluce. Měli jsme skvělé školení od produktového vývoje a ve firmě se všechno začalo měnit. Vytvořili se produktové týmy. Každý tým měl scrummastera, produkťáka, ux a programátory, kterří dělali i QA. Jediné co zůstalo bylo Operations. Potom jsem z firmy odešel a nevím zda se v tom posunuly dál, ale to je dost těžký krok zvláště pro větší firmu.

V Apiary přišel náš CTO Lukáš Linhart s tím, že vytvoříme SRE tým, který bude horizontálně podporovat ostatní týmy na produktech. Protože tyto věci mě baví, tak jsem do toho týmu šel a dnes ten malý tým vedu. Nejdříve jsem si myslel, že to je prostě takové agilní, startupové OPS. Ale až později mi došlo, že za tím je mnohem víc a že se to musí přenést do kultury celé firmy.

Podle mě nejdůležitější věc je, aby se byl společný pool lidí. Prostě přijmete jen programátory, žadné lidi co chtějí dělat jen OPS. Důležité, aby to byli hodně líní programátoři a nechtěli nic dělat ručně a když to dělají podruhé už si na to píší nějaký kód, zautomatizují to a příště už mohou řešit něco jiného. Snaha o automatizaci je klíčová. Každá firma chce růst a nechcete to dohánět neustálým nabíráním nových lidí, kteří pokrývají jen provoz.

Začněte SRE dělat hned ono se vám to v budoucnu vrátí. V Googlu začali v 6 lidech (2003) a těď mají v SRE asi 2500 lidí (odněkud zaslechnuto).

Musíte zapojit do OPS práce programátory. V [Apiary](https://apiary.io) každý programátor má službu a stará se i produkční systémy, je to potřeba, aby to každý z nich uměl. Využíváme hodně SaaS řešení což celou práci programátorům ulehčuje, když máte technický problém, je tu support od poskytovale co vám rád pomůže. Tohle funguje velmi dobře.

Klíčový rozdíl mezi SRE a DevOPS jsou error buggety. Aby jste neměli válku mezi DEV a OPS týmy, dá se to nastavit jednoduše. Stanovíte SLA mezi systémy. SLA může být interní nebo externí se zákazníky a tuto metriku nestanoví obvykle ani SRE nebo DEV tým. Je to spíš nástroj managementu. Důležité je jen, aby tým se svým SLA souhlasil, nepřišlo mu to nesmyslé. Potom se dá vypočítat **error budget** pomocí: `1 - SLA` a máte v hodnotu budgetu.

Budget musíte měřit a mít to v monitoringu, kde na to vidí jak DEV tak SRE tým. Když je DEV tým pod limitem svého error budgetu, může provádět deploy. Po překročení limitu nesmí nasazovat. To má tu výhodu, že je to čirá matematika, nikde tam není boj o moc apod. DEV se naučí hlídat si svůj error budget a PR, které mají málo testů nebo mají jiná rizika nebudou mít tak jednoduchý život.

Samozřejmě to není všechno kolem SRE, je toho mnohem více. Pro ty, které SRE problematika zajímá tak doporučuji sledovat [SRE Weekly](http://sreweekly.com/) nebo se zůčastnit [SRECon 2016](https://www.usenix.org/conference/srecon16).
