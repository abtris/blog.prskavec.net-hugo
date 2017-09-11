---
categories: [nodejs,raspberry]
comments: true
date: 2012-07-28T00:00:00Z
title: Raspberry Pi
url: /2012/07/28/raspberry-pi/
---

## Jak jsem si pořidil

Rapsberry Pi jsem objednal u [Farnellu](http://export.farnell.com/rp/order/) v červnu a na konci července mi přišel dopis ve kterém bylo zařízení.

<!--more-->

## Co jsem potřeboval, aby to fungovalo

Měl jsem zařízení, ale co dál aby to fungovalo.

Potřebujete:

- zdroj (700mA, 5V s microUSB) - použil jsem nabíječku z iphone a dal jiný usb kabel
- SD kartu, kam dáte systém (použil jsem 4GB class 10)
- kabel na připojení monitoru, použil jsem HDMI 
- klavesnici a myš (USB)

{{< figure class="center" src="/images/raspberry/raspberry.jpg" title="Raspberry running" >}}


Ze stránek [raspberry](http://www.raspberrypi.org/downloads) jsem si stáhnul Raspbian “wheezy” a pomocí dd jsme ho nahrál na sd kartu.

{{< figure class="center" src="/images/raspberry/debian.jpg" title="Raspbian “wheezy”" >}}

## Node.js na Raspberry PI

Po rozběhnutí systému není problém nainstalovat cokoliv potřebujete. Spustil jsem ssh server na nainstaloval NodeJS.

    sudo apt-get install nodejs npm

Není potom problém pustit aplikaci.

{{< figure class="center" src="/images/raspberry/nodejs.jpg" title="Node.js" >}}
    