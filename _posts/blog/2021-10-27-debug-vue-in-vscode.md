---
title: "在 VS Code 中调试 Vue"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - VS Code
toc: true
toc_label: "目录"
toc_icon: "cog"
---

万物皆可 VS Code, 作为一个前端攻城狮，让我们一起研究一下如何在 VS Code 中调试 Vue.

<!--more-->

## 环境

```shell
VS Code:   1.61.2
Chrome:    95.0.4638.54
@vue/cli:  4.5.0
```

## 准备
首先，通过 Vue 脚手架 `@vue/cli` 创建一个默认项目，并在如图所示 `HelloWorld.vue` 文件中添加代码和断点

![创建项目并添加断点](https://i.loli.net/2021/10/27/LF13BDG9Yw6tWZb.png)

## 在浏览器中展示源代码
在可以从 VS Code 调试你的 Vue 组件之前，你需要更新 webpack 配置以构建 source map。做了这件事之后，我们的调试器就有机会将一个被压缩的文件中的代码对应回其源文件相应的位置。这会确保你可以在一个应用中调试，即便你的资源已经被 webpack 优化过了也没关系。

如果你使用的是 `Vue CLI 3` 及以上，请设置并更新 vue.config.js 内的 devtool property：

```js
// vue.config.js
module.exports = {
  configureWebpack: {
    devtool: 'source-map'
  }
}
```

## 从 VS Code 启动应用

点击在侧边栏里的 `Run and Debug` 图标来到 Debug 视图，然后点击那个齿轮图标来配置一个 launch.json 的文件，选择 Chrome 环境。然后将生成的 launch.json 的内容替换成为相应的配置：

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "pwa-chrome",
      "request": "launch",
      "name": "vuejs: chrome",
      "url": "http://localhost:8080",
      "webRoot": "${workspaceFolder}/src",
      "breakOnLoad": true,
      "sourceMapPathOverrides": {
        "webpack:///src/*": "${webRoot}/*",
        // 2019年及其以后必须新增以下属性，具体参考下文链接
        "webpack:///./*.vue": "${webRoot}/*.vue"
      }
    }
  ]
}
```

> 注意：以上配置仅针对 `@vue/cli` 创建的应用程序，其他框架搭建的程序需要做对应的调整

> [sourceMapPathOverrides 配置参考](https://stackoverflow.com/questions/52648552/debugger-settings-for-chrome-in-vs-code-with-vue-js)

## 调试
在项目根目录通过命令行启动应用：

```
npm run serve
```

打开 Debug 视图，选择“vuejs：chrome”配置，然后按 F5 或点击那个绿色的 play 按钮

随着一个新的浏览器打开 http://localhost:8080，断点就应该自动命中了。

![命中断点](https://i.loli.net/2021/10/27/hJblwjCTMgpsZaY.png)

## 参考
> [在 VS Code 中调试](https://cn.vuejs.org/v2/cookbook/debugging-in-vscode.html)
