---
title: "微信小程序：Can't find variable: Proxy; [Component] Data Observer Error"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - 微信小程序
toc: true
toc_label: "目录"
toc_icon: "cog"
---

微信小程序：Can't find variable: Proxy; [Component] Data Observer Error. 对 Proxy 进行 polyfill.

<!--more-->

> [miniprogram-computed](https://github.com/wechat-miniprogram/computed)

## 问题

```js
// => Error: Can't find variable: Proxy; [Component] Data Observer Error
```

miniprogram-computed@4.0.3

```js
const propDef = new Proxy(data, handler); // error => Proxy 对象在部分低版本客户端中无法使用
```
## 微信小程序 `JavaScript` 支持情况
> [微信小程序 `JavaScript` 支持情况](https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/js-support.html)
### 无法被 Polyfill 的 API
以下 API 在部分低版本客户端中无法使用，请注意尽量避免使用

- `Proxy` 对象

## `miniprogram-computed` 版本之间主要区别的比较。

| 项目 | ^1.0.0 | ^2.0.0 | ^3.0.0 和 ^4.0.0 |
| ---- | ------ | ------ | ------ |
| 支持的基础库最低版本 | 2.2.3 | 2.6.1 | 2.6.1 |
| 支持 `watch` 定义段 | 否 | 是 | 是 |
| 性能 | 相对较差 | 相对较好 | 相对较好 |
| 支持 `mobx-miniprogram` 扩展库 | 不支持 | 不支持 | 支持 |
| 支持自定义 `behavior` 数据字段 / 初始化视图渲染 | 不支持 / 不支持 | 支持 / 不支持 | 支持 / 支持 |

## 解决方案

### 1、回退 `miniprogram-computed` 到 `v2.1.1`
**缺点：**
- **不支持**自定义 `behavior` 初始化视图渲染
  - 通过 `behavior` 声明的字段不支持视图渲染，可以通过 `this.data` 获取到最新数据

### 2、通过 `Object.defineProperty` 对 `Proxy` 进行 `polyfill`
**缺点：**

- **不支持**监听数组的变化

> [GoogleChrome/proxy-polyfill](https://github.com/GoogleChrome/proxy-polyfill/)

### 3、采用其他方案实现 `computed`，例如结合 `Vue` 运行时开发组件

### 4、放弃部分不支持 `Proxy` 对象低版本客户端
