---
categories: [docker]
comments: true
date: 2015-05-14T00:00:00Z
title: Amazon Elastic Beanstalk a docker
url: /2015/05/14/amazon-elastic-beanstalk-a-docker/
---

Amazon Elastic Beanstalk je Platform as Service podobný známému Heroku. Jen je součást Amazon Web Services. Podporuje řadu jazyků a v neposlední době přidal podporu [Dockeru](http://www.docker.io). Díky podpoře docker kontejnerů je možné pustit víceméně cokoliv.

<!--more-->

Amazon web services operují v několika regionech. Dnes je jich 10 a z toho máte dva i v evropě. Pokud chcete zkoušet novinky, které AWS poskytuje doporučuji použít region `us-east-1`.

Amazon kromě regionů, které vám umožňují poskytovat služby z geograficky nejbližšího místa vašim zákazníkům, také podporuje Availability Zones (AZ), které vám umožňují zvýšit spolehlivost v jednom regionu. V každém regionu je k dispozici několik zón. Pokud máte skupinu serverů ve škálovacím režimu je dobré je rozprostřít přes několik AZ a tím máte jistotu pokud dojde k výpadku jedné zóny, že vaše služba poběží.

Elastic Beanstalk podporuje PHP, NodeJS, Python, Ruby, Java, Go a Docker.

Teď k Elastic Beanstalku (EB). Služba využívá zdroje AWS. Na vstupu je Elastic Load Balancer, který vám směřuje provoz na vaši aplikaci.


## Docker

Pro funkci dockeru potřebujete buď `Dockerrun.aws.json` nebo `Dockerfile`. Já pro svůj příklad používám json file.

```
{
    "AWSEBDockerrunVersion": "1",
    "Image": {
        "Name": "registry:0.9.1"
    },
    "Volumes": [
    ],
    "Ports": [
        {
            "ContainerPort": "5000"
        }
    ]
}
```

tady je příslušný dockerfile, ale ten nemusíte použít. Buď json file nebo dockerfile.

```
FROM registry:0.9.1

ENV DEPS loose
RUN pip uninstall -y docker-registry-core && pip uninstall -y boto && pip install boto==2.34.0 && pip install docker-registry-core

RUN env

ENV SETTINGS_FLAVOR s3
ENV AWS_BUCKET $AWS_S3_BUCKET
ENV STORAGE_PATH /registry
ENV AWS_KEY $AWS_S3_ACCESS_KEY
ENV AWS_SECRET $AWS_S3_SECRET_KEY
ENV AWS_REGION $AWS_S3_REGION
ENV AWS_HOST s3.amazonaws.com
ENV AWS_PORT 443
ENV AWS_CALLING_FORMAT REGULAR
ENV DEBUG True
ENV LOGLEVEL debug

EXPOSE 5000

CMD ["docker-registry"]
```

Možná vás překvapí, že kontainer nemá definovaný port na který má být směřovám. EB ve verzi 1 směřuje všechno na port 80, kde nginx proxy. Pokud definujete více portů použije se jen ten poslední.

Není to moc užitečné a pokud potřebujete použít více portů nenašel jsem vhodné řešení. Konfigurční předpis, ale již existuje ve verzi 2, kde jsou možnosti mnohem širší a tyto problémy se dají dobře řešit. Ale zatím to není v oficiální dokumentaci a našel jsem to jen AWS labs.

## Nginx Proxy

Jak jsem říkal v předchozím odstavci, máte defaultní nginx proxy a veškeré nastavení se dá změnit, ale musíte to udělat pomocí adresáře `.ebextensions`.

Například konfigurace nginxu:

```
files:
  "/etc/nginx/docker-registry.conf":
    mode: "000644"
    owner: root
    owner: root
    content: |
      proxy_pass                        http://docker;
      proxy_http_version                1.1;
      proxy_set_header  Host            $http_host;
      proxy_set_header  X-Real-IP       $remote_addr;
      proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header  Authorization  "";
      proxy_read_timeout               900;
```

a tady konfigurace vlastní website, kde jsem přidal autorizaci.

```
files:
  "/etc/nginx/sites-available/elasticbeanstalk-nginx-docker-proxy.conf":
    mode: "000644"
    owner: root
    owner: root
    content: |
      server {
        listen 80;

        client_max_body_size 0;
        chunked_transfer_encoding on;

        location / {
          auth_basic            "Restricted";
          auth_basic_user_file  docker-registry.htpasswd;
          include               docker-registry.conf;
        }

        location /_ping {
          auth_basic off;
          include               docker-registry.conf;
        }

        location /v1/_ping {
          auth_basic off;
          include               docker-registry.conf;
        }
      }
```

## Závěr

Je potřeba definovat několik proměnných prostředí, které jsou vidět na skriptu pro spuštění.

```
docker run \
         -e SETTINGS_FLAVOR=s3 \
         -e AWS_BUCKET=$AWS_S3_BUCKET \
         -e STORAGE_PATH=/registry \
         -e AWS_KEY=$AWS_S3_ACCESS_KEY \
         -e AWS_SECRET=$AWS_S3_SECRET_KEY \
         -e SEARCH_BACKEND=sqlalchemy \
         -e DEBUG=True \
         -e LOGLEVEL=debug \
         -e AWS_REGION="us-east-1" \
         -p 5000:5000 \
         registry
```

PaaS jako Elastic Beanstalk je celkem použitelný, má CLI clienta, funguje s gitem. Má to některé věci, které mi chybí například na Heroku (VPC, auto scaling). Ale přesto mi například práce s heroku přijde příjemnější i když to teď [zabili změnou plánů](https://www.heroku.com/beta-pricing).

