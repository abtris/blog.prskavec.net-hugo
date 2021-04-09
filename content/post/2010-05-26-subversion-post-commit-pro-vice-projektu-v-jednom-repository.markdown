---
date: 2010-05-26T00:00:00Z
tags:
- svn
- scm
title: Subversion post-commit pro více projektů v jednom repository
type: post
url: /2010/05/26/subversion-post-commit-pro-vice-projektu-v-jednom-repository/
---

Pokud máte strukturu repository podobnou této:
<pre>/repository/project1/trunk
/repository/project1/branches
/repository/project1/tags
/repository/project2/trunk
/repository/project2/branches
/repository/project2/tags</pre>

a chtěli jste dělat nějakou akci pro jednotlivé projekty nebo jen pro některé je potřeba si trochu pohrát s post-commitem. Zde uvádím příklad na posílání mailu jen pro projekty, které začínají "php_".

<pre>#!/bin/sh
# POST-COMMIT HOOK
REPOS="$1"
REV="$2"
REPNAME="${1##*/}"
SVNLOOK="/usr/bin/svnlook"
AWK="/usr/bin/awk"
GREP="/bin/egrep"
ERR="/tmp/errors"

TEST=1

CHANGED=`$SVNLOOK changed  -r "$REV" "$REPOS" | $AWK '{print $2}' | $GREP ^php_`
for FILE in $CHANGED
do
	# echo $FILE &gt;&gt; $ERR
	TEST=0
done

if [ $TEST = 0 ]; then
	/srv/svn/tools/commit-email.pl "$REPOS" "$REV" adresat@email.com -h svn.hostname.cz -s "SVN: $REPNAME" &gt; /tmp/postcommit-log 2&gt;&amp;1
	# echo "Send mail" &gt;&gt; $ERR
	exit 0
fi
</pre>

Post commit je upraven tak, že pomocí svnlook si zjistí provedené změny a podle nich se zachová, tak se dá řídit celá logika akce, kterou chcete vykonat.

