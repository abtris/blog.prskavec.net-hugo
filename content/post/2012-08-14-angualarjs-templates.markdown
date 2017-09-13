---
tags: 
 - javascript
 - angularjs
comments: true
date: 2012-08-14T00:00:00Z
title: Šablony v AngularJS
url: /2012/08/14/angualarjs-templates/
---

Pokud začínáte s [AngularJS](http://www.angularjs.org) je dobré pro aplikace použít [angular-seed](https://github.com/angular/angular-seed).

<!--more-->

## Jednotlivé šablony v samostatných souborech

Šablony v angular-seed jsou rozděleny do samostatných souborů.

    app/
        js/
            app.js
        partials/
            partial1.html
            partial2.html

v app.js se potom načítají samostatně

    $routeProvider.when('/view1', {templateUrl: 'partials/partial1.html', controller: MyCtrl1});
    $routeProvider.when('/view2', {templateUrl: 'partials/partial2.html', controller: MyCtrl2});

Důležité je AngularJS sice používa template cache, ale pro každý soubor si sáhne samostatně a udělá 2 requesty, což nemusí být optimální obzvláště pokud byste měli 20 šablon.

Tento způsob se hodí při vývoji, abyste měli šablony samostatně pro přehlednost.

## Inline šablony

V manuálu najdete jak vkládat [šablony](http://docs.angularjs.org/api/ng.directive:script), přímo do stránek pomocí script tagu.

    <script type="text/ng-template" id="partial1.html">
    <p>This is the partial for view 1.</p>
    </script>

Ty se dají použít velmi dobře. Pokud je to menší kód, ale jinak je lepší mít v samostatných souborech. V kódu se na ně odkážete pomocí jména v id parametru.

    $routeProvider.when('/view1', {templateUrl: 'partial1.html', controller: MyCtrl1});


## Kombinace obou způsobů

Jak jsem to konzultoval s Vojtou Jínou z AngularJS teamu. Pro development je dobré použít jednotlivé šablony samostatně, ale pro nasazení je dobré spojit šablony do jednoho souboru, abyste ušetřili requesty.

Dá se použít například [GruntJS](http://gruntjs.com/) script pro vložení samostaných šablon pro vývoj do inline šablon. Ukázkový script udělal [Vojta Jína](https://github.com/vojtajina).

{% gist 3347478 %}

## Další možnosti

K tomu článku mě přivedl tento tweet.

{% blockquote @PatrikVotocek https://twitter.com/PatrikVotocek/statuses/235100756905189376 %}
Nevíte někdo jak v #angularjs docílit toho abych měl jeden (externí) soubor se všema šablonama?
{% endblockquote %}

To by předpokládalo řešení, že budeme mít soubor s šablonami a zkusíme ho načíst a zpracovat. Musíte vytvořit falešnou template cache a tu použít, díky Vojtovy za implementaci.

{% gist 3354046 %}

Rozhodně to, ale není něco co byste měli používat i když to funguje.

# Závěr

Držte se toho jak je to v AngularJS vymyšlené, zbastlit lze všechno, ale není to ideální a proto používejte samostatné šablony na vývoj a při deploymentu to dejte do stránky to je nejlepší způsob.
