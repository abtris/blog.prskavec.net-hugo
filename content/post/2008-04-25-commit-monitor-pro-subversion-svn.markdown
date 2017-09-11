---
date: 2008-04-25T00:00:00Z
meta:
  _edit_last: "1"
published: true
status: publish
tags:
- scm
- subversion
- windows
title: Commit monitor pro Subversion (SVN)
type: post
url: /2008/04/25/commit-monitor-pro-subversion-svn/
---

<p>Stefan K&#252;ng vytvořil tuto <a href="http://code.google.com/p/commitmonitor/">&#353;ikovnou utilitu</a>, kter&#225; monituruje commity na repozitory, kde si nastav&#237;te. Je to velmi užitečn&#233; pokud pracujete na v&#237;ce projektech a kr&#225;sně se v&#225;m to zobraz&#237; v taskbaru. </p>  <p>N&#225;hled obrazovky monitoru. </p>  <p><a href="http://blog.prskavec.net/wp-content/uploads/2008/04/image12.png"><img style="border-top-width: 0px; border-left-width: 0px; border-bottom-width: 0px; border-right-width: 0px" height="324" alt="image" src="http://blog.prskavec.net/wp-content/uploads/2008/04/image-thumb5.png" width="561" border="0" /></a> </p>  <p> Zobrazn&#237; v taskbaru potom co je detekov&#225;n nov&#253; commit.</p>  <p><a href="http://blog.prskavec.net/wp-content/uploads/2008/04/image13.png"><img style="border-top-width: 0px; border-left-width: 0px; border-bottom-width: 0px; border-right-width: 0px" height="160" alt="image" src="http://blog.prskavec.net/wp-content/uploads/2008/04/image-thumb6.png" width="261" border="0" /></a> </p>  <p>Nastaven&#237; repozitory, Commit Monitor (CM) použ&#237;v&#225; SVNParentPath directivu z Apache a tak&#233; dovoluje nastavit repozitory pomoc&#237; svnrobots.txt, kde můžete určit jak často se CM může připojovat k repozitory.</p>  <p><a href="http://blog.prskavec.net/wp-content/uploads/2008/04/image14.png"><img style="border-top-width: 0px; border-left-width: 0px; border-bottom-width: 0px; border-right-width: 0px" height="357" alt="image" src="http://blog.prskavec.net/wp-content/uploads/2008/04/image-thumb7.png" width="403" border="0" /></a> </p>  <p>Takto vyp&#225;d&#225; svnrobots.txt, hled&#225; se automaticky v web rootu, v repozitory root a trunk. Prvn&#237; nalezen&#253; se použije.</p>  <p><code># this is an example svnrobots.txt file     <br />#      <br /># the checkinterval line sets the minimum interval in minutes between      <br /># update checks:      <br />checkinterval = 90      <br /># the disallowautodiff line if it's present forces the check apps      <br /># to *not* do automatic diffs:      <br />disallowautodiff</code></p>  <p>Douf&#225;m, že v dal&#353;&#237; verzi přid&#225; možnost spu&#353;těn&#237; po commitu nějak&#233;ho batch souboru. Užil bych to pro aktualizaci logů ze SVN, kter&#233; prov&#225;d&#237;m zat&#237;m přes batch file.</p>
