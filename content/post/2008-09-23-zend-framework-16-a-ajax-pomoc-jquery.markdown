---
date: 2008-09-23T00:00:00Z
published: true
status: publish
tags:
- jquery
- zend-framework
title: Zend Framework 1.6 a ajax pomocí jQuery
type: post
url: /2008/09/23/zend-framework-16-a-ajax-pomoc-jquery/
---

Jak jsem psal v <a href="http://blog.prskavec.net/?p=163">Zend Framework 1.6 a moje zkušenosti s Dojo TabContainer</a>, nakonec jsem použil jQuery. Ve formuláři se dají měnit některá data, které jsou závislá na dalších, které automaticky předvyplňuji a na to jsem použil při změnách ajax. Docela mě potěšilo jak jednoduše a pěkně se to dá udělat pomocí <a href="http://jquery.com/">jQuery</a>.
<pre name='code' class="php">&lt;script&gt;
$(document).ready(function(){
     $("#example &gt; ul").tabs();
     $('#page1-deadline_vyzva').datepicker();
     $('#page3-datum_zahajeni').datepicker();
     $('#page3-datum_ukonceni').datepicker();

      $('#page1-editor').change(function(){
             $.ajax({
                        type: "POST",
                        url: "&lt;?php echo $this-&gt;baseUrl();?&gt;/user/info/idcvut/" + $(this).val(),
                        success: function(xml){
					if ($.browser.msie)
					{
						xmlDoc=new ActiveXObject("Microsoft.XMLDOM");
						xmlDoc.async="false";
						xmlDoc.loadXML(xml);
						$("#page1-navrhovatel_jmeno")[0].value = xmlDoc.getElementsByTagName("name")[0].childNodes[0].nodeValue;
						$("#page1-navrhovatel_email")[0].value = xmlDoc.getElementsByTagName("email")[0].childNodes[0].nodeValue;
						$("#page1-navrhovatel_telefon")[0].value = xmlDoc.getElementsByTagName("phone")[0].childNodes[0].nodeValue;
						$("#page1-navrhovatel_pracoviste")[0].value = xmlDoc.getElementsByTagName("dept")[0].childNodes[0].nodeValue;
					}
					else
					{
					$("#page1-navrhovatel_jmeno")[0].value = $("name", xml).text();
					$("#page1-navrhovatel_email")[0].value = $("email", xml).text();
					$("#page1-navrhovatel_telefon")[0].value = $("phone", xml).text();
					$("#page1-navrhovatel_pracoviste")[0].value = $("dept", xml).text();
					}
                        }
             }); // end ajax
      }); // end change function
}); // end document ready
&lt;/script&gt;
...</pre>
Původní skript jsem rošířil o několik dalších funkcí, přidal jsem datapickery pro datumy a potom ten ajax. Nejprve pomocí vybere prvek a přidáme mu attribut onChange.
<pre name='code' class="javascript">$('#page1-editor').change(function(){ … });</pre>
Do této funkce jsem vložil volání ajaxu. Je to jednoduché a dobře pochopitelné. Type je typ GET nebo POST, v url je URL Controlleru, který vrací XML s potřebnými daty. Pomocí $(this.val) si šáhnu na vyplněnou hodnotu v políčku na které jsem aplikoval onChange(). Success vrací data, které si jednoduše parsuji přes $("name", xml).text(), kde name je &lt;name&gt;&lt;/name&gt; z xml dat, které posílá php.

Je to jednoduché a moc pěkný kód, který najde uplatnění. Dobré je, že není potřeba do formuláře přidávat do atributů funkci onchange, jak jsem to musel dělat, když jsem nepoužívat jQuery.

Ještě přidám část kódu z controlleru:
<pre name='code' class="php">…
function infoAction()
{
$this-&gt;_helper-&gt;layout-&gt;disableLayout();
$this-&gt;_helper-&gt;getHelper('ViewRenderer')-&gt;setNoRender();
$idcvut = (int)$this-&gt;_request-&gt;getParam('idcvut');
//echo $idcvut;
$ssu = new Usermap($this-&gt;config);
// make xml
//Zend_Debug($i);
$i=$ssu-&gt;getUsermapInfoByID($idcvut);
$xml = '&lt;user&gt;&lt;name&gt;'.$i[‘cn’].'&lt;/name&gt;&lt;email&gt;'.$i['email'].'&lt;/email&gt;&lt;phone&gt;'.$i[‘phone’].'&lt;/phone&gt;&lt;dept&gt;'.$i[‘department’].'&lt;/dept&gt;&lt;/user&gt;';
echo $xml;
}
…</pre>
Snad to někomu pomůže, moc příkladů na Zend a jQuery ajax jsem nikde nenašel.
