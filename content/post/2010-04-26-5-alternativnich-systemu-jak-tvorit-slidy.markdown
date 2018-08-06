---
date: 2010-04-26T00:00:00Z
published: true
status: publish
tags:
- html
- javascript
- jush
- tools
title: 5 alternativních systémů jak tvořit slidy
type: post
url: /2010/04/26/5-alternativnich-systemu-jak-tvorit-slidy/
---

<!--:en-->Pokud občas nebo více přednášíte, určitě jste zkusili nějaký tento program na tvorbu slidů. Já žádný z nich nepovažuji za ideální, hlavně pokud potřebujete mít ve slidech zdrojové kódy. Pokud máte slidy v html nebo xml můžete je verzovat pomocí například Gitu nebo SVN, to vám půjde s binárními formáty také, ale neuvidíte ty diffy, které jsou užitečné.
<ul>
	<li><a href="https://office.microsoft.com/en-us/powerpoint/">Microsoft PowerPoint</a></li>
	<li><a href="https://www.openoffice.org/product/impress.html">OpenOffice Impress</a></li>
	<li><a href="https://www.apple.com/iwork/keynote/">Apple Keynote</a></li>
	<li><a href="https://prezi.com/">Prezi </a>- více v článku Martina Hassmana <a href="https://met.blog.root.cz/2010/04/11/umi-tohle-i-vas-prezentacni-system-aneb-jak-jsem-zkousel-prezi/">Umí tohle i váš prezentační systém? Aneb jak jsem zkoušel Prezi</a></li>
</ul>
Určitě jich bude ještě více. Pokud pracujete s HTML a XML máte další možnosti, ať jsou to <a href="https://www.miwie.org/presentations/html/dbslides.html">Docbook slides</a> nebo některá varianta založená na HTML nebo markupu (<a href="https://daringfireball.net/projects/markdown/syntax">Markdown</a>, <a href="https://en.wikipedia.org/wiki/Textile_%28markup_language%29">Textile</a>, <a href="https://texy.info/cs/syntax">Texy</a>).

Dnes tu představím 5 systémů, které generují HTML nebo se slidy v HTML přímo píšou.
<ol>
	<li>S5</li>
	<li>Swinger</li>
	<li>Slidedown</li>
	<li>W3C (Slidy, B5, slidemaker)</li>
	<li>JUSH slides</li>
</ol>
<strong>1. <a href="https://meyerweb.com/eric/tools/s5/">S5</a></strong>
Klasické slidy v HTML dole je vidět jak vypadá jednoduchý předpis.
<pre>


<title><em>[slide show title]</em></title>








<div>

<div></div>
<div></div>
<div>
<div></div>
</div>

</div>
<div>

<div>
<h1><em>[slide title]</em></h1>
</div>

</div>


</pre>
Demo je k dispozici na <a href="https://meyerweb.com/eric/tools/s5/s5-intro.html">https://meyerweb.com/eric/tools/s5/s5-intro.html</a>

<strong>2. <a href="https://github.com/quirkey/swinger">Swinger</a></strong>

Toto řešení je zajímavé, že máte k dispozici celý editor markupu a všechny data jsou uloženy v CouchDb. Aplikace i prohlížení slidů je jen HTML a Javascript.

<a href="https://blog.prskavec.net/wp-content/uploads/2010/04/swinger-screenshot.png"><img class="aligncenter size-medium wp-image-901" src="https://blog.prskavec.net/wp-content/uploads/2010/04/swinger-screenshot-300x177.png" alt="" width="300" height="177" /></a>

Online verze <a href="https://swinger.quirkey.com/">https://swinger.quirkey.com/</a>

<strong>3. <a href="https://github.com/nakajima/slidedown">Slidedown</a></strong>
Řešení založené na Ruby, které podle předpisu v markdownu vygeneruje html prezentaci včetně zvýraznění ruby syntaxe i šablon.
<pre>!SLIDE

# This is my talk

!SLIDE

## I hope you enjoy it

!SLIDE code

    def foo
      :bar
    end

!SLIDE

