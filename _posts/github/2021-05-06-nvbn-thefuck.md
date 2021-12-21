---
title: "一个可以快速纠正上次控制台命令的神奇指令"
excerpt_separator: "<!--more-->"
categories:
  - GitHub
tags:
  - Tool
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

## 常见问题

```
c:\python3\lib\site-packages\win_unicode_console\__init__.py:31: RuntimeWarning: sys.stdin.encoding == 'utf-8', whereas sys.stdout.encoding == 'ascii', readline hook consumer may assume they are the same
  readline_hook.enable(use_pyreadline=use_pyreadline)
```

> [Shell aliases](https://github.com/nvbn/thefuck/wiki/Shell-aliases#powershell)

```
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xb2 in position 6: invalid start byte
```

- 添加全局环境变量`PYTHONIOENCODING=utf-8`
- 在`$PROFILE`中添加`[Console]::OutputEncoding = [System.Text.Encoding]::UTF8`

> [UnicodeDecodeError: 'utf-8' codec can't decode byte 0xb2 in position 6: invalid start byte](https://github.com/nvbn/thefuck/issues/673)

## 参考
> [nvbn/thefuck](https://github.com/nvbn/thefuck)
