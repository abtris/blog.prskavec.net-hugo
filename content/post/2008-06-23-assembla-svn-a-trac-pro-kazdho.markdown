---
date: 2008-06-23T00:00:00Z
published: true
status: publish
tags:
- git
- svn
- scm
title: 'Assembla: SVN a Trac pro každého'
type: post
url: /2008/06/23/assembla-svn-a-trac-pro-kazdho/
---

Pokud hledáte free hosting pro svůj projekt a není to opensource na které se hodí <a href="http://sourceforge.net/">Sourceforge</a> nebo <a href="http://code.google.com/">Google Code</a>, tak je několik alternativ např. <a href="https://opensvn.csie.org/">OpenSVN</a> a <a href="http://www.assembla.com/">Assembla</a>. Já jsem si vybral Assembla, ve free variantě vám umožní hostovat projekty do 500MB což mě zatím stačí a pro ty na kterých aktuálně dělám také bude stačit. Tak není nic jednodušího než si založit projekt a potom vyexportovat repozitory u sebe ze SVN.
<pre>svnadmin dump path_to_rep &gt;file.dump</pre>
Pokud máte toho víc, možná užijete tento <code>export.bat</code>, aby to bylo jednodušší pro celé repository, také se to hodí na zálohování.
<pre>dir path_to_rep\rep_dir /b /O /AD &gt;dir1.txt

FOR /F  %%M IN (dir1.txt) DO (
  ECHO  %%M
  svnadmin dump path_to_rep/rep_dir/%%M  --incremental &gt;%%M.dmp
  zip -m -9 -g %%M.dmp.zip %%M.dmp
)</pre>
Jednotlivé dump soubory potom naimportujete do Assembla. Hlavní výhodou je to, že je k dispozici celý systém třeba jen pro vás a nejste nikde k nalezení pokud zrovna nechcete. Samozřejmě těžko můžu dát ruku do ohně za spolehlivost a důvěryhodnost firmy, která to provozuje, ale jejich filozifie se mi líbí. Samozřejmě bych uvítal třeba firmu, která by to poskytovala u nás, ale myslím si že to už dnes není potřeba. Na <a title="http://www.assembla.com/tour" href="http://www.assembla.com/tour">http://www.assembla.com/tour</a> najdete porovnání Free varianty s těmi komerčními. Commercial mi přijde při ceně kolem $150 (cca 2400 Kč) za rok celkem dobrá, myslím že provoz a údržba vlastního stroje na podobný účel musí přijít i malého soukromníka na mnohem větší peníze.

<h4>Aktualizace 17.12.2008</h4>
Už firma svoji politiku změnila a nemůžete si svůj projekt mít jako privátní zdarma, je to škoda, použitelné to ale zůstává pro otevřené projekty
