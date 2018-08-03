---
title: "Migrace na statický generátor Hugo z Ghost a Octopress"
date: 2017-09-28T11:46:48+02:00
tags:
 - jamstack
 - hugo
 - golang
---

Mám dva blogy, tento v češtině, který poháněl nejdříve Wordpress potom jsem ho migroval na Jekyll a [Octopress](https://octopress.org/), a [druhý v angličtině](https://www.prskavec.net), který běžel na publikačním systému [Ghost](https://ghost.org/).

V posledním roce se objevili problémy na Ghostu s novou verzí, nexistoval přímý upgrade. Tak jsem musel udělat export a když jsem se snažil instalovat novou verzi tak jsem zjistil, že nefuguje zase s poslední verzí NodeJS 8, kterou jsem měl na serveru.

Tak jsem se rozhodl pro radikální změnu, delší dobu jsem chtěl vyřešit několik věcí.

- migraci na TLS od Let's encrypt
- mít CDN pro static assety
- zrychlit publikační workflow, abych vystačil jen s Githubem
- otestovat Netlify o kterém jsem mluvil v [předchozím příspěvku](https://blog.prskavec.net/2017/06/28/jam-stack/)
- přejít z NodeJS a Ruby na Golang [statický generátor Hugo](https://gohugo.io/), který je rychlý, dobře udržovaný a není pro mě problém si tam upravit co potřebuju
- neměnit vzhled
- zachovat obsah

## Migrace z ghostu na hugo

Tak jako první jsem začal s migrací [www.prskavec.net](https://www.prskavec.net), který běžel na Ghostu 0.x.

1. našel jsem odpovídající [casper theme pro hugo](https://github.com/vjeantet/hugo-theme-casper)
2. udělal jsem export z Ghost adminu
3. použil jsem [ghostToHugo](https://github.com/jbarone/ghostToHugo) utilitu pro import obsahu.

{{< highlight bash "linenos=inline" >}}
hugo new site www.prskavec.net-hugo

ghostToHugo —location "Europe/Prague" prskavec-net.ghost.2017-08-22.json --hugo ./www.prskavec.net-hugo

cd themes
git clone https://github.com/vjeantet/hugo-theme-casper casper
{{< / highlight >}}

Pro deploy jsem potom použil [Netlify](https://www.netlifycms.org/), které nahradilo můj VPS server, kde před tím běžel blog.

## Migrace z octopress na hugo

1. našel jsem podobný vzhled [octopress pro hugo](https://github.com/parsiya/Hugo-Octopress)
2. použil jsem import v hugo

{{< highlight bash "linenos=inline" >}}
hugo import jekyll blog.prskavec.net/source blog.prskavec.net-hugo
{{< / highlight >}}

3. potom jsem musel projít všechny posty (150+) a udělat trochu úpravy v metadatech, protože tam byli věci ješte z dob importu Wordpressu.

4. upravil jsem theme podle toho, aby se to co nejvíce podobalo původnímu blogu a snažil jsem se zachovat url apod.

Pro deploy jsem potom použil [Netlify](https://www.netlifycms.org/), které nahradilo můj Github pages, kde před tím běžel blog.

![](/images/netlify-admin.jpg)

Jak vidíte na obrázku v administraci mám oba blogy, nic to nestojí a CDN s TLS máte v ceně a build process funguje automaticky z github master, případně si můžete upravit konfiguraci jak potřebujete pokud používáte něco jiného než Hugo.

Možná se budete ptát proč nepoužívám pro themes gitmodules, ale zjistil jsem, že dělám do těch vzhledů celkem dost úprav, které jsou specifické pro mne, tak si vzhledy raději přidám přímo do repozitáře.

## Závěr

Co jsem tím získal? Hlavně méně starostí s údržbou, nemusím řešit kompatibilitu a nainstalovené verze ruby, nodejs a v klidu si přes [homebrew](https://brew.sh/) jen udržovat poslední verzi Huga a na psaní vystačím s jakýmkoliv markdown editorem, aktuálně s [VSCode](https://code.visualstudio.com/), který používám jak pro vývoje v Go tak pro NodeJS i na psaní.
