---
date: 2009-07-20T00:00:00Z
published: true
status: publish
tags:
- pear
- php
- zend
title: PEAR a Zend Server CE 4.0.4
type: post
url: /2009/07/20/pear-a-zend-server-ce-4-0-4/
---

Pokud jste instalovali Zend Server CE ve verzi 4.0.4 a předtím jste neměli provedený upgrade PEARu z 1.8.0 na 1.8.1 možná se setkáte s chybovou hláškou pokud budete chtít něco z PEARu nainstalovat.

<pre>
pear.php.net is using a unsupported protocal - This should never happen.
install failed
</pre>

Řešení je následující, instalujte přímo z PEARu (řešení je pro Ubuntu, ale mělo by fungovat i jinde).

<pre>lynx -source http://pear.php.net/go-pear | php</pre>

Spustí se installer a tam nastavte cesty podle Zend Serveru a nainstalujete tam kam Zend Server instaluje také, neměl by potom být problém s upgradem.

<pre> 
 1. Installation prefix ($prefix) : /usr/local/zend/
 2. Temporary files <a href="http://www.directorydomain.org/">directory</a>     : /usr/local/zend/tmp
 3. Binaries directory            : /usr/local/zend/bin
 4. PHP code directory ($php_dir) : /usr/local/zend/share/pear/PEAR
 5. Documentation base directory  : /usr/local/zend/share/pear/doc
 6. Data base directory           : /usr/local/zend/share/pear/data
 7. Tests base <a href="http://www.directorydomain.org/">directory</a>          : /usr/local/zend/share/pear/test 
</pre>

Potom můžete PEAR normálně používat jak jste zvyklí, myslím, že brzo Zend vydají update balíčku, kde tento problém bude vyřešen, ale zatím tohle docela snadné a přitom elegantní řešení.
