---
title: "通过动态注册方法简化自定义确认弹窗逻辑"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - MySQL
toc: true
toc_label: "目录"
toc_icon: "cog"
---

在微信小程序中，需要通过自定义弹窗 `Confirm` 进行确认，如何有效地简化代码实现逻辑，增强代码可读性？

<!--more-->

## 需求
通过自定义弹窗 `Confirm` 进行确认，然后进行下一步

![自定义弹窗](https://i.loli.net/2021/07/12/QuZpRXiCvIO5fj2.png)

## 常规解决方案

```html
<!-- van-dialog Dialog 弹出框 https://vant-contrib.gitee.io/vant-weapp/#/dialog -->
<!-- ui-addr 自定义地址组件 -->
<van-dialog
  wx:if="{{ isNeedPostal }}"
  use-slot
  title="确认邮寄地址"
  show="{{ visibleConfirmPostal }}"
  show-cancel-button
  bind:close="onConfirmPostalClose"
>
  <ui-addr type="preview" src="{{ addrSrc }}" arrow-icon="{{ false }}"></ui-addr>
</van-dialog>
```

```js
// => page.js
const { makeDialogConfirm } = require('util.js');

Page({
  data: {
    // 是否展示邮寄地址确认弹窗
    visibleConfirmPostal: false
  },
  // 自定义确认弹窗事件回调
  onConfirmPostalClose() {},
  // 确认弹窗方法
  confrimPostal() {
    return makeDialogConfirm({ vm: this, visibleName: 'visibleConfirmPostal', callbackName: 'onConfirmPostalClose' })
  },
  onConfirmClick() {
    this.confrimPostal().then(() => {
      // do next
    })
  }
})
```

## 我的优化方案

```html
<!-- van-dialog Dialog 弹出框 https://vant-contrib.gitee.io/vant-weapp/#/dialog -->
<!-- ui-addr 自定义地址组件 -->
<van-dialog
  wx:if="{{ isNeedPostal }}"
  use-slot
  title="确认邮寄地址"
  show="{{ visibleConfirmPostal }}"
  show-cancel-button
  bind:close="onConfirmPostalClose"
>
  <ui-addr type="preview" src="{{ addrSrc }}" arrow-icon="{{ false }}"></ui-addr>
</van-dialog>
```

```js
// => page.js
const { makeDialogConfirm } = require('util.js');

Page({
  data: {
    // 是否展示邮寄地址确认弹窗
    visibleConfirmPostal: false
  },
  // 自定义确认弹窗事件回调
  onConfirmPostalClose({ detail }) {
    if (detail === 'confirm') {
      this.setData({
        visibleConfirmPostal: false
      })
      this.doNext()
    } else {
      reject(new Error('cancel 取消'))
      this.setData({
        visibleConfirmPostal: false
      })
    }
  },
  doNext() {
    // do next
  }
})
```

```js
// => util.js
/**
 * 生成自定义弹窗的 Confirm 方法，适用于通过 van-dialog 自定义弹窗
 * @param {object} param
 * @param {context} param.vm this 一般只当前页面或者组件
 * @param {string} param.visibleName 数据名，用于定义是否展示弹窗
 * @param {string} param.callbackName 方法名，需要重新定义的回调方法名
 * @returns {Promise}
 */
const makeDialogConfirm = param => {
  const { vm, visibleName, callbackName } = param
  vm.setData({
    [visibleName]: true
  })
  return new Promise((resolve, reject) => {
    vm[callbackName] = ({ detail }) => {
      if (detail === 'confirm') {
        resolve(true)
        vm.setData({
          [visibleName]: false
        })
      } else {
        reject(new Error('cancel 取消'))
        vm.setData({
          [visibleName]: false
        })
      }
    }
  })
}

module.exports = { makeDialogConfirm }
```

### 优势
- 确认逻辑可读性更高
- 邮寄地址确认弹窗确认需求对正常业务侵入性非常小
- 可复用性更强
