---
date: 2009-01-19T00:00:00Z
tags:
- linux
- svn
title: Export poslední revize ze Subversion
type: post
url: /2009/01/19/export-posledni-revize-ze-subversion/
---

Dneska za mnou přišel kolega, že mu vadí na SVN, že neumí exportovat poslední revizi s plnou cestou. Potřebuje to na server kde nemá shell aby mohl spustit patch, který si můžeme vygenerovat pomocí <a href="https://svnbook.red-bean.com/en/1.5/svn.ref.svn.c.diff.html"><code>svn diff</code></a>. A nechce všechny soubory jak to standardně dělá <a href="https://svnbook.red-bean.com/en/1.5/svn.ref.svn.c.export.html"><code>svn export</code></a>, ale jen ty které se změnili.

Trochu jsem se na to díval a myslím si, že řešení přímo jen pomocí SVN není, pokud někdo ví jak to udělat elegantně ať mi dá vědět. Já jsem na to napsal jednoduchý shell skript, který to řeší, třeba to bude někomu také ku prospěchu.
<pre name="code" class="bash">
EXPORTPATH=/tmp/testexport/
REPOS=file:///home/prskavecl/repos/project/
REPOSPATH=/home/prskavecl/repos/project/
REV="$( svnlook youngest $REPOSPATH )"

# function to list and export file by file
pathexport() # $1
{
mkdir -p $EXPORTPATH${2%/*}
svn export --force $1$2 $EXPORTPATH$2
}

# make export path
mkdir -p $EXPORTPATH
# list all changed files
for i in $( svnlook changed -r $REV $REPOSPATH ); do
if [ "${#i}" -gt "2" ]
then pathexport $REPOS $i
fi
done</pre>
Na začátku skriptu se nastaví proměnné s cestami. Před svnlook si skript zjistí poslední revizi a projde všechny soubory, které se v revizi změnili. K nim vytvoří příslušné adresáře a vyexportuje jednotlivé soubory. Neměl by být problém napsat obdobný skript i pro windows.

Více o exportu také najdete na <a href="https://svn.prskavec.net/">svn.prskavec.net</a>.
