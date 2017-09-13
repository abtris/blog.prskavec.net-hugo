---
tags:
 - git
comments: true
date: 2014-01-06T00:00:00Z
title: Git a pre-commit hook pro kontrolu syntaxe
url: /2014/01/06/git-a-pre-commit-hook-pro-kontrolu-syntaxe-v-mnoha-jazycich/
---

Pokud pracujete s gitem nebo jiným verzovacím systémem, určitě jste se setkali s hooky. Pro kontrolu než provedete commit, který se jmenuje pre-commit a hodí se zejména pro kontrolu syntaxe. Já mám několik hooků, které kontrolují php, js, xml a ruby. Říkal jsem si, že by to chtělo je refactorovat a udělat z nich použitelný kód. 

## Ochtra

Naštěstí jsem to dělat nemusel, protože vznikl malý projekt [ochtra](https://github.com/kvz/ochtra)  (One Commit Hook To Rule All).

Tento projekt teď umí kontrolovat Ruby, JavaScript, Python, Bash, Dash, Go, PHP, XML, JSON, YAML. A co se mi zvláště líbí je, že autor pěkně popsal instalaci, která je potřeba, aby vám hook fungoval automaticky, když použijete git clone.

### Instalace

Potřebujete git 1.7, kde je podpora pro git templates.

    mkdir -p ~/.gittemplate/hooks
    curl https://raw.github.com/kvz/ochtra/master/pre-commit -o ~/.gittemplate/hooks/pre-commit \
     && chmod u+x $_
    git config --global init.templatedir '~/.gittemplate'

Instalace vytvoří adresář s template pro git, která se použije pokud dáte git clone, případne git init.

Git init potřebujete pokud už máte projekty někde vyclonované.

    cd my-project
    rm .git/hooks/pre-commit
    git init

Celý projekt má i testy a podpora je pro všechno co mě napadlo. Pokud něco někomu chybí tak se ozvěte nebude to problém přidat.

