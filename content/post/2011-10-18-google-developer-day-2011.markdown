---
categories:
- google
- web
comments: true
date: 2011-10-18T00:00:00Z
title: Google Developer Day 2011
url: /2011/10/18/google-developer-day-2011/
---

GDD již tradičně v Clarionu, registrace se dneska pěkně protáhla pokud jste nepřišli včas a nevyužili některé jiné fronty než té první.

<!--more-->

Reportáž online:

## Keynote

9:09 Sedíme na keynote a uvidíme co se chystá ….

[Video na začátku](https://developers.google.com/go/stories) a potom Filip pokračuje v Keynote.

9:19 Brad Abrams - Google Plus Platform
Zatím se probíráme od roku 1990 do součastnosti naší minulostí a přesouváme se k mobile a cloud.
Aktivace telefonů s Androidem mezi únorem 2010 (60k) až k 550k v červenci 2011. 300k aplikací v Android Marketu.

9:35 Petr Nálevka - [Sleep as anDroid](https://market.android.com/details?id=com.urbandroid.sleep&hl=cs), ukázka aplikace.
Budík - umí měřit spánkové cykly. Umí nahrávat spánkovou aktivitu i zvuky, chrápání apod. Statistiky o spaní a návyky. Captcha pro budík, aby jste to snad nezamáčkli. Celé snímání je přes akcelerometr. Aplikace se dá snadno rozšiřovat. 330k aktivních uživatelů a 60k prodaných kusů.

9:43 AdMob - 910 miliard reklam už bylo zobrazeno.

9:45 Chrome a HTML5, Chrome Webstore

9:51 Ilmari Heikkinen - WebGL demo, [tree.js](http://mrdoob.github.com/three.js/), flux slider [transgallery](http://joelambert.co.uk/flux/transgallery.html), asteroids. Pěkné demo na Developers tools v Chrome, zvláště pretty print funkce pro javascript je hodně pěkná.

10:00 WebM Video, Web Audio Api, HTML5 s WebIntents, Native Client (C, CPP v sandboxu), JS a DOM. HTML5 aplikace lehce najdete přes Chrome Web Store a zítra HTML5 Hackathon zítra a jedou na Google IO.

10:05 Video, YouTube 3D - trailer na novou Dobu ledovou ve 3D

10:10 Mano Marks a Jarda Bengl - Google Maps
První Google Maps Mashup - [http://housingmaps.com](http://housingmaps.com). Google Fusion Tables - [demo s populací evropy](http://mano-demos.googlecode.com/svn/trunk/demos/europepopsliders.html). WebGL v mapách, klikněte vlevo dole vyzkoušet něco nového a mapy se přepnou do WebGL. Úžasné celé vektorové mapy místo obrázků a 3D budovy v normálních mapách provázání s satelitní mapou (ukázky New York, Rome, Venice).

{{< figure class="center" src="/assets/googlemaps_webgl_1.png" title="Venice" >}}
<center>Ukázka Venice, kde je vidět vektory a 3D domy.</center>

{{< figure class="center" src="/assets/googlemaps_webgl_2.png" title="Rome" >}}
<center>Při zoomu jsou některé budovy vidět v náhledu 45 stupňů.</center>

{{< figure class="center" src="/assets/googlemaps_webgl_3.png" title="Rome" >}}
<center>A dá se s nimi i otáčet.</center>

10:24 Cloud, webová verze Angry Birds používá (GWT, HTML5, Google Plus, App Engine). Mobilní hry často App Engine používání na backendu (např. TapZoo). App Engine pro Google Doodles ([ukázka](http://www.google.com/logos/2011/lespaul.html), záznamy za 2 dny nahráli uživatelé několik let záznamů.

[Google Cloud Storage](http://code.google.com/apis/storage/), [Google Prediction API](http://code.google.com/apis/predict/) jdou do provozu z Labs.

[Google Cloud SQL](http://code.google.com/apis/sql/) uvádějí jako novinku. Iein Valdez uvedl demo. Použije se JDBC driver pro Google Cloud SQL (GCSQL). Ukázka na App Engine byla v Javě a PHP.

10:40 Google Plus s Circles, Hangouts, Mobile. Novinky přidávájí stále a během 90 dnů od spuštění přidali přes 100 novinek. Teď mají kolem 40M uživatelů a 3,4 mld. fotek. Plugins (+1 button), anotace pro vaše výsledky hledání. APIs (RESTful, JSON, OAuth2) a knihovny pro všechny běžné jazyky ([Google Apis Explorer](https://code.google.com/apis/explorer). 

10:50 Jonathan Beri - Hangouts demo, přijde mi to jako Google Wave v nové verzi zaměřené více na to co lidé chtějí. Více o Google platform na [https://developers.google.com/+/](https://developers.google.com/+/).

Keynote končí.

## Ilmari Heikkinen - Tohle nejsou weby, které hledáte: Moderní webové aplikace v HTML5

Definuje moderní webovou aplikaci jako desktop UI s cloud backendem (příklad DJBreakPoint aplikace).

MVC Frameworks (Sproutcore, BackboneJS, ExtJS 4). Css Frameworks (Less, Sass). Divné, že nezmínil AngularJS, který Google přímo podporuje a vyvíjí. Responsive layout pro notebooky, mobily a tablety. 

- [formfactorjs.com](http://formfactorjs.com)
- [www.leviroutes.com](http://www.leviroutes.com) (pushState, řeší MVC JS Frameworky)

Ukázka webkitdirectory a drag and drop pro práci se soubory v Chrome. Ukázka ukládání na filesystem. Application cache pro ukládání.

	<html manifest='cache.appcache'>

- [LawnChair](http://westcoastlogic.com/lawnchair/) - simple json storage
- x-webkit-speech attribute například pro input.
- desktop notifikace - webkitNotifications
- performance tips - [http://bit.ly/rizNVE](http://bit.ly/rizNVE)

### Old browsers 
- Chrome Frame
- HTML5 Boiler Plate
- Modernizr
- jQuery
	

### Chrome Web store

- $5 poplatek za registraci
- možnost monetizace HTML5 aplikací
- [AppMator](http://appmator.appspot.com/) umí udělat manifest file

Slidy [http://bit.ly/nrjLs7](http://bit.ly/nrjLs7).

Reportáž skončila.

## Shodnocení GDD 2011

Později jsem se vrátil na GDD bohužel až na Ignite, ale ty rozhodně stály za to. Přišlo mi to jako skvělý závěr GDD2011 a co bych chtěl ocenit je i přes menší rozpočet spousta věcí zaujala. Ocenil bych více advanced přednášek, kde přednášející mi řekne něco co nevím, ale to je hodně individuální. 

Jinak celkem dobrá organizace, trochu registrace vázla, když člověk přišel později. Celkově jsem potkal spoustu lidí, které vidím jen tady.

Za mě jediné přání do dalšího roku, líbil by se mi například [Vojta Jína](https://github.com/vojtajina) nebo někdo s týmu [AngularJS](http://angularjs.org/) a nějaký workshop s nimi ať na GDD nebo mimo.

## Update 24.10.2011
K dispozici jsou [všechny videa](http://www.youtube.com/watch?v=1Xn6CfJ713g&feature=list_related&playnext=1&list=SP64D87C03BA3B2A88). Úžasné!











