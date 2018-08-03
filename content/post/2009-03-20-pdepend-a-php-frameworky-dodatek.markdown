---
date: 2009-03-20T00:00:00Z
published: true
status: publish
tags:
- php
- qa
title: pDepend a php frameworky dodatek
type: post
url: /2009/03/20/pdepend-a-php-frameworky-dodatek/
---

V příspěvku <a href="https://blog.prskavec.net/2009/03/pdepend-a-php-frameworky/">pDepend a php frameworky</a> jsem uvedl porovnání několika frameworků pomocí <a href="https://www.pdepend.org/">PHP Depend</a>. Do příspěvku už se nevešli <a href="https://www.akelos.org/">Akelos</a>, <a href="https://www.yiiframework.com/">YII</a> a <a href="https://kohanaphp.com/">Kohana</a>. Kohana je fork CodeIgniteru, tak vás nepřekvapí podobné výsledky, jen posunuté trochu více do abtraktního pole v grafu. YII neprošel kvůli chybě: <pre>Invalid token "{" on line 158 in file: /phpdepend/yii-read-only/framework/cli/views/shell/crud/controller.php</pre>  Ještě jsem zkoušel <a href="https://ez.no/ezcomponents">eZ components</a> a to havarovalo na
<pre>Fatal error: Maximum function nesting level of '100' reached, aborting! in /usr/share/php/PHP/Depend/Metrics/NPathComplexity/Analyzer.php on line 330</pre>

Protože příspěvek měl docela dobrou čtenost chci se zeptat zda někomu tam nechyběl nějaký zajímavý php framework, který třeba neznám, rád ho zahrnu.

Pokud se zajímáte o problematiku QA tak bych vám chtěl doporučit článek a slidy Sebastian Bergmanna (<a href="https://sebastian-bergmann.de/archives/856-Quality-Assurance-Tools-for-PHP.html">Quality Assurance Tools for PHP</a>, <a href="https://www.slideshare.net/sebastian_bergmann/quality-assurance-in-php-projects-1163460?type=powerpoint">Quality Assurance in PHP Projects</a>). Bergmann sice prosazuje CC a  PhpUc a já jsem pro nasazení Hudson v Continuous Integration, ale slidy jsou velmi zajímavé a určitě se k této problematice vrátím v nějakém dalším článku.

<h3>Kohana</h3> <a href="https://kohanaphp.com/">https://kohanaphp.com/</a>
<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/kohana-jdepend.png" alt="kohana-jdepend" />
<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/kohana-pyramid.png" alt="kohana-pyramid" />
<h3>Akelos</h3>
<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/akelos-jdepend.png" alt="akelos-jdepend" />
<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/akelos-pyramid.png" alt="akelos-pyramid" />
