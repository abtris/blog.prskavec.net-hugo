---
categories: [css]
comments: true
date: 2012-03-01T00:00:00Z
title: Jak snadno pracovat s CSS sprites
url: /2012/03/01/jak-snadno-pracovat-s-css-sprites/
---

{% blockquote cs.spritegen.website-performance.org http://cs.spritegen.website-performance.org/section/what-are-css-sprites/ Co jsou CSS sprites? %}
CSS spirty představují způsob, jak snížit počet HTTP požadavků, které klient vyšle k získání prvků obsažených na stránce. Obrázky se sloučí do jednoho většího a umístí se na určených X,Y souřadnicích. Pak pomocí CSS atributu background-position můžeme nastavit vzniklý obrázek na pozadí různým elementům stránky a pomocí dalších CSS vlastností umístíme pozadí tak, aby požadovaný jednotlivý obrázek padl do viditelné oblasti elementu na stránce.
{% endblockquote %}

<!--more-->

Poslouchal jsem [podcast](http://official.fm/tracks/352028) s [Vaškem Vančurou](http://vaclav.vancura.org), kde popisuje svoji práci s LESS a sprites pomocí software [TexturePacker](http://www.texturepacker.com/).

Zkoušel jsem Vaška přemluvit, aby něco napsal:

{% blockquote @vancura https://twitter.com/vancura/status/175122362516258816 %}
@vancura Me se to libilo, taky jsem si zavzpominal. Ale o tom jak pouzivas LESS a sprites nechces o tom neco napsat s nejakym prikladem?
@abtris Kdyby byl cas…
{% endblockquote %}

Ale nemá čas, tak jsem se rozhodl to udělat sám.

Zvolil jsem jednoduchou ukázku, stáhnul jsem [sadu icon](http://wefunction.com/2008/07/function-free-icon-set/) a potom jsem zvolit v Texturepackeru data format css a přidal jsem adresář s iconami. Program automaticky vytvoří pomocí publish sprite.png a sprite.css.

Výsledek můžete vidět zde:

{{< figure class="center" src="/assets/sprite.png" >}}

a css kód:

{% include_code sprite.css %}

## Shrnutí

Je to jen ukázka jak může tento program usnadnit práci, kterou jsem viděl obvykle dělat kodéry ve Photoshopu a pracně ručně nastavovat v CSS.

