---
title: "Site Reliable Engineering (SRE) a DevOPS"
date: 2021-02-09T08:31:09+01:00
tags:
    - sre
    - devops
---

V neděli 7. února jsem na ClubHouse místnosti Cloud Native mluvil s několika hosty o SRE a DevOPS. Zkusím shrnout svůj pohled i sem na blog pro ty kdo tam nebyli.

Já jsem se setkal s pojmem SRE v roce 2014 v Apiary. náš CTO Lukáš Linhart přinesl tuto vizi z prvního SREconu v US a v roce 2015 jsme vytvořili SRE team, který vlastně existuje dodnes.

SRE ale existovalo od roku 2003, ale jen uvnitř Google, lidé mimo Google ho neznali. V roce 2009 Patrick Debois přišel s pojmem DevOPS a založil DevOPS Days konferenci, která se úspěšně koná po celém světě.

DevOPS definovalo 5 hlavních pilířů a mi se podíváme jak to souvisí s SRE.

## Pilíře DevOPS

1. Redukovat organizační sila

    Jedna z nejdůležitějších myšlenek, odbourat přehazování přes zeď mezi týmy. Sám pamatuji, jak náš developer tým dostával od produktového týmu zadání, předával věci QA a operations. Byl jsem rád v LMC když jsme se toho zbavili a vytvořili více integrované týmy, které fungovali o dost lépe.

2. Akceptoval, že chyby jsou normální

    To je důležitý koncept, který ukazuje, že lidé ani technologie nejsou bez chyb. Ať jde o selhání disku nebo chybu operátora, musíme se na to koukat systémově a hledat systematické cesty jak tomu zabránit.

3. Implementovat postupné změny

    Pokud je vám blízký koncept continuous delivery, víte že malé změny mají menší dopad, pokud dojde k chybě a čím později se na chybu přijde tím větší náklady jsou s odstraněním problému. Tento přístup se tomu snaží bránit.

4. Využijte nástroje a automatizaci

    Je potřeba eliminovat ruční práci ve všech podobách, od základního review, pouštění testů až po deploy. Je vždy potřeba koukat na to co dává smysl automatizovat a hlídat kolik je podíl neproduktivní ruční práce (toil) vůči ostatní práci.

5. Měřte všechno

    Abyste dobře rozhodovali musíte mít data, bez měření to nejde a rozhodovat v organizaci na základě pocitů není spolehlivé a těžko se to opakuje a hledají se chyby. Proto nejdříve seberte potřebná data, zaměřte se na nejhorší problém, co jste našli a ten vyřešte a celé to opakujte.


Můžete se setkat s frází SRE implementuje DevOPS. V SRE najdete lehce prolnutí s těmito pilíři a zároveň spousta těchto principů je rozpracována díky [SRE books](https://sre.google/books/) do větších detailů.


## SRE implementuje DevOPS pilíře

Projdu všechny pilíře a jak k nim přistupuje SRE:

1. Redukovat organizační sila

    SRE má společnou odpovědnost s vývojáři. SRE také používá stejné postupy a nástroje jako vývojáři.

2. Akceptoval, že chyby jsou normální

    SRE přijímá riziko. SRE kvantifikuje chyby a dostupnost pomocí [SLI, SLO](https://cloud.google.com/blog/products/gcp/sre-fundamentals-slis-slas-and-slos).

3. Implementovat postupné změny

    SRE doporučuje rychlé a postupné změny, kde se měří dopad a riziko se řídí pomocí [Error Budgets](https://sre.google/sre-book/embracing-risk/).

4. Využijte nástroje a automatizaci

    SRE využívá svůj čas mimo oncall k likvidaci toil (manuálních úkonů), automatizaci procesů a vylepšení spolehlivosti.

5. Měřte všechno

    Základ všeho je v SRE měření na všech úrovních od vývoje až po produkci. Já třeba považuji metriky jako [Four Keys](https://cloud.google.com/blog/products/devops-sre/using-the-four-keys-to-measure-your-devops-performance) za velmi důležité a pomáhají řízení týmů napříč odděleními. V SRE knize se dočtete o [SRE Pyramid](https://sre.google/sre-book/part-III-practices/), která má jako základ monitoring a na tom opravdu stojí veškerá SRE práce.


Pokud si se chcete o SRE více vědět tak mě sledujte na [twitteru](https://twitter.com/abtris). Čtěte [SRE weekly](https://sreweekly.com/), zúčastněte se konferencí [SREcon](https://www.usenix.org/srecon) nebo mi prostě napište email. Rád poradím jak na vlastní SRE tým pokud ho potřebujete.
