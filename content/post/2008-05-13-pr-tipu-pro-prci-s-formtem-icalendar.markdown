---
date: 2008-05-13T00:00:00Z
tags:
- internet
- php
title: Pár tipů pro práci s formátem iCalendar
type: post
url: /2008/05/13/pr-tipu-pro-prci-s-formtem-icalendar/
---

Dělám na PHP aplikaci, která kromě RSS generuje také <a href="https://cs.wikipedia.org/wiki/ICalendar">iCalendar</a> pro Lighting a iCal na Mac OS X. Někomu stačí asi <a href="https://tools.ietf.org/html/rfc2445">RFC 2445</a> a hravě si s tím poradí, ale pro ty ostatní pár tipů, které mi pomohli a které mě trochu mátli. Ještě může bý napomocný <a href="https://www.kanzaki.com/docs/ical/">iCalendar Specification Excerpts</a>.  Data mám v mysql kde je datum a čas odděleně a pokud událost nemá čas (je <code>NULL</code>) tak je to celodenní událost. Pokud má jen čas od tak končí za nějakou stanovenou dobu třeba 90 min jako ve škole. Pokud má jen datum od tak je to jednodenní akce.  Pár zásad při tvorbě iCalendar exportu
<ul>
	<li>jednotlivé tagy jsou velkými písmeny a na samostatných řádcích</li>
	<li>tagy jsou volitelné a je jich mnoho vyberte si ty které opravdu potřebujete</li>
	<li>datum se dá udávat několika způsoby:
<ul>
	<li><code>DTSTART;VALUE=DATE:YYYYMMDD</code> - jen datum asi nejednoduší forma</li>
	<li><code>DTSTART:YYYYMMDDTHHMMSS</code> - formát s časem v lokálním formátu</li>
	<li><code>DTSTART:YYYYMMDDTHHMMSSZ</code> - tady pozor Z na konci udává, že máte hodnoty v UTC (pokud ne vzniká čas posun)</li>
	<li><code>DTEND</code> je obdobně</li>
	<li><code>RRULE:FREQ=DAILY;UNTIL=YYYYMMDDTHHMMSS</code> - pro opakované události</li>
</ul>
</li>
	<li><code>URL</code> - nesmí obsahovat entity (nepodporuje Google Calendar)</li>
	<li><code>DESCRIPTION</code> - dlouhý popis</li>
	<li><code>SUMMARY</code> - nadpis</li>
	<li><code>LOCATION</code> - místo konání akce</li>
</ul>
Formát si zkontrolujte <a href="https://severinghaus.org/projects/icv/">validátorem</a>, to nikdy neškodí.  Já potom zkouším genrovaný iCalendar v Lightingu, Windows Calendar a Google Calendar. iCal pro Mac OS X nemám snad to v něm bude fungovat také.  Pro správné zobrazení češtiny v Google calendar nezapomeňte hlavičku, ostatní si poradí i bez ní, nevím proč Google ne. <code>header("Content-Type: text/<a href="https://www.dadsplan.com/">calendar</a>; charset=utf-8"); header("Content-Disposition: attachment; filename=calendar.ics");</code> Základní objekt iCalendar, který popisuje "Den oslav dobytí Bastily" jako událost konající se od 14. července 1997 17:00 (UTC) do 15. července 1997 03:59:59 (UTC)
<pre>BEGIN:VCALENDAR
VERSION:2.0
PRODID:-//hacksw/handcal//NONSGML v1.0//EN
BEGIN:VEVENT
DTSTART:19970714T170000Z
DTEND:19970715T035959Z
SUMMARY:Bastille Day Party
END:VEVENT
END:VCALENDAR</pre>
<h3>Update</h3>
<ul>
	<li>v SQL se hodí použít <code>SELECT DATE_FORMAT(datum,'%Y%m%d') as datum, TIME_FORMAT(cas,'%H%i%s') as cas, ...</code></li>
	<li>jako konce řádek je dobré napsat přímo <code>chr(13).chr(10)</code></li>
	<li>u <code>DESCRIPTION</code> nezapoměňte <code>htmlspecialchars_decode(str_replace(chr(13).chr(10),"\\n"))</code></li>
</ul>
