---
title: "在使用 Babel 编译代码时，设置路径的自定义别名 alias"
excerpt_separator: "<!--more-->"
categories:
  - GitHub
tags:
  - Babel
toc: true
toc_label: "目录"
toc_icon: "cog"
---

一个用于在使用`Babel`编译代码时为模块添加新的解析器的`Babel`插件。这个插件允许你添加包含模块的新“根”目录。它还允许你为目录、特定文件甚至其他`npm`模块设置自定义别名。

<!--more-->

## 描述
```bash
// Use this:
import MyUtilFn from 'utils/MyUtilFn';
// Instead of that:
import MyUtilFn from '../../../../utils/MyUtilFn';

// And it also work with require calls
// Use this:
const MyUtilFn = require('utils/MyUtilFn');
// Instead of that:
const MyUtilFn = require('../../../../utils/MyUtilFn');
```

## Star
![Star](https://i.loli.net/2021/05/08/W3jbYpB7svhImwr.png)

## 参考
> [tleunen/babel-plugin-module-resolver](https://github.com/tleunen/babel-plugin-module-resolver)
