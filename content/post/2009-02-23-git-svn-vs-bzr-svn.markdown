---
date: 2009-02-23T00:00:00Z
published: true
status: publish
tags:
- bazaar
- git
- svn
- scm
title: Git-SVN vs Bzr-SVN
type: post
url: /2009/02/23/git-svn-vs-bzr-svn/
---

<strong>Aktualizace (1.7.2009)</strong>
Dnes jsem zkoušel novy git 1.6.0.4 (svn 1.5.4) na práci s SVN repozitory. Konečně práce s repozitory funguje bez problémů a když provádím clone netrvá to 25min, ale pár vteřin jako u bzr. Asi to byla nějaká chyba, kterou vývojáři opravili.


Nedávno se tu vedla debata o tom jak nejlépe pracovat s Subversion když jsme offline. Nejlepší řešení je přejít na distribuovaný verzovací systém. Bavíme se o řešení na straně klienta, server bude stále Subversion.

Vytvořil jsem tento skript pomocí kterého si můžu udělat kolik chci revizi do repozitáře, je to primitivní ale pro základní testování to stačilo. Vytvořil jsem repozitory, které má 150 commitů a chci s ním pracovat pomocí git-svn nebo bzr-svn.

<pre>
#!/bin/bash
# Settings
START=2
COUNT=50
WC=/home/prskavecl/websites/svn_tests/test2/test2

cd $WC
touch $WC/trunk/testfile
svn add $WC/trunk/testfile
# cyklus pro vytvoreni revizi
x=$START;     # inicializuje hodnotu x na 0
while [ "$x" -le $COUNT ]; do
  echo "Aktuální hodnota x: $x"
  # zvýšení hodnoty x o 1
  echo $x &gt;&gt;$WC/trunk/testfile
  x=$(expr $x + 1)
  svn commit --message "Insert value $x"
done
svn up
</pre>

Testování jsem prováděl na Dellu D830 (2,4 GHz,Intel Core 2 Duo, 2GB RAM) v Ubuntu 8.10.

Verze použitých programů:
<ul>
	<li>git 1.5.6.3,</li>
	<li>svn 1.5.1,</li>
	<li>bzr 1.6.1 (bzr-svn 0.4.13-2)</li>
</ul>

Postup testování, šlo mi hlavně porovnat rychlost práce. Proto provedu tyto operace.
<ol>
	<li>checkout</li>
	<li>odpojení od centrálního repozitory</li>
	<li>změnu v souboru</li>
	<li>lokální commit</li>
	<li>napojení na centrální repozitory</li>
	<li>přenesení změn do centrálního repozitory</li>
</ol>

<h3>bzr-svn</h3>
<ol>
	<li>bzr checkout file:///home/svn/repos/test</li>
	<li>bzr unbind</li>
        <li>nano /trunk/testfile</li>
	<li>bzr commit</li>
        <li>bzr bind</li>
   	<li>bzr push file:///home/svn/repos/test</li>
</ol>

Tento postup lze zkrátit pomocí <code>bzr commit --local</code> potom nemusíme použít bind a unbind.

Celá operace trvala vteřiny, ani jsem nepoznal, že pracuji s jiným SCM než je SVN.

<h3>git-svn</h3>
<ol>
	<li>git svn clone file:///home/prskavecl/repos/test</li>
        <li>není třeba, ale checkout stále jede...po uplynutí 52 min jsem to málem vzdal</li>
        <li>nano /trunk/testfile</li>
        <li>git commit -a (5s)</li>
        <li>není třeba</li>
        <li>git svn dcommit (25s)</li>
</ol>

Více o git a subversion najdete v článku <a href="https://www.root.cz/clanky/git-a-subversion/">Dana Horáka</a>.

<h3>Závěr</h3>
Vím, že spousta lidí fandí gitu, ale mě vůbec zatím nepřesvědčil, zatím u mne vede <a href="https://bazaar-vcs.org/">Bazzar</a>. Možná je to problém jen git-svn a ne samotného gitu, ale tento druh spolupráce je hrozně pomalý.
