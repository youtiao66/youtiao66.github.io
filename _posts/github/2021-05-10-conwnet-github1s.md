---
title: "github1s: 1秒极速用 VS Code 阅读 GitHub 源代码"
excerpt_separator: "<!--more-->"
categories:
  - GitHub
tags:
  - VS Code
toc: true
toc_label: "目录"
toc_icon: "cog"
---

只需要在任何你想要阅读代码库浏览器地址栏中的 `github` 后面添加 `1s` 然后点击`回车 Enter`，1秒极速用 `VS Code` 阅读 `GitHub` 源代码。

<!--more-->

## 使用
例如，你可以尝试访问 `VS Code` 源代码仓库：
[https://github1s.com/microsoft/vscode](https://github1s.com/microsoft/vscode)

![github1s](https://i.loli.net/2021/05/10/XU7eaBT4fJimGRP.png)

将以下代码片段保存为浏览器书签，您可以使用它在`github.com`和`github1s.com`之间快速切换

```
javascript: window.location.href = window.location.href.replace(/github(1s)?.com/, function(match, p1) { return p1 ? 'github.com' : 'github1s.com' })
```

## `Star: 18.1k`

## 参考
> [conwnet/github1s](https://github.com/conwnet/github1s)
