---
title: "如何禁用新版 Chrome Cookie 安全策略？"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - Chrome
toc: true
toc_label: "目录"
toc_icon: "cog"
---

Chrome 80 禁用第三方 Cookie。Chrome 80 开始，第三方 Cookie 中的 SameSite 属性未指定时，默认值将会设置为 Lax，此举可以提升站点安全性，从源头防御大量的 CSRF 漏洞，但同时也会导致跨站请求时 Cookie 下发失败。

<!--more-->

## 如何禁用 `Chrome Cookie` 安全策略？

- 在 `Chrome` 中打开新的标签页，并在地址栏中输入 `chrome://flags/`
- 搜索 `Cookie`，并禁用 `SameSite by default cookies` 和 `Cookies without SameSite must be secure`
![Disable SameSite Cookie](https://i.loli.net/2021/04/30/auojyRf2hvB7teU.png)

## SameSite

- **`Strict`**: 仅限于同站请求
- **`Lax`**: 同站请求，或者跨站的 GET 的会导致页面 URL 发生变化的导航行为，包括链接、GET 的 Form 提交。比如在 http://www.example.com 网站的页面中嵌入了一个链接 http://www.baidu.com，那么在用户点击此链接时发起的初始请求，会带上 http://www.example.com 中设置为 Lax 的 Cookie。而那些设置为 Strict 的 Cookie 是不会带上的。这种跨站链接还包括像嵌入到邮件中的链接。
- **`None`**: 没有限制，同站及跨站请求中都会带上，与传统的方式一样。

## Cookie 是什么？

`HTTP Cookie`（也叫 Web Cookie 或浏览器 Cookie）是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie 使基于无状态的HTTP协议记录稳定的状态信息成为了可能。

- 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
- 个性化设置（如用户自定义设置、主题等）
- 浏览器行为跟踪（如跟踪分析用户行为等）
- 广告
