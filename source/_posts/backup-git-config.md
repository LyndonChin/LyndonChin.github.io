---
title: 我的 git 配置
date: 2016-01-26 14:43:45
tags: Git
---

记录我的 git 配置文件。

<!--more-->

alias
---
<pre>
[alias]
    co = checkout
    br = branch
    ci = commit
    st = status
    last = log -1 HEAD
    unstage = reset HEAD --
    visual = !"gitk"
    lg1 = log --graph --abbrev-commit --decorate --date=relative --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
    lg2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all
    lg = !"git lg2"
</pre>

editor
---

```
git config --global core.editor "vim"
```
