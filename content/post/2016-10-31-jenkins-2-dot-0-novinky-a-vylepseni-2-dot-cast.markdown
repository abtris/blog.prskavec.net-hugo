---
categories: 
 - jenkins
comments: true
date: 2016-10-31T00:00:00Z
title: Jenkins 2.0 - novinky a vylepšení - 2.část
url: /2016/10/31/jenkins-2-dot-0-novinky-a-vylepseni-2-dot-cast/
---

V minulé části jsem probíral proč je důležité mít definice v souboru a proč potřebujeme Continues Delivery Pipelines.

V tomto příspěvku se budu věnovat dalším bodům:

- distributed job across multiple nodes
- autoscaling on traffic with lowest possible price
- solution for caching for installations
- docker support
- matrix builds

V Jenkinsu je podpora pro distribuované agenty, dnes můžete mít jednotlivé stroje v AWS (pomocí ec2, ec2-fleet pluginů), OpenStack, Docker, Kubernetes apod.

Aby jste byli schopni dosáhnou kvalitního autoscalingu za velmi dobrou cenu dají se velmi dobře využít [spot instance](https://aws.amazon.com/ec2/spot/) od AWS. Můžete ušetřit až 90% nákladů oproti normálním instancím.

<!--more-->

Pokud provozujete vlastní Jenkins je potřeba vyřešit cache, ideální řešení je [JFrog Artifactory](https://www.jfrog.com/artifactory/), které podporuje caching pro velké množství vývojových nástrojů. Bohužel toto řešení poměrně drahé. Ale existuje [Nexus repository](http://www.sonatype.org/nexus/), které má komunitní verzi. Ale bohužel v Nexus OSS chybí například podpora pro PHP Composer.

Matrix buildy jsou potřeba v Jenkinsu se jim říká Multi-configuration project. Můžete tu vytvořit vlatní matice podle čeho chcete a Jenkins vygeneruje potřebné projekty s parametry které potřebujete, podobně jako když by jste použili [Job DSL plugin](https://wiki.jenkins-ci.org/display/JENKINS/Job+DSL+Plugin). To se hodí od testovaní různých verzí operačního systému, verzí programovacího jazyka apod.

Poslední věc je podpora Dockeru. V Jenkinsu je několik pluginů pro Docker. Využití může být také různé. Buď to použijete na vytvoření agentů nebo přímo můžete pouštět projekt v docker containeru. Bohužel toto funguje spolehlivě jen na linuxu.

Takto se docker popíše v Jenkins pipelines.

```groovy
node('docker') {
 // My project sources include both build.xml and a Dockerfile to run it in.
 git 'https://git.mycorp.com/myproject.git'
 // Ready?
 docker.build('mycorp/ant-qwerty:latest').inside {
   sh 'ant dist-package'
 }
 archive 'app.zip'
}
```

Hostované řešení jako je TravisCI nebo CircleCI mají výhodu pokud potřebujete začít. Ale postupem, když máte více lidí nebo potřebujete větší flexibilitu tak se můžete dostat do potíží. Jenkins je náročnější na údržbu, ale umožňuje velkou flexibilitu.

## Shrnutí

Jenkins 2 přinesl deklarativní popis, lepší bezpečnost a stále zachoval zpětnou kompatibilitu. Vylepšené UI a tady se stále pracuje na dalších vylepšeních (Blue Ocean). S Blue Ocean potom přijde deklarativní popis continues delivery pipelines a také by měl tam být online visual editor pro pipelines. Na další rok mají plány také na přidání Configuration API a myslím, že by mohl Blue Ocean v budoucnu uplně nahradit dnešní UI a zároveň otevřít cestu pro to například kompletně používat Jenkins bez UI. Uvidíme jak to bude dlouho trvat, každopádně se už teď těším na další novinky.
