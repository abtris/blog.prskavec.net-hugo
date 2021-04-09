---
date: 2009-04-02T00:00:00Z
tags:
- centos
- linux
- php
- ubuntu
- zend-server
title: Ubuntu, CentOS 5 a PHP 5.2.9 v Zend Server CE
type: post
url: /2009/04/02/ubuntu-centos-5-a-php-529-v-zend-server-ce/
---

<h2>CentOS 5</h2>
Pokud jste někdo musel pracovat na serveru s touto distribucí tak vězte, že v repozitory i pro verzi 5.3 je PHP 5.1.6 a pokud potřebujete novější musíte si ji zkompilovat nebo sehnat jiný zdroj, kde může být problém s ověřením. Jako vývojáře mě příliš nezajímá na čem běží servery, kde se provozují moje aplikace, jen verze PHP a příslušné moduly je třeba hlídat.

Moje stanice je Ubuntu 8.10 a aktuální PHP 5.2.6-2ubuntu4.1 with Suhosin-Patch 0.9.6.2, které mi celkem přijde jako ok produkční verze i s ohledem, že mám i jeden Ubuntu server, kde je verze stejná.
<h2>Zend Server CE</h2>
Pokud chcete provozovat server s nejnovější verzí PHP 5.2.9 na linuxu nebo windows objevila se zajímavá alternativa <a href="https://www.zend.com/en/community/zend-server-ce">Zend Server CE</a>. CE (Community editon) je zdarma a nemá všechny vlastnosti <a href="https://www.zend.com/en/products/server/editions">Zend Serveru</a>, ale mě se líbí hlavně proto, že jsou k dispozici RPM a DEB balíčky s kterýmí je instalace bezproblémová (zkoušel jsem Ubuntu a CentOS5). Máte za chvíli k dispozici server s PHP pro vývoj včetně ladící konzole, která umožňuje jednoduchou administraci php.ini, čtení logů  a práci s extenzemi.

V základní instalaci Zend Server CE 4.0.0 beta je obsaženo:
<ul>
	<li>PHP 5.2.9</li>
	<li>Zend Framework  1.7.5</li>
	<li>Zend Data Cache 4.0</li>
	<li>Zend Debugger 5.2</li>
	<li>Zend Optimizer+ 4.0</li>
</ul>
Zend Server pro běh konzole na <a href="https://localhost:10082/ZendServer/">https://localhost:10082/ZendServer/</a> používá <a href="https://www.lighttpd.net/">LightHttpd</a> a konzole je napsaná v Zend Frameworku. Apache pro běh aplikací jak jsem měl nainstalován to neovlivní a přidá si to jen do konfigurace pár nastavení. Nic vám nebraní používat váš  document root jak jste zvyklí a to je velká přednost Zend Serveru.

Plná verze má další komponenty jako Guard Loader, Java Bridge, Monitor, Page Cache a ZDS (Zend Download Server). Tyto části jsem nevyzkoušel, ale Monitor pro předcházení problémů pomocí nastavených událostí nebo ZDS pro paralelní stahování souborů se zdají být také užitečné ale ne nezbytné.
<h2>Instalace na CentOS5</h2>
<ol>
	<li>Otevřete konzoli a přejděte do režimu root <code>su</code></li>
	<li>Vytvořte nový zdroj pro YUM:
<pre>nano /etc/yum.repos.d/zend.repo</pre>
Do souboru vložte tento obsah
<pre>[Zend]
name=Zend CE $releasever - $basearch - Released Updates
baseurl=https://repos.zend.com/rpm/ce/$basearch/
enabled=1
gpgcheck=0

[Zendce-noarch]
name=Zend CE - noarch
baseurl=https://repos.zend.com/rpm/ce/noarch
enabled=1
gpgcheck=0</pre>
</li>
	<li>Aktualizace balíčků:
<pre>yum clean all</pre>
</li>
	<li>Instalace Zend Serveru CE:
<pre>yum install zend-ce</pre>
</li>
</ol>
<a href="https://files.zend.com/help/Zend-Server-Community-Edition/zend-server-community-edition.htm#rpm_installation.htm">Detailní postup v angličtině pro RPM balíčky.</a>
<h2>Instalace na Ubuntu</h2>
<ol>
	<li>Otevřete konzoli a přejděte do režimu root <code>sudo -i</code></li>
	<li>Přidejte do seznamu repozitory:
<pre>nano /etc/apt/sources.list</pre>
nový zdroj (řádek):
<pre>deb https://repos.zend.com/deb/ce ce non-free</pre>
</li>
	<li>Veřejný klíč k repozitory:
<pre>wget https://repos.zend.com/deb/zend.key -O- |apt-key add -</pre>
</li>
	<li>Aktualizace balíčků:
<pre>apt-get update</pre>
</li>
	<li>Instalace Zend Serveru CE:
<pre>aptitude install zend-ce</pre>
</li>
</ol>
<a href="https://files.zend.com/help/Zend-Server-Community-Edition/zend-server-community-edition.htm#deb_installation.htm">Detailní postup v angličtině pro DEB balíčky.</a>
