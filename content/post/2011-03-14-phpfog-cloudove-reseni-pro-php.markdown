---
date: 2011-03-14T00:00:00Z
published: true
status: publish
tags:
- cloud
- php
- phpfog
title: phpfog - cloudové řešení pro PHP?
type: post
url: /2011/03/14/phpfog-cloudove-reseni-pro-php/
---

Pokud se zajímáte o cloudové technologie, tak možná znáte <a href="https://heroku.com/">Heroku</a>. Heroku je pěkné řešení pro Ruby, které vám umožní vytvářet aplikace v Sinatře nebo v Rails a deployment provádět pomocí Gitu. Pro PHP mi něco takového dlouho chybělo, ale začíná se částečně situace vylepšovat, protože je na světe <a href="https://phpfog.com">PHP Fog</a>.

<a href="https://blog.prskavec.net/wp-content/uploads/2011/03/phpfog-homepage-3.jpg"></a><a href="https://blog.prskavec.net/wp-content/uploads/2011/03/phpfog-homepage-4.jpg"><img class="aligncenter size-medium wp-image-6432" title="phpfog-homepage-4" src="https://blog.prskavec.net/wp-content/uploads/2011/03/phpfog-homepage-4-300x163.jpg" alt="" width="300" height="163" /></a>
<h2>Co vám přinese PHP Fog</h2>
Mě se na cloudovém řešení libí, že je to pro vývojáře jednoduché a praktické. Nemusím řešit server, jeho provoz. Jen si nastavím konfiguraci apache, php.ini a vytvořenou aplikaci si přes git push pošlu na server k deploymentu a za pár okamžiků to běží.

Podobně to lehce vyřeším i na svém serveru pomocí SSH a Capistrano, ale stejně se musím starat o instalaci VPS. Jednodušší varianta je jen hosting s SSH přístupem například co mám na <a href="https://www.hostmonster.com">Hostmonster</a>. Tam je problém, ale se škálováním pokud by se stal projekt úspěšný.

Ukážu jak jsem během 5min rozběhl Zend Framework projekt s CouchDb hostovanou na couchone hostingu. Můžete použít MySQL, další db přímo hosting nepodporuje. Doufám, že to časem rozšíří hlavně o podporu PostgreSQL a některé NoSQL (CouchDb, MongoDb).
<ul>
	<li>PHP Version 5.3.2</li>
	<li>Apache Version 2.2.14</li>
	<li>MySQL Version 5.1.41</li>
</ul>
<h2>Vytvoření nové aplikace</h2>
Po přihlášení, zatím přístup je omezen, registrace přes nějaké <a href="https://phpfog.com/#sign-up">Sign Up kódy</a>, dostupný link najdete na homepage PHP Fog. Pokud chci vytvořit novou aplikaci, musím si vybrat profil, kde jsou setupy pro známé frameworky nebo přímo aplikace.

<a href="https://blog.prskavec.net/wp-content/uploads/2011/03/phpfog-newapp-3.jpg"><img class="aligncenter size-medium wp-image-6422" title="phpfog-newapp-3" src="https://blog.prskavec.net/wp-content/uploads/2011/03/phpfog-newapp-3-300x171.jpg" alt="" width="300" height="171" /></a>

Já jsem zvolil zend framework a pokračoval k dalšímu kroku, kde si vybere formu hostingu. Na prvních 6 měsíců můžete zvolit variantu zdarma. V dokumentaci se píše, že bude po těch 6 měsících nějak zpoplatněna. To si myslím, že není moc dobrý tah a autoři doufám od toho upustí.

<a href="https://blog.prskavec.net/wp-content/uploads/2011/03/phpfog-price-3.jpg"><img class="aligncenter size-medium wp-image-6423" title="phpfog-price-3" src="https://blog.prskavec.net/wp-content/uploads/2011/03/phpfog-price-3-300x116.jpg" alt="" width="300" height="116" /></a>

Po zvolení tarifu tak se dostanete do nastavení kde je potřeba poladit pár věcí a udělat si checkout Git repository pro váš projekt. Najde si v nastavení také nastavení Vhostu apache a php.ini.

Hlavní nastavení je nahrání SSH public key pro přístup ke Gitu, podobně jako na Githubu. Škoda, že se nedá přímo integrovat například pomocí nastavení remote větve na Github server. SSH a FTP přístup není k dispozici.

<a href="https://blog.prskavec.net/wp-content/uploads/2011/03/phpfog-sourcecode-3.jpg"><img class="aligncenter size-medium wp-image-6424" title="phpfog-sourcecode-3" src="https://blog.prskavec.net/wp-content/uploads/2011/03/phpfog-sourcecode-3-300x99.jpg" alt="" width="300" height="99" /></a>

Potom jsem udělal git checkout, nahrál do repository kód a pomocí git push spustil aplikaci, která běží na doméně podle jména které zadáte při vytváření aplikace. Provoz aplikace částečně můžete kontrolovat pomocí nahlížení do logů apache přes webové rozhraní.

<a href="https://blog.prskavec.net/wp-content/uploads/2011/03/phpfog-logs-3.jpg"><img class="aligncenter size-medium wp-image-6421" title="phpfog-logs-3" src="https://blog.prskavec.net/wp-content/uploads/2011/03/phpfog-logs-3-300x157.jpg" alt="" width="300" height="157" /></a>
<h2>Závěr</h2>
Projekt je na začátku a doufám, že bude mít štěstí a že se mu brzo objeví i další konkurenti, protože do Heroku to má daleko, ale jdou dobrou cestou. Trochu mi přijde cenová politika trochu dražší než bežná jiná řešení, ale asi to bude daň za provoz na Amazon cloudu (MySQL).

Pokud jste to někdo další vyzkoušeli nebojte se o to s námi podělit, případně nevíte někdo o dalších alternativách pro PHP?

&nbsp;

&nbsp;
