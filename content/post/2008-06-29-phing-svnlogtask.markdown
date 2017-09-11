---
date: 2008-06-29T00:00:00Z
meta:
  _edit_last: "1"
published: true
status: publish
tags:
- phing
- php
- subversion
title: Phing - SvnLogTask
type: post
url: /2008/06/29/phing-svnlogtask/
---

<a href="http://phing.info/trac/">Phing</a> obsahuje ve verzi 2.3 tyto Tasky pro práci se Subversion.
<ul>
	<li>SvnCheckoutTask</li>
	<li>SvnExportTask</li>
	<li>SvnLastRevisionTask</li>
	<li>SvnUpdateTask</li>
</ul>
To se hodí a také ve svém buildu používám tento postup:
<ol>
	<li>udělám export HEAD revision (SvnExportTask)</li>
	<li>načtu si číslo HEAD revision (SvnLastRevisionTask)</li>
	<li>potom vygeneruju SVN Log (<strong>SvnLogTask</strong>)</li>
	<li>vygeneruju API dokumentaci pro build (PhpDocumentorTask)</li>
	<li>všechno zakomprimuji do souboru s dokumentací a se zdrojáky a uložím kam potřebuju (ZipTask)</li>
</ol>
V postupu je něco co není standardní součástí <a href="http://phing.info/docs/guide/current/">Phingu</a> i když si myslím, že se to tam objeví. Phing pro práci se SVN používá <a href="http://pear.php.net/package/VersionControl_SVN/">VersionControl_SVN</a> 0.3.1 alpha. Bohužel neexistuje zatím stable verze této PEAR knihovny což je škoda, protože funguje celkem dobře a má implementováno vše co potřebuji. Když používáte VersionControl_SVN dá se pracovat s několika návratovými typy:
<ul>
	<li>VERSIONCONTROL_SVN_FETCHMODE_ASSOC,</li>
	<li>VERSIONCONTROL_SVN_FETCHMODE_OBJECT,</li>
	<li>VERSIONCONTROL_SVN_FETCHMODE_XML,</li>
	<li>VERSIONCONTROL_SVN_FETCHMODE_RAW,</li>
	<li>VERSIONCONTROL_SVN_FETCHMODE_ALL,</li>
	<li>VERSIONCONTROL_SVN_FETCHMODE_ARRAY</li>
</ul>
V Phingu se používá, ale výhradně VERSIONCONTROL_SVN_FETCHMODE_ASSOC což není někdy úplně vhodné. V případě, že máte totiž zapnutý přepínač pro výpis v XML je lepší návratový typ VERSIONCONTROL_SVN_FETCHMODE_XML. Proto jsem musel upravit <code>phing\tasks\ext\svn\SvnBaseTask.php</code>.

Tuto část
<pre name='code' class="php">
$options = array('fetchmode' => VERSIONCONTROL_SVN_FETCHMODE_ASSOC, 'svn_path' => $this->getSvnPath());
</pre>
jsem nahradil tímto kódem
<pre name='code' class="php">
		// Set up runtime options. Will be passed to all
		// subclasses.
		if ($mode=="log")
		{
		$options = array('fetchmode' => VERSIONCONTROL_SVN_FETCHMODE_XML, 'svn_path' => $this->getSvnPath());		
		}
		else
		{
		$options = array('fetchmode' => VERSIONCONTROL_SVN_FETCHMODE_ASSOC, 'svn_path' => $this->getSvnPath());
		}
</pre>

Po této úpravě, která jistě by šla udělat lépe. Jsem se dal do psaní vlastního tasku SvnLogTask.php. Task vrátí log z repozitory v XML formátu. Pokud chceme plain text, tak to prožene XSLT transformací a potom ještě vymaže whitespaces z celého dokumentu.
<pre name='code' class="php">
require_once 'phing/Task.php';
require_once 'phing/tasks/ext/svn/SvnBaseTask.php';

