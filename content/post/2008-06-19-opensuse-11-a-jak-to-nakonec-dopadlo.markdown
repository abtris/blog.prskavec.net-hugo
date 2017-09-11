---
date: 2008-06-19T00:00:00Z
meta:
  _edit_last: "1"
published: true
status: publish
tags:
- ostatnÃ­
title: OpenSuSE 11 a jak to nakonec dopadlo
type: post
url: /2008/06/19/opensuse-11-a-jak-to-nakonec-dopadlo/
---

Dnes vyšlo čerstvé <a href="http://software.opensuse.org/">OpenSUSE 11</a> a já jsem myslel, že by to mohlo být to pravé pro můj Sony VIAO VGN-TZ31XN. Ale bohužel jsem opět zkrachoval na tom, že moje rozlišení 1366x768 nějak nebrali v úvahu. Ať jsem to nastavil nebo ne. Detekce to nepozná a ruční nastavení nefungovalo. Nemyslím, že pokud nefunguje taková triviální věc jako nastavení rozlišení tak by to obyčejný člověk používal.  Tak jsem se vrátil k <a href="http://www.ubuntu.cz">Ubuntu 8.04</a>, kde není sebemenší problém a všechno funguje jak má.
<h3>Problémy s Lighting 0.8 na Ubuntu</h3>
Jen jsem narazil vždy na problém s instalací Lightingu 0.8 do Thunderbirda. Úplně mi to rozhodilo Thunderbird a nezobrazoval se kalendář kde měl. Tak mě napadlo si pořádně přečíst release notes a co se dočtu:
<p><em>Many modern Linux distributions only package libstdc++6, which is incompatible with Lightning. Therefore please install the package "libstdc++5" or "compat-libstdc++" on your system before installing Lightning) </em></p>

 Ano, Ubuntu je také taková moderní distribuce. Tak jsem vypnul dolněk a potom jsem příslušnou knihovnu nainstaloval.

<pre class="brush: plain">sudo apt-get install libstdc++5</pre>
Když jsem doplněk pustil, je všechno ok. Třeba to někomu pomůže, já jsem na to přišel čistě náhodou.  Kdyby někdo věděl jak na Sony VAIO rozchodit to OpenSUSE budu rád za radu.
