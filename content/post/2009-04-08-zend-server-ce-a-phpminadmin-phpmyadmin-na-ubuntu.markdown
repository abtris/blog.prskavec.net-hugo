---
date: 2009-04-08T00:00:00Z
meta:
  _edit_last: "1"
  _encloseme: "1"
  _pingme: "1"
published: true
status: publish
tags:
- php
- phpminadmin
- phpmyadmin
- zend server
title: Zend Server CE a phpMinAdmin, phpMyAdmin na Ubuntu
type: post
url: /2009/04/08/zend-server-ce-a-phpminadmin-phpmyadmin-na-ubuntu/
---

Zend Server CE má administrační konzoli, která umožňuje práci s extenzemi php, čtení logu apod. Celá konzole běží na lighthttpd a nemá z vlastní konfiguraci, která neodpovídá té, kterou máte pro Apache nebo IIS.
<h2>Instalace phpMyAdmin do konzole</h2>
Předpokládám, že Zend Server nainstalovaný z DEB balíku z repozitory jak jsem popisoval v<a href="http://blog.prskavec.net/2009/04/ubuntu-centos-5-a-php-529-v-zend-server-ce/"> minulém článku</a>.
<pre>sudo apt-get install phpmyadmin-zend-ce</pre>
Po instalaci, která u mě obsahovala i vlastní Mysql5 server je k dispozici phpmyadmin <a href="https://localhost:10082/phpmyadmin/">https://localhost:10082/phpmyadmin/</a> v konzoli.

<h2>Instalace phpMinAdmin do konzole</h2>
PhpMinAdmin se neumí připojit přímo bez extenze mysql, mysqli nebo pdo_mysql. Proto je potřeba aby v 
<pre>sudo vim /usr/local/zend/gui/lighttpd/etc/php-fcgi.ini</pre>
přidat řádek
<pre>extension=mysql.so</pre>
pokud ho tam nemáte např. z instalace phpMyAdmina a nezapomeňte restartovat Zend Server
<pre>sudo /etc/init.d/zend-server restart</pre>
potom stačí vytvořit adresář a stáhnout do něj phpMinAdmina
<pre>
sudo mkdir /usr/local/zend/gui/lighttpd/htdocs/phpminadmin
cd /usr/local/zend/gui/lighttpd/htdocs/phpminadmin
sudo wget http://switch.dl.sourceforge.net/sourceforge/phpminadmin/phpMinAdmin-1.9.1.php
sudo mv phpMinAdmin-1.9.1.php index.php
</pre>
Phpminadmin je k dispozici: <a href="https://localhost:10082/phpminadmin/">https://localhost:10082/phpminadmin/</a>

Pokud máte nastaveny přístup pro port 10082 jen pro localhost nemusíte se bát, že se někdo dostane přes chybu v  PMA do db. <a href="http://www.flickr.com/photos/abtris/sets/72157616497712368/">Zend Server v obrazech. </a>
