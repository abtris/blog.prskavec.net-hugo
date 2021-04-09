---
tags:
 - scm
 - git
date: 2011-10-31T00:00:00Z
title: Průřez historií verzovacích systémů
url: /2011/10/31/prurez-historii-verzovacich-systemu/
---

<iframe src="https://player.vimeo.com/video/31341006?title=0&amp;byline=0&amp;portrait=0&amp;color=ffffff" width="430" height="242" frameborder="0" webkitAllowFullScreen allowFullScreen></iframe>

Na videu je vidět průřez časovou osou jak čas běžel a jaké verzovací systémy nám postupně vznikali a vznikají.

Verzovací systémy jsou tu od roku 1972. Source Code Control System (SCCS) byl první verzovací systém, který položil základ všem verzovacím systémům až po dodnes.
Dnes jsou nejznámější systémy: Subversion, Git a Mercurial. Ale každý měl nějaké předchůdce a vznikl protože ty předchozí nevyhovovali.

<!--more-->

SCCS byl komerční a nahradil ho později Revision Control System (RCS), který byl open source náhradou za SCCS, na jeho základech byl v roce 1990 vytvořen Concurrent Versions System (CVS), který většina vývojářů už zná. Mezi jeho hlavní nevýhody jsem vždy považoval nemožnost přejmenování adresářů a správu jednotlivých souborů. Subversion v roce 2000 přišel s novinkou ve verzovaní celého stromu adresářů. Atomicita operací se rozšířila na celé adresáře ne jen na soubory jako v CVS. Subversion podporuje attributy, které se přidávání k souborů a s těmito metadaty dále pracuje.

SCCS ale kromě verzovacích systémů, které mají architekturu klient server byl také předchůdcem distribuovaných verzovacích systémů. První distribuovaný verzovací systém byl v roce 1996 Sun WorkShop TeamWare. Larry McVoy, který navrhl TeamWare vytvořil později BitKeeper, který se používal do roku 2005 pro vývoj jádra linuxu. V roce 2005 po sporu mezi společností BitKeeper a komunitou, přestali poskytovat zdarma BitKeeper a tak se Linus Torvalds rozhodl začít s vlastním verzovacím systémem Git. Na stejnou věc reagoval i Matt Mackall, když o měsíc později po Gitu uvedl Mercurial.

Verzovacích systémů je samozřejmě desítky, stále vznikají nové a spousta jich žije i když je asi dost lidí ani nezná. Nedají se vyzkoušet všechny. Osobně mám nejvíce zkušeností s tím čím jsem si prošel v práci a to je CVS, SVN a Git. Za ty roky co s nimi pracuji si myslím, že jsem si vždy polepšil a uvidíme co bude dál. Dnes považuji Git za naprosto dostačující k mé práci, jediné co bych mu vytknul je verze pro windows, kde často řeším problémy, které na linuxu ani neexistují.


