---
date: 2009-11-05T00:00:00Z
tags:
- nosql
- php
title: NoSQL Databáze v PHP
type: post
url: /2009/11/05/nosql-databze-v-php/
---

V&#269;era jsem byl na p&#345;edn&aacute;&scaron;ce <a href="https://twitter.com/honzakral">Honzy Kr&aacute;le</a> na t&eacute;ma <em>Necho&#271;te s kan&oacute;nem na data aneb key value datab&aacute;ze</em>. P&#345;edn&aacute;&scaron;ka nebyla jen o key value datab&aacute;z&iacute;ch, ale obecn&#283; o <a href="https://nosql-databases.org/" target="_blank">NO SQL</a>.

Honza shrnul probl&eacute;my <a href="https://en.wikipedia.org/wiki/RDBMS">RDBMS</a> datab&aacute;z&iacute; s ohledem na <a href="https://en.wikipedia.org/wiki/ACID">ACID</a> a co m&#367;&#382;ou p&#345;in&eacute;st jin&eacute; typy datab&aacute;z&iacute; ne&#382; sql. Nap&#345;&iacute;klad <em>key value</em> a <em>dokumentov&eacute;</em> pro nasazen&iacute; ve webov&yacute;ch slu&#382;b&aacute;ch, kde mnoh&eacute; tak&eacute; vznikly pou&#382;&iacute;v&aacute;j&iacute; je Google, Amazon, Facebook a jin&iacute;.

P&#345;ehled <a href="https://en.wikipedia.org/wiki/NoSQL">NoSQL</a> najdete tak&eacute; ve wikipedii.

Key value datab&aacute;ze nebo &uacute;lo&#382;i&scaron;t&#283;
<ul>
	<li><a href="https://www.danga.com/memcached/">Memcached</a> - v <a href="https://cz2.php.net/memcache">php</a> velmi dob&#345;e pou&#382;iteln&aacute; pro cachov&aacute;n&iacute;</li>
	<li><a href="https://code.google.com/p/redis/">Redis</a> - <a href="https://code.google.com/p/php-redis/">php-redis</a></li>
	<li><a href="https://riak.basho.com/">Riak</a> - <a href="https://hg.basho.com/riak/src/tip/client_lib/jiak.php">jiak.php</a></li>
	<li><a href="https://1978th.net/tokyocabinet/">Tokyo Cabinet</a> - <a href="https://mamasam.indefero.net/p/tyrant/">PHP Tyrant</a>, <a href="https://code.google.com/p/phptyrant/">phptyrant</a>, <a href="https://pecl.php.net/package/tokyo_tyrant">PECL</a> a <a href="https://www.php.net/manual/en/book.tokyo-tyrant.php">p&#345;&iacute;slu&scaron;n&aacute; &#269;&aacute;st v manu&aacute;lu</a></li>
	<li><a href="https://incubator.apache.org/cassandra/">Cassandra</a> - uk&aacute;zka pro <a href="https://wiki.apache.org/cassandra/ClientExamples">php klienta</a></li>
</ul>
Dokumentov&eacute; datab&aacute;ze
<ul>
	<li><a href="couchdb.apache.org/">CouchDb</a> - <a href="https://arbitracker.org/phpillow.html">PHPillow</a>, <a href="https://github.com/dready92/PHP-on-Couch">PHP On Couch</a>, <a href="https://www.topdog.za.net/php_couchdb_extension">PHP CouchDB Extension</a>, <a href="https://github.com/weierophinney/phly/tree/master/Phly_Couch">Phly_Couch</a>, <a href="https://github.com/shevron/sopha/tree/master">sopha</a></li>
	<li><a href="www.mongodb.org/">MongoDb</a> - k mongu je <a href="https://pecl.php.net/package/mongo">PECL extenze</a> a popis najdete v <a href="https://php.net/manual/en/class.mongodb.php">manu&aacute;lu php</a></li>
</ul>
Toto t&eacute;ma je &scaron;irok&eacute; a hodn&#283; se o tom diskutovalo a Honza p&#345;edvedl implementaci Twitter serveru v Pythonu a ukl&aacute;d&aacute;n&iacute; dat do Redisu. Lekce &scaron;lo k&oacute;d &scaron;k&aacute;lovat a z jedn&eacute; datab&aacute;ze za&#269;&iacute;t ukl&aacute;dat do deseti r&#367;zn&yacute;ch.

CouchDb datab&aacute;ze je nap&#345;&iacute;klad nasazena v nov&eacute;m <a href="https://www.linux-magazine.com/Online/News/Relaxed-Ubuntu-9.10-CouchDB-to-be-Integrated">Ubuntu 9.10 </a>a bude se jej&iacute; podpora pro Ubuntu One a synchronizaci dat ur&#269;it&#283; roz&scaron;i&#345;ovat. Pokud pou&#382;&iacute;v&aacute;te Ubuntu, bal&iacute;&#269;ek s aktu&aacute;ln&iacute; verz&iacute; najdete v repository. Za v&yacute;hodu jednoduch&eacute;ho nasazen&iacute; CouchDb je jeho REST api a p&#283;kn&yacute; webov&yacute; klient pro administraci. Nev&yacute;hodou bude v&yacute;kon ve srovn&aacute;n&iacute; s MongoDb, kde je nativn&iacute; klient a dobr&yacute; jazyk pro dotazy. V CouchDb mus&iacute;te pro psan&iacute; materializovan&yacute;ch pohled&#367; pou&#382;&iacute;vat javascript. MongoDb podporuje index a celkov&#283; je v mnoha v&#283;cech vysp&#283;lej&scaron;&iacute;. Ale chyb&iacute; nap&#345;&iacute;klad podpora v ubuntu, bal&iacute;&#269;ek nenajdete, mus&iacute;te si ji zkompilovat sami.

Pro Zend Framework pokud v&iacute;m se p&#345;ipravuje implementace <a href="https://framework.zend.com/wiki/display/ZFPROP/Zend_Couch+-+Matthew+Weier+O%27Phinney">CouchDb</a>. Jak pou&#382;&iacute;t v Zend Frameworku <a href="https://raphaelstolt.blogspot.com/2009/09/logging-to-mongodb-and-accessing-log.html">MongoDb pro ukl&aacute;d&aacute;n&iacute; log&#367;</a> v kombinaci s Zend Tool ukazuje Raphael Stolt.

Pokud v&iacute;te o v&#283;t&scaron;&iacute;m nasazen&iacute; t&#283;chto datab&aacute;z&iacute; nebojte se to uv&eacute;st v koment&aacute;&#345;&iacute;ch. Nap&#345;&iacute;klad port&aacute;ly <a href="https://www.jobs.cz" target="_blank">jobs.cz</a> a <a href="https://www.prace.cz">prace.cz</a> pou&#382;&iacute;vaj&iacute; memcache, ale takov&yacute;ch nasazen&iacute; budou stovky. M&aacute; n&#283;kdo v &#268;ech&aacute;ch nasazen&eacute; ve velk&eacute;m CouchDb nebo MongoDb?
<div id="_mcePaste" style="overflow: hidden; position: absolute; left: -10000px; top: 61px; width: 1px; height: 1px;">https://arbitracker.org/phpillow.html</div>