class SvnLogTask extends SvnBaseTask
{
    private $name = 'log.xml';
    private $xml = true;
    private $verbose = false;
    /**
     * The setter for the attribute "name"
     */
    public function setName($str) {
        $this->name = $str;
    }
    /**
     * The setter for the attribute "xml"
     */
    public function setXML($str) {
        $this->xml = $str;
    }   
    /**
     * The setter for the attribute "verbose"
     */
    public function setVerbose($str) {
        $this->verbose = $str;
    }       
	/**
	 * The main entry point
	 *
	 * @throws BuildException
	 */
	function main()
	{
		$this->setup('log');
		$this->log("Log SVN"); 
       
         $switches = array('verbose' => $this->verbose);
         $output=$this->run(array(), $switches);

        $doc = new DOMDocument();
        $doc->formatOutput = true;        
        $doc->loadXML($output);             
        // output format 
        
        if ($this->xml=="true")
        {        
          $doc->save($this->getToDir()."/".$this->name);
        }
        else
        {                 
         // print in plain
         $xsl = new DOMDocument;
         $xsl->load(dirname(__FILE__).'\LogTxt.xsl');
         $proc = new XSLTProcessor;
         $proc->importStyleSheet($xsl); // attach the xsl rules
         $output=$proc->transformToDoc($doc)->firstChild->wholeText;

         $pat[0] = "/^\s+/";
         $pat[1] = "/\s{2,}/";
         $pat[2] = "/\s+\$/";
         $rep[0] = "";
         $rep[1] = " ";
         $rep[2] = "";
         $after=preg_replace($pat, $rep, $output);
         $str=str_replace("\\n ","\n", $after);         
         file_put_contents($this->getToDir()."/".$this->name,$str);
        }
    }
}
</pre>

Potom se to dá už použít v <code>build.xml</code>. 
<pre name='code' class="xml">
        &lt;taskdef name="svnlog" classname="phing.tasks.ext.svn.SvnLogTask" /&gt;
        &lt;svnlog
           svnpath="${svnpath}"
           repositoryurl="${rep}"
           name="CHANGELOG"
           xml="true"
           verbose="true"
           todir="${tmp}\log"/&gt;
</pre>

Moje vlastní XSLT transformace (<code>phing\tasks\ext\svn\LogTxt.xsl</code>) na plain text není přiliš vhodná pro nějaké systémové řešení. Lepší je parametr nechat zapnutý. Vzít XML a pomocí XsltFilter aplikovat vlastní transformaci.

<pre name='code' class="xml">
&lt;?xml version="1.0"?&gt;
&lt;xsl:stylesheet version="1.0" 
xmlns:xsl="http://www.w3.org/1999/XSL/Transform"&gt;
 
&lt;xsl:template match="log"&gt;       			
			CHANGELOG\n
			&lt;xsl:apply-templates select='logentry'/&gt;
&lt;/xsl:template&gt;
 
 
&lt;xsl:template match="logentry"&gt;       
			\n R&lt;xsl:apply-templates select='@revision'/&gt;\n
			&lt;xsl:apply-templates select='date' /&gt; - &lt;xsl:apply-templates select='author'/&gt;\n
			&lt;xsl:apply-templates select='msg'/&gt;\n			
&lt;/xsl:template&gt;
 
&lt;/xsl:stylesheet&gt;
</pre>
Nakonec přidám ještě moje build.propeties a buid.xml jednoho moje projektu pro ilustraci.

