---
date: 2008-09-11T00:00:00Z
meta:
  _edit_last: "1"
published: true
status: publish
tags:
- javascript
- jquery
- zend framework
title: Zend Framework 1.6 a moje zkušenosti s Dojo TabContainer
type: post
url: /2008/09/11/zend-framework-16-a-moje-zkuenosti-s-dojo-tabcontainer/
---

Zrovna dělám na jednom malém projektu, který jsem hned začal psát zrovna jak vyšel ZF 1.6, celkem standardní věci až na to, že jsem potřeboval rozdělit formulář na více stránek a udělat záložky.

Postupoval jsem <a href="http://framework.zend.com/manual/en/zend.dojo.form.html#zend.dojo.form.decorators.dijitContainer">podle manulálu</a>, vyvořil jsem si formulář se subformy a celkem to dobře funguje. Potom když jsem potřeboval rozbrazit záložky narazil jsem na několik problémů se kterými jsem si různě poradil.

Takto vypadají <a href="http://dojotoolkit.org/book/dojo-book-0-9/part-2-dijit/layout/tab-container">záložky pomocí Dojo frameworku</a>.

<a href="http://blog.prskavec.net/wp-content/uploads/2008/09/image.png"></a>

Nejdříve byl problém přidat záložku kde byl jen text. Tak jsem vytvořil vlastní element a ten potom vracel jen co jsem do něj napsal za text. Dalším problémem byl konec fomuláře jak je vidět na obrázku tak špatně uzavíral a číst ho mizela.

<a href="http://blog.prskavec.net/wp-content/uploads/2008/09/image1.png"></a>

To jsem opravil tak, že jsem celou strukturu záložek přesunul do View.
<pre name='code' class='php'>
&lt;?php echo $this-&gt;form; ?&gt;
</pre>
jsem musel udělat
<pre name='code' class='php'>
&lt;?php
$this-&gt;dojo()-&gt;enable();

echo "&lt;form method='".$this-&gt;form-&gt;getMethod()."' enctype='application/x-www-form-urlencoded' &gt;";

// Container with tabs
$this-&gt;tabContainer()-&gt;captureStart('tab1', array(), array('style' =&gt; 'width:950px;height:800px;'));

    // First tab "Dates"
    $this-&gt;contentPane()-&gt;captureStart('pane1', array(), array('title' =&gt; 'Vstupní evidence'));
        echo $this-&gt;form-&gt;getSubform('page1');
    echo $this-&gt;contentPane()-&gt;captureEnd('pane1');

    // Second tab "FAQ"
    $this-&gt;contentPane()-&gt;captureStart('pane2', array(), array('title' =&gt; 'Příprava rozpočtu'));
      echo $this-&gt;form-&gt;getSubform('page4');     
    echo $this-&gt;contentPane()-&gt;captureEnd('pane2');

    // Third tab "Closable"
    $this-&gt;contentPane()-&gt;captureStart('pane3', array(), array('title' =&gt; 'Podání projektu'));
        echo $this-&gt;form-&gt;getSubform('page2');
    echo $this-&gt;contentPane()-&gt;captureEnd('pane3');

    // Fourth tab "Splitted"
    $this-&gt;contentPane()-&gt;captureStart('pane4', array(), array('title' =&gt; 'Realizace projektu'));
      echo $this-&gt;form-&gt;getSubform('page3');
    echo $this-&gt;contentPane()-&gt;captureEnd('pane4');

echo $this-&gt;tabContainer()-&gt;captureEnd('tab1');

echo $this-&gt;form-&gt;submit;

echo "&lt;/form&gt;"
?&gt;
</pre>
 

Je to složitější ale všechno vypadalo jak mělo, jen byl problém s poslední stranou formuláře. Je tam málo položek a tabContainer se neumí přizpůsobit výšce vloženého obsahu. To se mi nepodařilo vyřešit jinak než nahradit Dojo <a href="http://docs.jquery.com/UI/Tabs">jQuery UI</a>, který jsem byl zvyklý používat doteď.
<pre name='code' class='php'>
  &lt;script&gt;
  $(document).ready(function(){
    $("#example &gt; ul").tabs();
  });
  &lt;/script&gt;
&lt;?php
foreach ($this-&gt;notice as $n) {
    echo '&lt;div class="error"&gt;'.$n . '&lt;/div&gt;';
}
echo "&lt;form method='".$this-&gt;form-&gt;getMethod()."' enctype='application/x-www-form-urlencoded' &gt;";

// Container with tabs
?&gt;
  &lt;div id="example" class="flora"&gt;
            &lt;ul&gt;
                &lt;li&gt;&lt;a href="#page-1"&gt;&lt;span&gt;Vstupní evidence&lt;/span&gt;&lt;/a&gt;&lt;/li&gt;
                &lt;li&gt;&lt;a href="#page-2"&gt;&lt;span&gt;Příprava rozpočtu&lt;/span&gt;&lt;/a&gt;&lt;/li&gt;
                &lt;li&gt;&lt;a href="#page-3"&gt;&lt;span&gt;Podání projektu&lt;/span&gt;&lt;/a&gt;&lt;/li&gt;
                &lt;li&gt;&lt;a href="#page-4"&gt;&lt;span&gt;Realizace projektu&lt;/span&gt;&lt;/a&gt;&lt;/li&gt;
            &lt;/ul&gt;
            &lt;div id="page-1"&gt;
            &lt;?php echo $this-&gt;form-&gt;getSubform('page1'); ?&gt;
            &lt;/div&gt;
            &lt;div id="page-2"&gt;
            &lt;?php
                  echo "&lt;div style='padding:1em;'&gt;";
                  include("../application/views/scripts/index/rozpocet.phtml");
                  echo "&lt;/div&gt;";
            ?&gt;
            &lt;/div&gt;
            &lt;div id="page-3"&gt;
            &lt;?php echo $this-&gt;form-&gt;getSubform('page2'); ?&gt;
            &lt;/div&gt;
            &lt;div id="page-4"&gt;
            &lt;?php echo $this-&gt;form-&gt;getSubform('page3'); ?&gt;
            &lt;/div&gt;
        &lt;/div&gt;
&lt;?php
echo $this-&gt;form-&gt;submit;
echo "&lt;/form&gt;";

?&gt;
</pre>
 

Po úpravě stylu to vypadá jinak, ale je to funkční jak chci a přesně kopíruje rozměry formuláře. 

<a href="http://blog.prskavec.net/wp-content/uploads/2008/09/image2.png"></a>

Přijde mi, že v Dojo je spousta jednoduchých věcí jako jsou třeba ty taby zatím celkem nedotažené. Co si myslíte o té integraci ZF s Dojo?
