---
title: "在微信小程序中获取全局对象，并对他进行 polyfill"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - 微信小程序
  - lodash
  - polyfill
toc: true
toc_label: "目录"
toc_icon: "cog"
---

微信小程序中的全局对象和其他环境中有所不同，通过 `global` 获取到的全局对象和 `V8 JavaScript` 引擎差距较大，很多时候需要兼容。

<!--more-->

## 问题
通过 `yarn add lodash.debounce` 添加到原生微信小程序项目中的防抖代码无法直接运行。

![TypeError](https://i.loli.net/2021/05/13/UIYDCo3Q1BWS4Gv.png)

```js
// => TypeError: Cannot read property 'now' of undefined
```

这是因为，`lodash` 获取全局对象的方式在微信小程序中不适用

```js
/** Detect free variable `global` from Node.js. */
var freeGlobal = typeof global == 'object' && global && global.Object === Object && global;
// => global.Object is undefined 

/** Detect free variable `self`. */
var freeSelf = typeof self == 'object' && self && self.Object === Object && self;
// => freeSelf is undefined 

/** Used as a reference to the global object. */
var root = freeGlobal || freeSelf || Function('return this')();
// => root => {}

var now = function() {
  return root.Date.now();
};
// => root.Date is undefined
```

## 解决方案

```js
// lodash-polyfill.js
var g = typeof window !== 'undefined' &&
window.Math === Math ? window : typeof global === 'object' ? global : this

if (!g.Object) {
  g.Object = Object
}

if (!g.Date) {
  g.Date = Date
}
```

```js
// app.js
require('./lodash-polyfill.js')
```

这样，就可以开开心心地在项目中使用 `lodash.debounce` 啦(\*^▽^\*)。
