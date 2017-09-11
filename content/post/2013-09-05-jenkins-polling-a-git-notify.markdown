---
categories: [jenkins]
comments: true
date: 2013-09-05T00:00:00Z
title: Jenkins polling a git-notify
url: /2013/09/05/jenkins-polling-a-git-notify/
---

Minulý rok jsem psal o tom, že [polling v Jenkinsu je zlo](http://blog.prskavec.net/2012/06/jenkins-scm-polling-je-zlo/). To stále platí, ale i když máte tento přístup nemusí to stačit.

<!--more-->

Tady je příklad hooku pro gitolite, který používáme na některých repository.

{% gist 5625369 %}

Občas je potřeba notifikovat jen větev, která se změní, aby to nepustilo zbytečně joby, kde změny neproběhli.

Nejdůležitější části, jsou detekce větve, escape lomítka ve jménech větve.

    branch=$(git rev-parse --symbolic --abbrev-ref $1)
    escaped_branch=$(echo $branch | sed s:/:%2F:g)

potom vlastní git-notify volání curlem.

    REPOSITORY_BASENAME=$(basename "$PWD")
    curl "http://jenkins.firma.cz/git/notifyCommit?url=ssh://git@git.firma.cz/$REPOSITORY_BASENAME&branch=$escaped_branch"


Snad to bude někomu užitečné také, pokud si chcete o Jenkinsu popovídat [více, dejte vědět](http://blog.prskavec.net/skoleni/).