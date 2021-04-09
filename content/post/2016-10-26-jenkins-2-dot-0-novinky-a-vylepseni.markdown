---
tags:
 - jenkins
date: 2016-10-26T00:00:00Z
title: Jenkins 2.0 - novinky a vylepšení
url: /2016/10/26/jenkins-2-dot-0-novinky-a-vylepseni/
---

Jenkins je nejznámější řešení na Continues Integration, který existuje už řadu let. Od září je venku konečně verze 2.x (aktuálně 2.19.1 LTS), která obsahuje několik zásadních novinek.

Jenkins používám řadu let a také ho [školím ve firmách](/skoleni-a-kurzy/) co chtějí toto řešení nasadit. Před 2 lety jsem si řekl, že není Jenkins moc dobrá cesta. Žádné použitelné novinky se dlouho neobjevovali a vůbec se nezlepšovalo použití pro větší nasazení Jenkinusů ve firmách.

<!--more-->

Já mám na každé CI tento seznam požadavků:

- job definitions in repository
- autoscaling on traffic with lowest possible price
- pipelines / workflow
- solution for caching for installations
- docker support
- matrix builds
- distributed job across multiple nodes

a hned ten nejzásadnější právě dlouho Jenkins nesplňoval. Od verze 2.0 je mezi základními rozšířeními podpora pro Continues Delivery Pipelines (dále jen pipelines), které jdou psát do souboru jako `Jenkinsfile`, který můžete mít s projektem v repozitáři a Jenkins umí spolupracovat s SCM například s Github, kde mu stačí dát jméno organizace nebo uživatele a on proskenuje vaše repositáře a tam kde najde `Jenkinsfile` pro ty repositáře vytvoří úlohy ke zpracování.

Ukázka jak takový soubor s pipelines vypadá pro malý projekt.

```groovy
node {
   stage('Preparation') {
      git 'https://github.com/abtris/bee.git'
   }
   stage('Deps') {
       env.PACKAGE="github.com/abtris/bee"
       env.GOPATH="~/go"
       env.GOROOT="/usr/local/opt/go/libexec"
       sh 'glide --no-color install'
       sh 'mkdir -p release'
   }
   stage('Test') {
       sh 'make xunit'
   }
   stage('Build') {
         parallel (
            linux64: { sh "goos=linux goarch=amd64 make build" },
            linux32: { sh "goos=linux goarch=386 make build" },
            mac64: { sh "goos=darwin goarch=amd64 make build" },
            win64: { sh "goos=windows goarch=amd64 make build" }
          )
   }
   stage('Results') {
      archive 'release/*'
      junit 'tests.xml'
   }
}
```
Jenkinsfile je [DSL](https://en.wikipedia.org/wiki/Domain-specific_language) v programovacím jazyku [Groovy](https://www.groovy-lang.org) obsahuje blok s názvem `node` kde si definujete, kde a co se má co spouštět. V ukázce tam není žádná podmínka a tak Jenkins rozhodně sám kde to spustí. Pokud by jste to chtěli upřesnit tak se dá se specifikovat zda to má být master nebo naopak, že to nemá být master (doporučeno) a také jde přímo označit agenta (slave), kterého máte připojeného k Jenkins masteru. Může to být instance na Amazon Web Services (AWS) nebo v jiném cloudu, tak jakýkoliv jiný počítač, který si k tomu určíte a pustíte na něm potřebného klienta.

Další klíčovým slovem je `stage`, kde si názvem rozdělíme pipeline do nějakých logických celků. Tyto části, pokud to má smysl, můžeme zpracovávat paralelně jako je to v ukázce v části `Build`. Využití paralelního zpracování je tam, kde chcete zkrátit čas celého build a pokud na to máte volné prostředky (agenty).

Vše spouštím pomocí linuxového shellu pomocí klíčového slova `sh`. Mohl bych testovat pomocí `isUnix()` zda jsme na stroji, který toho je vůbec schopen, to vám pomůže pokud používáte různé stroje, některé s Windows a jiné s Linuxem.

V poslední části `Results` archivujeme artefakty (tady binárné soubory) vytvořené při buildu a výsledky testů z kroku `Test`.

To je vše a v Jenkinsu to potom vypadá takto:

![](/images/jenkins/pipelines-parallel.png)

Takto se pipelines zobrazují v novém UI BlueOcean.

Pipelines mají před sebou velkou budoucnost, připravují se vylepšení v podobě online editoru a více deklarativního zápisu než dnes.

Dalším požadavkům, které mám na CIE se budu věnovat zase v dalším příspěvku.
