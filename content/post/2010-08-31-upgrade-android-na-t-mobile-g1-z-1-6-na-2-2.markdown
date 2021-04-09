---
date: 2010-08-31T00:00:00Z
tags:
- android
title: Upgrade Androidu na T-mobile G1 z 1.6 na 2.2 (aktualizováno 22.3.2011)
type: post
url: /2010/08/31/upgrade-android-na-t-mobile-g1-z-1-6-na-2-2/
---

Mám G1 koupenou u T-mobille. Mám ji sotva rok a ve světě Androidu je celkem už skoro nepoužitelný telefon. Nikdy jsem nepřišel na chuť nahrávat si tam nové romky apod. Ale když vydal Google nový Android 2.2 Froyo s JIT tak jsem si říkal, že tohle přesně G1 potřebuje. Bohužel T-mobile i HTC se vykašlali na uživatele a jediné co chtějí aby jste si koupili nový telefon. Protože svoji G1 mám celkem rád a nechtěl jsem ji poslat do věčných lovišť tak jsem přistoupil na upgrade CyanogenMod 6.

Nečekejte nějaký úplný návod, rady apod. Pokud se chcete ptát využijte <a href="https://androidforum.cz/">Android forum</a>. Já jsem ale použil postup, který je ve <a href="https://wiki.cyanogenmod.com/index.php?title=Full_Update_Guide_-_HTC_Dream">wiki</a>.
<ul>
	<li><a href="https://wiki.cyanogenmod.com/index.php?title=Universal_Androot">UniversalRoot</a></li>
	<li><a href="https://wiki.cyanogenmod.com/index.php?title=DangerSPL_and_CM_5_for_Dream">Danger SPL</a></li>
	<li><a href="https://forum.cyanogenmod.com/index.php?/files/category/3-htc-dream-htc-magic/">potřebné soubory ke stažení</a> (radio, rom, google apps tiny)</li>
</ul>
Postupoval jsem asi takto:
<ol>
	<li>Získal jsem root pomocí universal root</li>
	<li>Z marketu jsem nainstaloval ROM Manager</li>
	<li>Pomocí něj jsem nainstalo recovery rom RA 1.7</li>
	<li>Přes recovery jsem nainstaloval poslední radio, restartoval a ověřil ještě jednou, že je tam opravdu ta poslední verze, pokud to neuděláte a nainstalujete Danger SPL riskujete brick</li>
	<li>Potom pomocí recovery rozdělil SD kartu (nevím zda bylo nutné, měl jsem 2GB)</li>
	<li>Na SD kartu jsem uložil Rom a Google Apps Mini (full neobsahovala market)</li>
	<li>Na SD kartu jsem nahrál Danger SPL a nainstaloval a restartoval, potom jsem pomocí (Camera + tl. Vypnutí tel. najel do režimu, kde jsou vidět tančící androidi místo pruhů  a ověřil, že je vše ok, vypnete pomocí Přijmutí hovoru + Menu + Vypnutí tel.)</li>
	<li>Nastartoval jsem do Recovery (při bootu držet Home nebo z aplikace ROM manager) a dal jsem wipe a CyanogenMod 6 rom</li>
	<li>Stejným postupem jsem (bez wipe) jsem aplikoval Google Apps mini a provedl konečný restart</li>
	<li>První nabíhání trvalo trochu déle a jinak to jede ok.</li>
</ol>
Každopádně všem, kteří to chtějí se svojí G1 zkusit přeji štěstí a ať se jim zdaří upgrade jako mě a netrvá jim to tak dlouho. Já jsem se zasekl hlavně na tom, že jsem stáhl poškozený zip romky a než jsem na to přišel tak to chvíli trvalo a už jsem se bál, že mám z toho brick, ale recovery mě zachránila. Kontrolujte MD5 všech stažených souborů!

Nakonec pár obrázků z mojí webové kamery:

<a href="https://blog.prskavec.net/wp-content/uploads/2010/08/20100831-qnn71wxa9egmhrp3yjd3dkqddt.png"><img class="aligncenter size-full wp-image-4244" title="g1-with-22" src="https://blog.prskavec.net/wp-content/uploads/2010/08/20100831-qnn71wxa9egmhrp3yjd3dkqddt.png" alt="" width="252" height="462" /></a>
<a href="https://blog.prskavec.net/wp-content/uploads/2010/08/Cam-2-2.png"><img class="aligncenter size-medium wp-image-4247" title="g1-with-2.2-firmware-info" src="https://blog.prskavec.net/wp-content/uploads/2010/08/Cam-2-2-300x225.png" alt="" width="300" height="225" /></a>

&nbsp;
<h3>Aktualizace 22.3.2011</h3>
<ul>
	<li><a href="https://forum.xda-developers.com/showthread.php?t=831139">Aktualizace radia a kernelu</a>, přidá vám 14MB RAM a umožňuje lepší chod nových ROMS 2.2.X</li>
	<li>Poslední romku, kterou zkouším je <a href="https://forum.xda-developers.com/showthread.php?t=811620">Official AOSP 2.2 OTA</a>, který nemá podporu češtiny, ale zatím mi to přijde jako nejlepší 2.2 romka, kterou jsem měl. Ještě případně poreferuji, zatím to vypadá dobře, předchozí pokusy mě přivedli až k tomu, že telefon byl tak pomalý až nepoužitelný po cca měsíci používaní. Obzvláště poslední CM pro G1.</li>
</ul>
