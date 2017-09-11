---
date: 2008-04-10T00:00:00Z
published: true
status: publish
tags:
- internet
- scm
- subversion
title: StatSVN a FishEYE
type: post
url: /2008/04/10/statsvn-a-fisheye/
---

<a href="http://blog.prskavec.net/?p=49">Nedávno</a> jsem si tu stěžoval, že sháním něco co dává náhled na práci se SVN, sám jsem si naprogoramoval malý jednoduchý tool, který zobrazuje aktuální working copy (wc)  u mě na disku a k tomu příslušné changelogy, které se generují skriptem. Rozhodně jsem si nemyslel, že by něco takového nenapdalo nikoho před tím. Časem jsem narazil na dva produkty, které zhruba splňují co jsem od toho čekal.

První je StatSVN a druhý FishEYE. Nevýhodou FishEYE je jenom cena $1200 pro 10 uživatelů není pro organizaci moc, ale pro mě ano.

FishEYE je webová aplikace běžící pod Tomcatem napsaná v Javě. Umožňuje skvělý náhled na changelog v repository, krásně zobrazuje diffy mezi verzemi, příslušnou historii, ukazuje i smazané adresáře v předchozích revizích. Náhled na to jak se rozrůstá projekt ukazuje Line history, při nějaké stagnaci nebo prudké změně se dá lehce najít co se dělo. K dispozici máte kompletní informace na jednom místě a vlastní repository mohou být klidně na různých serverech a FishEYE si dělá vlastní index a po přidání celého většího repository to chvíli trvá než si ho zaindexuje.

<img style="border-top-width: 0px; border-left-width: 0px; border-bottom-width: 0px; border-right-width: 0px" src="http://blog.prskavec.net/wp-content/uploads/2008/04/image1.png" border="0" alt="image" width="477" height="476" />

StatSVN je opensource projekt, poskytuje podobné informace. Je také napsán v Javě, ale funguje jako řádkový interpretr, který pošlete na log exportovaný ze SVN. Funguje to trochu méně komfortně, ale výsledky jsou podobné.

Nejdříve potřebujete wc vašeho projektu ze SVN. Typický příkaz na řádce vypá takto:

<code>svn co svn://server/repo/trunk/modulename</code>

Samozřejmě můžete checkout udělat třeba pomocí TSVN. Potom je potřeba udělat log ve formátu XML. To bohužel TSVN pokud vím zatím neumí a na to potřebujete příkaz:

<code>svn log -v --xml &gt; logfile.log</code>

Potom už můžete spustit StatSVN a pohnat ho na log.

<code>java -jar /path/to/statsvn.jar /path/to/module/logfile.log /path/to/module</code>

StatSVN vytvoří HTML report v aktuálním adresáři (<a href="http://www.softwareengineering.ca/statsvn/statsvn/">ukázka takového reportu</a>).
<ul>
	<li>Homepage StatSVN <a title="http://www.statsvn.org/" href="http://www.statsvn.org/">http://www.statsvn.org/</a></li>
	<li>Homepage FishEYE <a title="http://www.atlassian.com/software/fisheye/" href="http://www.atlassian.com/software/fisheye/">http://www.atlassian.com/software/fisheye/</a></li>
</ul>
