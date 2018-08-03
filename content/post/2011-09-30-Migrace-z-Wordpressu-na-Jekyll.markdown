---
tags:
- wordpress
- jekyll
comments: true
date: 2011-09-30T00:00:00Z
title: Migrace z Wordpressu na Jekyll
url: /2011/09/30/Migrace-z-Wordpressu-na-Jekyll/
---

## Co jsem řešil

V poslední době jsem neměl moc času na psaní a když jsem chtěl psát, chtěl jsem se tomu věnovat, mít na to jednoduchý prostředek jako je Textmate (ByWord, iA Writer, WriteRoom, OmmWriter) a přitom se nemuset handrkovat s HTML a s tím jak mi občas nevhodné styly ničí text nebo kusy kódu.

Wordpress u mě na hostingu ještě narazil na limit starého MySQL a mě se také nechtělo migrovat na nový databázový server, potom mi také chybělo verzování v Gitu na který jsem zvykl.

Protože jsem před časem viděl, že [Github pages](https://pages.github.com) se dají dobře použít pro vlastní blog, rozhodl jsem se na to přejít se svými blogy [TopTopic?](https://blog.prskavec.eu) a [Prskavčí blog](https://blog.prskavec.net).

<!--more-->

## Jak jsem postupoval

1. Migrace starého obsahu z Wordpressu
2. CNAME a DNS
3. Nastavení stylů a práce s [Jekyll](https://github.com/mojombo/jekyll/wiki/)
	- komentáře
	- twitter
	- atom feed
4. Publikace na github.com


## Jekyll - statický generátor stránek v Ruby

Podporuje publikování v textile a markdown, případně html.

Instalace pomocí rubygems
{{< highlight sh >}}
sudo gem install jekyll
{{< / highlight >}}


Pro ladění na lokálním počítači jde pustit lokální instanci, která běží [https://localhost:4000](https://localhost:4000).

{{< highlight sh >}}
jekyll --server --auto
{{< / highlight >}}

Jekyll funguje tak, že na základě předpisu generuje statické stránky do adresáře `_site`. Parametr `auto` umí po editaci automaticky přegenerovat změněný soubor.

## Migrace z Wordpressu

1. Založte si adresář a začněte verzovat gitem.

{{< highlight sh >}}
mkdir blog.prskavec.net
cd blog.prskavec.net
git init
{{< / highlight >}}

1. Je potřeba udělat export z Wordpressu, pomocí `export.php`.

- Například u mě to bylo `https://blog.prskavec.net/wp-admin/export.php`.
- Na výsledné xml je potřeba použít [importní script](https://github.com/mojombo/jekyll/tree/master/lib/jekyll/migrators).
- K dispozici jsou importní scripty pro Wordpress, CSV, Drupal, Posterous, Tumblr, Typo, Textpattern, Wordpress.com a další
2. Já jsem si zkopíroval do adresáře `_import` zkopíroval scripty pro wordpress. Potom jsem provedl import.

	ruby -r './_import/wordpressdotcom.rb' -e 'Jekyll::WordpressDotCom.process("prskavblog.wordpress.2011-09-28.xml")'

3. Všechny posty se uloží automaticky do `_post`.

## CNAME a DNS

1. Do repository přidáte soubor CNAME který musí obsahovat doménu na které chcete Github pages provozovat, samozřejmě pokud vám nestačí stávající co dostanete od Githubu.
2. DNS - musíte si zřídít CNAME záznam, který bude ukazovat na váš záznam na githubu. U mě `abtris.github.com` pro doménu 3 řádu. Pokud by jste chtěli doménu 2 řádu musíte zřídit A záznam na 207.97.227.245.

## Nastavení stylů a práce s Jekyll

Základní struktura

	|-- _config.yml
	|-- _layouts
	|   |-- default.html
	|   `-- post.html
	|-- _posts
	|   |-- 2007-10-29-why-every-programmer-should-play-nethack.textile
	|   `-- 2009-04-26-barcamp-boston-4-roundup.textile
	|-- _site
	`-- index.html

`_config.yml`

	sitename: Prskavčí blog
	tagline: Jen další blog o všem možné, teď od Prskavců.
	url: https://blog.prskavec.net
	ga: UA-XXXXX-XX
	pygments:    true
	markdown:    maruku
	permalink:   pretty
	exclude: LICENSE, README.markdown, Rakefile
	disqus:
	   id: prskavecblog
	feedburner:
	   id: prskavec

Nejdůležitější odkazy:

- [Použití](https://github.com/mojombo/jekyll/wiki/Usage)
- [Konfigurace](https://github.com/mojombo/jekyll/wiki/Configuration)
- [Ukázkové stránky](https://github.com/mojombo/jekyll/wiki/Configuration)
- [Důležité proměnné](https://github.com/mojombo/jekyll/wiki/Template-Data)
- [Značkovací jazyk Liquid, který se používá v Jekyll šablonách](https://github.com/shopify/liquid/wiki/liquid-for-designers)
- [Pluginy](https://github.com/mojombo/jekyll/wiki/Plugins)
- [Lexers for Pygments](https://pygments.org/docs/lexers/) - syntax hightlighting, příslušné [syntax.css](https://github.com/mojombo/tpw/blob/master/css/syntax.css) třeba tady.
Samozřejmě pro svůj blog potřebujete nějak přispůsobit vzhled, pro Jekyll to není tak snadné jako Wordpress, který má spoustu pluginů a vzhledů. Ale není problém, některý z Wordpressu převést.

Pluginy některé existují, ale musíte si vystačit s tím co jde vygenrovat, případně z Javascriptem, ale to není málo.

Pomocí javascriptu jsem přidat [twitter](https://tweet.seaofclouds.com/) a komentáře přes [Disqus](https://disqus.com).

Pro ATOM je k dispozici šablona, obdobně si můžete vygenerovat RSS, archív apod.

## Publikace na Github

1. Založte si repositář na Githubu.
2. V adminu zapnout github pages.
3. Podle návodu přidejte do repositáře soubory a pushnete obsah na server.

## Shrnutí

Jekyll není pro každého, ale pro mě jako pro programátora je to vyhovující systém. Článek jako tento napíšu ve stejném editoru jako programuju ať je to [Textmate](https://macromates.com/) nebo [PHPStorm](https://www.jetbrains.com/phpstorm/). A stránky jsou jen další repository na [githubu](https://github.com).

