---
date: 2009-03-16T00:00:00Z
tags:
- php
- qa
title: pDepend a php frameworky
type: post
url: /2009/03/16/pdepend-a-php-frameworky/
---

<h3>pDepend</h3>
<a href="https://www.pdepend.org">https://www.pdepend.org</a>

Php Depend (pDepend) patří do skupiny nástrojů QA (Quality assurance) pro kód a je odvozen od <a href="https://clarkware.com/software/JDepend.html">JDepend</a>, kde je také popis části metrik, které pDepend používá. V nedávné době byla spuštěny nové stránky projektu a verzí 0.9.4 se mi zdá již velmi použitelný. Abych vyzkoušel jak to funguje vzal jsem si z SVN repozitářů několik frameworků a pustil nad nimi pDepend.
<h3>CodeIgniter</h3>
<a href="https://codeigniter.com">https://codeigniter.com</a>

<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/ci171-jdepend.png" alt="" />

<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/ci171-pyramid.png" alt="" />
<h3>Nette</h3>
<a href="https://nettephp.com/">https://nettephp.com/</a>

<img style="max-width: 800px;" src="https://blog.prskavec.net/wp-content/uploads/2009/03/nette-jdepend.png" alt="" />

<img style="max-width: 800px;" src="https://blog.prskavec.net/wp-content/uploads/2009/03/nette-pyramid.png" alt="" />
<h3>Zend Framework</h3>
<a href="https://framework.zend.com/">https://framework.zend.com/</a>

<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/zend-jdepend.png" alt="" />

<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/zend-pyramid.png" alt="" />
<h3>Symfony 1.2</h3>
<a href="https://www.symfony-project.org/">https://www.symfony-project.org/</a>

<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/sf12-jdepend.png" alt="" />

<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/sf12-pyramid.png" alt="" />
<h3>Solar</h3>
<a href="https://solarphp.com/">https://solarphp.com/</a>

<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/solar-jdepend.png" alt="" />
<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/solar-pyramid.png" alt="" />

<h3>CakePHP</h3> <a href="https://cakephp.org/">https://cakephp.org/</a>

<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/cake-jdepend.png" alt="cake-jdepend" title="cake-jdepend"  />
<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/cake-pyramid.png" alt="cake-pyramid" title="cake-pyramid"  />


<h3>Prado</h3> <a href="https://www.pradosoft.com/">https://www.pradosoft.com/</a>

<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/prado3-jdepend.png" alt="prado3-jdepend" title="prado3-jdepend"  />
<img src="https://blog.prskavec.net/wp-content/uploads/2009/03/prado3-pyramid.png" alt="prado3-pyramid" title="prado3-pyramid"  />

<h3>Závěr</h3>
Nejsem expert na QA abych mohl fundovaně hovořit na základě těchto dat o kvalitě frameworků, jen mě zaujalo několik věcí. Z grafů je vidět nakolik je mezi frameworky používaná abtrakce a nakolik jsou si v konstrukci podobné Symfony a Zend a jak se liší Nette a CodeIgniter.
Z pyramid mě zaujali čísla kolem LOC (počet řádků kódu) v porodnání s počtem metod (NOM) a tříd (NOC). ANDC (průměrná hodnota odvozených tříd) nám řekne jak to framework s dědičností. AHH (průměrná výška hiarchie) je metrika která udá průměr hloubku v dědičnosti.
<a href="https://www.manuel-pichler.de/archives/31-Using-the-Overview-Pyramid.html">Podrobný popis pyramid</a> vám odpoví na otázky co znamená, která značka a hlavně je tam tabulka referenčních hodnot jednotlivých metrik.

Bohužel jakákoliv metrika vám nikdy neřekne nic o tom, jak je který framework dobrý nebo zda vám bude vyhovovat.
<div class="zemanta-pixie"><img class="zemanta-pixie-img" src="https://img.zemanta.com/pixy.gif?x-id=0edc62c8-1f17-4faa-a8c9-fa5ade65fd27" alt="" /></div>
