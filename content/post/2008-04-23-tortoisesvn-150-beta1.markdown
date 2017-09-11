---
date: 2008-04-23T00:00:00Z
meta:
  _edit_last: "1"
  _encloseme: "1"
  _pingme: "1"
published: true
status: publish
tags:
- scm
- subversion
- windows
title: TortoiseSVN 1.5.0-beta1
type: post
url: /2008/04/23/tortoisesvn-150-beta1/
---

V nové verzi jsou toto hlavní novinky:
<ul>
	<li>Merge Tracking</li>
	<li>Cyrus SASL support for svnserve</li>
	<li>Sparse checkouts</li>
	<li>Changelist support</li>
	<li>Log message caching</li>
	<li>Repository browser</li>
	<li>Revision graph</li>
	<li>Client side hook <a href="http://www.darkcode.cc">scripts</a></li>
	<li>TortoiseMerge</li>
</ul>
Pro <strong>Cyrus SASL support for svnserve</strong> a <strong>Merge tracking</strong> bude potřeba i vlastní Subversion 1.5 pokud ho používate a nemáte TSVN zcela samostatně. Bude možný také opakovaný merge (Repeated Merge) a rozšíří se informace o každé Merge.

<a href="http://blog.prskavec.net/wp-content/uploads/2008/04/image8.png"><img style="border-right: 0px;border-top: 0px;border-left: 0px;border-bottom: 0px" src="http://blog.prskavec.net/wp-content/uploads/2008/04/image-thumb1.png" border="0" alt="image" width="644" height="189" /></a>

<strong>Sparse checkouts</strong> umožní checkout jen na část repozitory, což bude u velkých projektů hodně užitečné. K dipozici budou parametry podle kterých se provede volba.
<ul>
	<li>Fully recursive</li>
	<li>Immediate children, including folders</li>
	<li>Only file children</li>
	<li>Only this item</li>
</ul>
<strong>Changelist support</strong> mi docela chyběl, pokud pracujete na více problémech součastně a chcete je při commitu rozdělit do více části, které spolu souvisí, aby se dalo zpětně vysledovat, která změna patří ke které.

<a href="http://blog.prskavec.net/wp-content/uploads/2008/04/image9.png"><img style="border-right: 0px;border-top: 0px;border-left: 0px;border-bottom: 0px" src="http://blog.prskavec.net/wp-content/uploads/2008/04/image-thumb2.png" border="0" alt="image" width="473" height="246" /></a>

TSVN bude v nové verzi lokálně uchovávat kopii zpráv  (<strong>Log message caching</strong>), u vetších repository dojde i urychlení načítání.

<strong>Repository browser</strong> starý

<a href="http://blog.prskavec.net/wp-content/uploads/2008/04/image10.png"></a><a href="http://blog.prskavec.net/wp-content/uploads/2008/04/image11.png"><img style="border-right: 0px;border-top: 0px;border-left: 0px;border-bottom: 0px" src="http://blog.prskavec.net/wp-content/uploads/2008/04/image-thumb3.png" border="0" alt="image" width="268" height="211" /></a>

<strong>Repository browser v nové verzi</strong>, přibyl levý panel a umožňuje drag a drop operace.

<img style="border-right: 0px;border-top: 0px;border-left: 0px;border-bottom: 0px" src="http://blog.prskavec.net/wp-content/uploads/2008/04/image-thumb4.png" border="0" alt="image" width="309" height="211" />

Další věci mi nepřijdou tak důležité, vylepšený je <strong>TortoiseMerge</strong>, práce s konci řádek včetně MAC OS, Undo funkce se také hodí a možnost přímé editace není k zahození.

<strong>Revision Graph</strong> by měl vypadat lépe a mít nové možnosti, zatím jsem to netestoval. Jako dobé vylepšení jsou <strong>Client-side hook scripts</strong>. Ty umožnují na straně klienta provádět operace Start, Pre a Post u Commit a Update.

Ještě bych chtěl upozorňit na to, že verze 1.5 provádí upgrade formátu Working Copy, tak pozor při testování a nepřecházejte ukvapeně.
