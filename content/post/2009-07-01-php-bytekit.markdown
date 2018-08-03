---
date: 2009-07-01T00:00:00Z
published: true
status: publish
tags:
- php
- qa
title: PHP Bytekit
type: post
url: /2009/07/01/php-bytekit/
---

<h3>Bytekit</h3>
<p><a href="https://www.bytekit.org/">Bytekit</a> je nová extenze pro PHP, která umožňuje analyzovat PHP kód na úrovni opcodes, které generuje compiler v PHP. Také nám umožňuje pomocí disassembleru sledovat jak probíhá program na úrovni compileru. Je to další z nástrojů, které umožní zlepšit kvalitu kódu a určitě přinese řadu další analytických nástrojů. Autorem je Stefan Esser, kterého jistě každý zná. Jeho <a href="https://www.suspekt.org/">blog</a> a příspěvky k bezpečnosti PHP jsou známy. </p>

<h3>Bytekit-cli</h3>
<p>
Samotná extenze, ale není všechno. Abychom mohli ji dobře využít tak <a href="https://sebastian-bergmann.de/archives/871-bytekit-cli.html">Sebastian Bergmann</a> vytvořil <a href="https://github.com/sebastianbergmann/bytekit-cli/tree/master">bytekit-cli</a>, které nám umožní ho využít.
V článku od SB jsou pěkné příklady, které doporučuji vyzkoušet. Bytekit zatím není dostupné přes PECL tak ho musíte zkompilovat ze zdrojového kódu, známým postupem (phpize, configure, make, make install) a zkompilovanou extenzi přidat do konfigurace PHP. Potom si z githubu můžete stáhnout aktuální verzi bitekit-cli a můžete si hrát.</p>

Například otestuji jak je na tom s přímým výstupem Zend Framework.

<pre>
abtris@ubuntu# bytekit --rule=DirectOutput ZendFramework/

 - Direct output of variable $message
    in ZendFramework/demos/Zend/Gdata/YouTubeVideoApp/operations.php:1094

  - Direct output of variable $playlistEntries
    in ZendFramework/demos/Zend/Gdata/YouTubeVideoApp/operations.php:906

  - Direct output of variable $message
    in ZendFramework/demos/Zend/Gdata/YouTubeVideoApp/operations.php:270

  - Direct output of variable $message
    in ZendFramework/demos/Zend/Gdata/YouTubeVideoApp/operations.php:276

  - Direct output of variable $user
    in ZendFramework/demos/Zend/Gdata/Photos.php:796

  - Direct output of variable $albumId
    in ZendFramework/demos/Zend/Gdata/Photos.php:797

  - Direct output of variable $photoId
    in ZendFramework/demos/Zend/Gdata/Photos.php:798

  - Direct output of variable $user
    in ZendFramework/demos/Zend/Gdata/Photos.php:825

  - Direct output of variable $albumId
    in ZendFramework/demos/Zend/Gdata/Photos.php:826

  - Direct output of variable $photoId
    in ZendFramework/demos/Zend/Gdata/Photos.php:827

  - Direct output of variable $user
    in ZendFramework/demos/Zend/Gdata/Photos.php:741

  - Direct output of variable $albumId
    in ZendFramework/demos/Zend/Gdata/Photos.php:742

  - Direct output of variable $user
    in ZendFramework/demos/Zend/Gdata/Photos.php:688

  - Direct output of variable $type
    in ZendFramework/demos/Zend/WebServices/Amazon/amazon-search.php:153

  - Direct output of variable $type
    in ZendFramework/demos/Zend/WebServices/Amazon/amazon-search.php:157

  - Direct output of variable $keywords
    in ZendFramework/demos/Zend/WebServices/Flickr/flickr-composite.php:92

  - Direct output of variable $form
    in ZendFramework/demos/Zend/ProgressBar/ZendForm.php:209

  - Direct output of variable $ret
    in ZendFramework/demos/Zend/OpenId/test_server.php:264

  - Direct output of variable $id
    in ZendFramework/demos/Zend/OpenId/test_consumer.php:115

  - Direct output of variable $response
    in ZendFramework/library/Zend/Rest/Server.php:277

  - Direct output of variable $output
    in ZendFramework/library/Zend/Cache/Frontend/Class.php:226

  - Direct output of variable $data
    in ZendFramework/library/Zend/Cache/Frontend/Page.php:280

  - Direct output of variable $data
    in ZendFramework/library/Zend/Cache/Frontend/Output.php:101

  - Direct output of variable $data
    in ZendFramework/library/Zend/Cache/Frontend/Output.php:65

  - Direct output of variable $output
    in ZendFramework/library/Zend/Cache/Frontend/Function.php:107

  - Direct output of variable $response
    in ZendFramework/library/Zend/Json/Server.php:198

  - Direct output of variable $data
    in ZendFramework/library/Zend/ProgressBar/Adapter/JsPull.php:111

  - Direct output of variable $output
    in ZendFramework/library/Zend/Debug.php:102

  - Direct output of variable $exceptions
    in ZendFramework/library/Zend/Controller/Response/Abstract.php:734

  - Direct output of variable $content
    in ZendFramework/library/Zend/Controller/Response/Abstract.php:546

  - Direct output of variable $output
    in ZendFramework/library/Zend/Tool/Framework/Client/Console.php:194
