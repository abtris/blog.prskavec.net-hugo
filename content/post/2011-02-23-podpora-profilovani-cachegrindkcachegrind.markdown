---
date: 2011-02-23T00:00:00Z
tags:
- php
- xdebug
title: Podpora profilování cachegrind/KCachegrind v Xdebugu
type: post
url: /2011/02/23/podpora-profilovani-cachegrindkcachegrind/
---

Dnes je část profilování v Xdebugu (<a title="Xdebug je open-source nástroj na ladění PHP. " href="https://xdebug.org">https://xdebug.org</a>) ukládána do souborů v KCacheGrind formátu. Tato funkce byla přidána do Xdebugu, ale není dle <a href="https://kcachegrind.sourceforge.net/html/CallgrindFormat.html">specifikace formátu</a>. Byla vytvořena revezním inženýrstvým a tato stávající implementace obsahuje chyby a nepřesnosti.

Od verze 0.6 je KCacheGrind více striktní ohledně interpretace formátu a to způsobuje chyby při jeho používání s výstupy Xdebugu  <a href="https://bugs.kde.org/show_bug.cgi?id=256425">https://bugs.kde.org/show_bug.cgi?id=256425</a>.

<strong>Vybraná částka</strong> bude použita, aby <strong>Derick Rethans</strong> mohl správně vyřešit problém s integrací Xdebugu/KCacheGrindu. Správně vyřešit znamená, že předělá celou část zapisu profilovacích souborů. Rozhodně se tedy nejedná jen o jednoduchou opravu chyby jako je přidání jednoho řádku.

<strong>Dnes 2.3.2011 byla částka úspěšně vybrána, všem co přispěli na dobrou věc děkuji.</strong>