Google is [here](https://google.com)

!SLIDE

# Questions?</pre>
Zvýraznění syntaxe se dělá takto, například pro javascript.
<pre>@@@ js
    function foo() {
      return 'bar';
    }
@@@</pre>
Demo je dostupné zde <a href="https://nakajima.github.com/slidedown/">https://nakajima.github.com/slidedown/</a>

<strong>4. W3C Talks Tools (<a href="https://www.w3.org/Talks/Tools/">Slidy, B5, Slidemaker/slideme</a>)</strong>

Struktura je velmi jednoduchá, základní část je tvořena tagem <div class="slide"></div>

a jsou přidané třídy pro speciální chování.
<pre><div>
  <h1>Analysts - "Open standards programming will become
  mainstream, focused around VoiceXML"</h1>
  <!-- use CSS positioning and scaling for adaptive layout -->
  <img src="trends.png" width="50%" style="float:left" alt="projected growth of VoiceXML" />

  <blockquote>
    VoiceXML will dominate the voice environment, due to its
    flexibility and eventual multimodal capabilities
  </blockquote><br />

  <p style="text-align:center">Source Data Monitor, March
  2004</p>
</div></pre>
<a href="https://www.w3.org/Talks/Tools/Slidy/#%281%29">Slidy demo</a>

<strong>5. <a href="https://abtris.github.com/slides/">JUSH Slides</a></strong>

Poslední je moje vlastní řešení je založené na W3C Slidy a je doplněné o <a href="https://jush.sourceforge.net/">JUSH zvýrazňovač</a>, který pomůže v tom co já nejvíce potřebuji.

Kromě zvýraznění přidá JUSH linky na dokumentaci u klíčových slov pro html, javascript, php a další. To udělá ze slidů dobrý studijní materiál.

Za další výhodu vidím jednoduchý předpis v html, jen používání xmp tagu není ideální.
<pre><div class="slide">
    <h1>Filter Input</h1>

<pre><form method="post">
   <label for="username">Username:</label>
   <label for="password">Password:</label>
   <label for="color">Select:</label>
                                            Red
                                            Blue


</form></pre>

<pre>// c_type extension
$clean = array();
if (c_type_aplha($_POST['username']) {
    $clean['username'] = $_POST['username'];
}
// filter
$args = array('username' =&gt; FILTER_SANITIZE_STRING, ...);
$myinputs = filter_input_array(INPUT_POST, $args);
</pre>
    <ul>

        <li>&lt;a href=&quot;<a href="https://cz.php.net/manual/en/filter.filters.validate.php">https://cz.php.net/manual/en/filter.filters.validate.php</a>"&gt;Validate filters</a></li>
        <li>&lt;a href=&quot;<a href="https://cz.php.net/manual/en/filter.filters.sanitize.php">https://cz.php.net/manual/en/filter.filters.sanitize.php</a>"&gt;Sanitize filters</a></li>
    </ul>
</div></pre>
Pokud máte nějaké další zajímavé řešení podělte se s námi v komentářích.<!--:--><!--:cs-->Pokud občas nebo více přednášíte, určitě jste zkusili nějaký tento program na tvorbu slidů. Já žádný z nich nepovažuji za ideální, hlavně pokud potřebujete mít ve slidech zdrojové kódy. Pokud máte slidy v html nebo xml můžete je verzovat pomocí například Gitu nebo SVN, to vám půjde s binárními formáty také, ale neuvidíte ty diffy, které jsou užitečné.
<ul>
	<li><a href="https://office.microsoft.com/en-us/powerpoint/">Microsoft PowerPoint</a></li>
	<li><a href="https://www.openoffice.org/product/impress.html">OpenOffice Impress</a></li>
	<li><a href="https://www.apple.com/iwork/keynote/">Apple Keynote</a></li>
	<li><a href="https://prezi.com/">Prezi </a>- více v článku Martina Hassmana <a href="https://met.blog.root.cz/2010/04/11/umi-tohle-i-vas-prezentacni-system-aneb-jak-jsem-zkousel-prezi/">Umí tohle i váš prezentační systém? Aneb jak jsem zkoušel Prezi</a></li>
</ul>
Určitě jich bude ještě více. Pokud pracujete s HTML a XML máte další možnosti, ať jsou to <a href="https://www.miwie.org/presentations/html/dbslides.html">Docbook slides</a> nebo některá varianta založená na HTML nebo markupu (<a href="https://daringfireball.net/projects/markdown/syntax">Markdown</a>, <a href="https://en.wikipedia.org/wiki/Textile_%28markup_language%29">Textile</a>, <a href="https://texy.info/cs/syntax">Texy</a>).

Dnes tu představím 5 systémů, které generují HTML nebo se slidy v HTML přímo píšou.
<ol>
	<li>S5</li>
	<li>Swinger</li>
	<li>Slidedown</li>
	<li>W3C (Slidy, B5, slidemaker)</li>
	<li>JUSH slides</li>
</ol>
<strong>1. <a href="https://meyerweb.com/eric/tools/s5/">S5</a></strong>

Klasické slidy v HTML dole je vidět jak vypadá jednoduchý předpis.
<pre>


<title><em>[slide show title]</em></title>








<div>

<div></div>
<div></div>
<div>
<div></div>
</div>

</div>
<div>

<div>
<h1><em>[slide title]</em></h1>
</div>

</div>


</pre>
Demo je k dispozici na <a href="https://meyerweb.com/eric/tools/s5/s5-intro.html">https://meyerweb.com/eric/tools/s5/s5-intro.html</a>

<strong>2. <a href="https://github.com/quirkey/swinger">Swinger</a></strong>

Toto řešení je zajímavé, že máte k dispozici celý editor markupu a všechny data jsou uloženy v CouchDb. Aplikace i prohlížení slidů je jen HTML a Javascript.

<a href="https://blog.prskavec.net/wp-content/uploads/2010/04/swinger-screenshot.png"><img class="aligncenter size-medium wp-image-901" src="https://blog.prskavec.net/wp-content/uploads/2010/04/swinger-screenshot-300x177.png" alt="" width="300" height="177" /></a>

Online verze <a href="https://swinger.quirkey.com/">https://swinger.quirkey.com/</a>

<strong>3. <a href="https://github.com/nakajima/slidedown">Slidedown</a></strong>

Řešení založené na Ruby, které podle předpisu v markdownu vygeneruje html prezentaci včetně zvýraznění ruby syntaxe i šablon.
<pre>!SLIDE

# This is my talk

!SLIDE

## I hope you enjoy it

!SLIDE code

    def foo
      :bar
    end

!SLIDE

Google is [here](https://google.com)

!SLIDE

# Questions?</pre>
Zvýraznění syntaxe se dělá takto, například pro javascript.
<pre>@@@ js
    function foo() {
      return 'bar';
    }
@@@</pre>
Demo je dostupné zde <a href="https://nakajima.github.com/slidedown/">https://nakajima.github.com/slidedown/</a>

<strong>4. W3C Talks Tools (<a href="https://www.w3.org/Talks/Tools/">Slidy, B5, Slidemaker/slideme</a>)</strong>

Struktura je velmi jednoduchá, základní část je tvořena tagem <div class="slide"></div>

a jsou přidané třídy pro speciální chování.
<pre><div>
  <h1>Analysts - "Open standards programming will become
  mainstream, focused around VoiceXML"</h1>
  <!-- use CSS positioning and scaling for adaptive layout -->
  <img src="trends.png" width="50%" style="float:left" alt="projected growth of VoiceXML" />

  <blockquote>
    VoiceXML will dominate the voice environment, due to its
    flexibility and eventual multimodal capabilities
  </blockquote><br />

  <p style="text-align:center">Source Data Monitor, March
  2004</p>
</div></pre>
<a href="https://www.w3.org/Talks/Tools/Slidy/#%281%29">Slidy demo</a>

<strong>5. <a href="https://abtris.github.com/slides/">JUSH Slides</a></strong>

Poslední je moje vlastní řešení je založené na W3C Slidy a je doplněné o <a href="https://jush.sourceforge.net/">JUSH zvýrazňovač</a>, který pomůže v tom co já nejvíce potřebuji.

Kromě zvýraznění přidá JUSH linky na dokumentaci u klíčových slov pro html, javascript, php a další. To udělá ze slidů dobrý studijní materiál.

Za další výhodu vidím jednoduchý předpis v html, jen používání xmp tagu není ideální.
<pre><div class="slide">
    <h1>Filter Input</h1>

<pre><form method="post">
   <label for="username">Username:</label>
   <label for="password">Password:</label>
   <label for="color">Select:</label>
                                            Red
                                            Blue


</form></pre>

<pre>// c_type extension
$clean = array();
if (c_type_aplha($_POST['username']) {
    $clean['username'] = $_POST['username'];
}
// filter
$args = array('username' =&gt; FILTER_SANITIZE_STRING, ...);
$myinputs = filter_input_array(INPUT_POST, $args);
</pre>
    <ul>

        <li>&lt;a href=&quot;<a href="https://cz.php.net/manual/en/filter.filters.validate.php">https://cz.php.net/manual/en/filter.filters.validate.php</a>"&gt;Validate filters</a></li>
        <li>&lt;a href=&quot;<a href="https://cz.php.net/manual/en/filter.filters.sanitize.php">https://cz.php.net/manual/en/filter.filters.sanitize.php</a>"&gt;Sanitize filters</a></li>
    </ul>
</div></pre>
Pokud máte nějaké další zajímavé řešení podělte se s námi v komentářích.<!--:-->
