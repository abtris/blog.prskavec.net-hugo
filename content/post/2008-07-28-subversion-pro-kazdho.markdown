---
date: 2008-07-28T00:00:00Z
published: true
status: publish
tags:
- scm
- svn
title: Subversion pro každého
type: post
url: /2008/07/28/subversion-pro-kazdho/
---

Protože jsem v češtině nenašel žádnou knihu, kterou bych mohl strčit do ruky někomu kdo se mě ptá jak začít pracovat se Subversion a návody na webu nejsou zcela ucelené, tak jsem se rozhodl <a href="http://svn.prskavec.net">napsat takovou příručku pro každého</a> kdo chce se Subversion pracovat.

Nesnažím se o překlad <a href="http://svnbook.red-bean.com/">SVN book</a> i když ten by této knize výrazně pomohl, ale snažím se shrnout postupy a praxe co nejjednodušeji, aby to začátečník pochopil. Já už se SVN, ale dělám delší čas a nemám potřebný odstup a proto budu vděčný za zpětnou vazbu co týká obsahu, co přidat a co je zbytečné a případných chyb.

Ke psaní jsem použil <a href="http://www.docbook.cz">Docbook</a> a jako editor XMLmind, PSPad a SciTE. Do html převádím docbook za pomoci PHP tímto jednoduchým skriptem.

<pre name='code' class="php">
&lt;?php
$xml_filename="svn-kniha.xml";
$xsl_filename="c:\Program Files\XMLmind_XML_Editor\addon\config\docbook\xsl\xhtml\docbook.xsl";

$doc = new DOMDocument();
$xsl = new XSLTProcessor();

$doc-&gt;load($xsl_filename);
$xsl-&gt;importStyleSheet($doc);

$doc-&gt;load($xml_filename);

file_put_contents("index.html",str_replace("&lt;/head&gt;","&gt;&lt;link href='default.css' type='text/css' rel='stylesheet' /&gt;
&lt;/head&gt;",$xsl-&gt;transformToXML($doc)));

?&gt;
</pre>
