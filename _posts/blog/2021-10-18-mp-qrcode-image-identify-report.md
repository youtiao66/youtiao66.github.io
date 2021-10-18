---
title: "微信小程序中二维码图片识别调研报告"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - 微信小程序
toc: true
toc_label: "目录"
toc_icon: "cog"
---

微信小程序中，到底能识别哪些种类二维码图片？基于此我们能够实现哪些业务？

<!--more-->

## 背景

实际业务场景中，我们常常会遇到二维码图片，更需要识别二维码图片来丰富产品的应用场景。那么在微信小程序原生应用中，到底能识别哪些种类二维码图片？基于此我们能够实现哪些业务？

## 环境

```shell
# Redmi K40
Android:   11
微信:      Version 8.0.15
微信小程序: SDKVersion 2.20.0

# iPhone 13
iOS:       15.0.2
微信:      Version 8.0.13
微信小程序: SDKVersion 2.19.6
```

## 长按图片

**基础功能**如下：

✅ 转发

✅ 保存图片

✅ 收藏

```html
<!-- wxml -->
<image src="qrocode-image.jpg" show-menu-by-longpress />
```

### 小程序码

| 类别 | 功能 |
| --- | --- |
| Android | 打开小程序 ✅ |
| iOS | 打开小程序 ✅ |

### 名片二维码

| 类别 | 功能 |
| --- | --- |
| Android | 打开名片 ✅ |
| iOS | 打开名片 ✅ |

### 群聊二维码

| 类别 | 功能 |
| --- | --- |
| Android | 前往群聊 ✅ |
| iOS | 前往群聊 ✅ |

### 支付二维码

| 类别 | 功能 |
| --- | --- |
| Android | 支付 ❌ |
| iOS | 支付 ❌ |

### 公众号二维码

| 类别 | 功能 |
| --- | --- |
| Android | 识别公众号 ❌ |
| iOS | 识别公众号 ❌ |

### 搜一搜

图片搜一搜，搜图片功能

| 类别 | 功能 |
| --- | --- |
| Android | 识别公众号 ❌ |
| iOS | 识别公众号 ❌ |

![前往图中包含的小程序](https://i.loli.net/2021/10/18/9y1Fd75Lntpl38K.jpg)

![打开对方的名片](https://i.loli.net/2021/10/18/qSefTZgwb2hyatc.jpg)

## 内嵌 H5 页面长按图片

```html
<!-- wxml -->
<web-view src="https://example.com/test.html" />
```

```html
<!-- https://example.com/test.html -->
<img src="qrocode-image.jpg" />
```

结果均同上，估计微信均采用相同的方式长按识别图片
