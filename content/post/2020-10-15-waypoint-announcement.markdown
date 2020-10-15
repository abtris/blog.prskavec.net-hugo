---
title: "Hashicorp Waypoint"
date: 2020-10-15T20:27:19+02:00
---

Dnes jsem na twitteru viděl oznámení o novém softwaru od Hashicopu a po [Boundary](https://www.hashicorp.com/blog/hashicorp-boundary), který oznámili včera je [Waypoint](https://www.waypointproject.io/) další zajímavá novinka.

![](/images/waypoint-twitter.png)

## Proč mě to zaujalo?

Waypoint project spojuje několik důležitých věcí dohromady a vytváří nástoj pro vývojáře podobný tomu co poskytuje dnes [Heroku Docker](https://devcenter.heroku.com/categories/deploying-with-docker). S tím rozdílem, že to není napojeno přímo na někoho jako AWS, GDP, Azure, OCI. Ale přes systém pluginů se budou moci všechny služby jednoduše napojit pokud někdo integraci udělá. Při spuštění projektu je k je verze hodně mladá.

```
$ waypoint version
Waypoint v0.1.1 (44e2d3d6)
```

Základ, ale funguje a doporučuji si zkusit jejich [getting started](https://www.waypointproject.io/docs/getting-started) s dockerem. Všechno fungovalo jak bych očekával. Na [konfiguraci](https://www.waypointproject.io/docs/waypoint-hcl) se používá HCL místo [YAMLu](https://noyaml.com/), což já ocením. Pokud pracujete jako já často s terraformem syntaxi už umíte.

Po instalaci waypointu si pustíte server, kde je celé UI pro deployment, logy, atd. Deployment probíhá tak, že se využívají [buildpacky](https://buildpacks.io/), které usnadňují práci pro jednotlivé vývojáře. Samozřejmě není to řešení pro každého, ale mě se líbí, že v kombinaci například s kubernetes nemusí vývojáři nic složitého řešit a mají deployment stejně jednoduchý jako na Heroku.

## Závěr

Uvidíme, kterým směrem se projekt posune. Určitě uvidíme pluginy pro různé cloudy a možná i další rozšíření, které dnes mě ani nenapadá. Podobných projektů je více, ale tento mi přijde nejpropracovanější a stojí za ním zajímavá firma, která přinesla hodně zajímavých nástrojů na trh.
