---
date: 2008-07-01T00:00:00Z
meta: {}
published: true
status: publish
tags:
- ide
- php
- svn
title: NetBeans, PDT a Subversion 1.5
type: post
url: /2008/07/01/netbeans-pdt-a-subversion-15/
---

<p>Před časem, jsem přešel z <a href="https://www.eclipse.org/pdt/">Eclipse PDT</a> na <a href="https://php.netbeans.org/">NetBeans IDE Early Access for PHP</a> a to hlavně z důvodu, že projektový adresář je umístněný libovolně mimo zdrojové kódy a také pro lepší práci se subversion než mi poskytoval <a href="https://subclipse.tigris.org/">Subclipse</a>. Když vyšel nový Subversion 1.5 měl jsem obavy, že budu muset čekat na novou verzi pluginu pro Subversion jako je to u PDT. </p>  <p>Subclipse 1.4.x pro Eclipse je už venku, tak to naštěstí u PDT není problém. Ale jak to je u NetBeans, čekám na říjen kdy má být verze 6.5 s podporou PHP, ale dosavadní funkčnost mi celkem vyhovuje, asi je to tím, že stejně edituji nejvíce v <a href="https://www.pspad.com/cz/">PsPadu</a> a <a href="https://php.vrana.cz/editor-scite.php">SciTE</a>.</p>  <p>NetBeans, ale s Subversion 1.5 fungují bez problémů a to proto, že potřebují ke svému běhu jen binární soubory Subversion,&#160; používají totiž běžného řádkového klienta. Tak pokud odinstalujete starou verzi Subversion a nainstalujete novou, stačí potom nastavit cestu do $PATH, případně se vás NetBeans zeptají kde Subversion najdou. Jednoduché a geniální. </p>  <p>Samozřejmě to má i svoje nevýhody, podporují se základní funkčnosti a až v dalších verzích budou, doufám, k dispozici funkce, které jsou v klientu nové (changelisty, merge tracking atd.). Klient v NetBeansech nemá implementovány i další vlastnosti jako nemožnost propojení s Issue trackerem jak je možné v Subclipsu nebo TSVN. Já primárně používám TSVN a proto mi tyto necnosti příliš nevadí s spíše se mi líbí diff přímo při editaci, ukázání anotací apod. Náhled na používání Subversion v NetBeans je na obrázku. </p>  <p><a href="https://blog.prskavec.net/wp-content/uploads/2008/07/image.png"></a> </p>  <p>Jak jsou na tom ostatní prostředí Komodo IDE, Zend? Těším se na komentáře.</p>
