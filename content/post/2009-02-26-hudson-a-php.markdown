---
date: 2009-02-26T00:00:00Z
tags:
- continuous-integration
- hudson
- php
title: Hudson a PHP
type: post
url: /2009/02/26/hudson-a-php/
---

<h3>Hudson, extensible continuous integration engine</h3>
<a href="https://hudson.dev.java.net/">Hudson</a> je obdoba <a href="https://blog.prskavec.net/category/continuous-integration/">CruiseControl a Xinc</a>, ale oproti těm má jednu zásadní výhodu a to, že jeho vývoj jede poměrně slušným tempem a programátoři v Javě co znám ho používají. Pokud jsme chtěli mít jeden server, kde běží integrace pro Javu a PHP je podle mě v této době nejlepší sáhnout právě po Hudson.
<h3>Instalace</h3>
<ul>
	<li>ze stránek si <a href="https://hudson.gotdns.com/latest/hudson.war">stáhnete war file</a> a buď ho spustíte přímo <code>java -jar hudson.war</code> (potřebujete volný port 8080 a JDK 1.5+)</li>
	<li>nebo provede instalaci do některého servlet serveru (Tomcat, Jetty, JBoss, ...)</li>
	<li>potom už na <a href="https://localhost:8080">https://localhost:8080</a> běží hudson</li>
	<li>pro php doporučuji nainstalovat tyto pluginy
<ul>
	<li><a href="https://hudson.gotdns.com/wiki/display/HUDSON/Checkstyle+Plugin">Hudson Checkstyle Plug-in</a></li>
	<li><a href="https://wiki.hudson-ci.org/display/HUDSON/Clover+Plugin">Hudson Clover plugin</a></li>
	<li><a href="https://wiki.hudson-ci.org/display/HUDSON/PMD+Plugin">Hudson PMD Plug-in</a></li>
</ul>
</li>
</ul>
<a href="https://blog.prskavec.net/wp-content/uploads/2009/02/01_dashboard-hudson_1235663237019.png"><img class="aligncenter size-medium wp-image-370" src="https://blog.prskavec.net/wp-content/uploads/2009/02/01_dashboard-hudson_1235663237019-300x222.png" alt="01_dashboard-hudson_1235663237019" width="300" height="222" /></a>
<h3>Build</h3>
<ul>
	<li>pro build jsem použil <a href="https://phing.info">Phing</a>, ale stejně můžete použít <a href="https://ant.apache.org">Ant</a></li>
</ul>
<pre>

<!-- $Id: build.xml 102 2009-02-26 14:39:10Z abtris $ -->






<!-- Main Target -->


<!-- Create dirs -->



<!-- PHP API Documentation -->






<!-- PHP CodeSniffer -->
 ${builddir}/reports/checkstyle.xml" escape="false" /&gt;

<!-- PHPUnit -->



</pre>

<h3>Příklad konfigurace</h3>
<dl>
	<dt><strong>Project name</strong></dt>
       <dd>StartPage</dd>
	<dt><strong>Source Code Management</strong></dt>
<dd>Subversion
<ul>
    <li>Repository URL: <code>https://localhost/svn/start_page/trunk</code></li>
    <li>Local module <a href="https://www.directorydomain.org/">directory</a> (optional): <code>source</code></li>
    <li>Use update: true</li>
</ul>
</dd>
<dt><strong>Build</strong></dt>
<dd>Execute shell

    <ul>
      <li><code>phing -f $WORKSPACE/source/build.xml -Dws=$WORKSPACE -Dtmp=$WORKSPACE</code></li>
</ul>
</dd>

<dt><strong>Post-build action</strong></dt>

<dt>Publish Javadoc</dt>
<dd>
    <ul>
      <li>Javadoc <a href="https://www.directorydomain.org/">directory</a> = <code>build/start_page/apidocs/</code></li>
     <li>Retain javadoc for each successful build = <code>false</code></li>
   </ul>
</dd>
<dt>Publish JUnit test result report</dt>
<dd>Test report XMLs = <code>build/start_page/reports/phpunit.xml</code></dd>

<dt>Publish Checkstyle analysis results</dt>
<dd>Checkstyle results = <code>build/start_page/reports/checkstyle.xml</code></dd>

<dt>Publish PMD analysis results</dt>


<dd>PMD results = <code>build/start_page/reports/phpunit.pmd.xml</code></dd>

<dt>Publish Clover Coverage Report</dt>

    <dd>Clover report directory = <code>build/start_page/reports/coverage/</code></dd>

</dl>
<br />
<h3>Screenshots</h3>
V galerii jsou obrázky z Hudsona, nepřidal jsem obrázky, které vedou na dokumentaci v html formátu na code coverage report v html. Jsou tam jen výstupy, které zobrazuje Hudson nebo pluginy.

[gallery link="file"]

<h3>Závěr</h3>
Moje řešení není dokonalé, ale funkční a doufám, že to pomůže rozmnožit Hudson i mojo Java komunitu. Pokud Hudson pro PHP někdo použív budu rád, když se podělíte o zkušenosti v komentářích.
