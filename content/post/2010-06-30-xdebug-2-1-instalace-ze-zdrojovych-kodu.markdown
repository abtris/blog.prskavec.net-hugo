---
date: 2010-06-30T00:00:00Z
published: true
status: publish
tags:
- php
- xdebug
title: Xdebug 2.1 instalace ze zdrojových kódů
type: post
url: /2010/06/30/xdebug-2-1-instalace-ze-zdrojovych-kodu/
---

Včera vyšela nová verze <a href="https://derickrethans.nl/xdebug-2.1-released.html">Xdebug 2.1</a>. Z hlavních novinek bych zdůraznil podporu pro PHP 5.3 a další můžete vyčíst z Derickova oznámení.

Xdebug jsem instaloval na Mac OS X 10.6.4 a Ubuntu 9.10 a bez problémů jsem to zkompiloval ze zdrojových kódů.

V Ubuntu i na Macu je potřeba mít podporu pro kompilaci ze zdrojových kódů. V ubuntu je to balíček build-essential a autoconf a na Macovi Xcode s příslušenstvím, případně si přes Port doinstalujete co potřebujete.
<ul>
	<li>Stáhnout zdrojové kódy xdebugu, rozbalit a dat kompilovat.
<pre class="bash">wget https://www.xdebug.org/files/xdebug-2.1.0.tgz
tar -xzf xdebug-2.1.0.tgz
cd xdebug-2.1.0/
phpize
./configure --enable-xdebug
make</pre>
</li>
	<li>Knihovnu najdete v <code>xdebug-2.1.0/modules/xdebug.so</code></li>
	<li>Knihovnu nakopirujte k ostatním knihovnám, případně kam chcete</li>
	<li>Přidejte nakonec do <code>php.ini</code> řádek
<pre class="bash">zend_extension=/cesta-k-modulum/xdebug.so</pre>
</li>
	<li>Nezapomeňte restartovat server</li>
</ul>
Pokud máte nějaké potíže s kompilací nebo s Xdebugem, klidně se ozvěte ve komentářích.

P.S.: Pokud instalujete xdebug poprvé nezapomeňte zkontrolovat zda máte správně konfiguraci pro debugging pro IDE (např. NetBeans)
<pre class="bash">xdebug.remote_enable=1
xdebug.remote_host=localhost
xdebug.remote_port=9000
xdebug.remote_handler=dbgp
xdebug.profiler_enable=1
xdebug.profiler_output_dir="/tmp/xdebug"
</pre>
