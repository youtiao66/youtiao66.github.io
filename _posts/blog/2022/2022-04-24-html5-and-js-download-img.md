---
title: "HTML5 + JavaScript 实现在客户端浏览器中下载图片"
categories:
  - Blog
tags:
  - JavaScript
  - HTTP
---

最近在项目中遇到下载图片的需求，需要点击直接下载图片。如何在客户端浏览器中通过点击直接下载图片呢？不是预览，也不是在新标签打开图片。

<!--more-->

## a 标签
首先，我想到的就是通过 a 标签并添加 href 属性为图片链接地址。

```html
<a href="./test.png">下载</a>
```

点击“下载”链接时，浏览器会直接打开图片并展示。但这并不是我们想要的效果，我们是希望直接下载到本地，应该怎么实现呢？



## download 属性
此属性指示浏览器下载 URL 而不是导航到它，因此将提示用户将其保存为本地文件。如果属性有一个值，那么此值将在下载保存过程中作为预填充的文件名（如果用户需要，仍然可以更改文件名）。此属性对允许的值没有限制，但是 `/` 和 `\` 会被转换为下划线。大多数文件系统限制了文件名中的标点符号，故此，浏览器将相应地调整建议的文件名。

```html
<!-- 默认文件名 -->
<a href="test.png" download>下载</a>
<!-- 文件名: example.png -->
<a href="test.png" download="example">下载</a>
<!-- 文件名: example.jpg -->
<a href="test.png" download="example.jpg">下载</a>
```

> **ℹ️注意：** **此属性仅适用于[同源 URL][same-origin]**

非同源 URL 仍然会直接打开图片并展示。

```html
<!-- 同源 URL 直接下载 -->
<a href="test.png" download>下载</a>
<!-- 非同源 URL download 属性不生效 -->
<a href="https://example.com/test.png" download>下载</a>
```



## 异步下载
通过 `XMLHttpRequest` 请求图片，把响应保存为 `Blob` 类文件对象，然后通过动态创建 `a` 标签下载图片

```js
/**
 * 下载文件
 * @param {string} url 下载文件地址
 * @param {*} options
 * @param {string} options.filename 文件名称
 * a 标签 download 属性仅允许下载同源文件，通过 XMLHttpRequest 跨域可以绕过该限制
 */
function downloadFile(url, options = {}) {
  const x = new XMLHttpRequest()
  x.open('GET', url, true)
  x.responseType = 'blob'
  x.onload = function () {
    const url = window.URL.createObjectURL(x.response)
    const a = document.createElement('a')
    a.href = url
    a.download = options.filename || ''
    a.click()
  }
  x.send()
}
```

> **ℹ️注意：** 该方法仍然需要服务端允许跨域。兼容支持 `HTML5 <a> download` 属性和 `Blob` 的浏览器




[same-origin]: https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy
