---
date: 2008-07-15T00:00:00Z
published: true
status: publish
tags:
- php
- zend-framework
title: Gettext a PHP
type: post
url: /2008/07/15/gettext-a-php/
---

Gettext je Open Source nástroj na překlad aplikací. Kdo s tímto nástrojem pracuje může můj článek rovnou vynechat, protože tyto věci zná.

Getext má jedinou nevýhodu, kterou lze celkem přejít, nejde přímo lidsky číst, ukládájí se v binárním tvaru do souboru s příponou *.mo. Pokud používáte nějakou vlastní metodu pro překlad určitě to bude něco z toho co nabízí Zend Framework (ZF) v Zend_Translate (pole, csv, xml – tbx, xliff, xmltm, gettext) nebo nějakou metodu založenou na databázi. Sám jsem zkusil během let většinu těchto metod a celkem se s nimi pracovalo dobře pokud se aplikace moc nerozrostla a případně pokud neměl překládat někdo kdo neuměl pracovat s prostředím ve kterém jsem pracoval.

Na Gettextu se mi líbí hlavně aplikace pro editaci <a href="http://www.poedit.net">poEdit</a>. Aplikace umí parsovat zdrojové kódy pro překlad. To je asi nejlepší co moje dřívější metody nikdy neměli, gettext si najde sám co překládat a umožňuje použít už hotové překlady pro automatické překlady.

Pro práci s Gettextem potřebujete buď ZF nebo extenze pro gettext. ZF v 1.5 zatím nepodporuje množná čísla.

Ukázka použití getextu jako extenze PHP.
<pre>

</pre>
V ZF nemusíte tak striktně dodržovate cesty a práce s gettextem je obdobná.

<pre>setLocale('cs');

// tisk

echo $t-&gt;_(‘Vítejete v aplikaci’);

?&gt;
</pre>
Aplikace poEdit je podle mě asi nejznámější a teké nejlepší pro práci s Gettextem, edituje soubory zdrojové v *.po a kompiluje výsledek do *.mo při ukládání. Je potřeba jen mít správně nastavené cesty k projektu se kterým právě pracujete.

Pro práci s ZF je jen potřeba přidat pro v konfiguraci další koncovky do PHP parseru, já jsem používal překlad kromě php skriptech také v v phtml. Bez úpravy nastavení by nenašel xgettext vaše řetězce k předkladu.

<a href="http://blog.prskavec.net/wp-content/uploads/2008/07/image1.png"></a>

Osobně se mi líbí na gettextu, že můžu mít poEdit současně otevřený s Eclipse nebo jiným editorem a překlady průběžně doplňovat a když je to větší projekt tak jednoduše to celé dělám v jednom jazyce případně v CZ, EN variantě a potom může kdokoliv lehce přeložit text do dalšího jazyka.

Zdrojáky ukázky jsou ke stažení na <a href="http://ladislav.prskavec.net/download/gettext.zip">zip archivu</a>.

V ZF 1.6 budou změny v <a href="http://weblog.ronnieweb.net/index.php?text=21-nove-view-helpery-a-zmeny-v-zend-translate-v-zend-frameworku-1-6">Zend_Translate</a>.


Používá někdo nějakou vlastní metodu s parsováním pro překlad textů obdobně jako to dělá getext?
