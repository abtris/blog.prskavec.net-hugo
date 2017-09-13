---
date: 2009-09-14T00:00:00Z
published: true
status: publish
tags:
- microsoft
- msie
title: IE 8 bug s cookie a doménou jobs.cz
type: post
url: /2009/09/14/ie-8-bug-s-cookie-a-domenou-jobs-cz/
---

V práci jsme se setkali se zajímavým problémem, který se vyskytuje ojediněle, ale zato postihuje jen určitou doménu. Máme problém s Microsoft Internet Explorerem 8 a doménou jobs.cz.

IE8 odmítá nastavit cookies na doménu druhé úrovně. Nefunguje to pouze a jen pro jobs.cz, pro všechny jiné domény je to v pořádku. Je to záhada, kterou nebude jednoduché vyřešit.

Fungují všechny starší verze MS IE i všechny ostatní prohlížeče i všechny jiné domény druhé úrovně, které jsme zkusili. Nefunguje zápis serverový ani klientský. Cookies na domény třetí úrovně (www.jobs.cz apod.) rovněž bez problémů.

Zkušební skript může vypadat takto:
<pre>


<title>cookie test</title>





YAHOO.util.Cookie.set("jobsClientFull", "client", { path: "/"});
YAHOO.util.Cookie.set("jobsClient2ndA", "client", { path: "/", domain: ".jobs.cz" });
YAHOO.util.Cookie.set("jobsClient2ndB", "client", { path: "/", domain: "jobs.cz" });
YAHOO.util.Cookie.set("jopsClientFull", "client", { path: "/"});
YAHOO.util.Cookie.set("jopsClient2ndA", "client", { path: "/", domain: ".jops.cz" });
YAHOO.util.Cookie.set("jopsClient2ndB", "client", { path: "/", domain: "jops.cz" });


<h2>Cookies obtained</h2>




</pre>
nebo v jednodušší podobě
<pre>&lt;?php
setcookie(&quot;serverTestCookie&quot;, &quot;server&quot;, 0, &quot;/&quot;, &quot;.<a href="http://jobs.cz/">jobs.cz</a>");
?&gt;


<title>cookie test</title>
&lt;script type=&quot;text/javascript&quot; src=&quot;<a href="http://yui.yahooapis.com/2.7.0/build/yahoo/yahoo-min.js">http://yui.yahooapis.com/2.7.0/build/yahoo/yahoo-min.js</a>"&gt;
&lt;script type=&quot;text/javascript&quot; src=&quot;<a href="http://yui.yahooapis.com/2.7.0/build/cookie/cookie-min.js">http://yui.yahooapis.com/2.7.0/build/cookie/cookie-min.js</a>"&gt;



YAHOO.util.Cookie.set("clientTestCookie", "client", { path: "/", domain: ".<a href="http://jobs.cz/">jobs.cz</a>" });


<h2>Cookies obtained</h2>




</pre>
příslušní virtualní host si na lokálním počítači nastavím třeba takto
<pre>
DocumentRoot /srv/cookie
ServerName <a href="http://jobs.cz/">jobs.cz</a>
ServerAlias <a href="http://www.jobs.cz/">www.jobs.cz</a> <a href="http://jobs.cz/">jobs.cz</a> <a href="http://jops.cz/">jops.cz</a> <a href="http://www.jops.cz/">www.jops.cz</a>
</pre>
a k tomu nějakého hosta
<pre>127.0.0.1    wwww.jobs.cz jobs.cz www.jops.cz jops.cz</pre>
Po prvním načtení bude kolekce cookies samozřejmě prázdná, ale už po tom druhem jsou v IE8 nastaveny pouze cookies na doménu třetí úrovně, druhé nic.

<strong>Závěr</strong>

Problém zřejmě souvisí s implementaci prohlížečích viz (<a href="http://docs.google.com/gview?a=v&amp;q=cache%3AtwPPceC8eq4J%3Awww.ietf.org%2Fproceedings%2F66%2Fslides%2Fdnsop-1.pdf+opera+cookie+domain+problem&amp;hl=cs&amp;pli=1">1</a>, <a href="http://tools.ietf.org/html/draft-pettersen-dns-cookie-validate-00">2</a>). V <a href="http://en.wikipedia.org/wiki/List_of_Internet_top-level_domains">TLD</a> existuje .jobs a proto se dá usuzovat, že k ní IE8 chová nějak jinak. Ale proč to nepostihuje např. travel.cz, museum.cz info.cz je záhadou. Zatím to vypadá na chybu v IE8, která nám opravdu hodně znepříjemňuje život.

Všichni si to můžete vyzkoušet na <a href="http://lmc.jobs.cz/cookie.php">http://lmc.jobs.cz/cookie.php</a>.

Udělejte refresh stránky a dostanete tento výsledek kromě MS IE8, proboha proč?

<a href="http://blog.prskavec.net/wp-content/uploads/2009/09/cookie_test_ok.png"><img src="http://blog.prskavec.net/wp-content/uploads/2009/09/cookie_test_ok.png" alt="cookie_test_ok" width="205" height="161" class="aligncenter size-full wp-image-709" /></a>

<a href="http://blog.prskavec.net/wp-content/uploads/2009/09/cookie_test_fail.png"><img src="http://blog.prskavec.net/wp-content/uploads/2009/09/cookie_test_fail-300x182.png" alt="cookie_test_fail" width="300" height="182" class="aligncenter size-medium wp-image-712" /></a>

Pokud by byl někdo schopný pomoci s nápravou této chyby budeme vděční za pomoc.

<strong>Aktualizace:</strong>
Pokud si dáte v Internet Exploreru <a href="//urlmon.dll/ietldlist.xml">res://urlmon.dll/ietldlist.xml</a> dostanete se k blokovaným TLD doménám. Bohužel na něm je z ČR i jobs.cz.
