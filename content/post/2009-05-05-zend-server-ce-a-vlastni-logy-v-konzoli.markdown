---
date: 2009-05-05T00:00:00Z
tags:
- php
- zend-framework
- zend-server
title: Zend Server CE a vlastni logy v konzoli
type: post
url: /2009/05/05/zend-server-ce-a-vlastni-logy-v-konzoli/
---

<h2>Zend Server 4.0.2</h2>
<p>
Zend aktualizoval <a href="https://static.zend.com/topics/Zend-Server-Release-Notes-V4.0.2.txt">Zend Server na verzi 4.0.2</a>, přidána hlavně podpora pro <a href="https://devzone.zend.com/article/4524-Zend-Framework-1.8.0-Released">ZF 1.8</a> a další drobné změny. Jen mi z repozitory pro Ubuntu nefunguje Zend_Tool, doufám, že tuto drobnost brzo opraví zatím ji stejně s Zend Studiem moc nevyužiji.
</p>

<p><em>Update 6.5.2009</em> po <a href="https://forums.zend.com/viewtopic.php?f=44&amp;t=615">mém upozornění</a> dnes Zend provedl update ZF 1.8 v repozitory pro Zend Server CE a už to funguje, soubor najdete v <code>/usr/local/zend/share/ZendFramework/bin/zf.sh</code>. Doporučuji si udělat symlink nebo přidat adresář do $PATH.</p>

<h2>Zend Debbuger a PHPUnit</h2>
<p>
Jen mi trochu vadí, že kvůli code coverage a dalším možnostem co má PHPUnit s Xdebugem jsem nucen vypnout Zend Debugger v Zend Serveru a dát si tam Xdebug. Zend Debugger má sice tyto možnosti k dispozici přes Zend Studio, ale pokud to voláte z Antu tak jsem nepřišel na to jak rozchodit PHPUnit a Zend Debugger dohromady.
</p>
<h2>Logy</h2>
<p>
Pokud chcete na konzoli Zend Serveru vidět další logy. Konfigurační soubor je <code>/usr/local/zend/gui/application/data/logfiles.xml</code>. Logy je dobré směrovat do <code>/usr/local/zend/var/log/</code>. Pomocí symlinků si přidejte do tohoto adresáře logy, které jsou uloženy v <code>/var/log/apache2/</code>.
</p>
<pre>



		PHP Error Log

                error_log


		Server Error Log
		/usr/local/zend/var/log/error.log



		Server Access Log
		/usr/local/zend/var/log/access.log



                Server Workspace Access Log
                /usr/local/zend/var/log/access-workspace.log



                Server Workspace Error Log
                /usr/local/zend/var/log/error-workspace.log




</pre>
<p>
Pokud budete mít problémy s právy nazapomeňte přidat uživateli <code>zend</code> práva na čtení potřebných logů a práva r+x na adresář <code>/var/log/apache2</code>, kde je máte uložené.
</p>
<p>
Potom by jste v konzoli měli vidět toto.
</p>
<p>
<a href="https://blog.prskavec.net/wp-content/uploads/2009/05/zend-server_1241527431260.png"><img src="https://blog.prskavec.net/wp-content/uploads/2009/05/zend-server_1241527431260-300x168.png" alt="zend-server_1241527431260" class="aligncenter size-medium wp-image-501" /></a>
</p>
