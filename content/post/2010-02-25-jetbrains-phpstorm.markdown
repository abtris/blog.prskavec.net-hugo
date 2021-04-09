---
date: 2010-02-25T00:00:00Z
tags:
- ide
- idea
- php
- phpstorm
title: JetBrains PhpStorm
type: post
url: /2010/02/25/jetbrains-phpstorm/
---

Vývojových prostředí pro PHP je docela hojnost. Sám používám v práci Zend Studio 7.1 a doma Netbeans 6.8. Minulý rok přišla firma JetBrains, která je známá svým IDE pro Javu IDEA, s vývojovým prostředím pro PHP, které se jmenuje <a href="https://www.jetbrains.com/webide/features/index.html#PHP_IDE">PhpStorm (dříve WebIDE)</a>. V současné době je nové IDE stále ve vývoji. Brzo se dočkáme beta verze a myslím do léta snad i finální verze.

<a href="https://blogs.jetbrains.com/webide/">PhpStorm</a> (dále PS) je na platformě IDEA, což považuji za velkou výhodu. Dají se použít pluginy pro Ideu v PhpStorm. Kvalita pluginů v Idea mi přijde o poznání lepší než pro Eclipse.

[caption id="attachment_818" align="aligncenter" width="300" caption="JetBrains PhpStorm (WebIDE)"]<a href="https://blog.prskavec.net/wp-content/uploads/2010/02/PhpStormWI-94.335-1.png"><img src="https://blog.prskavec.net/wp-content/uploads/2010/02/PhpStormWI-94.335-1-300x266.png" alt="" width="300" height="266" class="size-medium wp-image-818" /></a>[/caption]

<strong>Hlavní přednosti vidím v těchto bodech:</strong>
<ul>
	<li>Podpora pro Git, je to jediné IDE s opravdu slušným pluginem. Podpora pro SVN je samozřejmě také. Zkoušel jsem plugin pro Git v Netbeansech i Eclipse a nikde mi to moc dobře nefungovalo, nebo mi tam chyběli potřebné příkazy.</li>
	<li>Vynikající editor, který je o poznání chytřejší než například v Netbeans. Je to vidět zvláště pokud něco refaktorujete. Pozná zda funkce již závorky má či nemá, nedoplňuje dvojité uvozovky nesmyslně jak se mi to stává často v Netbeans.</li>
	<li>Podpora XSLT, XML. Pokud používáte jako šablonovací systém XSL tak to velmi ulehčuje práci. Podpora pro XSLT je i v Zend Studiu, ale tady to mají vyladěné do detailů. Mě to funguje spolehlivěji než v Zendu.</li>
	<li>Multiplatformost je daná tím, že aplikace je napsaná v Javě a proto není problém ani Windows, Linux nebo Mac.</li>
	<li>Podpora pro Smarty (pokud používáte)</li>
	<li>Podpora pro PhpUnit</li>
	<li>Podpora pro Debuggery (xdebug už funguje, zend debugger slibují)</li>
	<li>Podpora pro Phpdoc (doplňování) </li>
	<li>Editor s dopňovaním pro JS a HTML</li>
</ul>

Pěkná věc je třeba produktivity guide, radí co a jak dělat lépe:
[caption id="attachment_828" align="aligncenter" width="300" caption="JetBrains PhpStorm Productivity Guide"]<a href="https://blog.prskavec.net/wp-content/uploads/2010/02/Screenshot-Productivity-Guide.png"><img src="https://blog.prskavec.net/wp-content/uploads/2010/02/Screenshot-Productivity-Guide-300x262.png" alt="JetBrains-PhpStorm-Productivity-Guide" width="300" height="262" class="size-medium wp-image-828" /></a>[/caption]

<strong>Nevýhody a nejasnosti v současnosti</strong>
<ul>
	<li>Horší podpora formátovaní, chybí podpora checkstyle. Formátování lze celkem detailně sice nastavit, ale zatím nefungoval náhled a chtělo by to podporu pro PEAR, Zend checkstyle.</li>
	<li>Zatím neznámá licenční politika a cena, ale předpokládám ze to bude podobné jako Zend studio, kterému chtějí konkurovat jak uvedli minulý rok na konferenci ZendCon'09</li>
	<li>Zatím na linuxu chybí installler, na windows jsem to nezkoušel.</li>
	<li>Chybí globální nastavení pro include path pro doplňovaní syntaxe, musíte do projektu přidat zatím ručně, líbilo by se mi to v globálním nastavení pro PHP.</li>
	<li>Nepodporuje worksety. V Eclipse jsem si oblíbil worksety pro různé typy projektů (Zend, Nette, Examples,...), to mi přijde užitečné, ale není to nutné.</li>
	<li>Chybí UI pro Phpunit, který je moc pěkně udělaný v Netbeans. Pokud programujete podle TDD je to dost užitečné.</li>
</ul>

<strong>Závěr</strong>
Myslím, že mezi IDE, které jsou zdarma mi přijde v současné době nejlepší asi Netbeans. Vývoj postupuje celkem pěkně dopředu, vylepšené automatické formátování v 6.9 bude jistě přínosem. Jedinou nevýhodu vidím v editoru, který se občas chová divně, ale dá se to přežít.

Z komerčních znám jen PhpStorm a ZendStudio a přijde mi práce v obou podobná. Jen v PhpStrormu jsou trochu dál. Je to tím, že IDEA jako prostředí je daleko před ostatními a spousta funkcí v něm obsažená pro Javu se do ostatním Java IDE pomalu dostává. Pro PHP je IDE od JetBrains sice nové, ale oni mají velké zkušenosti s vývojem IDE a myslí to s konkurencí pro Zend Studio vážně a na té práci je to vidět.

Pokud máte zkušenosti s PhpStorm na jiných platformách podělte se s ostatními v komentářích.
