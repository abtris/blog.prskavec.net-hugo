---
categories: [web]
comments: true
date: 2014-12-11T00:00:00Z
title: Zapier a zasílání zůstatku z banky zdarma na mobil
url: /2014/12/11/zapier-a-zasilani-zustatku-z-banky-zdarma-na-mobil/
---

## Motivace
Banky posílají změnu zůstatku emailem a SMS. Za SMS začínájí účtovat třeba i 2Kč což mi přijde fakt hrůza. Tak jsem si řekl jak dostat ten email do telefonu pomocí push notifikace, aby mě to nestálo moc peněz a dalo se případně používat univerzálně.

<!--more-->

## Pushover
Služba [Pushover](https://pushover.net/) není zcela zdarma, ale zaplatíte jen jednorázově při nákupu aplikace nebo při aktivaci desktop notifkací. A získáte tím možnost zasílat notifikace skoro z čehokoliv a kam potřebujete. Můžete konfigurovat kdy nechcete být rušeni (noc, víkendy apod.). 

## Parse emails by Zapier 
Služba [Zapier](https://zapier.com/) má zajímavou [službu na parsování dat z emailu](http://parser.zapier.com/), která se zatím zdarma a nenašel jsem jinou, který by byla dobrá pro tento můj problém. Existují sice služby na parsování emailů, ale ty jsou spíše děláný na zpracování velkého množství emailů a nejsou zdarma.

Na službě si zřídíte emailovou schránku na kterou si přepošlete email z banky, označíte si v něm část co chcete posílat na mobil. Služba potom toto políčko poskytne jako placeholder pro zpracování a poslání dále.

## Zapier
V Zapieru potom nakonfigurujete zpracování emailů z vaší schránky na parseru a poslání na mobil pomocí pushover. Zkoušel jsem třeba i hangouts, ale nedošlo mi to všechna zařízení a není to tak komfortní jako pushover.

## Závěr
I bez programování můžete vyzrát na chytráky co si snaží za služby s minimálními nároky účtovat hromady peněz a [Zapier](https://zapier.com/) nebo [IFTTT](https://ifttt.com) jsou na  to super pomocníci.

