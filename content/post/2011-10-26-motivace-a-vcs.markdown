---
categories: [vcs]
comments: true
date: 2011-10-26T00:00:00Z
title: Motivace a Verzovací systémy
url: /2011/10/26/motivace-a-vcs/
---

## Proč bychom potřebovali verzovací systém?
Toto je častá otázka, kterou slyším. Spousta lidí používá sdílené adresáře pro práci s dokumenty. Přejmenovávají soubory a adresáře. Pokud znáte dokumenty typu Projekt-Final-Update!!.doc apod. tak víte o čem mluvím.

## Každý programátor pracuje v jiné podadresáři?
Tak to trochu může lidem připadat, ale programátoři používají verzovací systém, který dokumentovou databází, která sleduje změny a pomůže v mnoha věcech.

<!--more-->

### Záloha a Obnova 
Soubory, které ukládáme do verzovacího systému si nesou infomaci o času uložení a není problém skočit v čase co existuje repozitář kam chcete třeba co jste dělali 29.7.2009? Není problém.

### Synchronizace
Pokud si verzujete například konfiguraci nastavení linuxu. Příklady najdete na githubu v repositářích nazvaných [dotfiles](https://github.com/search?type=Repositories&language=&q=dotfiles&repo=&langOverride=&x=16&y=29&start_value=1). Tak si pomocí verzovacího systému lehce udržujete konfiguraci na všech počítačích, které používáte. Obdobně není problém to dělat s jakýmikoliv dokumenty.

### Jednoduché Undo
Modifikujete soubor a nefunguje to a potřebujete vrátit změny zpět. Není problém obnovit poslední dobrou verzi. Maličkost.

### Undo
Kdykoliv máte k dispozici undo, pokud například najdete chybu, kterou jste udělali před rokem není problém se vrátit zpět a podívat se jaké změny jste dělali a proč.

### Sledování změn
Soubory jsem změněny, ale verzovací systém uchovává také zprávu, kde se často vysvětluje proč změna byla udělaná, dost často obsahuje například odkaz do bug trackeru apod. To jsou věci, které se často ze samotných souborů vyčíst nedají.

### Sledování vlastníka
Každá změna je podepsaná a můžete kdykoliv zjistit, kdo který řádek modifikoval. To se hodí pokud potřebujete zjistit kdo změnu udělal.

### Pískoviště (sandbox)
Výhoda verzovacích systémů je právě, že můžete pracovat na velkých změnách v izolované oblasti bez toho aby jste se báli, že ovlivníte celek. 

### Větve a merge
Větve slouží jako velké pískoviště, můžete mít desítky větví, které mohou po dlouhou dobu fungovat izolovaně a později pokud budete chtít spojíte (merge) je kam potřebujete.

### Závěr
Sdílené adresáře jsou rychlé a jednoduché, ale rozhodně tyto vlastnosti verzovacích systémů jsou něco co je o dost lepší.

Verzovacích systémů jsou desítky, od SCCS (1972), přes CVS (1990), Subversion (2000) k distribuovaným jako je Darcs (2002), Git (2005) nebo Mercurial (2005). K nejnovějším přírůstkům patří Verocity (2011), které přidává například přímo podporu SCRUMu.

Volbu nechám na vás u mě od roku 2009 [vítězí Git](http://blog.prskavec.net/2009/11/proc-jsem-presel-z-mercurial-na-git/) a ani za 2 roky se v tom nic nezměnilo.
