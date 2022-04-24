---
title: "虚拟形象方案调研"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - Other
---

虚拟形象设计与实现方案调研

<!--more-->

## 方案一 - 基于 Live2D 虚拟形象设计与实现

**Live2D**是一种应用于电子游戏的绘图渲染技术，技术由日本Cybernoids公司开发。通过一系列的连续图像和人物建模来生成一种类似三维模型的二维图像，对于以动画风格为主的冒险游戏来说非常有用，缺点是Live 2D人物无法大幅度转身，开发商正设法让该技术可显示360度图像。

**Live2D Cubism**编辑器可以仅用一幅插画创建各种形式的动画

**自定义虚拟形象和动画**

- 设计虚拟形象，并拆分为头部（头发，脸部，眼睛，嘴等结构），身体（身体，手，足等结构）等等，可使用 PhotoShop
- 使用 Live2D Cubism 编辑器预设动作和动画效果
- 可以录制语音的方式预制声音
- 导出虚拟形象和动画

**也可购买现有的虚拟形象和动画**

### Live2D Cubism SDK for Web

- 通过 Live2D Cubism SDK for Web 实现在浏览器中加载虚拟形象和动画、声音，基于导出的配置
- 通过该 SDK 内部提供的方法利用 JS 和虚拟形象进行交互，比如`startMotion`, `setExpression`等方法

### 出版许可

如果您的企业在最近一个会计年度的销售额达到或超过1000万日元（**约合50万人民币**），您必须得到Cubism SDK的出版授权许可（出版许可协议）。

* [Cubism SDK发行许可证](https://www.live2d.com/zh-CHS/download/cubism-sdk/release-license/)

> [二次元live2d看板娘效果中的web前端技术](https://www.zhangxinxu.com/wordpress/2018/05/live2d-web-webgl-js/)

> [网页添加 Live2D 看板娘](https://www.fghrsh.net/post/123.html)


## 方案二 - 第三方数字人方案合作
> [百度智能云曦灵-智能数字人平台](https://cloud.baidu.com/product/baidudigitalhuman.html)
