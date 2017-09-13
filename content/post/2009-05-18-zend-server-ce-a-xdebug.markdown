---
date: 2009-05-18T00:00:00Z
published: true
status: publish
tags:
- xdebug
- php
- zend
- zend-server
title: Zend Server CE a Xdebug
type: post
url: /2009/05/18/zend-server-ce-a-xdebug/
---

O práci s Zend Serverem jsem už tu psal. Normálně je nainstalovaný Zend Debugger, který má tu nevýhodu, že neumí spolupracovat s PHPUnit. Proto, když píšu testy a mám v Hudsonu automatické zpracování reportů potřebuji <a href="http://www.xdebug.org">Xdebug</a>. Teď si ukážeme jak to na Ubuntu přidat do Zend Serveru podporu pro Xdebug. Pro windows by to mělo fungovat obdobně jen se vyhnete kompilaci Xdebugu ze zdrojového kódu a máte si možnost stáhnout zkompilovanou knihovnu.

Nechápu moc Zend proč se trochu nesnaží, aby se dal Zend Debugger používat stejným způsobem jako Xdebug. Obzvláště když vím, že v Zend Studiu je code coverage a profiling dostupný.

Teď už jak na to v Ubuntu:
<ol>
	<li>Zend Server CE nainstalovaný dle <a href="http://blog.prskavec.net/2009/04/ubuntu-centos-5-a-php-529-v-zend-server-ce/">postupu</a></li>
	<li>Musíte mít nainstalovaný balíček pro kompilaci
<pre>sudo apt-get install build-essential
sudo apt-get install autoconf</pre>
</li>
	<li>Stáhnout zdrojové kódy xdebugu, rozbalit a dat kompilovat.
<pre>wget http://www.xdebug.org/files/xdebug-2.0.4.tgz
tar -xzf xdebug-2.0.4.tgz
cd xdebug-2.0.4/
/usr/local/zend/bin/phpize
./configure --enable-xdebug --with-php-config=/usr/local/zend/bin/php-config
make</pre>
</li>
	<li>Knihovnu najdete v <code>xdebug-2.0.4/modules/xdebug.so</code></li>
	<li>Knihovnu nakopirujte do <code>/usr/local/zend/lib/debugger/xdebug.so</code></li>
<pre>sudo cp modules/xdebug.so /usr/local/zend/lib/debugger/xdebug.so</pre>
	<li>Upravte soubor <code>/usr/local/zend/etc/ext.d/debugger.ini</code> a zakomentujte řádek
<pre>;zend_extension_manager.dir.debugger=/usr/local/zend/lib/debugger</pre>
</li>
	<li>Přidejte nakonec do <code>/usr/local/zend/etc/php.ini</code> řádek
<pre>zend_extension=/usr/local/zend/lib/debugger/xdebug.so</pre>
</li>
	<li>Nezapomeňte restartovat server <code>sudo /etc/init.d/zend-server restart</code></li>
</ol>
Po instalaci by jste neměli mít problém spustit Ant s kompletním phpunit taskem i na Zend Serveru CE.
<pre>



</pre>
