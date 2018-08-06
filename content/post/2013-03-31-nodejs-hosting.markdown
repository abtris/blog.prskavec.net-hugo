---
tags:
 - javascript
 - nodejs
comments: true
date: 2013-03-31T00:00:00Z
title: NodeJS Hosting
url: /2013/03/31/nodejs-hosting/
---

Mám několik webů, které jsou na NodeJS. Spousta lidí zná moje weby o javascriptu [PragueJS](https://praguejs.cz), které běží na NodeJS a je napsaný [ExpressJS](https://expressjs.com/). Web je napsaný v coffee-scriptu. Nic extra, ale řešil jsem kde web hostovat.

<!--more-->

## Heroku
Jako první jsem zvolil [Heroku](https://heroku.com/), kde musíte upravit a lehce nastavení v package.json, aby Heroku vědělo jakou verzi NodeJS a NPM má pro kompilaci použít.

package.json

	...
	"main" : "app.coffee",
  	"scripts": {
    	"start": "NODE_ENV=production coffee app.coffee"
  	},
  	"engines": {
    	"node": "0.8.x",
    	"npm":  "1.2.x"
  	},
  	...

Nastavit environment properties na production

	heroku config:set NODE_ENV=production

 a nastavení portu na kterém to na Heroku běží (PORT).

  	app.set 'port', process.env.PORT or 5000
	app.listen app.settings.port

Pro spuštění je potřeba nastavit Procfile, kde je co se má pouštět.

Procfile

	web: coffee app.coffee

Kompletní dokumentaci najdete přímo na stránkách [Heroku](https://devcenter.heroku.com/articles/nodejs).

## OpenShift
Pro zálohu jsem využil [RedHat PaaS Openshift](https://www.openshift.com/), kde musíte využít jejich vlastní nástroj na vytvoření základ aplikace.

V .openshift adresáři se nastaví všechno včetně kompilace různé verze NodeJS.

.openshift/action_hooks/pre_start_nodejs-0.6

	export NODE_ENV=production
	export PATH=$PATH:~/app-root/repo/node_modules/coffee-script/bin


Web běží za proxy a narozdíl od Heroku je potřeba nastavit kromě portu (OPENSHIFT_NODEJS_PORT) i interní IP adresu (OPENSHIFT_INTERNAL_IP).

	  app.set 'port', process.env.OPENSHIFT_NODEJS_PORT || 8080;
	  app.set 'ipaddress', process.env.OPENSHIFT_INTERNAL_IP

Musel upravit package.json, aby šel přímo pustit coffee-script, protože ho OpenShift ho zatím v době implementace nepodporoval.

	...
	"main" : "app.coffee",
	  "scripts": {
	    "start": "~/app-root/repo/node_modules/coffee-script/bin/coffee app.coffee"
	},
	...

## Další hostingy

Na [webu](https://saewitz.com/node-dot-js-websocket-hosting-roundup/) najdete zajímavé srovnání z pohledu podpory WebSockets. V tomhle ohledu je problém hlavně v nejvíce používanému Heroku, kde je podpora velmi špatná.

Další hostingy jako jsou [Nodejitsu](https://www.nodejitsu.com), [AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/), [Modulus](https://modulus.io/) a [AppFog](https://www.appfog.com/) jsou podobné a služby poskytují. Nejlepší podporu pro Websockety má Nodejitsu a Modulus, které jsou placené. Na Openshiftu jsem websockety nezkoušel podpora byt tam měla být.

Všechy moje zdrojáky jsou k dispoci na Githubu, [Heroku na masteru](https://github.com/abtris/cologne-js) a [Openshift](https://github.com/abtris/cologne-js/tree/openshift).





