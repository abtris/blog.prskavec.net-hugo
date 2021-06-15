---
title: "Software Development versus Software Engineering"
date: 2021-06-15T09:30:42+02:00
---

Pokud si vyhledáte tyto pojmy narazíte na mnoho zdrojů, které ten rozdíl myslím, že celkem dobře popisují.

> The difference between software engineering and software development begins with job function. A software engineer may be involved with software development, but few software developers are engineers. [^1]


> "A software engineer is involved in software development. Not all software developers are engineers." [^2]

Software Development je prostě jen podmnožina práce, kterou dělá Engineering. Ale pak přijde otázka, mám teda najímat Software Engineering nebo Software Developers? Když čtu na CV, že člověk má vystudovaný software engineering a jiný software development bude to rozdíl?

My se snažíme najmout Software Engineers, ale často dostaneme Software Developers. Je to totiž problém pokud je člověk po škole bez zkušeností tak těžko se některé principy uchopí hned od začátku obzvláště na velkých distribuovaných systémech jako je vývoj IaaS.

Nikdo to nemůže mít nováčkům za zlé protože ani mi jsme ty zkušenosti před lety neměli, je potřeba na to myslet a dát jim dobrého mentora a případně do týmu architekta, který pohlídá, aby to moc nepokazilo.

Před lety v začátcích Apiary si pamatuju jak [Zdeněk Němec - @zdne](https://twitter.com/zdne) říkal: "Hlavně neprogramujte!". Mnoho vývojářů má totiž tendence jakýkoliv problém řešit hned programováním a tak se nabaluje váš kód často ne úplně s dobrým řešením. Chybí analýza problému, zamyšlení nad tím zda třeba smazání kódu není taky řešení, jak bude věc škálovat a hlavně jak se to bude udržovat v budoucnu. Každý kus kódu, který přidáte totiž musíte udržovat a to je řádově větší práce než ho napsat.

Za ta léta jsem viděl často, že “rozumné” rozhodnutí v jednom čase nadělá problémy později. Nemůžete zároveň předvídat všechno, ale je dobré mít dvě věci, které problémům pomohou.

Dokumentaci rozhodnutí jak a proč jste to udělali, z čeho architektonické rozhodnutí, volba knihovny apod. vycházeli co jste zvažovali apod. [Architectural Decision Records
](https://adr.github.io) nebo [RFC](https://works.hashicorp.com/articles/rfc-template) jsou pro to dobré řešení, to vám dá analýzu a zároveň pokud se k tomu někdo vrací později, může dobře pochopit context vašich rozhodnutí a pomůže mu to s jeho řešením.
Testy, které problém dobře zapouzdří, aby se jakýkoliv refactoring dělal mnohem snadněji.

Pokud vývojáři mají ještě v mysli praktiky z Clean Code [^4] nebo podobné jako je Refactoring [^5]. Mě kdysi otevřela oči kniha Refactoring Ruby [^6]. Tyto praktiky jsou užitečné pro každého kdo se snaží psát kód v týmu udržitelným stylem.

Bohužel dost velká část kódu dnes není napsaná jen vývojáři a jako SRE se setkáváme často s masivním kódem, který vůbec nesplňuje základy toho co tu zmiňuji. Potom je velká práce upravit kód, aby se daly vyřešit problémy se škálováním nebo performance.

Často dnes se tlačí na DevOps a týmy, které mají všechno na triku od vývoje až po provoz a ač si myslím, že bourání bariér mezi odděleními (QA, Dev, Ops) je dobrá věc tak zároveň si nemyslím, že to je jednoduché právě z toho hlediska, že ne každý je dobrý ve všem a často ani nechce a je spíš donucen okolnostmi řešit věci, které nechce a nebo nemá zkušenosti a nedostane se mu podpory od zkušenějších, protože je v týmu nemají apod. Nedostatek rolí, lidí a schopností v týmech je velký problém pro většinu manažerů v IT [^3], kteří ze skupiny vývojářů musí vytvořit tým, který má rozumět všemu.

Jak se na tuto problematiku koukáte vy? Je to něco co vás trápí pokud vyvíjíte produkty co tu mohou být roky.

[^1]: https://www.computersciencedegreehub.com/faq/software-engineering-vs-software-development/

[^2]: https://www.fivestardev.com/blog/whats-the-difference-between-software-engineering-and-software-development

[^3]: [T. Winters, T. Manshreeck, and H. Wright, Software Engineering at Google: Lessons learned from programming over time. Sebastopol, CA: O’Reilly Media, Inc., 2020.
](https://abseil.io/resources/swe-book)

[^4]: R. C. Martin, Ed., Clean code: a handbook of agile software craftsmanship. Upper Saddle River, NJ: Prentice Hall, 2009.

[^5]: M. Fowler, Refactoring: improving the design of existing code, Second edition. Boston: Addison-Wesley, 2019.

[^6]: J. H. Fields Shane/ Fowler, Martin/ Beck, Kent (CON), Refactoring: Ruby Edition. Prentice Hall, 2013.