</pre>

Používá Zend Framework někde EVAL?

<pre>
abtris@ubuntu#bytekit --rule=DisallowedOpcodes:EVAL ZendFramework/
 - Disallowed opcode "EVAL"
    in ZendFramework/demos/Zend/Gdata/Health.php:92

  - Disallowed opcode "EVAL"
    in ZendFramework/demos/Zend/Gdata/Health.php:99

  - Disallowed opcode "EVAL"
    in ZendFramework/demos/Zend/Gdata/Health.php:126

  - Disallowed opcode "EVAL"
    in ZendFramework/demos/Zend/Gdata/Health.php:129

  - Disallowed opcode "EVAL"
    in ZendFramework/demos/Zend/Gdata/Health.php:159

  - Disallowed opcode "EVAL"
    in ZendFramework/demos/Zend/Gdata/Health.php:164

  - Disallowed opcode "EVAL"
    in ZendFramework/demos/Zend/Gdata/Health.php:195

  - Disallowed opcode "EVAL"
    in ZendFramework/demos/Zend/Gdata/Health.php:200

  - Disallowed opcode "EVAL"
    in ZendFramework/demos/Zend/Gdata/Health.php:223

  - Disallowed opcode "EVAL"
    in ZendFramework/demos/Zend/Gdata/Health.php:230

  - Disallowed opcode "EVAL"
    in ZendFramework/demos/Zend/Gdata/Health.php:271
</pre>

<h3>Závěr</h3>
<p>Stefan Esser ve své <a href="https://www.suspekt.org/downloads/DPC_Secure_Programming_With_The_Zend_Framework.pdf">přednášce o bezpečnosti Zend Frameworku</a> se zmiňuje o možnosti například kontrolovat EVAL v pre-commitu tasku, obdobně jako například dnes kontroluji syntaxi php.</p>
<p>Koncem září se budu věnovat porovnání PHP Frameworků z hlediska nástrojů pro QA, které jsem dělal v článku <a href="https://blog.prskavec.net/2009/03/pdepend-a-php-frameworky/">pDepend a php frameworky</a>, uvidíme jak jednotlivé frameworky za tu dobu pokročili a také si budu všímat frameworků pomocí dalších nástrojů mezi které bytekit jistě zahrnu.</p>
<p>Nesmím opomenout připomenout také, že je venku PHP 5.3, na které se čekalo poměrně dlouho a doufám jen, že nebude tolik plné chyb jako jeho přechůdci 5.2 a 5.1. Uvidíme zda na příchod PHP 5.3 jsou připraveny i jiné frameworky než <a href="https://nettephp.com">Nette</a>.</p>
