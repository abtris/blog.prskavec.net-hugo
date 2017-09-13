---
date: 2009-03-30T00:00:00Z
published: true
status: publish
tags:
- ide
- php
title: Zend Studio 6.1.1. zkušenosti z prvních 14 dní
type: post
url: /2009/03/30/zend-studio-611-zkusenosti-z-prvnich-14-dni/
---

V poslední době jsme přešli ve firmě z Eclipse PDT 2.0 na Zend Studio 6.1.1, přechod byl víceméně bezbolestný, ale pár drobností mě trápilo, vše jsem ale pořešil zatím k mojí spokojenosti.
<h3>xDebug</h3>
Narozdíl od PDT mi vadilo, že není volitelný debugger a funguje jen Zend Debugger. Ale to jde naštěstí lehce napravit.
<ol>
	<li>Zavřete Zend Studio pokud zrovna běží jinak jděte na další bod. Cesty jsou jak je mám na linuxu, na windows to bude obdobné.</li>
	<li>Otevřete konzoli</li>
	<li>Přejděte na adresář kde je nainstalováno Zend studio. (u mě například /opt/ZendStudio):
<code>cd /opt/ZendStudio</code></li>
	<li>Přejděte do adresáře plugins (/opt/ZendStudio/plugins):
<code>cd plugins</code></li>
	<li>Vytvořte nový adresář pojmenovaný disabled (i.e. /opt/ZendStudio/plugins/disabled):
<code>mkdir disabled</code></li>
	<li>Přesuňte soubory začínající  com.zend.php.debug  do vytvořeného adresáře.
<code>mv com.zend.php.debug* disabled</code></li>
	<li>Vraťte se do adresáře Zend Studia (/opt/ZendStudio)  a nastartujete Zend Studio s parametrem clean:
<code>./ZendStudio -clean</code></li>
	<li>Xdebug je k dispozici v nastaveních pro PHP Debugging.</li>
</ol>
<h3>Projekt přímo ze SVN</h3>
Pokud do SVN neukládáte údaje o projektech, tak je potřeba při checkoutu projektu udělat to pomocí wizarda jinak nebude fungovat doplňování php a další funkce pro PHP Projekty nebo Zend Framework projekty.

Pokud to neuděláte takto jde to udělat ručně modifikací souboru <code>.project</code>.

V Navigator otevřít .project a provést úpravy
<pre>


start_page








</pre>
nahradit (z .project PHP projektu)
<pre>


org.eclipse.php.core.PhpIncrementalProjectBuilder




org.eclipse.php.core.ValidationManagerWrapper





org.eclipse.php.core.PHPNature

</pre>
Pokud máte více repository locations v Eclipse a používáte Subversive (SVN client pro Eclipse používaný také v Zend Studiu) lze celé nastavení vyexportovat z PDT a naimportovat v Zend Studiu.
<ol>
	<li>NEW → Repository location nebo Open perspective SVN Repository Exploring</li>
	<li>Pravým tlačítkem na Repository location and Find/Check Out As</li>
	<li>Check out as a project configured using the New Project Wizard</li>
	<li>Zvolte podle potřeby PHP Project, Zend Framework</li>
	<li>Nastavte si jméno a dokončete tlačítkem finish</li>
</ol>
<h3>External Tools</h3>
Pro externí program který mi dělá balíky jsem potřeboval přidat program a na rozdíl od Eclipsy to nešlo, je potřeba upravit nastavení dle obrázku a potom se to chová již stejně jako Eclipse PDT.

Run → External Tools → External Tools configurations

<a href="http://blog.prskavec.net/wp-content/uploads/2009/03/external_tools_filter.jpg"><img class="aligncenter size-medium wp-image-448" src="http://blog.prskavec.net/wp-content/uploads/2009/03/external_tools_filter-300x224.jpg" alt="external_tools_filter" width="300" height="224" /></a>
