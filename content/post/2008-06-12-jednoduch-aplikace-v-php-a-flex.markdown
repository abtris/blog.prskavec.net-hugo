---
date: 2008-06-12T00:00:00Z
tags:
- adobe
- adobe-air
- flex
- php
title: Jednoduchá aplikace v PHP a Flex
type: post
url: /2008/06/12/jednoduch-aplikace-v-php-a-flex/
---

Protože jsem byl na Adobe Air nedalo mi to a řekl jsem si že udělám jednoduchou aplikaci pro vyhledávání v našem univerzitním LDAPu. Stáhnul jsem si trial verzi Flex Builduru 3, to se ocení zvláště pokud chcete dát aplikaci nějaký design.  Výsledná aplikace vypadá takto, skládá se z několika prvků, textové pole (mx:TextInput), tlačítka (mx:Button), výběr (mx:ComboBox), ty jsou strčené do společného hboxu. Potom dole je umístěný datagrid (mx:DataGrid) pro zobrazování dat. <a href="https://blog.prskavec.net/wp-content/uploads/2008/06/image.png"><img style="border-top-width: 0px;border-left-width: 0px;border-bottom-width: 0px;margin: 5px;border-right-width: 0px" src="https://blog.prskavec.net/wp-content/uploads/2008/06/image-thumb.png" border="0" alt="image" width="404" height="314" /></a>
<h3>Tvorba aplikace</h3>
Vytvořte projekt: File -&gt; New -&gt; Flex project <a href="https://blog.prskavec.net/wp-content/uploads/2008/06/image1.png"><img style="border-top-width: 0px;border-left-width: 0px;border-bottom-width: 0px;border-right-width: 0px" src="https://blog.prskavec.net/wp-content/uploads/2008/06/image-thumb1.png" border="0" alt="image" width="625" height="558" /></a> Tady je vidět, že nejdůležitější je nastavit mx:HTTPService na komunikaci s naším PHP skriptem, který funguje normálně jako u webové aplikace nebo spíše u AJAXové aplikace. Data vracím v XML, aby se dobře zobrazovali pomocí datagridu, ale mohl by to být i obyčejný text.
<pre>   			{txtInput.text}
   			{department.text}</pre>
Tady je část mx:Script, která se stará o ošetření toho co přijde z PHP a zobrazuje to.
<pre>			import mx.rpc.events.ResultEvent;
			import mx.rpc.events.FaultEvent;
			import mx.collections.ArrayCollection;

			public function handleResultXML(event:ResultEvent):void {
				// the result object is our xml root
				// we get the row nodes of our result object
				// the data of the dataprovider populates the component
				myDataGrid.dataProvider = event.result.row;
			}
			// this function is called when we get an error from the server
			public function handleFault(event:FaultEvent):void {
					//txtArea.text = "Fault Response from HTTPService call:\n " + event.fault.toString();
			}</pre>
Vlastní PHP skript je jednoduchý, používá mojí knihovnu, kde zadáte část jména a vrátí celé jméno i s tituly a k tomu odpovídající telefon.
<pre>$debug=false;
/**
  * Flex SSU
  */
if (!$debug) $name=$_REQUEST['query'];
else $name='prskavec';
error_log("[".date('r')."] Query: ".$_REQUEST['query']." Department:".$_REQUEST['department']."\n",3,"./flex.log");

if ($_REQUEST['department']=="VIC")
$vic=true;
else
 $vic=false;

include "inc/config.inc.php";
include_once "inc/ldap_class.php";
$ldap=new Ldap($config);

print "";
print "";

foreach ($ldap-&gt;get_person($name,$vic) as $person)
{
  $phone=$ldap-&gt;ldap_phone($person['cvutphone']['0']);
   if (!empty($phone))
   {
    if ($debug) var_dump($person);
    print "";
    print "".$person['cn;lang-cs']['0']."";
	print "".$phone."";
    print "".$person['preferredemail']['0']."";
	print "";

    error_log("[".date('r')."] Result: ".$person['cn;lang-cs']['0']." | ".$phone." | ".$person['preferredemail']['0']."\n",3,"./flex.log");
    }
}
print "";</pre>
Pokud vám všechno funguje v Flex builderu, stačí dát Project -&gt; Export Release Build a vytvořit AIR soubor, který je nainstalovatelný jako desktopová aplikace.
<h3>Závěr</h3>
Přijde mi to rozhodně jednodušší než PHP-GTK. Jednoduchá aplikace, kterou používám denně, obdobných aplikací jsou dnes desítky a nejlepší mi přijde, že se dá udělat pěkné rozhraní k online službě v PHP a nedá to moc práce a na straně PHP není žádná práce navíc. Vlastní tvorba GUI dá zabrat podle mě všude.
