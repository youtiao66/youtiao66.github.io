---
title: "通过 husky + lint-staged 实现在 Git 提交前进行文件美化和代码校验"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - Node.js
toc: true
toc_label: "目录"
toc_icon: "cog"
---

许多人非常倾向于在 `Git` 提交之前进行 prettier 和 eslint, 这样能够保证提交后的文件经过美化和校验的。不需要通过命令行执行，也不需要等待 CI 工具执行完成，还不需要通过编辑器处理，更不需要通过热加载工具操作。

<!--more-->

通过 `husky` + `lint-staged` 实现在 `Git` 提交前进行 `prettier` 和 `eslint`

## 环境

```
node: v12.22.1
npm:  v6.14.12
yarn: v1.15.2
```

**依赖**

```
{
  "devDependencies": {
    "husky": "^7.0.0",
    "lint-staged": "^11.2.0",
    "prettier": "^2.4.1"
  },
  "scripts": {
    "prepare": "husky install"
  },
  "lint-staged": {
    "src/**/*.css": [
      "prettier --write"
    ]
  }
}
```

## 使用

> [husky - 简化原生 Git hooks](https://github.com/typicode/husky)

`husky-init` 是一个快速初始化 `husky` 项目的一次性命令

```shell
npx husky-init && yarn
```

它将自动配置 `husky`, 修改 `package.json` 和创建一个可以编辑的 `pre-commit hook` 示例, 该示例默认为 `npm test`

> [lint-staged - 对 Git 暂存文件执行校验器](https://github.com/okonet/lint-staged)

```shell
yarn add --dev lint-staged
```

把 `.husky/pre-commit` 中的 `npm test` 修改为 `npx lint-staged`

```shell
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx lint-staged
```

配置 `package.json`

```
{
  "lint-staged": {
    "src/**/*.css": [
      "prettier --write"
    ]
  }
}
```

> [Prettier - 代码美化](https://github.com/prettier/prettier)

推荐配置 `.prettierrc.json`

```json
{
  "printWidth": 120,
  "singleQuote": true,
  "semi": false,
  "trailingComma": "none"
}
```

> [ESLint - 校验并修复 JavaScript 代码](https://github.com/eslint/eslint)

类同 `Prettier`


