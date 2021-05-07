---
title: "一个可以快速纠正上次控制台命令的神奇指令"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - GitHub
  - HelloGitHub
  - thefuck
toc: true
toc_label: "目录"
toc_icon: "cog"
---

The Fuck 是一个可以快速纠正上次控制台命令的神奇指令

<!--more-->

![gif with examples](https://i.loli.net/2021/05/06/vzXHCOJg8yRkeTd.gif)

## 例如
```bash
➜ git push
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin master


➜ fuck
git push --set-upstream origin master [enter/↑/↓/ctrl+c]
Counting objects: 9, done.
...
```

```bash
➜ git brnch
git: 'brnch' is not a git command. See 'git --help'.

Did you mean this?
    branch

➜ fuck
git branch [enter/↑/↓/ctrl+c]
* master
```

## Star
![star](https://i.loli.net/2021/05/06/6mvqHk7sQKJbwNF.png)

## 参考
> [nvbn/thefuck](https://github.com/nvbn/thefuck)
