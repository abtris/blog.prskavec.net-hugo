---
date: 2008-12-22T00:00:00Z
published: true
status: publish
tags:
- ubuntu
title: SONY VAIO TZ31XN a Ubuntu 8.10
type: post
url: /2008/12/22/sony-vaio-tz31xn-a-ubuntu-810/
---

Sony VAIO TZ31XN je malý mobilní notebook, který je trhu asi rok, dnes ho nahradil modernější řada TT, ale krásný notebook je vybaven Windows Vista, které jsou ale k jeho hardware (1,2GHz, 2G RAM) nepřiměřené svými nároky. Notebook není nijak rychlý, pracovat se s ním dá, ale není to nic moc. Zkusil jsem tedy, jak na stejném hardware poběží jiný operační systém a zvolil jsem Ubuntu 8.10.

S touto linuxovou distribuci mám nějaké zkušenosti jako uživatel, jde snadno nainstalovat a přijde mi pro notebooky povedená, většina hardware funguje již při prvním spuštění. Neměl jsem větší problémy, jen mi nefungovala kamera ve Skype, což jsem také po troše hledání vyřešil.
<h3>Přehled hardware</h3>
<pre># lspci
00:00.0 Host bridge: Intel Corporation Mobile 945GM/PM/GMS, 943/940GML and 945GT Express Memory Controller Hub (rev 03)
00:02.0 VGA compatible controller: Intel Corporation Mobile 945GM/GMS, 943/940GML Express Integrated Graphics Controller (rev 03)
00:02.1 Display controller: Intel Corporation Mobile 945GM/GMS/GME, 943/940GML Express Integrated Graphics Controller (rev 03)
00:1b.0 Audio device: Intel Corporation 82801G (ICH7 Family) High Definition Audio Controller (rev 02)
00:1c.0 PCI bridge: Intel Corporation 82801G (ICH7 family) PCI Express Port 1 (rev 02)
00:1c.1 PCI bridge: Intel Corporation 82801G (ICH7 Family) PCI Express Port 2 (rev 02)
00:1c.2 PCI bridge: Intel Corporation 82801G (ICH7 Family) PCI Express Port 3 (rev 02)
00:1c.3 PCI bridge: Intel Corporation 82801G (ICH7 Family) PCI Express Port 4 (rev 02)
00:1d.0 USB Controller: Intel Corporation 82801G (ICH7 Family) USB UHCI Controller #1 (rev 02)
00:1d.1 USB Controller: Intel Corporation 82801G (ICH7 Family) USB UHCI Controller #2 (rev 02)
00:1d.2 USB Controller: Intel Corporation 82801G (ICH7 Family) USB UHCI Controller #3 (rev 02)
00:1d.3 USB Controller: Intel Corporation 82801G (ICH7 Family) USB UHCI Controller #4 (rev 02)
00:1d.7 USB Controller: Intel Corporation 82801G (ICH7 Family) USB2 EHCI Controller (rev 02)
00:1e.0 PCI bridge: Intel Corporation 82801 Mobile PCI Bridge (rev e2)
00:1f.0 ISA bridge: Intel Corporation 82801GBM (ICH7-M) LPC Interface Bridge (rev 02)
00:1f.1 IDE interface: Intel Corporation 82801G (ICH7 Family) IDE Controller (rev 02)
00:1f.3 SMBus: Intel Corporation 82801G (ICH7 Family) SMBus Controller (rev 02)
02:00.0 Ethernet controller: Marvell Technology Group Ltd. 88E8055 PCI-E Gigabit Ethernet Controller (rev 13)
03:00.0 Network controller: Intel Corporation PRO/Wireless 4965 AG or AGN [Kedron] Network Connection (rev 61)
09:04.0 CardBus bridge: Ricoh Co Ltd RL5c476 II (rev ba)
09:04.1 FireWire (IEEE 1394): Ricoh Co Ltd R5C832 IEEE 1394 Controller (rev 04)
09:04.3 System peripheral: Ricoh Co Ltd R5C843 MMC Host Controller (rev ff)
09:04.4 System peripheral: Ricoh Co Ltd R5C592 Memory Stick Bus Host Adapter (rev 11)
</pre>
<pre>
# lsusb
Bus 005 Device 006: ID 044e:3017 Alps Electric Co., Ltd
Bus 005 Device 005: ID 05ca:183a Ricoh Co., Ltd
Bus 005 Device 002: ID 054c:02d5 Sony Corp.
Bus 005 Device 003: ID 0409:005a NEC Corp. HighSpeed Hub
Bus 005 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 002: ID 147e:2016
Bus 004 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 003 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 001 Device 002: ID 046d:c03e Logitech, Inc. Premium Optical Wheel Mouse
Bus 001 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
</pre>
<h3>Webcamera</h3>
Pro fungování webcamery je nutné zkompilovat driver.

Pro kompilaci je nutný základní balíček a subversion pokud ho nemáte nainstalovaný tak
<pre>
apt-get install build-essential
apt-get install subversion
</pre>

potom je potřeba stáhnout nejnovější verzi ze SVN a tu zkompilovat
<pre>
svn co http://svn.mediati.org/svn/r5u870/trunk/ webcam
cd webcam
make
make install
</pre>

a potom nainstalovat ovladač.
<pre>
modprobe r5u870
</pre>
funkčnost ověřit můžete pomocí např. gstreamer-properties nebo ve skype.

Pokud budete mít nějaké dotazy k notebooku a jeho běhu pod linuxem, napište do komentářů.
