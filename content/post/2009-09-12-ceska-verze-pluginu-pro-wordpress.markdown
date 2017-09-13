---
date: 2009-09-12T00:00:00Z
published: true
status: publish
tags:
- wordpress
title: Česká verze pluginů pro Wordpress
type: post
url: /2009/09/12/ceska-verze-pluginu-pro-wordpress/
---

<p><strong>Co k tomu potřebujete?</strong></p>
<ul>
<li>plugin (<a href="http://wordpress.org/extend/plugins/wp-recentcomments/">WP-RecentComments</a>)</li>
<li><a href="http://www.neoease.com/">prográmátora</a>, který s překladem počítá</li>
<li><a href="http://www.poedit.net/">poEdit</a></li>
<li>znalost nějakého jazyka</li>
<li>ochota něco takového vůbec dělat</li>
</ul>
<p><strong>Jak se to dělá</strong></p>
<p>Pokud nejste zběhlý s programováním, tak asi to může být pro vás problém a proto jsem také napsal tento článek, třeba vám to pomůže pokud budete chtít něco takového dělat.</p>
<p>Když si stáhnete plugin WP-RecentComments na disk, rozbalíte ho někam do adresáře uvidíte tuto strukturu:</p>
<pre>drwxr-xr-x 2 abtris 4.0K 2009-09-12 21:14 avatars
-rw-r--r-- 1 abtris  18K 2009-09-12 21:14 core.php(
drwxr-xr-x 2 abtris 4.0K 2009-09-12 21:14 css
drwxr-xr-x 2 abtris 4.0K 2009-09-12 21:14 js
drwxr-xr-x 2 abtris 4.0K 2009-09-12 21:28 languages
-rw-r--r-- 1 abtris  12K 2009-09-12 21:14 README.txt
-rw-r--r-- 1 abtris 8.6K 2009-09-12 21:14 screenshot-1.png
-rw-r--r-- 1 abtris 8.0K 2009-09-12 21:14 screenshot-2.png
-rw-r--r-- 1 abtris  16K 2009-09-12 21:14 screenshot-3.png
-rw-r--r-- 1 abtris  18K 2009-09-12 21:14 wp-recentcomments.php</pre>
<p>V adresáři languages najde to co potřebujete, je tam hromada souborů s příponou po a mo. To jsou soubory ve formátu pro <a href="http://en.wikipedia.org/wiki/GNU_gettext">gettext</a> a mo jsou zkompilované soubory (ty vás nezajímají) a v po souborech najdete zdrojový kód, který je potřeba otevřít pomocí editoru (doporučuji poEdit).</p>
<p>Potom stačí jen přeložit potřebný text, z angličtiny do češtiny a po uložení  se vytvoří soubor (wp-recentcomments-cs_CZ.po) s příponou mo. Pojmenování s koncovkou cs_CZ je potřeba dodržet, aby fungovalo dobře rozpozání jazyka ve Wordpressu.</p>
<a href="http://blog.prskavec.net/wp-content/uploads/2009/09/Screenshot-Poedit-home-abtris-Desktop-wp-recentcomments-languages-wp-recentcomments-cs_CZ.po.png"><img src="http://blog.prskavec.net/wp-content/uploads/2009/09/Screenshot-Poedit-home-abtris-Desktop-wp-recentcomments-languages-wp-recentcomments-cs_CZ.po-300x253.png" alt="Screenshot-Poedit : -home-abtris-Desktop-wp-recentcomments-languages-wp-recentcomments-cs_CZ.po" width="300" height="253" class="aligncenter size-medium wp-image-670" /></a>
<p>Potom si nahrajte upravený plugin na server a máte ho v provozu česky jak vidíte u mne na stránkách.</p>
<p>Dobré je vaši verzi poslat také autorovi ke zveřejnění. Česká verze WP-RecentComments se tam časem až programátor uzná objeví také.</p>
<p>Ukázka přeloženého widgetu dále, pokud máte nějaké připomínky k překladu neváhejte je také uvést v komentářích.</p>
<p><strong>Moje nastavení widgetu pro WP-RecentComments</strong></p>
<p>Nastavení najdete v administraci, Vzhled -&gt; Widgety. Přidáte widget do sidebaru a potom po kliknutí na pravou ikonku rozbalí se vám menu, kterou vidíte na obrázku.</p> 
<p><img class="aligncenter size-full wp-image-666" src="http://blog.prskavec.net/wp-content/uploads/2009/09/Widgety-‹-Prskavci-blog-—-WordPress_1252784358148.png" alt="Widgety ‹ Prskavčí blog — WordPress_1252784358148" width="268" height="672" /></p>
