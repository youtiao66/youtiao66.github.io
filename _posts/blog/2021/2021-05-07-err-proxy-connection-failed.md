---
title: "同一个 WiFi 下，手机可以上网，但是电脑不可以"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - Windows
toc: true
toc_label: "目录"
toc_icon: "cog"
---

同一个 WiFi 下，手机可以上网，但是电脑不可以。如何解决 ERR_PROXY_CONNECTION_FAILED 错误？

<!--more-->

## 问题：ERR_PROXY_CONNECTION_FAILED
![ERR_PROXY_CONNECTION_FAILED](https://i.loli.net/2021/05/07/3GEJh4MIK5bapC2.jpg)

## 解决方案

### 方法一：重置 `Internet` 设置
- 打开 **IE 浏览器**, 点击右上角**齿轮按钮**，选择 **Internet 选项**
![Internet 选项](https://i.loli.net/2021/05/07/e1J6lkAsEZm2HUL.png)
- 选择顶部 **高级** 选项卡，然后点击 **重置**，将 Internet Explorer 设置重置为默认设置
![重置](https://i.loli.net/2021/05/07/Dpn365BwcfZtR4Y.png)
- 再次点击 **重置** 并退出，重启浏览器并重试

### 方法二：刷新 `DNS`

- 同时按下 `Windows 键 + R`, 输入 `cmd`, 打开**命令提示符**
![cmd](https://i.loli.net/2021/05/07/1bawspcfHDhRETj.png)
- 输入一下命令 `ipconfig /flushdns`, 然后点击 **回车 Enter**
![flushdns](https://i.loli.net/2021/05/07/lqcvD9tJFEdTXVx.png)
- 退出命令提示符，重启浏览器并重试

### 方法三：重新获取 IP 地址

- 同时按下 `Windows 键 + R`, 输入 `cmd`, 打开**命令提示符**
- 依次键入一下指令，然后分别点击 **回车 Enter**
```bash
ipconfig /release
ipconfig /flushdns
ipconfig /renew
```
- 退出命令提示符，重启浏览器并重试

## 其他

`ERR_PROXY_CONNECTION_FAILED`是基于浏览器的错误，当代理服务器设置出现问题时，可能会出现在任何的`Windows`操作系统版本上。代理服务器作为连接`Internet`与`Intranet`的桥梁，在实际应用中发挥着极其重要的作用，它可用于多个目的，最基本的功能是连接，此外还包括安全性、缓存、内容过滤、访问控制管理等功能。更重要的是，代理服务器是`Internet`链路级网关所提供的一种重要的安全功能，它的工作主要在开放系统互联（OSI）模型的对话层。
