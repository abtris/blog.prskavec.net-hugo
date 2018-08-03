---
tags:
 - svn
 - scm
comments: true
date: 2011-10-12T00:00:00Z
title: Subversion 1.7
url: /2011/10/12/Subversion-1.7/
---

## Dlouhé čekání na nový Subversion

Myslím, že vetšina lidí si shlédnutí roadmapy Subversion a čekaní 2 roky na změny, které potřebují migrovala na jiný verzovací systém. Ale celosvětově je Subversion stále velice používaný verzovací systém.

## Novinky ve verzi 1.7

Byla opraveny [spousty chyb](https://svn.apache.org/repos/asf/subversion/tags/1.7.0/CHANGES) což jistě všichni uživatelé ocení. A přidáno několik novinek, ty nejvýznamější zmínim dále.

<!--more-->

### WC-NG

Za dlouho očekávanou novou verzí pracovní kopie (WC, working copy) je centralizace adresářů `.svn` do jednoho podobně jako to mají jiné verzovací systémy. Používá se nějaká forma SQLite, ale autoři upozorňují, že není bezpečné přistupovat k souboru s metadaty běžnými nástroji pro SQLite.

### HTTPv2

Nový jednoduší HTTP protokol má samozřejmě hlavně zvýšit výkon SVN, kdy operace se serverem jsou největší zátěží uživatelů. Zatím, ale nemám k dispozici žádné porovnání rychlosti, kde by to bylo reálně vidět.

### svnrdump

Příkaz `svnrdump` umožňuje to samé jako `svnadmin dump` na serveru i vzdáleně. Pokud jste ve staré verzi chtěli zálohovat pomocí svnadmin, museli jste mít přístup přímo k file systému, kde byl svn server. To teď odpadá.

### svn patch

`svn patch` složí jako systémový příkaz patch k aplikací diffů (patchů).

### Distribuce

Postupně se objevují binární balíčky s Subversion 1.7

- Windows: [VisualSVN server](https://www.visualsvn.com/server/download/)

## Závěr

Nic převratného se nestalo, Subversion postupuje dále správným směrem a doufám, že další plánované změny budou probíhat trochu rychleji. Pokud Subversion nepoužíváte a chcete můžete se dozvědět více [na mém školení, nejbližší je 20-21.10.2011](https://www.gopas.cz/Kurzy/Katalog-kurzu/Programovani/Design-architektura-metody-vyvoje/Verzovaci-system-Subversion-GOC1014.aspx).
