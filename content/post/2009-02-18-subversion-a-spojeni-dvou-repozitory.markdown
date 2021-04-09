---
date: 2009-02-18T00:00:00Z
tags:
- scm
- svn
title: Subversion a spojení dvou repozitory
type: post
url: /2009/02/18/subversion-a-spojeni-dvou-repozitory/
---

Pokud se vám někdy stane, že pracujete na projektu a máte vzdálený SVN server dejme tomu např. s 500 revizemi.

Teď ale jedete někam pryč kde nemáte připojení k internetu nebo má server výpadek. Pracovat na projektu musíte, tak např. pomocí TSVN uděláte lokální repozitory a importujete working copy a pracujete dál, uděláte 50 revizí a server zase začne fungovat.
<h3>Co teď? Máte podle mne tyto možnosti.</h3>
<br />
<ol>
	<li>Můžete vzít zase svoji WC a tu naimportovat do původního repozitory</li>
	<li>Nebo zahodit staré SVN a udělat dump nového a to naimportovat místo starého.</li>
	<li>Nebo mít na serveru obě repozitory.</li>
	<li>Spojení obou repozitory</li>
</ol>
<h3>SVN Merge</h3><br />
Já jsem pro spojení obou repozitory, to se dá udělat také různě. Napadají mě dvě možnosti jak to provést.
<ol>
	<li>Vygenerovat diff patche pro jednotlivé revize v nové repozitory a ty aplikovat postupně na WC ze původního repozitory.</li>
<li>Udělat dump repozitory pomocí <code>svnadmin dump</code> a potom použít příkaz <code>svnadmin load</code>  s nastavením <code class="option">--parent-dir</code> a import provést do jiné branche.</li>
	<li><del datetime="2009-02-19T08:58:49+00:00">Udělat dump pomocí <code>svnadmin dump</code> s parametrem <code>--incremental</code> a se stejným parametrem jej i importovat. Tento postup uvádějí <a href="https://svnbook.red-bean.com/en/1.5/svn.reposadmin.maint.html#svn.reposadmin.maint.tk.svnadmin" target="_blank">v manuálu</a>, ale nemám ho zatím odzkoušený.</del></li>
       <li>můžete také použít <a href="https://svn.borg.ch/svndumptool/">svndumptool</a> a udělat merge na úrovni dump souborů, to mi přijde nejlepší řešení pokud požadujete jednu větev vývoje.</li>

</ol>
Pokud máte nějaké jiné zkušenosti se spojováním repozitory doufám, že to uvedete v komentářích. Ozvěte se také pokud by byl zájem, můžu uvést nějaký příklad s tím jak se to dělá pomocí příkazového řádku, krok za krokem.
