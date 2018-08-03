---
tags:
 - docker
comments: true
date: 2016-02-05T00:00:00Z
title: Suchý docker
url: /2016/02/05/suchy-docker/
---

[Suchý únor](https://suchejunor.cz/) je skvělá akce a tak jsem říkal, zda s Dockerem nebudeme také na suchu. Našťěstí včera se situace změnila a vyšli nové verze Docker Engine, Docker Swarm and Docker Compose.


{% blockquote Docker docs https://docs.docker.com/engine/breaking_changes/ %}
Pokud budete upgradovat, buďte opatrní, nový formát image není zpětně kompatibilní.
{% endblockquote %}

V originálu si novinky můžete prostudovat na blogu Dockeru:

- [Docker 1.10: New Compose file, improved security, networking and much more!](https://blog.docker.com/2016/02/docker-1-10/)
- [Docker Engine 1.10 Security Improvements](https://blog.docker.com/2016/02/docker-engine-1-10-security/)
- [Compose 1.6: New Compose file for defining networks and volumes](https://blog.docker.com/2016/02/compose-1-6/)

pokud si chcete přečíst novinky v češtině pokračujte v mém článku.

<!--more-->

Novinek je spousta, ale budu se věnovat jen těm zásadním.

## Docker Engine 1.10

Docker 1.10 používá nový systém pro ukládání image a layers. Není to zpětně kompatibilní, pokud uděláte upgrade, nemůžete použít staršího clienta. Migrace, která se spustí může trvat poměrně dlouho, pozor pokud máte docker v produkci. Při migraci se počítají SHA256 checksumy pro každý soubor.

Tyto změny s podporou paralelního pushovaní by měli urychlit `docker push` a `docker pull`, zvláště při pushi by to mělo být znát.

Další změny se týkají bezpečnosti. Podpora [seccomp profiles](https://en.wikipedia.org/wiki/Seccomp), User namespaces, Authorization plugins. V dalších verzích budou PID Control Group.

- nový příkaz `docker update` umožňuje měnit resources za běhu například pro navýšení paměti apod.
- příkaz `docker images` podporuje flag `--format`
- nový `status=dead` pro filtrování v `docker ps`
- deprecated `-f` pro `docker tag`
- přidaný support pro `**` v `.dockerignore` pro více úrovní adresářů
- nový logging ovladač pro Splunk
- podpora více tagů v buildu

## Docker Compose 1.6

Nový formát [version 2](https://docs.docker.com/compose/compose-file/#version-2) pro `docker-compose.yml`.

```yaml
version: '2'
services:
  web:
    build: .
    ports:
     - "5000:5000"
    volumes:
     - .:/code
  redis:
    image: redis
```

Migrace většinou nebude [složitá](https://docs.docker.com/compose/compose-file/#upgrading), stačí přidat `version: 2` a `services:` na začátek a odsadit.

### Nové vlastnosti

- nový příkaz `events` stejný jako [`docker events`](https://docs.docker.com/engine/reference/commandline/events/)
- nový příkaz `config` pro validaci `docker-compose.yml` včetně interpolací a použítí extendu
- nový příkaz `create` pro vytvoření kontejnerů bez spuštění
- nový příkaz `down` pro stopnutí a odstranění všeho co pouští příkaz `up`
- přídané nové konfigurační vlastnosti `cpu_quota`, `stop_signal`
- flag `--abort-on-container-exit` stopne všechny kontainery pokud jeden vyhodí exit


## Docker Machine 0.6

Zásadní vylepšení developer experience v příkazech docker-machine start, docker-machine stop a ostatních nemusíte používat argument `default` pokud ho přímo nespecifikujete.

Driver pro EC2 umí používat default VPC and automaticky číst přístupové údaje z `~/.aws/credentials`.

Spousta drobných změn, které mi nepřišli už důležité.

## Závěr

Každý release přinese spoustu změn. Celý ekosystém dockeru je poměrně mladý, ale o to [více dynamický](https://twitter.com/icecrime/status/694558000615288834).

V Praze se každý měsíc konají [docker meetupy](https://www.meetup.com/Docker-Prague-Czech-Republic/), pokud vás tato problematika zajímá. Přijďte nebo nabídněte svoji přednášku pořadatelům.

