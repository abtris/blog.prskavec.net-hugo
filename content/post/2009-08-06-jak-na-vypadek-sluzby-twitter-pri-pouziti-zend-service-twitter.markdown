---
date: 2009-08-06T00:00:00Z
published: true
status: publish
tags:
- php
- twitter
- zend framework
title: Jak na výpadek služby Twitter při použití Zend_Service_Twitter
type: post
url: /2009/08/06/jak-na-vypadek-sluzby-twitter-pri-pouziti-zend-service-twitter/
---

Během dneška (6.8.2009) byl výpadek služby Twitter a koukal jsem, že mi to položilo <a href="https://php-frameworks.net">php-frameworks.net</a> na kolena.

Jak jsem se zjišťovat co s tím a proč mi to hlásí:
<pre>
Zend_Http_Client_Adapter_Exception: Unable to Connect to tcp://twitter.com:80. Error #110: Connection timed out in /srv/lib/php/Zend/Http/Client/Adapter/Socket.php on line 213
</pre>

Tento kód nějak selhal a výpadek nastal dříve, asi to vypadá na nějakou chybu v Zendu, protože se Zend_Service_Twitter_Exception nevrátí i při výpadku spojení jak jsem očekával.

<pre>
try {
	$twitter = new Zend_Service_Twitter($config-&gt;twitter-&gt;username, $config-&gt;twitter-&gt;password);
} catch (Zend_Service_Twitter_Exception $e) {
	$this-&gt;logger-&gt;err("Exception caught importing twitter: {$e-&gt;getMessage()}\n");
}
</pre>

Řešení je otestovat vlastní připojení předem ke kterému jsem se nakonec uchýlil.

<pre>
		// Testing connect to twitter
		try {
		$client = new Zend_Http_Client('https://twitter.com', array(
			'maxredirects' =&gt; 0,
			'timeout'      =&gt; 5));
		$response = $client-&gt;request();
		} catch (Zend_Http_Client_Adapter_Exception $e) {
			$this-&gt;logger-&gt;err("Exception caught connect twitter: {$e-&gt;getMessage()}\n");
		}

		// if have $response try connect twitter
		if (isset($response)) {
			try {
				$twitter = new Zend_Service_Twitter($config-&gt;twitter-&gt;username, $config-&gt;twitter-&gt;password);
			} catch (Zend_Service_Twitter_Exception $e) {
				$this-&gt;logger-&gt;err("<a href="https://www.unlocalize.com/">Exception</a> caught importing twitter: {$e-&gt;getMessage()}\n");
			}
			if (isset($twitter)) {
			$response = $twitter-&gt;status-&gt;friendsTimeline(array("count" =&gt; $config-&gt;twitter-&gt;count));
			$this-&gt;view-&gt;twitter = $response;
			} else {
			$this-&gt;view-&gt;twitter = null;
			}
		}
</pre>

