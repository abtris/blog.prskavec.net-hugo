---
date: 2009-11-03T00:00:00Z
published: true
status: publish
tags:
- git
- mercurial
- scm
title: Proč jsem přešel z Mercurial na Git
type: post
url: /2009/11/03/proc-jsem-presel-z-mercurial-na-git/
---

Verzovací systémy už používám asi 5 let, vystřídal jsem CVS, Subversion, Mercurial, Bazaar a Git. V nedávné době v souvislosti také s tím, že Nette přešlo na Git a vůbec se spousta open source projektů přesunula na Github.com jsem také přešel na distribuovaný systém.

V práci používám Subversion a také přispívám do několika projektů, které pracují na Subversion. Dělal jsem migrace z CVS na SVN ve firmě, kde pracuji apod. Subversion má jednu velkou výhodu, kterou nemají distribuované systémy a to velmi dobré a detailní ACL a různé metody autentizace (LDAP, Active Directory). To si myslím udrží ve spoustě firem Subversion ještě po dlouhou dobu.

Ale protože pracuju na linuxu, pro verzování lokálních projektů a pracovních skriptů apod. používám Git, dříve Mercurial.

Proč jsem nejdříve zvolil Mercurial?
<ol>
	<li>Bitbucket, jednoduchý, přehledný, rychlý a privatní repository v free variantě (proti Github.com)</li>
	<li>Jednoduší přechod z SVN (viz <a href="http://svn.prskavec.net/ch07.html#id3029856">Přechod od Subversion k Mercurial</a>)</li>
</ol>
Proč jsem přešel na Git ?
<ol>
	<li><a href="http://www.karmi.cz/">Karel Minařík</a> mě přesvědčil o výhodách Gitu a odpovídal mě na dotazy, které jsem měl a předvedl mi killer feature (git-filter-branch).</li>
	<li>Mercurial mi při práci na projektu vyhodil tuto zprávu "files over 10MB may cause memory and performance problems".</li>
	<li>Zvykl jsem si na syntax Gitu, udělal jsem si hromadu aliasů a to chce prostě trochu čas.</li>
</ol>
Proč mi vadí to upozornění na přidání souboru většího než 10MB? Protože, pokud to tam mají určitě vědí proč. Performance je Gitu velká výhoda a já od verzovacího systému chci hlavně jednu věc a to verzovat cokoliv.

V současnosti používám Subversion, Mercurial i Git současně a nijak mi to nevadí. Git, ale preferuju pro nové projekty. Stávající ponechám tam kde jsou případně pořeším co udělám s těmi nepodporovanými keywords v Gitu, ale stejně mi nepřijdou v poslední době důležité, protože stejně jsou v každém souboru jiné a nenese to tu informaci, kterou většina chce a to je verze revize z SVN. Tohle spíše je lepší přesunout do build scriptu nebo na deployment.

Doufám, že jsem Borkovi Bernardovi odpověděl na co chtěl, případné dotazy do komenářů.
