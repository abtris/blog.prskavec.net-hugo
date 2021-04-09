---
categories: [konference]
comments: true
date: 2016-07-14T00:00:00Z
title: SREcon'16 Europe
url: /2016/07/14/srecon-16-europe/
---

Letošní [SREcon](https://www.usenix.org/conference/srecon16europe) byl zase v Dublinu. Od 11.7. do 13.7. se zde setkali velcí hráči (Google, Facebook, Microsoft, Amazon) s těmi menšími a vyměňovali si spoustu zkušeností. Letos jsem se mohl poprvé zúčastnit. Nebyl jsem z ČR sám, zastoupení měli Avast, Seznam nebo Skype či Algolia. Dohromady asi 5 lidí.
Konference byla vyprodaná a hodně míst měli lidé z Googlu a pokud vás zajímalo jak se pracuje v Dublinském Googlu nebo Facebooku mohli jste si o tom s lidmi promluvit.
Celá konference byla ve 4 sálech a to jeden hlavní, který se po keynote rozdělil na dva a potom poslední dva byli hlavně pro workshopy a lighting talky.

## Co to SRE je?

Pokud nevíte [co to je SRE](https://blog.prskavec.net/2016/03/co-to-je-sre/), tak kromě mého článku existuje skvělá kniha [Site Reliability Engineering](https://shop.oreilly.com/product/0636920041528.do) od lidí z Googlu, kde se všechno detailně vysvětluje a skoro každý přenášející něco z knihy citoval. Je to taková bible SRE a vůbec první kniha zastřešujíc tento obor.

<!--more-->

## Nejzajímavější přednášky

K přednáškám chci jen dodat, jsou to mé postřehy, pokud vás to zaujme měli by být videa za pár týdnů veřejně dostupné na youtube.

### Splicing SRE DNA Sequences in the Biggest Software Company on the Planet - Greg Veith, Microsoft

Greg mluvil o transformaci v Microsoftu a jeho práci na zavádění SRE do práce týmů kolem MS Azure. Začali budovat SRE kolem roku 2009 a doteď transformace probíhá nejen v MS Azure, ale v celé firmě. Pro zavedení byli klíčové tři věci. Za prvé vlastní start SRE týmu, zavedení principů. Za druhé aplikaci principů, aby jste model otestovali. Za třetí musíte akcelerovat a vylepšovat ho tak, aby zahrnul celou firmu. Gregova přednáška byla také hodně o tom jak je MS velký firma a tak je to práce pro hodně lidí, přesto hlavní SRE tým je asi jen 5-7 lidí.

Diskuze o tom kolik SRE lidí má být ve firmě byla celkem častá. Osobně se mi libí názor z LinkedIN, kde uvádějí 1:10 poměr SRE a Engineeringu. U velkých firem jako je Google je to asi 5%. Tomu 1:10 odpovídáme i v Apiary a myslím, že je dobré se toho držet.


### Doorman: Global Distributed Client-Side Rate Limiting - Jos Visser, Google

Jos mluvil o projektu [Doorman](https://github.com/youtube/doorman), který používají v Youtube limitaci zdrojů v distribuovaném systému pro omezení zátěže MySQL. Doorman je napsaný v Go, používá pro komunikaci gRPC. Zajímavé je hlavně to, že nemá žádný diskový prostor pro ukládání stavu a drží informace jen v paměti. Pokud dojde k výpadku tak se pustí learning mode a od klientů se dozví všechno co potřebuje a potom pokračuje ve funkci.

 ### Building and Running SRE Teams - Kurt Andersen, LinkedIn
Kurt mluvil o knize [Team of Teams](https://www.amazon.com/Team-Teams-Rules-Engagement-Complex/dp/1591847486) a jak aplikovat vojenské postupy do SRE a systému rozhodování. Jako klíčové viděl hlavně posun důležitých rozhodnutí na lidi co jsou nejblíže tomu tu akci potom vykonat.


### The Production Engineering Lifecycle: How We Build, Run, and Disband Great Reliability-focused Teams - Andrew Ryan, Facebook

Andrew mluvil o Production Engineeringu (PE) ve Facebooku. Je to obdoba SRE. Mají asi 700 lidí v šesti kancelářích po celém světe. Drží poměr 1:10 podobně jako LinkedIn. Hodně řeší nakolik je v každém týmu potřeba PE a zda je produkt opravdu tak důležitý pro firmu, aby bylo nutné mít oncall (min. 12+ PE). Pokud ne tak se oncall přesune na vývojáře a PE může působit jako poradce jak věci zlepšit. Naopak na core produktech mají například pro cache pět paralelních rotací na oncallu.

### How to Improve Your Service by Roasting It - Jake Welch, Microsoft

Jake mluvil o tom jak přenést znalosti o komplexních systémech od autorů k dalším lidem do firmy. Doporučil vytvořit skupinku a věnovat 45min týdně po dobu 10 týdnů na předávání znalostí. Je důležité, aby to někdo řídil. Role Roast Master je pro úspěch klíčová. Musí hlídat jak tón řeči a vůbec vyjadřování, aby to nesklouzlo k osobním antipatijím a drželo se to v racionální rovině. Agendu je potřeba správně rozdělit po komponentách nebo subsystémech a držet se toho, na konci potom stanovit co se bude probírat příště a nepřekračovat čas 45min.


### What SRE Means in a Start-up - Brian Scanlon, Intercom

Brian mluvil o SRE ve startupu, kde je situace jednoduší pokud se prostě nastaví politika od začátku, často se používá hostovaná infrastruktura, která celou věc usnadňuje. Celkem to kopírovalo to jak to děláme mi v Apiary.

Hodně je vidět jaký velký rozdíl a kolik práce to dá pokud chcete firmu předělat na SRE (brownfielding) nebo začnete hned.

### The Many Ways Your Monitoring Is Lying to You - Sebastian Kirsch, Google, skirsch@google.com

Skvělá přednáška o tom jak nemůžete věřit každému grafu v monitoringu. Doporučuji schlédnout až bude video, bylo tam hodně příkladů. Jde o to, že věci, které uváděl nejsou pro mě například vůbec nové díky lekcím z numerické matematiky a statistiky na ČVUT, ale ne každý má statistický background a nebývá to až tak běžná součást Computer Science.

### Next-generation Alerting and Fault Detection - Dieter Plaetinck, [raintank](https://raintank.io)
Dieter hodně zdůrazňoval použití machine learning na detekci anomálií a to jak to prakticky moc nefunguje a proto se uchylujeme k streamovanému zpracování dat pomocí nástrojů jako je Spark nebo Riemann.io. Ale nejzajímavější věc zmíněná v přednášce je [Bosun](https://bosun.org), také IDE pro alerting, kde se dá dělat historický testing, ladění alertů, vyhodnocování na základě dalších dat. Pro podrobosti doporučuji projít články na [blogu](https://dieter.plaetinck.be).


### DNS @ Shopify - Emil Stolarsky, Shopify
Krátký lighting talk o mangementu DNS používající Git a CI. Čerstvé open source přímo před konferencí.

- https://github.com/Shopify/record_store
- https://github.com/Shopify/ejson



### Availability Objectives of SoundCloud’s Microservices - Bora Tunca, SoundCloud

SoudCloud je autorem Prometheus.io monitoringu a měli dvě přednášky co jsem viděl. Mají dnes microservice architekturu rozdělenou do několika vrstev a podle toho je daná požadovaný dostupnost a také pracují s graceful degradation pokud to jde. Jako příklad jsou třeba statistky k jednotlivým trackům. Modul stats má SLO 95% a tracks mají 99.995% a tak je to zohledněné i na UI.

### The Knowledge: Towards a Culture of Engineering Documentation - Riona MacNamara, Staff technical writer, Google

Riona mluvila o hrozném stavu dokumentace v Google. Přirovnala ji k testování v roce 2005. Jak to celkově zhoršuje produktivitu, spolehlivost a rychost. Zkoušeli to několikrát vyřešit pomocí vytvoření komplexního dokumentačního systému, který vytvoří někdo pro celou firmu a zeshora se to prosadí.

Tento přístup nefungoval a proto v roce 2014 úplně změnili přístup, začali ze zdola nahoru od jednotlivých engineerů a vytvořili systém g3doc, který dnes používá 10k projektů v google a mají přes 17k autorů, kteří do dokumentace přispívají.

Klíčové bylo dát dokumentaci přímo do repositářů projektu, mít to v jednoduchém formátů čitelném přímo v IDE. Zvolili markdown, který si potom silně vylepšili. Mají potom na serverové straně systém, který vygeneruje HTML a publikuje dokumentaci na url, které se dá lehce podle jména projektu odhadnout.

Projekt bohužel není open source, ale snad jednou bude. Díky spoustě googlerů, kteří do projektu v rámci svých 20% přispěli je tam hodně funkcí, které mi máme díky Sphinx.

## Odkazy na zdroje

- https://www.franklinangulo.com/blog/2016/7/11/srecon-2016-dublin-day-1
- https://www.slideshare.net/goldshtn/the-next-linux-superpower-ebpf-primer
- https://github.com/dastergon/awesome-sre

Budu se snažit průběžně doplňovat zdroje a přidám odkazy na videa až budou.

## Shrnutí

Pokud se zajímáte o transformaci engineeringu a říká vám něco DevOps nebo SRE je to konference pro vás. Pokud vás to zajímá a chtěli by jste si o tom promluvit, klidně mě kontaktujte. Bohužel jsem nenašel vhodnou konferenci v Česku, kde o tom mluvit, aby se to dostalo více do povědomí firmem.

Další SREcon bude příští rok zase v USA (March 13–14, 2017, SF), Evropě (Dublin) a také poprvé v Asii (May 22–24, 2017, Singapur).
