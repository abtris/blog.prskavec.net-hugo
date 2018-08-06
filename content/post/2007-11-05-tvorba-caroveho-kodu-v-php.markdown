---
date: 2007-11-05T00:00:00Z
published: true
status: publish
tags:
- barcode
- php
title: Tvorba čárového kódu v PHP
type: post
url: /2007/11/05/tvorba-caroveho-kodu-v-php/
---

Nedávno jsem tvořil apliakci, která obsahuje čárový k<span>ód. Doposud jsem neměl s tím co dočinění, ale získal jsem cenné zkušenosti, které se mohou hodit i vám.</span>

<span>Čárový k<span>ód (barcode) má <a HREF="https://en.wikipedia.org/wiki/Barcode">různé normy.</a></span></span> Já jsem musel podle naší aplikace použít Code 128B. Je několik možností jak danou problematiku řešit.
<ol>
	<li>máte font, který umí přímo psát v daném čárovém k<span>ódu</span></li>
	<li>budete generovat obrázek pomocí nějaké aplikace (tuto variantu jsem zvolil já)</li>
	<li>pokud generujete PDF, můžete použít XSL-FO a generovat k<span>ód přímo v něm</span></li>
</ol>
Vzhledem k problémům s nákupem fontu a časové tísni jsem zvolil variantu 2. Použil jsem knihovnu <a HREF="https://www.ashberg.de/php-barcode/">PHP-barcode</a>, kterou musím doporučit. Mám ji odzkoušenou jak pro Windows tak Linux, kde je v produkčním nasazení. Udělal jsem si pár úprav zdrojáků (výhoda php), které mi ořezávají výšku k<span>ódu, aby se vešel do našeho designu. Generoval jsem PDF, kde jsem použil PNG obrázek čárového k<span>ódu, výhodu oproti použití fontu vidím hlavně v tom, že nemusím vkládat font, což nemusí některé licenční podmínky u těchto fontů dovolit.</span></span>

Pro volání ve vlastním k<span>ódu jsem použil CURL a vlastní  generování se chová jako samostatná aplikace.</span>

<pre> 	/**
* Generovani unikatniho jmena souboru
*/
$filename="barcode_".$sid.".png";
/**
* URL pro generovani caroveho kodu
*/
$url=$config-&gt;barlib."barcode.php?<a href="https://www.parfumes.sk/giorgio-armani/c-2681/">code</a>=".$input[croom]."&amp;encoding=128B&amp;scale=1&amp;mode=png&amp;filename=".$filename;
$logger-&gt;debug($url);
/**
* Curl inicializace
*/
$ch = curl_init();
/**
*  Set URL and other appropriate options
*/
curl_setopt($ch, CURLOPT_URL, $url);
/**
* Vypinani kontroly certifikatu u SSL spojeni
* Stejne pozor ! u IE7 je nutny bezproblemovy certifikat, jinak muze byt problem se stahnutim PDF
*/
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false);
curl_setopt($ch, CURLOPT_HEADER, false);
curl_setopt($ch, CURLOPT_RETURNTRANSFER,1);
// grab URL and pass it to the browser
$res=curl_exec($ch);
// close cURL resource, and free up system resources
curl_close($ch);
/**
* Konec generovani PNG pro barcode
*/</pre>
