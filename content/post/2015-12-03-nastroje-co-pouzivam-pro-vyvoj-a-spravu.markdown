---
categories: []
comments: true
date: 2015-12-03T00:00:00Z
title: Nástroje co používám pro vývoj a správu
url: /2015/12/03/nastroje-co-pouzivam-pro-vyvoj-a-spravu/
---

Sublime Text, Terminal

## Sublime Text jako vývojové prostředí

[Sublime](http://www.sublimetext.com/) používá dost lidí, u nás v Apiary je to rozdělné mezi Emacs, Vim, Sublime, Atom a Webstorm. Řekl bych, že Sublime je asi nejvíc používaný, ale to se také mění. Já ho preferuju hlavně pro jeho rychlost startu.

<a href="{{ root_url }}/images/sublime.png"><img class="center" src="{{ root_url }}/images/sublime.png" alt="Sublime Text" /></a>

<!--more-->

Používám tyto balíčky:

- [Package Control](https://packagecontrol.io/) – balíčkovací systém
- [API Blueprint](https://packagecontrol.io/packages/API%20Blueprint) - podpora pro práci s jazykem [API Blueprint
- [Alignment](https://packagecontrol.io/packages/Alignment) - zarovnávání na různé znaky, obvykle např. require na začátku js souborů
- [All Autocomplete](https://packagecontrol.io/packages/All%20Autocomplete)
- [Better CoffeeScript](https://packagecontrol.io/packages/Better%20CoffeeScript) - podpora pro CoffeeScript
- [Color Highlighter](https://packagecontrol.io/packages/Color%20Highlighter) - zobrazí barvu v CSS
- [FileDiffs](https://packagecontrol.io/packages/FileDiffs) - porovnávání souborů přímo v Sublime, podpora clipboard, velmi užitečné
- [Focus File on Sidebar](https://packagecontrol.io/packages/Focus%20File%20on%20Sidebar)
- [GhostText](https://packagecontrol.io/packages/GhostText) - s pluginem v Chrome je to combo pro editaci textarea na webu přímo v Sublime
- [Git](https://packagecontrol.io/packages/Git)
- [Hayaku](https://packagecontrol.io/packages/Hayaku%20-%20tools%20for%20writing%20CSS%20faster) - jednoduší psaní CSS
- [JSCS Formatter](https://packagecontrol.io/packages/JSCS-Formatter) - lepší formátování pro Javascript
- [Pretty JSON](https://packagecontrol.io/packages/Pretty%20JSON) - formátovaní JSON
- [Markdown Preview](https://packagecontrol.io/packages/Markdown%20Preview) - podpora pro Markdown
- [SidebarEnhancements](https://packagecontrol.io/packages/SideBarEnhancements)
- [Stylus](https://packagecontrol.io/packages/Stylus) - podpora pro jazyk
- [Terraform](https://packagecontrol.io/packages/Terraform) - podpora pro jazyk
- [TrailingSpaces](https://packagecontrol.io/packages/TrailingSpaces) - příkazy pro mazání, zobrazení whitespace z dokumentů
- [GoSublime](https://packagecontrol.io/packages/GoSublime) - podpora pro jazyk Go
- [GoImports](https://packagecontrol.io/packages/GoImports) - podpora pro zakomentování importů v Go
- [Pretty YAML](https://packagecontrol.io/packages/Pretty%20YAML) - formátovaní YML
- [AutoSpell](https://packagecontrol.io/packages/AutoSpell) - kontrola překlepů v angličtině
- [Docker Based Build Systems](https://packagecontrol.io/packages/Docker%20Based%20Build%20Systems) - pouštění kódu v dockeru pro různé jazyky
- [Dockerfile Syntax Highlighting](https://packagecontrol.io/packages/Dockerfile%20Syntax%20Highlighting) - zvýrazňování syntaxe pro Dockerfile
- [Seti UI](https://packagecontrol.io/packages/Seti_UI) - vzhled který používám

## Terminal

Možná se divíte, že terminál uvádím, ale i tady lze nastavit tisíce věcí.

- [iTerm2](https://www.iterm2.com/)
- [oh-my-zsh](http://ohmyz.sh/)
- [ack](http://beyondgrep.com/)
- [dotfiles](https://dotfiles.github.io/)

Pokud používáte pro programování bash doporučuji na Macu, hlavně upgradovat přes homebrew na verzi 4, která bohužel stále není v základu Mac OS X.

Mých top 10 příkazů v terminalu. Dostal se tam jen 1 alias a to `gco -  git checkout`.

```
     1  2015  20.152%    git
     2  834   8.34083%   heroku
     3  734   7.34073%   gco
     4  499   4.9905%    cd
     5  465   4.65047%   cat
     6  451   4.51045%   docker
     7  317   3.17032%   npm
     8  264   2.64026%   ack
     9  252   2.52025%   curl
    10  157   1.57016%   brew
```

Pokud nemáte vlastní dotfiles, doporučuji si je vytvořit a uchovávat si konfiguraci pro svůj terminál.


