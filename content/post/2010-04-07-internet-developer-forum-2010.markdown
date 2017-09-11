---
date: 2010-04-07T00:00:00Z
meta:
  _edit_last: "1"
  _encloseme: "1"
  _pingme: "1"
published: true
status: publish
tags:
- '#vyvojka'
- iinfo
- konference
- ostatnÃ­
- reportaz
title: Internet Developer Forum 2010
type: post
url: /2010/04/07/internet-developer-forum-2010/
---

Dnes 7.4.2010 se koná v NTK v Praze, Dejvicích konference pro vývojáře webových aplikaci, kterou pořádá <a href="http://konference.iinfo.cz/">Iinfo.cz</a>

V průběhu dne tu na blogu budu aktualizovat reportáž z konference a postřehy.

9:01 Konference stále nazačala, ale sedím v sále a mám wifi připojení a přes NTK-SIMPLE, pro jistotu jsem si rychle zřídil na svoji OpenCard členství v knihovně NTK.

9:03 Petr Krčmář oznamuje, že se start protáhne, snad to nebude platit o celé konferenci.

9:15 Petr Krčmář zahajuje konferenci

<strong>Pro koho děláme web - Adam Fendrych</strong>

Klasická přednáška o přístupnosti, fakta dobrá, trochu chybí tempo a vtip. Celkem ze začátku nic zajímavého.

Zaujali mě základní údaje pro návrh UI
<ul>
	<li> věk</li>
	<li> pohlaví</li>
	<li> sociální skupina, zázemí</li>
	<li> vzdělání</li>
	<li> zkušenosti s počítačem</li>
	<li> znalost tématu</li>
	<li> základní potřeby</li>
</ul>
Prý si máme vybudovat vztah se svými personami pro testování. Konstra persony je celkem detailní, obsahuje jméno, věk, pohlaví a specifikace potřeb a jak na něj váš web odpoví. Prostě use case s pěknou fotkou ;-)
Person bychom měli více, mít i negativní pro kterou web neděláme. Musí vycházet z výzkumu a měli by je přijmou všichni kdo se podílí na webu.

Segmentace webu podle cílových skupin nutí uživatele se rozhodnout a pokud se nedokáže zařadit jednoznačně a bez přemýšlení. To není vždy jednoduché.

Kritice podléhá web <a href="http://www.cvut.cz">ČVUT.</a> To je celkem zajímavé, hlavně že to je moje dílko i když poněkud starší. Naštěstí přednášející opravdu nebyl nikdy cílová skupina.

Proč testovat?
<ul>
	<li> Dokonalý design nikdo nenavrhne</li>
	<li> Ověřit funkčnost řešení s uživateli</li>
	<li> Odhady chování uživatelů jsou ze 75% chybné!</li>
</ul>

Já si myslím, že dokonalý design prostě neexistuje. Ale testovat je určitě potřeba.

dále přednáška pokračuje <a href="http://www.test147.com/testovaci">testování</a> wireframů, papírových wireframů.

<em>Kdo to všechno udělá?</em>
Investor, Manažer, Textař (copywriter), Grafik, Kodér, Programátor, Marketér

Doufejte, že nikdo z nich.

U nás často dělá všechno jediný člověk UX Designer.

24.3. <a href="http://www.uxcamp.cz">UXCamp.cz</a>
 
10:40 Po přestávce pokračuje <strong>Daniel Steigerwald - Třídy, dědičnost a OOP v javascriptu</strong>

Daniel má pěkný styl, dobře se poslouchá a nenudí. Navazuje na svůj seriál na zdrojáku. Pěkná přednáška, hlavně doporučení, že dobrá kniha o JS neexistuje. Hlavně ne Croforda.

11:30 David Grudl přišel včas, ale ne dříve a pokračuje s <strong>NETTE, RIA, UX, AJAXE to rýmuje</strong>

David je stálice konferencí a jeho přednáška zatím vypadá podobně jako na WebExpu.

Kdy používat RIA, nejlépe nikdy! To se mi líbí. Použít jen pro to když máte skutečně pádný důvod.
Celá živá ukázka byla o autocomplete v Nette.  

Oběd, no nic moc. Mohli by se pro příště polepšit. Moderování přebral Petr Koubský.

13:30 <strong>Honza Král a NOSQL Databáze</strong>

Naposledy jsem Honzu viděl ve Fractal baru, kde předváděl Redis v akci při implementaci twitteru v Pythonu, článek o NOSQL <a href="http://blog.prskavec.net/2009/11/nosql-databze-v-php/">najdete i tady na blogu</a>.

Pro masivní datové úložiště, které je potřeba škálovat Honza doporučuje <a href="http://cassandra.apache.org/">Cassandru</a>.

Nasazení NOSQL v Čechách? Analýza logů pomocí ukládání přímo do MongoDb.

14:30 <strong><a href="http://www.knesl.com">Jiří Knesl</a> - Základní chyby vývojářů a Agile jako řešení</strong>
<ul> 
   <li>prototypování pomocí Blueprint a HTML, CSS s použitím verzovacího systému (ukázka pomocí Mercurialu)</li>
  <li>testovat (unittesty, testy seleniem) - na radu Honzy Krále zkuste Selenium nahradit pomocí <a href="http://twill.idyll.org/">Twill</a></li>
  <li>řízení- agilní řízení, funguje na bázi procesů a je do jisté míry samořídící
   <ul>
      <li>čas</li>
     <li>priority</li>
     <li>komunikace</li>
     <li>produktivita</li>
     <li>zábava</li>
     <li>peníze</li>
</ul>
</li>
</ul>

Celkově mi přijde, že to není moc přínosné v reálném nasazení pokud je firma řízená procesy jako třeba ta naše. Je to škoda, že ne všechno je testovatelné na úrovni unittestů a naše silně heterogenní systémy PHP, PHP CMS a JAVA v jednom se nedají na úrovni kódu testovat na úrovni, která by stačila. Řešení pomocí <a href="http://www.automatedqa.com/products/testcomplete/">TestComplete</a> (nebo Selenium) není úplně spasitelné.

v 15:50 bude konference zakončena panelovou diskuzí.
