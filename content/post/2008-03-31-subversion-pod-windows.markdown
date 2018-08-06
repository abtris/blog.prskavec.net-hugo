---
date: 2008-03-31T00:00:00Z
published: true
status: publish
tags:
- microsoft
- scm
- svn
- windows
title: Subversion pod Windows
type: post
url: /2008/03/31/subversion-pod-windows/
---

Pokud jste vývojáři a používáte Subversion (SVN) pod Windows máte několik možností jak to dělat. Donedávna jsem používal jen klienta buď Subclipse nebo <a href="https://tortoisesvn.tigris.org/">Tortoisesvn</a> (TSVN) a tím jsem to řešil. Buď jsem se vzdáleně připojil na SVN server nebo jsem používal lokální repozitory, které umí TSVN vytvořit a zpracovat. Pro vývoj je vcelku jedno které řešení používáte, pokud máte stálé připojení k internetu, musíte používat stejně centrální repozitory. Pro některé moje projekty, ale vlastní repozitory server nemám a hostuji to jen lokálně a celé repozitory jen zálohuji jako soubory.

Pár let co používám SVN to bylo v pořádku, pro vývoj to stačí. Pokud jsem používal Linux, tak to bylo o to jednoduší, měl jsem tak plný SVN server (1.4.6 aktuálně) a lokální nebo vzdálená administrace je totéž. Podobně se můžete zachovat i na Windows pokud k tomu máte motivaci, kterou jsem donedávna neměl. K instalaci na windows dopoučiji balíček <a href="https://tortoisesvn.net/downloads">TortoiseSVN-1.4.8.12137-win32-svn-1.4.6.msi</a> a aby vám fungoval server jako service je dobré to doplňit o <a href="https://svnservice.tigris.org">svnservice-1.0.0.msi</a>. Po jednoduché instalaci těchto balíčků a nastavení SVNROOT můžete ve svém klientovi přistupovat přes svn protokol a používat všechny běžné příkazy.

Já jsem měl v motivaci ve vytváření changelogu pro deploy pomoci <em>svn.exe log --xml svn://localhost/rep_name</em> což přes klienta TSVN nějak rozumně nešlo a při exportu přes schránku jsem měl problém s exportem češtiny ve výsledném dokumentu. Takto to jednoduše obejdu a můj jednoduchý skript udělá pro každý projekt aktuální changelog.
