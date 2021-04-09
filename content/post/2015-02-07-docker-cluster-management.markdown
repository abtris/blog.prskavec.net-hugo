---
tags:
 - docker
 - coreos
 - messos
 - k8s
date: 2015-02-07T00:00:00Z
title: Docker cluster management
url: /2015/02/07/docker-cluster-management/
---

- Update: přidal jsem do článku další věci zmíněné v komentářích, všem děkuji za příspěvky.
- Update 23.2.2015: Přidán odkaz na Centurion od New Relic

V poslední době se zabývám technologiemi pro řízení clusterů s docker konteinery.

Pokud by to někoho zajímalo, zkusím jsem shrnout s čím jsem se potkal a kde vidím možné využití.

Nástroje, které můžete použít cluster management:

- [Apache Mesos](https://mesos.apache.org/) (cpp, api for java, python, c++)
- [Docker Swarm](https://github.com/docker/swarm/) (golang)
- [CoreOS Fleet](https://coreos.com/using-coreos/clustering/) (golang)
- [Google Kubernetes](https://kubernetes.io/) (golang)
- [Spotify Helios](https://github.com/spotify/helios) (java)
- [New Relic Centurion](https://github.com/newrelic/centurion) (ruby)

potom k tomu ješte patří některé frameworky pro Mesos a to [Marathon](https://mesosphere.github.io/marathon/) a [Chronos](https://airbnb.github.io/chronos/). [A Kubernetes Framework for Apache Mesos](https://github.com/mesosphere/kubernetes-mesos).

<!--more-->

Ještě potřebujete nástroj pro service discovery:

- [CoreOS Etcd](https://coreos.com/using-coreos/etcd/) (golang)
- [Hashicorp Consul](https://consul.io/) (golang)
- [Apache Zookeeper](https://zookeeper.apache.org/) (java)

Etcd používá Kubernetes a další projekty. Service discovery je potřeba pro management clusteru. Můžete použít stejně jednoduše i ostatní projekty.

Apache Mesos umí pracovat s různorodým prostředím včetně Amazon ECS service. Framework Marathon vám potom slouží k pouštění dlouho běžících konteinerů pro aplikace a Chronos typicky pro batch processing například pro zpracování velkých dat. S Mesosem musíte provozovat ZooKeeeper pro discovery service.

Protože se snažím Javě vyhnout a raději volím nástroje v jiných jazycích, kde mám větší znalosti. Pro discovery service clusteru bych raději použil Consul nebo Etcd.

Google Kubernetes je poměrně vyspělý nástroj používaný pro Google Cloud a adaptovaný například RedHatem pro OpenShift V3. Tam mám trochu výhradu, že to nemá podporu pro více uživatelů a neumí pracovat s externím filesystémy co vím. Ale dá se používat na tvorbu dokonce hybridních clusterů mezi více poskytovateli (AWS, Google Cloud, OpenShift, fyzické stroje).

Docker Swarm je nástroj pro řízení clusterů konteinerů přímo od Dockeru, ale kromě základních příkladů není k dispozici nic většího, poporuje několik discovery service (etcs, consul, zookeeper). Nevím o nikom, kdo by to používal ve větším měřítku.

CoreOS Fleet je [systemd](https://coreos.com/using-coreos/systemd/) a [etcd](https://coreos.com/using-coreos/etcd/) a nemám s ním zkušenosti vůbec žádné. Projekty kolem [CoreOS](https://coreos.com/) jsou zajímavé. Mají vlastní technologii konteinerů [Rocket](https://coreos.com/blog/rocket/).

Stejně jako CoreOS snaží se o podobnou věc i [Project Atomic](https://www.projectatomic.io/) od RedHatu. Vytvořit základní systém pro práci s konteinery.

Závěr je asi takový, že pokud budete chtít řídít vlastní cluster asi zvolíte buď Mesos nebo Kubernetes. Osobně asi budu volit Kubernetes, co vy?
