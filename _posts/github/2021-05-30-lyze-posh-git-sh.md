---
title: "posh-git-sh: 在命令行提示符查看当前 `git` 仓库的状态"
excerpt_separator: "<!--more-->"
categories:
  - GitHub
tags:
  - Git
toc: true
toc_label: "目录"
toc_icon: "cog"
---

该脚本允许你在命令行提示符查看当前 `git` 仓库的状态。状态展示是通过 `Windows PowerShell module dahlbyk/posh-git` 复制过来的.

<!--more-->

![example](https://i.loli.net/2021/05/30/b4FpQRVyrTUP9Kh.png)

## 使用
1. 把这个文件拷贝到某处 (例如, `~/git-prompt.sh`)
2. 在你的 `~/.bashrc` 文件中添加下面这一行
```shell
source ~/git-prompt.sh
```
3. 如果你使用 `bash`, 你应该在你的 `PROMPT_COMMAND` 中执行 `__posh_git_ps1`
```shell
PROMPT_COMMAND='__posh_git_ps1 "\u@\h:\w " "\\\$ ";'$PROMPT_COMMAND
```

## 为了实现上图所示效果，需要在 `.bashrc` 文件中添加以下代码

```bash
# => 该方法可以持久化，也可以解决新标签不展示的问题
export PROMPT_COMMAND='__posh_git_ps1 "\\[\[\e[0;32m\]\u@\h \[\e[0;33m\]\w" " \[\e[1;34m\]\n\$\[\e[0m\] ";'$PROMPT_COMMAND
```

## `Star: 311`

## Unix
> [lyze/posh-git-sh](https://github.com/lyze/posh-git-sh)

## Windows
> ***PowerShell Gallery***安装方式如果下载失败或超时，建议通过***从源码方式安装***

> [dahlbyk/posh-git](https://github.com/dahlbyk/posh-git)

> [在 PowerShell 中使用 Git](https://git-scm.com/book/zh/v2/%E9%99%84%E5%BD%95-A%3A-%E5%9C%A8%E5%85%B6%E5%AE%83%E7%8E%AF%E5%A2%83%E4%B8%AD%E4%BD%BF%E7%94%A8-Git-Git-%E5%9C%A8-PowerShell-%E4%B8%AD%E4%BD%BF%E7%94%A8-Git)
