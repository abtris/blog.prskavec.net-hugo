---
categories: [javascript]
comments: true
date: 2013-06-13T00:00:00Z
title: Jaký bude AngularJS 1.2?
url: /2013/06/13/angularjs-1-dot-2/
---

Pokud sledujete dění kolem frameworku [AngularJS](http://angularjs.org) tak jste jistě zaznamenali, že se pracuje na nové verzi 1.2, která je teď blízko k dokončení. V masteru mají dnes verzi pojmenovanou jako verzi 1.1.8 a brzy se snad dočkáme finální verze. Zkusím zde popsat nejdůležitější věci z [prezentace](https://docs.google.com/presentation/d/1WHCcp3G3HxoE7b_ut_ERKJF4zQK_P4qFlESjE2E9AUQ/preview?sle=true#slide=id.geaf70e8e_16) na meetupu 11.6. co prezentovali Igor Minár a Brad Green.

<!--more-->

## ng-animate 
[Deklarativní animace](http://yearofmoo-articles.github.io/angularjs-2nd-animation-article/app/#/) pro aplikace v Angularu. Podpora pro CSS animace a přechody s možností fallbacku pomocí JS. Pro vlastní animace je dobré použít některou s knihoven pro CSS animace (Greensock.js, [Animate.css](http://daneden.me/animate/), Custom CSS3 Keyframes).
Direktiva se stará o práci s DOMem, nastavuje potřebné třídy a timing, také zabraňuje vnořeným animacím.

## $http
Měla by být přidána podpora pro blob a authentication (metoda withCredentials). Budete si moci více nastavit XSRF header a názvy cookie. Přidá se podpora pro zrušení requestů a podpora pro around interceptors.

Around interceptors se dají dobře využít například při authentication, asynchronní zpracování request/response a práci s chybami. Například chcete provést request a server vrátí, že vypršela session a pomocí interceptoru vyvoláte přihlášení a potom pokračuje původní request.

## $resource
Více konfiguračních nastavení (hlavičky, url), api bude vylepšené o podporu promise a měli by být k dispozici i interceptory.

## $route a ngView
Separátní moduly, rozšíření o možnost zachycení všech parametrů a přidána podpora pro animace.

		when('/users/:userId/params/*params/'

## ngRepeat
V této snad nejpotřebnější direktivě je přidána podpora pro animace, potom je možnost opakovat sadu elementů (multi-element repeater) pomocí ng-repeat-start a ng-repeat-end a v neposlední řadě podpora track by paramteru, kdy se dá kontrolovat asociace mezi modelem a DOMem.

## Controller as
Tato konstrukce nám umožní jednoduší zápis a není potřeba používat v controlleru $scope (samozřejmě pokud ho nepotřebujete např. při $watch()).

		<body ng-controller="DemoController as demo">
		<tr ng-repeat="student in demo.students">
			<td>{{student.name}}</td>
		</tr>
		</body>
		
		function DemoController() {
		 	this.students = [ ... ]
		}
 
## ng-if
Tato direktiva přejatá z projektu AngularUI umožňuje udělat podmínku na kompilaci části šablony a reší některé problémy, které se nadali zvládnout pomocí ngShow a ngHide.

## Expressions
Podpora pro nové operátory: ===, !==, ?
Konečně lze napsat:

		ng-class="loggedIn ? 'user': 'anonym'"

## ngTouch
Podpora pro touch eventy v ngClick a ngSwipe.

## Shrnutí 
Přibude jestě několik vylepšení pro bezpečnost (CSP) a vylepší error hlášky, také by se měla zlepšit dokumentace a bude interaktivní tutorial. Dema a detaily najdete také ve videu na [youtube](https://www.youtube.com/watch?v=W13qDdJDHp8&feature=c4-overview).