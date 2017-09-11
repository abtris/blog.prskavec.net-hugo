---
date: 2009-02-03T00:00:00Z
meta:
  _edit_last: "1"
  _encloseme: "1"
published: true
status: publish
tags:
- wordpress
title: Úplná lokalizace WP Theme iNove 1.2.3
type: post
url: /2009/02/03/uplna-lokalizace-wp-theme-inove-123/
---

<p>Pokud se někomu líbí vzhled, který používám a stáhl jsi ho od autora, zjistil, že není lokalizováno do češtiny všechno co vidí. Protože jsem autor lokalizace tak to musím uvést na pravou míru. Lokalizoval jsem co je v překladovém souboru, ale autor nehodlá jak mi sdělil některé věci lokalizovat, nepřijde mu to důležité. Opravdu nechápu proč, ale je to jeho právo, týká se to hlavně textů v postraní lište.</p>  <p>Pokud to chcete upravit otevřete si soubor sidebar.php a najděte tyto řádky a udělejte tyto změny (originál zaměňte za počeštěnou verzi):</p>  <pre class="php">65: $posts_widget_title = 'Recent Posts';
65: $posts_widget_title = 'Poslední příspěvky';

67: $posts_widget_title = 'Random Posts';
67: $posts_widget_title = 'Náhodné příspěvky';

92:&lt;h3&gt;Recent Comments&lt;/h3&gt;
92:&lt;h3&gt;Poslední komentáře&lt;/h3&gt;

102:&lt;h3&gt;Tag Cloud&lt;/h3&gt;
102:&lt;h3&gt;Oblak štítků&lt;/h3&gt;

119:&lt;h3&gt;Categories&lt;/h3&gt;
119:&lt;h3&gt;Kategorie&lt;/h3&gt;

135:&lt;h3&gt;Archives&lt;/h3&gt;
135:&lt;h3&gt;Archív&lt;/h3&gt;

153:&lt;h3&gt;Meta&lt;/h3&gt;
153:&lt;h3&gt;Meta&lt;/h3&gt;</pre>

<p>Pro ty co umějí aplikovat patch je tu diff ze SVN: <a href="http://blog.prskavec.net/wp-content/uploads/2009/02/sidebar.diff">sidebar.diff</a></p>

<p>Ve verzi iNove 1.2.3. je drobný bug (comments.php), díky kterému se nepřeloží některé věci pokud jste přihlášení, opraveno to bude v další verzi.</p>

<p>Nenechte se zmást, že moje verze nevypadá jako vaše po instalaci, udělal jsem další drobné úpravy velikosti písma, změny barvy nadpisů, přidal jsem další položky do sidebar.php.</p>
