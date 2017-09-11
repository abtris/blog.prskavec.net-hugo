---
date: 2009-04-29T00:00:00Z
meta:
  _edit_last: "1"
  _encloseme: "1"
  _pingme: "1"
published: true
status: publish
tags:
- continuous integration
- hudson
- php
title: PhpHudson
type: post
url: /2009/04/29/phphudson/
---

Vyvořil jsem pro mě <a href="http://code.google.com/p/php4hudson/">užitečnou třídu v php</a> pro práci s <a href="http://hudson.dev.java.net">Hudsonem</a>, která má zatím implementovány základní věci z remote api, kterým Hudson disponuje. Knihovna používá Curl a pracuje s Hudsonem přes REST.

Používám tuto knihovnu např. pro migraci všech jobů z jednoho hudsona na druhý.

Lehce můžeme totiž stáhnout všechny konfigurační soubory do jednoho adresáře.
<pre>
getAllConfigs("/tmp/hudson/");
</pre>
Potom můžeme projít adresář a založit jednotlivé joby.
<pre>
createJob(basename(str_replace("-config.xml","",$file)), file_get_contents($dir.$file));
    }
    closedir($handle);
}
</pre>

Pokud by chtěl někdo třídu používat, pracujte prosím se zdrojovým kódem:  
<pre>svn checkout http://php4hudson.googlecode.com/svn/trunk/ php4hudson-read-only</pre>
