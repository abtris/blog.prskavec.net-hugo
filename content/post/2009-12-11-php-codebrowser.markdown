---
date: 2009-12-11T00:00:00Z
meta:
  _edit_last: "1"
  _encloseme: "1"
  _pingme: "1"
published: true
status: publish
tags:
- hudson
- php
- phpundercontrol
- phpunit
- php_codebrowser
- qa
title: PHP CodeBrowser a Hudson
type: post
url: /2009/12/11/php-codebrowser/
---

<p>Před časem se <a href="http://blog.thinkphp.de/archives/464-PHP_CodeBrowser-Release-version-0.1.0.html">objevil</a> nový reportovací nástroj, který vezme logy z <a href="http://www.phpunit.de/">PHPUnit</a>, <a href="http://pear.php.net/package/PHP_CodeSniffer/">PHP_Codesniffer</a> a umí je pěkně zobrazit.</p>
<p><strong>Instalace</strong> je z PEAR (<a href="http://pear.phpunit.de/get/">http://pear.phpunit.de/get/</a>)<br />
<code><br />
pear config-set preferred_state alpha<br />
pear channel-discover pear.phpunit.de<br />
pear install --alldeps phpunit/PHP_CodeBrowser<br />
</code></p>
<p>nebo z SVN</p>
<p><code>svn co svn://phpunit.de/phpunit/phpcb/trunk PHP_CodeBrowser</code></p>
<p><strong>Integrace</strong><br />
Tento tool jde integrovat do CIE</p>
<ul>
<li>phpundercontrol (<a href="http://phpundercontrol.org/download.html">dostupné od verze 0.5</a>)</li>
<li>atlassion bamboo (<a href="http://twitter.com/s_bergmann/status/6499094572">zatím asi není oficiálně dostupně ale, jde to</a>)</li>
<li>hudson</li>
</ul>
<p>Postup integrace do Hudsonu si ukážeme</p>
<ol>
<li>Potřebujete do Hudsonu doinstalovat plugin <a href="http%3A%2F%2Fwiki.hudson-ci.org%2Fdisplay%2FHUDSON%2FHTML%2BPublisher%2BPlugin">HTML Publisher Plugin</a></li>
<li>Nastavíte build script by používal phpcb<br />
<code><br />
<br />
<br />
&lt;arg line=&quot;--log reports/logs/<br />
--source source/<br />
--output reports/phpcb/" /&gt;<br />
<br />
</code></li>
<li>Nastavíte HTML Publisher plugin, aby četl html, které vyrobí phpcb<a href="http://blog.prskavec.net/wp-content/uploads/2009/12/hudson_phpcb.png"><img class="aligncenter size-medium wp-image-781" src="http://blog.prskavec.net/wp-content/uploads/2009/12/hudson_phpcb-300x37.png" alt="hudson_phpcb" width="300" height="37" /></a>
<ul>
<li><em>HTML Directory to archive</em><br />
Cesta k reportům podle toho jak jste si to nastavili v build skriptu, u mne <code><strong>reports/phpcb/</strong></code></li>
<li><em>Index page[s]</em><br />
Defaultně je <strong>index.html</strong>, ponechejte.</li>
<li><em>Report title</em><br />
Nastavte co chcete, dal jsem <strong>PHP CodeBrowser</strong></li>
</li>
</ol>
<p>Výsledek bude vypadat nějak takto:</p>
<p><a href="http://blog.prskavec.net/wp-content/uploads/2009/12/hudson-phpcb-3.png"><img class="aligncenter size-medium wp-image-783" src="http://blog.prskavec.net/wp-content/uploads/2009/12/hudson-phpcb-3-300x117.png" alt="hudson-phpcb-3" width="300" height="117" /></a></p>
<p>PHP_CodeBrowser sám o sobě nepřidává žádnou funkcionality, ale umožňuje získané výsledky pěkně a přehledně zobrazit. To se hodí a pokud jste příznivci použítí jednoduchého Hudsonu jako já, doufám že vám to bude k užitku.</p>
<p>PHP_CodeBrowser můžete samozřejmě použít i bez jakékoliv integrace, protože je to běžné HTML. Pokud používáte verzi z SVN jako já pozor na to, že verze pokud jí pouštíte symlinkem nefunguje úplně správně musíte být v adresáři kde máte PHP_CodeBrowser a je lepší zadat plné cesty k logům phpunit i zdrojovým kódům.</p>
