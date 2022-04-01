---
title: "VSCode + Vue2 如何解决部分模块无法 Ctrl + 鼠标左键点击 跳转到定义的问题？"
categories:
  - Blog
tags:
  - VSCode
---

在使用 VSCode + Vue2 + Webpack + JavaScript 开发过程中，总会遇到一部分模块无法通过 *Ctrl + 鼠标左键点击* 跳转到文件定义的问题，那到底是为什么？怎么解决呢？

<!--more-->

## 开发环境

### 基本开发环境

- VSCode
- Vue2
- Webpack
- JavaScript

> VSCode 已安装 [Vetur][Vetur] 插件

### `Webpack` 路径别名已配置

```js
{
  ...
  resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src')
    }
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        include: [resolve('src')],
        use: 'vue-loader'
      },
      {
        test: /\.js$/,
        include: [
          resolve('src'),
          resolve('test'),
          resolve('node_modules/element-ui/packages/table/'),
          resolve('node_modules/element-ui/src/utils/')
        ],
        use: 'babel-loader?cacheDirectory=true'
      },
      ...
    ]
  },
  ...
}
```

### `jsconfig.json` 已配置

```json
{
  "compilerOptions": {
    "target": "es2017",
    "allowSyntheticDefaultImports": false,
    "baseUrl": "./",
    "paths": {
      "@/*": ["src/*"]
    }
  },
  "exclude": ["node_modules", "dist"],
  "include": ["src"]
}
```

### 基本现状

- 项目运行正常✅
- 编译一切正常✅



## 常见模块加载规则

以 `ES Module` 绝对路径为例：

1. 直接加载指定文件，例如 `/src/utils/example.js` 或 `@/utils/example.js`
2. 如果找不到确切文件，尝试添加扩展名 `.js`, `.vue`加载该文件。例如 `/src/utils/example` 或 `@/utils/example`
3. 如果仍然找不到确切文件，尝试加载该文件夹下的 `index.js`, `index.vue`。例如 `/src/utils` 或 `@/utils`



## 实际遇到的问题和解决办法

```js
// example.js

// => ✅ recommand 但是不可以通过 ctrl + 鼠标左键单机 跳转到定义
import utils from '@/utils'
// => 🚫 not recommand 但是可以
import utils from '@/utils/index'
```

以上示例 `example.js` 中，推荐第一种写法。但是不可以通过 `ctrl + 鼠标左键单机` 跳转到定义

```js
// example.vue

// => ✅ recommand 可以通过 ctrl + 鼠标左键单机 跳转到定义
import utils from '@/utils'
// => 🚫 not recommand
import utils from '@/utils/index'


// 争论的主要焦点，是否为推荐的写法暂无定论
// => 可以通过 ctrl + 鼠标左键单机 跳转到定义
import Navbar from '@/components/Navbar/index.vue'
// => 不可以
import Navbar from '@/components/Navbar'
```

以上示例 `example.vue` 中, `js` 模块推荐第一种写法。也可以通过 `ctrl + 鼠标左键单机` 跳转到定义。毫无争议

**以上示例 `example.vue` 中, `vue` 模块写法是争论的主要焦点**。

一部分人主张: `vue` 文件是 `js`, `json` 以外的其他文件，应当参考 `css` 或 `less` 等样式文件模块加载时指明文件扩展名。其中就包括 `Vetur` 插件的开发团队，所以 `@/components/Navbar` 暂时无法跳转到定义。

另一部分人希望: `vue` 文件也可以像 `js`, `json` 一样, 省略 `index.vue` 的写法后仍然可以直接跳转到文件定义。比如 `Webpack + vue-loader` 就实现了该写法的编译。不要担心，我们可以通过安装另一个 VSCode 插件解决这个问题，这个插件就是[《别名路径跳转》][vue-alias-skip]，它还可以解决上一个示例 `example.js` 中 `@/utils` 这样的推荐写法不可以通过 ctrl + 鼠标左键单机 跳转到定义的问题

> [VSCode 别名路径跳转插件][vue-alias-skip]

> [Vetur 推荐指定扩展名的 Issue][vetur-issuecomment]



[Vetur]: https://marketplace.visualstudio.com/items?itemName=octref.vetur
[vue-alias-skip]: https://marketplace.visualstudio.com/items?itemName=lihuiwang.vue-alias-skip
[vetur-issuecomment]: https://github.com/vuejs/vetur/issues/423#issuecomment-340235722
