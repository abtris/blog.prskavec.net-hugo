---
date: 2008-04-28T00:00:00Z
published: true
status: publish
tags:
- scm
- svn
- windows
title: Subversion a hook skripty pod Windows
type: post
url: /2008/04/28/subversion-a-hook-skripty-pod-windows/
---

Pokud pracujete s TSVN nebo přímo s repozitory pod Windows časem přijdete na to, že potřebujete občas nějakou operaci před commitem nebo commitem k tomu slouží hook skripty. Hook je program, který je spuštěn nějakým triggrem, každé repozitory obsahuje předdefinované skripty. Nutnou podmínkou je mít samozřejmě nainstalovaný i Subversion ne jenom TSVN.
Adresář repozitory: <code>\path-to-repozitory\project-name</code>
<code>[conf] [dav] [hooks] [locks] format README.TXT</code>
v adresáři hooks jsou skripty:
<code>
post-commit.tmpl
post-lock.tmpl
post-revprop-change.tmpl
post-unlock.tmpl
pre-commit.tmpl
pre-lock.tmpl
pre-revprop-change.tmpl
pre-unlock.tmpl
start-commit.tmpl
</code>
tyto skripty vám určují možné spouštěče, kdy se který skript vykoná. Já osobně používám jen post-commit a to tak, že jsem vytvořil post-commit.bat, který obsahuje tento kód, který přegeneruje changelog.
<code>
"c:\Program Files\Subversion\bin\svn.exe" log -v --xml svn://localhost/rep_testing/start_page/trunk &gt;c:\rootwww\wc_testing\startpage_changelog.xml
</code>
ještě mě napadlo, že můžete např. udělat automatický export pro deploy na jiný stroj:
<code>"c:\Program Files\Subversion\bin\svn.exe" export --force file:///rootwww/rep_cvut/akce/trunk c:/tmp/export/akce</code>
a k němu vygenerovat příslušný textový changelog:
<code>"c:\Program Files\Subversion\bin\svn.exe" log file:///rootwww/rep_cvut/akce/trunk &gt;c:/tmp/export/akce/changelog.txt</code>
Ty skripty samozřejmě mohou dělat mnohem více.

Generovat můžete diff soubory pro jednotlivé revize a někam je ukládat:
<code>svn diff path-to-working-copy -c revision_number --no-diff-deleted &gt;diff_revision_number.txt</code>
Pokud potřebujete např. provést update a potom poslat mail doporučuju článek <a href="http://blog.pengoworks.com/index.cfm/2008/2/5/SVN-postcommit-for-Windows">SVN post-commit for Windows</a>, kde autor ve stejné době jako já řešil něco obdobného i když z jiného důvodu.
