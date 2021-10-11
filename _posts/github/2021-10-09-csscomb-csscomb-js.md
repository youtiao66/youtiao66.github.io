---
title: "CSScomb: 规范 CSS 属性顺序"
excerpt_separator: "<!--more-->"
categories:
  - GitHub
tags:
  - Node.js
toc: true
toc_label: "目录"
toc_icon: "cog"
---

`CSScomb` 是一款 `CSS` 代码样式格式化工具，你可以轻松地通过自定义配置让你的样式表更加美化和一致。它的主要特点是以指定顺序给 **CSS 属性排序**

<!--more-->

## 安装

```
npm install csscomb --save-dev
```

## 使用

```shell
# 命令行
csscomb assets/css
# git bash
csscomb --tty-mode assets/css
```

## 配置

新建 `.csscomb.json` 配置文件并放置于项目根目录

该项目中有[多个预定义的配置](https://github.com/csscomb/csscomb.js/tree/master/config)可供选择：

- csscomb (默认配置)
- zen
- yandex (推荐)

推荐 `yandex` 配置，该配置仅处理属性顺序。 CSS 样式美化另外通过 `Prettier` 实现

### 自定义语法映射

默认情况下 CSScomb 仅通过对应的语法处理 `*.css`, `*.less`, `*.sass` and `*.scss` 文件。
你可以通过 `syntax` 配置自定义语法映射。比如通过 css 语法处理 `*.wxss` 文件。

```json
{
  "syntax": {
    ".wxss": "css"
  }
}
```

## lint-staged + CSScomb

通过 `husky + lint-staged + CSScomb` 实现在 Git 提交前进行 CSS 属性排序

> 其他细节请参考我的另一篇博客[《通过 husky + lint-staged 实现在 Git 提交前进行文件美化和代码校验》](https://youtiao66.github.io/blog/husky-lint-staged-prettier-eslint/)

```json
{
  "lint-staged": {
    "src/**/*.wxss": [
      "csscomb --tty-mode",
      "prettier --parser css --write"
    ]
  }
}
```

**注意：lint-staged + CSScomb 时，必须以 TTY 模式运行命令**

```js
// CSScomb 源代码
if (options['tty-mode'] || process.stdin.isTTY) {
  // 处理文件
  processFiles(options._);
} else {
  // 处理命令行标准输入输出
  processSTDIN();
}
```

**代码解析：**

- `options['tty-mode'] => --tty-mode`
- 命令行执行情况下，尽管未指定 `--tty-mode`, `process.stdin.isTTY` 仍然为 `true`
- `lint-staged + CSScomb` 情况下， `process.stdin.isTTY === undefined`, 未指定 `--tty-mode` 会错误地执行 `processSTDIN`
- 在 `Git Bash` 中 `process.stdin.isTTY === undefined`, 同上
- 所以配置中必须显示地指定 TTY 模式，否则会导致 CSScomb 任务一直在处理中，没有报错也无法中止（因为一直在等待标准输入）。

## 参考
> [CSScomb](https://github.com/csscomb/csscomb.js)
