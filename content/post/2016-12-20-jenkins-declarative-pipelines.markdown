---
tags: 
 - jenkins
comments: true
date: 2016-12-20T00:00:00Z
title: Jenkins Declarative Pipelines
url: /2016/12/20/jenkins-declarative-pipelines/
---

Dnes Jenkins [zveřejnil betu](https://jenkins.io/blog/2016/12/19/declarative-pipeline-beta/) nového formátu pro popis Continues Delivery Pipelines.

Pipeline se serie kroků, které vám dovolí orchestovat práci, kterou potřebujete k buildu, testovaní a nasazení aplikace. Pipelines jsou definovány v souboru `Jenkinsfile` a je uložen v
kořenovém adresáři repozitáře projektu.

<!--more-->

Stávájící formát pipelines je psaný v Groovy DSL a vypadá takto:

```groovy
node {
   stage('Preparation') {
      git 'https://github.com/abtris/bee.git'
      poll: true

   }
   stage('Deps') {
       env.PACKAGE="github.com/abtris/bee"
       env.GOPATH="/Users/abtris/go"
       env.GOROOT="/usr/local/opt/go/libexec"
       sh 'glide --no-color install'
       sh 'mkdir -p release'
   }
   stage('Test') {
       sh 'make xunit'
   }
   stage('Build') {
         parallel (
            linux64: { sh "GOOS=linux GOARCH=amd64 go build -o release/bee-linux-amd64 ${PACKAGE}" },
            linux32: { sh "GOOS=linux GOARCH=386 go build -o release/bee-linux-386 ${PACKAGE}" },
            mac64: { sh "GOOS=darwin GOARCH=amd64 go build -o release/bee-darwin-amd64 ${PACKAGE}" },
            win64: { sh "GOOS=windows GOARCH=amd64 go build -o release/bee-windows-amd64 ${PACKAGE}" }
          )
   }
   stage('Results') {
      archive 'release/*'
      junit 'tests.xml'
   }
}
```

a nový formát zjednodušuje tento zápis i pro ty kteří Groovy nevládnou a je více deklarativní s podporou pro editor, který Jenkins Blue Ocean tým vyvíjí.

Tady je přepsaný příklad ze shora do nového formátu.

```groovy
pipeline {
  agent any
  environment {
    PACKAGE="github.com/abtris/bee"
    GOPATH="/Users/abtris/go"
    GOROOT="/usr/local/opt/go/libexec"
  }
  stages {
     stage('Preparation') {
        steps {
          git 'https://github.com/abtris/bee.git'
        }
     }
     stage('Deps') {
        steps {
           sh 'glide --no-color install'
           sh 'mkdir -p release'
       }
     }
     stage('Test') {
        steps {
         sh 'make xunit'
        }
     }
     stage('Build') {
        steps {
           parallel (
              linux64: { sh "GOOS=linux GOARCH=amd64 go build -o release/bee-linux-amd64 ${PACKAGE}" },
              linux32: { sh "GOOS=linux GOARCH=386 go build -o release/bee-linux-386 ${PACKAGE}" },
              mac64: { sh "GOOS=darwin GOARCH=amd64 go build -o release/bee-darwin-amd64 ${PACKAGE}" },
              win64: { sh "GOOS=windows GOARCH=amd64 go build -o release/bee-windows-amd64 ${PACKAGE}" }
            )
        }
     }
     stage('Results') {
        steps {
          archive 'release/*'
          junit 'tests.xml'
        }
     }
  }
}
```

Celou syntaxi si můžete prohlédnout [v dokumentaci](https://github.com/jenkinsci/pipeline-model-definition-plugin/blob/master/SYNTAX.md).

Hlavní rozdíl je v tom, že přidali další bloky jako je hlavní `pipelines` a je potřeba deklarovat vždy agenta, který může být v různém formátu. Vyměnilo nazvosloví a z `node` je `agent`.

Build můžete pustit jednoduše například v Dockeru pomocí: `agent docker:'node:6.3'` nebo pokud nechcete to řešit tak můžete dát `agent any` jako v příkladu, to se hodí například pro lokální testování a není to rozhodně vhodné pro nějaké větší nasazení, kdy potřebujete orchestrovat jednotlivé agenty.

Deklarace `environment` na začátku zpřehledňuje celý zápis a každá `stage` má teď `steps`, které jsou nové. Přidali sekci `post`, která má kroky `always`, `success` a `failure` pro ošetření konce buildu a poslání notifikací. Nechybí ani `options`, `parameters` a `triggers`, kde nastavíte co potřebujete.

I když celý tento projekt v beta fázi, přijde mi to jako krok správným směrem a spolu s čím dál lepším [Blue Ocean](https://jenkins.io/blog/2016/05/26/introducing-blue-ocean/) to bude příští rok hlavní novinka v Jenkinsu.
