---
tags:
- continuous-integration
- conference
- webexpo
date: 2011-09-25T00:00:00Z
title: Deployment prakticky - WebExpo 2011
url: /2011/09/25/deployment-prakticky-webexpo-2011/
---

Letos jsem přednášel na Webexpu na téma deployment. Ve 30 minutách se nedá říci dost, aby jste se dozvěděli všechno tak zkusím v tomto článku shrnout o čem jsem mluvil a přidat nějaké zdroje, které pomůžou v dalších úsilí při automatizaci deploymentu.

<h2 id="deployment">Deployment</h2>
<ul>
	<li>Nasazení dané aplikace na dané prostředí v daném čase s možností vrácení původní verze aplikace pokud možno s minimálními prostředky a náklady.</li>
</ul>
<h2 id="configuration_management">Configuration Management</h2>
<ul>
	<li>Služba, která se stará, aby námi zadaná konfigurace byla konzistentní napříč prostředím a na všech strojích a je to jeden nezbytných kroků pro CI.</li>
</ul>
<h2 id="continuous_integration">Continuous Integration</h2>
CI pomáhá dramaticky snížit počet chyb při vývoji i v produkci pokud se to dělá dobře a důsledně.

Pro dobré fungovaní Continuous Integration potřebujete automatizovat deployment na prostředí. Na prostředích se často střídají aktuální feature větve vývojářů nebo se tam provádí update z větve pro release či sprint. K tomu se vše provádí několikrát denně. Abychom tohle mohli dělat potřebuje deployment mít plně automatický.
<pre><code>"Automated deployment, tied into good CI discipline, is essential to making this work." - Martin Fowler
</code></pre>
<h2 id="tlatko_push">Tlačítko push</h2>
Deployment musí být tak jednoduchý, aby ho zvládnul kdokoliv, kdo ho potřebuje a má potřebné oprávnění. Zdá se celkem jednoduché, ale není tomu tak. Pokud je potřeba například znalost linuxu (vytváření symlinku, ssh klient apod.), může to být pro část týmu třeba dost velká překážka, aby si něco nasadili a raději řeknou developerovi. On to má přece za minutku nasazené. Ale v tom je problém, pokud developer není po ruce, nebo nemá čas, tak část práce stojí. Obzvláště při agilním vývoji a krátkém sprintu to může být dost znát.

Pokud ještě se deployment dělá ručně, občas se na něco zapomene a jakákoliv ruční práce je zdrojem chyb. Celkově dojde k úspoře nákladů, protože čas ztrávený deploymentem firmě nikdo nezaplatí.

Za tlačítkem push si můžete představit jakýkoliv spouštěč, který vám vyhovuje (git push do příslušné větve, tlačítko na webu apod.).

Pokud nemáte zkušenost s gitem, tak rozvedu ten git push. Ostatní mohou odstavec přeskočit.

Git jako distribuovaný systém funguje jen lokálně a může mít svoje vzdálené kopie repository podobně jako v architektuře klient server (Subversion). Ale nemusí mít jen jedno místo kam se zdrojový kód odesílá pomocí git push. Toho se dá využít a používá se to tak, že kromě serveru kam ukládáte zdrojový kód (u mě například Github.com), který je označený jako origin (git push origin master - odešle kód tam), mohu například přidat místo kde se pushem přímo nasadí (git push heroku master - kód se odešle na cloud Heroku a dojde k jeho nasazení). Obdobně kromě místa, lze použít určité větve a mít na serveru hook script, který detekuje změny a provede nasazení.
<ul>
	<li><a href="https://stackoverflow.com/questions/279169/deploy-a-project-using-git">https://stackoverflow.com/questions/279169/deploy-a-project-using-git</a></li>
	<li><a href="https://ryanflorence.com/deploying-websites-with-a-tiny-git-hook">https://ryanflorence.com/deploying-websites-with-a-tiny-git-hook </a></li>
