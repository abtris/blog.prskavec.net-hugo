---
date: 2010-04-30T00:00:00Z
published: true
status: publish
tags:
- events
- scm
- svn
- teched
title: Subversion dnes a zítra
type: post
url: /2010/04/30/subversion-dnes-a-zitra/
---

<!-- 		@page { margin: 0.79in } 		P { margin-bottom: 0.08in } -->Já osobně jsem začal pracovat s někde kolem roku 2000 se systémem CVS z kterého jsem nebyl vůbec nadšený, hned jak jsem se pokusil něco přejmenovat. Později jsem přešel na Subversion, který mě v té době nadechl a to mi celkem vydrželo po dlouhou dobu než jsem před několika lety začal koketovat s distribuovanými systémy a teď asi rok už aktivně používám Git a pře ním jsem si hrál s Bazaar a Mercurialem. Na Subversion jsem, ale nijak nezanevřel a stále ho používáme ve firmě jako jediný verzovací systém v poměrně slušném nasazení (asi 100 repositářů, několik desítek GB místa). Znám limity Subversion a podle toho s ním zacházím.

Verzovací systémy jsou tu už 38 let, první verzovací systém SCCS vznikal v Bellových laboratořích v roce 1972. Dodnes existuje na mnoha unixových systémech v GNU variantě.

Protože SCCS to byl proprietární verzovací systém, vytvořil na Walter F. Tichy  na Pordue University open source variantu tohoto software pojmenovanou RCS. RCS se dodnes používá například v Twiki pro uchovávání historie stránek.

Následovali další jako CVS, PRCS (Project Revision Control systém). CVS byla původně jen nadstavba na RCS, sada skriptů, které používali příkazy RCS. Většinu vlastností CVS má poděděnou od RCS.

Subversion vznikl nový verzovací systém, který odstraní chyby co lidem vadili v CVS a byl napsán úplně znova i když filozoficky se hodně držel původního CVS, ale byl tree oriented, což přineslo některé výhody, ale i nevýhody.

Protože řada vývojářů nebyla s CVS spokojená vznikali další systémy a za některými stál i Microsoft, který přišel se svým verzovacím systémem Delta v roce 1994. Tento systém dlouho nevydržel, protože MS udělal výhodou akvizici a koupil firmu SourceSafe a další rok už vydal nový verzovací systém VSS. Tento systém měl mnoho nevýhod a pokud někdo s ním pracoval určitě mi dá za pravdu, že to nebylo jednoduché. Problémy se zamykáním souborů, přenos přes sdílené disky apod. V roce 2005 naštěstí MS představil moderní centralizovaný systém, který je součásti TFS, který jako datové úložiště používá MS SQL.

Larry McVoy je klíčová osobnost návrhu verzovacích systémů. Stál u vývoje verzovacího systému  Sun TeamWare, který vedl Evan Adams a potom na základě těchto zkušeností vytvořil BitKeeper. To jsou první distribuované verzovací systémy. TeamWare byl postavený na základech SCCS, přesto přinesl spoustu nových věcí, které dali později vzniknou dnešním systémům. Implementace TeamWare začala v roce 1991, to můžeme považovat za datum vzniku distribuovaných systémů.

TeamWare to začal, ale BitKeeper může za vznik Gitu a Mercurialu, kteří patří dnes mezi nejznámější distribuované verzovací systémy. Společnost BitKeeper se na základě událostí v roce 2005 rozhodla odebrat (reverse enginnering na linux konferenci pomoci telnetu) volné použití linuxové komunitě, která BitKeeper používala k vývoji jádra.

To vedlo ke vzniku Gitu (Linus Torvald) a Mercurial (Matt Mackall) v roce 2005. Git je inspirován BitKeeperem a Monotone (sha1 hash), také velkou část práce na něm udělal Petr Baudiš, který stojí například za frontendem Cogito  a příklad git-filter-branch.

Výhody Gitu vidím v těchto vlastnostech:
<ul>
	<li>pružnost 	(moderní různá worklow)</li>
	<li>lokální 	větve</li>
	<li>jednoduchý a 	rychlý merge</li>
	<li>rychlost a 	offline commit</li>
	<li>vysoký 	failover, díky lokálním kompletním kopiím</li>
</ul>
Tak proč všichni nepoužívají Git, ale Subversion? Je proto několik důvodů.
<ol>
	<li>ne všichni 	považují tyto výhody za důležité, priority mají jinde</li>
	<li>pro SVN mluví 	vysoká rozšířenost, snadná dostupnost v mnoha IDE</li>
	<li>vynikající 	TortoiseSVN, který pomáhá více než by se na první pohled mohlo 	zdát</li>
	<li>dostupný 	placený support včetně tvoření balíků pro linux, windows 	(CollabNet)</li>
	<li>http, apache 	pro snadné začlenění do každé sítě</li>
	<li>napojeni 	ldap, active diretory nastavení práv až na úrověň každého 	adresáře</li>
	<li>znalost</li>
</ol>
Proto více než 80% firem dnes používá Subversion.

Dnes je důležité používat určitě aktuální verzi Subversion, vůbec nedoporučuji pracovat s verzemi 1.4 a nižší, které nepodporovali mergeinfo. Subversion má desetileté výročí. Ale stále je to malé dítě, které potřebuje dospět. Už jsou pryč dětské nemoci, ale některé problémy přetrvávají.

Subversion byl začleněn pod Apache Foudation do toho doufám, že se trochu akceleruje vývoj Subversion k následujícím verzím. Tým kolem SVN si je dobře vědom věcí, které SVN scházejí a dobře vědí, že to není jednoduché je odstranit. Proto existuje tato roadmapa.

Je to roadmapa přibližně na 4 roky dopředu, klíčový bude letošek a uvidíme jak se podaří vývoj verze 1.7 o které budu teď. K ostatním verzím se vrátím později.

Verze 1.7 by měla obsahovat tyto novinky
<ul>
	<li>httpv2</li>
	<li>wc-ng</li>
	<li>svn patch</li>
</ul>
Nová verze HTTPv2 by měla přinést větší rychlost při zachování všech výhod http protokolu.

Pracovní kopie nové generace je důležitá součást nutná pro další následné změny, které odstraní dnešní problémy (tree conflict, rename tracking). Bude to sqlite databáze v jednom adresáři .svn, kde budou všechny potřebná metadata o repository.

Svn patch jen umožní to co umí příkaz patch v linux, drobnost, která v svn chyběla.

Ve verzi 1.8, která by měla být za rok bychom se snad díky nové WC-NG mohli již dočkat sledování přejmenovaných souborů napříč historii. Nastavení pro jednotlivé repository ne jen globálně a vylepšení problémů s tree conflicty.

Co bude v dalších verzích uvidíme, autoři slibují nový FS, nový editor a další vylepšení. Uvidíme zda se do budoucna dočkáme třeba i offline commitů.

Na závěr bych chtěl shrnout co by jste si měli odnést těchto 4 věci:
<ul>
	<li>pokud máte 		subversion, ujistěte se že používáte poslední verze</li>
	<li>pokud chcete 		přejít na DVCS mějte pro to dobré zdůvodnění nedělejte to 		je kvůli módě</li>
	<li>na 		subversion se stále pracuje</li>
	<li>existují 		distribuované verzovací systémy a přinášejí něco nového</li>
</ul>
Tento článek je přepis mojí přednášky na <a href="http://www.slideshare.net/ladislavprskavec/subversion-dnes-a-ztra">Teched 2010</a>.