<pre name='code' class="bash">
# Property files contain key/value pairs
# key=value
tmp = c:\tmp
svnpath = c:\apps\svn\bin\svn.exe
rep = file:///rootwww/rep_cvut/akce/trunk
wc =  c:\rootwww\wc_cvut\akce
</pre>
Tady v build.propeties si všimněte jen jediného detailu, ale ten vás může stát hodně času. Cesty k svn a k repozitory neobsahují mezery, pokud máte Subversion např. v Program Files může to nadělat více problémů než užitku a chyby se projevují různě a ne zcela systémově. Doporučuji se tomu předem vyhnout, ušetříte si čas a nervy.
<pre name='code' class="xml">
&lt;?xml version="1.0" ?&gt;
&lt;project name="akce2" basedir="." default="main"&gt;

    &lt;!-- Sets the DSTAMP, TSTAMP and TODAY properties --&gt;  
    &lt;tstamp/&gt;  
  
    &lt;!-- Load our configuration --&gt;  
    &lt;property file="./build.properties" /&gt;
    &lt;taskdef name="svnlog" classname="phing.tasks.ext.svn.SvnLogTask" /&gt;
    
    &lt;property name="package"  value="${phing.project.name}" override="true" /&gt;
    &lt;property name="builddir" value="${tmp}/build/${phing.project.name}" override="true" /&gt;
    &lt;property name="srcdir"   value="${project.basedir}" override="true" /&gt;


    &lt;target name="svn" description="SVN executes"&gt;
         &lt;!-- Export HEAD copy do /tmp/ --&gt;       
       &lt;svnexport
       svnpath="${svnpath}"
       repositoryurl="${rep}"
       force="yes"
       todir="${tmp}\export\${phing.project.name}"/&gt;
        &lt;!-- Získání čísla HEAD --&gt;
        &lt;svnlastrevision
           svnpath="${svnpath}"
           workingcopy="${wc}"
           propertyname="svn.lastrevision"/&gt;       
        &lt;!-- Vygenerování aktuálního logu --&gt;           
        &lt;svnlog
           svnpath="${svnpath}"
           repositoryurl="${rep}"
           name="CHANGELOG"
           xml="false"
           verbose="true"
           todir="${tmp}\export\${phing.project.name}"/&gt;    
    &lt;/target&gt;

    &lt;target name="phpdoc" description="API Documentation" depends="svn"&gt;
        &lt;!-- Generování phpdoc dokumentace --&gt;
        &lt;phpdoc title="Akce2008 API Documentation"
          destdir="${builddir}/apidocs"
          sourcecode="yes"
          defaultpackagename="Akce2008"
          output="HTML:default:default"&gt;
           &lt;fileset dir="${tmp}/export/${phing.project.name}"&gt;
              &lt;include name="*/*.php" /&gt;      
              &lt;exclude name="inc/phpmailer/**" /&gt;
              &lt;exclude name="build/**" /&gt;
           &lt;/fileset&gt;
            &lt;projdocfileset dir="."&gt;
                  &lt;include name="CHANGELOG" /&gt;
             &lt;/projdocfileset&gt;           
        &lt;/phpdoc&gt;
      &lt;/target&gt;

        &lt;!-- Fileset for all files --&gt;
        &lt;fileset dir="${tmp}/export/${phing.project.name}" id="allfiles"&gt;
            &lt;include name="**" /&gt;
            &lt;exclude name="build.xml" /&gt;
            &lt;exclude name="build.properties" /&gt;
        &lt;/fileset&gt;

    &lt;!-- Main Target --&gt;
    &lt;target name="main" description="main target" depends="phpdoc"&gt;
        &lt;!-- Zdrojové kódy pro příslušnou revizi --&gt;
        &lt;zip destfile="${builddir}/${phing.project.name}-R${svn.lastrevision}-${DSTAMP}${TSTAMP}.zip" basedir="${tmp}/export/${phing.project.name}" /&gt;                    
        &lt;!-- Vygenerovanou dokumentaci přesunu do ZIP s číslem příslušné revize --&gt;
        &lt;zip destfile="${builddir}/${phing.project.name}-apidocs-R${svn.lastrevision}-${DSTAMP}${TSTAMP}.zip" basedir="${builddir}/apidocs" /&gt;        
        &lt;!-- Smazaní temp adresáře --&gt;
        &lt;delete dir="${builddir}/apidocs" includeemptydirs="true" verbose="true" failonerror="true" /&gt;

    &lt;/target&gt;
&lt;/project&gt;
</pre>
Vlastní build se pustí pomocí <code>phing</code> z příkazové řádky tam kde máte <code>build.xml</code> uložený. Pokud se nedaří Phing nainstalovat a nakonfigurovat pro chod např. s PHPDoc, tak doporučuji postup ze stránek Phingu.
<pre name='code' class="bash">
pear channel-discover pear.phing.info
pear install -a phing/phing
pear install channel://pear.php.net/VersionControl_SVN-0.3.1
</pre>
Doufám, že někomu rady budou k užitku, Phing je skvělý produkt i když většina asi dá za přednost <a href="http://ant.apache.org/">Antu</a> pokud nedělají čistě PHP.