</ul>
<h2 id="jak_to_nechat_na_jinch">Jak to nechat na jiných</h2>
Pokud můžete a děláte projekt, kde se můžete dovolit využít hostingu, cloudu, který deployment vyřeší za vás, udělejte to. Ušetří vám to čas i prostředky. Samozřejmě ne vždy to jde, ale pro ty případy tu bude další kapitola.
<h3 id="herokucom"><a href="https://heroku.com">Heroku.com</a></h3>
<ul>
	<li>Ruby, Node.js, Closure, Java (CLI client, rake pro příkazy jako jsou migrace db, hodně dodatečných služeb - <a href="https://addons.heroku.com/">https://addons.heroku.com/</a>)</li>
	<li>částečná podpora pro PHP a Python přes Facebook - <a href="https://developers.facebook.com/blog/post/558/">https://developers.facebook.com/blog/post/558/</a>.</li>
</ul>
Další služby, recenze některých jsem dělal dříve.
- <a href="https://www.pagodabox.com">https://www.pagodabox.com</a>
- <a href="https://phpfog.com">https://phpfog.com</a>
- <a href="https://orchestra.io/">https://orchestra.io/</a>
- <a href="https://www.engineyard.com">https://www.engineyard.com</a>
- <a href="https://cloudcontrol.com/">https://cloudcontrol.com/</a>
- <a href="https://relbit.com">https://relbit.com</a>
- <a href="https://nodester.com/">https://nodester.com/</a>

Samozřejmě jsem nemohl vyzkoušet podrobně každou cloudovou službu, na to nemám rozpočet a ne každá má k dispozici zdarma potřebný program pro vývojáře. Důležité aspoň pro mě je podpora práce z CLI kvůli možnosti automatizace, případně nějaké API, které to umožňuje. A také podpora vlastního nastavení a úkolů, aby jste mohli například automaticky spustit test nebo migraci databáze. Svoje zkušenosti nebo rady a doporučení uveďte do komentářů, budu rád.
<h2 id="jak_si_to_vyrobit_sm">Jak si to vyrobit sám</h2>
<h3 id="rozdlen_nstroj_pro_deployment">Rozdělení nástrojů pro deployment</h3>
<ul>
	<li>balíčkovací systémy (<a href="https://en.wikipedia.org/wiki/Listofsoftwarepackagemanagement_systems">https://en.wikipedia.org/wiki/List<em>of</em>software<em>package</em>management_systems</a>)
<ul>
	<li>podle operačního systému (RPM, DEB)</li>
	<li>podle jazyka (phar, pear, gem, npm, cpan)</li>
</ul>
</li>
	<li>nástroje
<ul>
	<li>capistrano - <a href="https://capify.org/">https://capify.org/</a></li>
	<li>fabric - <a href="https://fabfile.org">https://fabfile.org</a></li>
	<li>cfengine - <a href="https://cfengine.com/">https://cfengine.com/</a></li>
	<li>puppet - <a href="https://puppetlabs.com/">https://puppetlabs.com/</a></li>
	<li>chef - <a href="https://www.opscode.com/">https://www.opscode.com/</a></li>
	<li>ant - <a href="https://ant.apache.org/">https://ant.apache.org/</a>, maven - <a href="https://maven.apache.org/">https://maven.apache.org/</a>, buildr - <a href="https://buildr.apache.org/">https://buildr.apache.org/</a></li>
	<li>make, rake, phake, phing apod.</li>
</ul>
</li>
</ul>
<h3 id="capistrano">Capistrano</h3>
Instalace
- pro instalaci potřebujete ruby (windows - <a href="https://rubyinstaller.org/">https://rubyinstaller.org/</a>, mac - součást OS)
- pokud budete deployment dělat pro ruby aplikaci nepotřebujete samozřejmě railsless-deploy
<pre><code>gem install capistrano capistrano-ext railsless-deploy
</code></pre>
Zdrojový kód si stáhněte z <a href="https://github.com/abtris/webexpo2011-wordpress.git">githubu</a>.
<pre><code>git clone https://github.com/abtris/webexpo2011-wordpress.git
</code></pre>
Zajímají nás tyto soubory:
<pre><code>Capfile
templates/apache.conf.erb
config/deploy/development.rb
config/deploy/production.rb
config/deploy/apache.rb
config/deploy/deploy.rb
</code></pre>
které nám umožňují dělat deployment:
cap deploy
cap development deploy
cap production deploy

a configuration management:
<pre><code>cap apache:setup
cap apache:start
cap apache:stop
cap apache:restart
</code></pre>
<h3 id="praktick_ukzka_capistrana_na_videu">Praktická ukázka Capistrana na videu</h3>
<ul>
	<li>používám virtuální server pro deployment, který je vytvořen pomocí Vagrant (<a href="https://www.vagrantup.com">www.vagrantup.com</a>) a Chef</li>
	<li>ukázka jak se vytváří Jenkins server pro testovaní - <a href="https://github.com/abtris/vagrant-hudson">https://github.com/abtris/vagrant-hudson</a></li>
</ul>
<iframe width="420" height="315" src="https://www.youtube.com/embed/MisWQe4JhEQ" frameborder="0" allowfullscreen></iframe>
<h2 id="jenkins_jak_se_to_dl_v_jobscz">Jenkins - jak se to dělá v Jobs.cz</h2>
<ul>
	<li>v LMC (jobs.cz, prace.cz) je vývoj v Javě a PHP. Pro oboje používáme od roku 2008 na build Hudson dnes Jenkins. Pro Javu se přechází na build postavená na Mavenu a v PHP používáme build napsaný v Antu, který vytváří RPM balíčky pro nasazení CFEngine nebo pomocí skriptu přímo z Jenkins.</li>
	<li>jak vyrobit RPM pro PHP - ukázka s kódem k dispozici <a href="https://github.com/abtris/barcamp2010praha">https://github.com/abtris/barcamp2010praha</a></li>
</ul>
etc/demoapp.spec
<pre><code>Summary: Demo App
Name: @@PACKAGE_NAME@@
Version: @@VERSION@@
Release: @@BUILD@@
License: Copyright by Ladislav Prskavec
Group: System Environment/Internet
BuildRoot: @@BUILDROOT@@

#topdir
%define _topdir @@TOPDIR@@
%define _target_os Linux

%description
Barcamp 2010 demo app

%install
mkdir -p $RPM_BUILD_ROOT/srv/www/demoApp/@@CURRENT@@
cp -r ${FILENAME}-${VERSION}/* $RPM_BUILD_ROOT/srv/www/demoApp/@@CURRENT@@/

%clean
[ "${RPM_BUILD_ROOT}" != "/" ] &amp;&amp; rm -rf ${RPM_BUILD_ROOT}

%files
/srv/www/demoApp/@@CURRENT@@

%defattr(755,root,root)
%dir /srv/
%dir /srv/www
%dir /srv/www/demoApp/
%dir /srv/www/demoApp/@@CURRENT@@/

%post
echo "run: ln -nfs /srv/www/demoApp/@@CURRENT@@/ /srv/www/demoApp/current"
</code></pre>
a k tomu příslušný build script
<pre><code>&lt;project name="project_name" default="clean" basedir="."&gt;
&lt;!-- task definition for cycle --&gt;
&lt;taskdef resource="net/sf/antcontrib/antlib.xml"/&gt;
    &lt;description&gt;
        barcamp demo app
    &lt;/description&gt;

  &lt;!-- options from properties file --&gt;
  &lt;!-- ${ws} is in Hudson $WORKSPACE --&gt;
  &lt;echo message="reading properties from: ${ws}/etc/build.properties" /&gt;
  &lt;echo message="module name ${modulename}" /&gt;
  &lt;property file="${ws}/etc/build.properties"/&gt;
  &lt;!-- options from .spec file --&gt;
  &lt;property name="spec_file" value="/etc/${modulename}.spec" /&gt;
  &lt;!--  make unique rpm_build_root, solve problems with branches --&gt;
  &lt;exec executable="mktemp" outputproperty="rpm_build_root"&gt;
    &lt;arg value="-d" /&gt;
    &lt;arg value="-t" /&gt;
    &lt;arg value="${modulename}.XXXXXXXXXX" /&gt;
  &lt;/exec&gt;
  &lt;echo message="RPM Build root set to: ${rpm_build_root}" /&gt;
  &lt;!-- set global properties for this build --&gt;
  &lt;property name="build" location="${rpm_build_root}/BUILD"/&gt;
  &lt;property name="rpms" location="${rpm_build_root}/RPMS"/&gt;

 &lt;target name="init"&gt;
    &lt;!-- Create the time stamp --&gt;
    &lt;tstamp/&gt;
    &lt;!-- Create the build directory structure used by compile --&gt;
    &lt;mkdir dir="${build}"/&gt;
    &lt;mkdir dir="${rpms}/i386"/&gt;
  &lt;/target&gt;

 &lt;target name="gitrevision" unless="REV" depends="init"&gt;
   &lt;exec executable="git" output="${rpm_build_root}/gitinfo"&gt;
     &lt;arg value="rev-list"/&gt;
     &lt;arg value="--all"/&gt;
   &lt;/exec&gt;
   &lt;exec executable="wc" output="${rpm_build_root}/wcinfo"&gt;
     &lt;arg value="-l" /&gt;
     &lt;arg value="${rpm_build_root}/gitinfo" /&gt;
   &lt;/exec&gt;
   &lt;exec executable="awk" outputproperty="REV"&gt;
     &lt;arg value="{ print $1 }"/&gt;
     &lt;arg value="${rpm_build_root}/wcinfo"/&gt;
   &lt;/exec&gt;
   &lt;echo message="REV: ${REV}" /&gt;
 &lt;/target&gt;

 &lt;target name="gitexport" depends="gitrevision"&gt;
  &lt;mkdir dir="${build}/${modulename}-${REV}" /&gt;
  &lt;copy todir="${build}/${modulename}-${REV}"&gt;
    &lt;fileset dir="../"&gt;
      &lt;exclude name="../.git/*"/&gt;
    &lt;/fileset&gt;
  &lt;/copy&gt;
   &lt;echo message="Coping files ..." /&gt;
 &lt;/target&gt;

 &lt;!-- take spec files  --&gt;
 &lt;target name="spec" depends="gitexport"&gt;
        &lt;!-- get spec files --&gt;
        &lt;path id="spec.files"&gt;
            &lt;fileset  dir="${build}/${modulename}-${REV}/etc"&gt;
                &lt;include name="*.spec"/&gt;
            &lt;/fileset&gt;
        &lt;/path&gt;
        &lt;!-- convert slashes to unix (necessary for Win) --&gt;
        &lt;pathconvert pathsep="," targetos="unix" property="files" refid="spec.files"/&gt;

        &lt;echo message="Call RPM" /&gt;
        &lt;!-- Call RPM for all spec files (task for need antcontrib --&gt;
        &lt;for list="${files}" param="item"&gt;
        &lt;sequential&gt;
            &lt;antcall target="replace"&gt;
                &lt;param name="spec_file" value="@{item}"/&gt;
            &lt;/antcall&gt;
            &lt;antcall target="rpm"&gt;
                &lt;param name="spec_file" value="@{item}"/&gt;
            &lt;/antcall&gt;
        &lt;/sequential&gt;
        &lt;/for&gt;

 &lt;/target&gt;

  &lt;!-- replace tokens in spec files --&gt;
  &lt;target name="replace"&gt;
    &lt;replace file="${spec_file}" token="@@CURRENT@@" value="${major_version}-${REV}"/&gt;
    &lt;replace file="${spec_file}" token="@@TOPDIR@@" value="${rpm_build_root}"/&gt;
    &lt;replace file="${spec_file}" token="${VERSION}" value="${REV}"/&gt;
    &lt;replace file="${spec_file}" token="${FILENAME}" value="${modulename}"/&gt;
    &lt;basename property="spec.filename" file="${spec_file}" suffix=".spec" /&gt;
    &lt;echo message="${spec.filename}" /&gt;
    &lt;replace file="${spec_file}" token="@@PACKAGE_NAME@@" value="${spec.filename}"/&gt;
    &lt;replace file="${spec_file}" token="@@VERSION@@" value="${major_version}"/&gt;
    &lt;replace file="${spec_file}" token="@@BUILD@@" value="${REV}"/&gt;
    &lt;replace file="${spec_file}" token="@@BUILDROOT@@" value="${rpm_build_root}-${spec.filename}/" /&gt;
    &lt;replace file="${spec_file}" token="@" value=""/&gt;
  &lt;/target&gt;

 &lt;!-- rpmbuild  --&gt;
 &lt;target name="rpm"&gt;
   &lt;echo message="SPEC: ${spec_file}" /&gt;
   &lt;exec executable="rpmbuild"&gt;
      &lt;arg value="--target=i386"/&gt;
      &lt;arg value="-bb"/&gt;
      &lt;arg value="${spec_file}"/&gt;
   &lt;/exec&gt;

 &lt;/target&gt;

  &lt;!-- copy RPM files to TOP_DIR and delete tmp dirs --&gt;
  &lt;target name="clean" depends="spec"&gt;
    &lt;copy todir="${target_path}"&gt;
        &lt;fileset  dir="${rpms}/i386/"&gt;
        &lt;include name="*.rpm"/&gt;
    &lt;/fileset&gt;
    &lt;/copy&gt;  &lt;/target&gt;

&lt;/project&gt;
</code></pre>
<h3 id="infrastructure_as_code">Infrastructure As code</h3>
Pokud máte k dispozici více prostředí, nebo pokud používáte masivnější virtualizaci jistě víte, že to bez automaticace nejde.

K nejznámějším nástrojům patří CFEngine, Puppet a Chef.
<pre><code>1993 - CFEngine
2002 - CFEngine 2
2005 - Puppet
2009 - CFEngine 3, Chef
</code></pre>
O chefu jsem se zmiňoval již dříve, je vhodný právě například pro tvorbu virtualizací ve velkém. Pro příklad uvedu cluster <a href="https://blog.cyclecomputing.com/2011/09/new-cyclecloud-cluster-is-a-triple-threat-30000-cores-massive-spot-instances-grill-chef-monitoring-g.html">Nostromu</a>, který měl 30 000 jader.

Puppet
- https://www.puppetlabs.com
- napsaný v Ruby, odvozený od cfengine
- klient aktualizuje konfiguraci v pravidelných intervalech a reportuje zpět na server (Puppet Master)
- perfektní pro “sysadmins”
- deklarativní popis nastaveni

Chef
- https://www.opscode.com/chef
- v Ruby, odvozený od Puppetu
- Ruby DSL
- cloud provisioning
- imperativní popis, více flexibilní, více pro “vývojáře”
- pokud by vás to zajímalo doporučuji zajít na <a href="bit.ly/skolenichef">školení Radima Marka</a> (GoodData) - s kódem WEBEXPO2011 by měla být sleva 15%
<h2 id="slidy">Slidy</h2>
<div style="width:425px" id="__ss_9388737"> <strong style="display:block;margin:12px 0 4px"><a href="https://www.slideshare.net/ladislavprskavec/deployment-prakticky" title="Deployment prakticky" target="_blank">Deployment prakticky</a></strong> <iframe src="https://www.slideshare.net/slideshow/embed_code/9388737" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe> <div style="padding:5px 0 12px"> View more <a href="https://www.slideshare.net/" target="_blank">presentations</a> from <a href="https://www.slideshare.net/ladislavprskavec" target="_blank">Ladislav Prskavec</a> </div> </div>
<h2 id="zvr_co_si_odnst">Závěr - co si odnést</h2>
<ul>
	<li>Existují kvalitní nástroje pro deployment, použijte je!</li>
	<li>Každý oprávněný uživatel si lehce zjistí informaci o verzi aplikace na příslušném prostředí a jednoduše si nasadí verzi aplikace, kterou potřebuje na prostředí, kde to potřebuje.</li>
	<li>Jedině automatizací se vyhnete chybám. Každý ruční postup jednou selže.</li>
	<li>Uspoříte velké náklady, protože čas ztraceny deploymentem nepřináší žádný zisk.</li>
</ul>

{% blockquote Ladislav Prskavec - Kurz Jenkins https://webexpo.cz/academy/kurzy/jenkins-jak-na-continuous-integration-v-php WebExpo Academy %}
<h4>Jenkins - jak na Continuous Integration v PHP</h4>
Pochopení a využití procesu Continuous Integration s využitím nástroje Jenkins vám pomůže zvýšit kvalitu softwaru, který vyvíjíte, a zároveň snížit čas na jeho dodávku. Continuous Integration vám umožňuje kontrolovat kvalitu softwaru průběžně po malých částech a minimalizovat tak riziko rozsáhlých chyb, jak tomu bylo v případě klasického vodopádového přístupu.
{% endblockquote %}

