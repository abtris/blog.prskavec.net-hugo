---
categories: [javascript,angularjs]
comments: true
date: 2013-08-29T00:00:00Z
title: Firebase a AngularJS
url: /2013/08/29/firebase-a-angularjs/
---

Dnešní většina aplikací v javascriptu má architekturu klient server. Pokud nechcete psát nějaký backend pro vaši aplikaci, můžete se tomu vyhnout pokud použijete nějaký druh úložiště (databáze), která vám k tomu přidá i funkce, které má nějaký backend napsaný např. v nodejs nebo php.

<!--more-->

Těmto řešením se věnuje web [nobackend.org](http://nobackend.org/solutions.html), kde se dají najít tyto řešení:

   * [Backendless](https://backendless.com/)
   * [deployd](http://deployd.com/) -  [example in angularjs](http://docs.deployd.com/docs/collections/examples/a-simple-todo-app-with-angular.md) 
   * [Firebase](http://www.firebase.com)
   * [Hoodie](http://hood.ie/)
   * [Kinvey](http://www.kinvey.com/)
   * [Parse](https://parse.com/)
   * [remoteStorage](http://remotestorage.io/)
   * [Sockethub](http://sockethub.org/)

Bohužel jsem neměl tolik času, abych podrobně prozkoumal všechny řešení. O Firebase jsem se dozvěděl z přednášky na meetupu:

<iframe width="640" height="360" src="//www.youtube.com/embed/C7ZI7z7qnHU?rel=0" frameborder="0" allowfullscreen></iframe>

Z této přednášky jsem vycházel pro svoji přednášku na [PragueJS](http://www.praguejs.cz).

## Firebase

Zajímavé na tomto projektu je, že je to poměrně mladý projekt, ale který vznikl z projektu Envolve (2009), což je skupinový chat pro tisíce websites. Zjistili, že vyvíjený backend je užitečný pro více druhů aplikací než jenom chat a nabídli ho jako produkt Firebase. 

Hlavní věci, které dělají Firebase zajímavou jsou:
- JSON formát pro data
- je to dokumentová databáze v mnoha směrech mi připomíná práci např. s CouchDb
- REST každý dokument má HTTP endpoint s kterým se dá pracovat, můžete používát API v přes HTTP, nativní knihovny pro Android (Java) a iOS (Objective-C) a javascript.
- přímo od autorů je knihovna pro práci s AngularJS - [AngularFire](http://angularfire.com/) a pro práci s Backbone - [BackFire](https://github.com/firebase/backfire).
- real-time synchronizace dat, pokud přistupujete z více klientů tak se změny projeví v milisekundách
- automatic scaling je pěkná věc pokud potřebujete opravdu řešit hodně klientů, autoři slibují, že je jedno pokud máte 1 klienta nebo 1 milion a že nebudete muset nic na aplikaci měnit, což je dost ambiciózní, ale vzhledem ke klientům jako je Atlassian, CodeAcademy, CBS a další řekl bych že už si to více než ověřili
- security - bezpečnost je řešena celkem jednoduše a přitom celkem se dá dobře nastavit přes [security rules](https://www.firebase.com/docs/security/security-rules.html). Vlastní přihlašovnání je podporováno přes jejich [Firebase Simple Login](https://www.firebase.com/docs/security/authentication.html) nebo můžete použít nějakou vlastní implementaci.
- servery nepotřebujete. Firebase je schopná nahradit aplikaci psanou na serveru, jde jen o to zvážit kdy to ještě stačí a kdy už potřebujete nějaké další části infrastruktury navíc.

## AngularFire

AngularFire je modul, který řeší vlastní authentikaci pomocí angularFireAuth.

Stačí mít includnuté tyto soubory pro práci s firebase.

    <script src="https://cdn.firebase.com/v0/firebase.js"/>
    <script src="https://cdn.firebase.com/v0/firebase-simple-login.js"/>
    <script src="angularFire.js"/>


potom v controlleru se předá angularFireAuth přes dependency injection

    function MyController($scope, angularFireAuth) {
        var url = "https://<my-firebase>.firebaseio.com/";
        angularFireAuth.initialize(url, {scope: $scope, name: "user"});
    }

v šabloně potom máte vlastní přihlašování

    <span ng-show="user">
    {{user.name}} | <a ng-click="logout()">Logout</a>
    </span>
    <span ng-hide="user"><a ng-click="login()">Login</a></span>


metody pro login přes Firebase simple login, které použijí facebook takto jednoduše deklarujete

    $scope.login = function() {
        angularFireAuth.login("facebook");
    };
    $scope.logout = function() {
        angularFireAuth.logout();
    };

AngularFire podporuje [implicitní](http://angularfire.com/documentation.html#implicit) a [explicitní data binding](http://angularfire.com/documentation.html#explicit). Příklady najdete v dokumentaci.

## Firebase Open Source

Na [githubu](http://firebase.github.io/) najdete všechny příklady a zdrojové kódy, ke všemu co se vyvíjí kolem Firebase. Postupně to přibývá a většina demo příkladů jsou reálně použitelné za ty nejzajímavější bych zmínil:

- [Firepad](http://www.firepad.io/)
- [Firechat](http://firebase.github.io/firechat/)
- [Firefeed](http://firefeed.io/)

## Závěr 

Tyto nové databáze jsou velmi zajímavé, ale těžko se asi porovná to nejzajímavější a to jak se vypořádají s konflikty při synchronizaci. Pokud používáte nějako jako je Evernote nebo nějaký todo list a máte ho na počítači, mobilu a tabletu. Sami víte jak je obtížné pokud nejsou všechny zařízení stále online občas udržet konzistenci. Ve Firebase se to dá částečně řešit pomocí security rules, ale stejně si občas myslím, že může být problém.

