---
date: 2009-12-28T00:00:00Z
published: true
status: publish
tags:
- ide
- netbeans
- php
- phpunit
title: NetBeans 6.8 a PHPUnit
type: post
url: /2009/12/28/netbeans-6-8-a-phpunit/
---

Pokud používáte pro vývoj v PHP nějaké IDE, je to většinou PDT based (Eclipse, Zend Studio) nebo Netbeans. Samozřejmě jsou tu i další a vznikají nové, které stojí za zmínku. Mě oslovilo WebIDE od autorů IDEA firmy JetBrains, kde si myslím roste velká konkurence Zend Studiu.

V práci používám primárně Zend Studio a pro sebe většinou Netbeans. V Netbeans nejvíce oceňují propojení s PHPUnit a pokud rád vyvíjíte metodikou TDD. V verzi 6.7 bylo propojení s PHPUnit již vytvořeno, ale mělo některé chyby, které mi vadili a díky také doufám mému reportování a spolupráci s vývojáři Netbeans odstraněny.

Nejvíce mi vadilo, že se nečetl konfigurační soubor phpunit.xml a nesprávně byla nastavená cesta při spuštění unit testů. Vývojáři to nakonec udělali tak, že cesta se odvozuje právě od konfiguračního souboru, pokud existuje, případně od spouštěných testů. Přibyla také záložka v nastavení projektu, kde se dají detaily nastavit.

<a href="https://blog.prskavec.net/wp-content/uploads/2009/12/Netbeans68-Project-Properties.png"><img src="https://blog.prskavec.net/wp-content/uploads/2009/12/Netbeans68-Project-Properties-300x210.png" alt="" width="300" height="210" class="aligncenter size-medium wp-image-802" /></a>

Poslední detail, který se dostal do Netbeans bylo ještě zobrazování nekompletních a přeskočených testů v GUI, které nefugovalo. GUI vychází z parsování xml výstupu PHPUnit a umí zobrazit víceméně vše co xml soubor poskytne.

<a href="https://blog.prskavec.net/wp-content/uploads/2009/12/NetBeans68-Testresults.png"><img src="https://blog.prskavec.net/wp-content/uploads/2009/12/NetBeans68-Testresults.png" alt="" width="540" height="289" class="aligncenter size-full wp-image-803" /></a>

Myslím, že vývoj Netbeans pro PHP jde správným směrem, v další verzi přibyde jistě podpora pro Zend Framework, který používám já i když to nepovažuji za nijak nutné. Spíše bych ocenil doladědí editoru v detailech. Například pokud máte již funkci končící závorkami a provedete doplnění názvu pomocí autocomplete Netbeans nepozná zda závorky tam jsou či nejsou a nechová se podle toho. Jsou to detaily, ale mnohe tyto detaily v editaci mi pijou krev. Obdobně problémy s automatickým doplňováním závorek, apostrofů, často se chová divně.

Pokud porovnáte WebIDE a Netbeans tak v práci s PHP je to srovnatelné, ale pro to ostatní HTML, editace, JS, XSLT, XML tak WebIDE vede protože vychází z geniálního IDE IDEA, které považuji za nejlepší pro JAVU a nejen ji. WebIDE má také podporu pro Git co ocením.

Myslím, že ve IDE, které jsou zdarma Netbeans začíná podle mne už porážet PDT hlavně rychlejším vývojem a menšími nároky na hardware. Také jsem ocenil nativní podporu pro Mercurial, trochu mi chybí podpora pro Git.
