---
title: "HTTP 413 错误（Request entity too large 请求实体太大）"
categories:
  - Blog
tags:
  - HTTP
  - nginx
---

HTTP 413 错误（Request entity too large 请求实体太大）“the server responded with a status of 413 (Request Entity Too Large)”。

<!--more-->

## 来龙去脉

### 微信小程序用户反馈有上传文件【未捕获异常】的问题。实际表现为弹窗提示“服务异常”的默认错误提示信息

```js
;(async () => {
  await Promise.all([
    // 自定义图片上传方法
    uploadImg({ filePath: file1 }),
    uploadImg({ filePath: file2 }),
    uploadImg({ filePath: file3 })
  ])
})().catch(err => {
  // 自定义错误捕获方法，若未提取到错误信息则提示“服务异常”
  lizard.handleErrorAlert(err)
})
```

### 测试和开发在开发工具和手机上均未能复现该问题。

> **ℹ️注意：以下为猜测**

1. ~~部分设备不支持同时上传多个文件~~
2. [~~the request was rejected because no multipart boundary was found~~][multipart-boundary]

~~第二个猜测请参考~~ [~~wx.uploadFile 上传文件失败~~][multipart-boundary]

按以上两个方案修改后，生产环境仍然有上传文件【未捕获异常】的问题

### 通过微信小程序实时日志的功能，实现日志汇聚并实时上报到小程序后台。

> [实时日志](https://developers.weixin.qq.com/miniprogram/dev/framework/realtimelog/)

```js
;(async () => {
  await uploadImg({ filePath: file1 })
  await uploadImg({ filePath: file2 })
  await uploadImg({ filePath: file3 })
})().catch(err => {
  // 自定义封装实时日志接口，具体参考上方引用
  wxPromise.rlogm.addFilterMsg('REALNAME_UPLOAD_CONFIRM')
  wxPromise.rlogm.info(err)

  lizard.handleErrorAlert(err)
})
```

### 通过 `REALNAME_UPLOAD_CONFIRM` 搜索并发现未捕获异常 `ERR_INCOMPLETE_CHUNKED_ENCODING`

```js
FilterMsg内容： REALNAME_UPLOAD_CONFIRM
TraceId： xxxx-xxxxxxx-xxxxxxxxxxxxxxx_1653898433
[16:15:43]  {"errno":600001,"errMsg":"uploadFile:fail upload fail:-355:net::ERR_INCOMPLETE_CHUNKED_ENCODING"}
[16:15:43]  uploadFile:fail upload fail:-355:net::ERR_INCOMPLETE_CHUNKED_ENCODING
```

**微信开放社区相关问题：**

1. [wx.downloadFile 下载图片资源出现异常？](https://developers.weixin.qq.com/community/develop/doc/0006e8a7a34c4030038cd37e151000?highLine=ERR_INCOMPLETE_CHUNKED_ENCODING)
2. [微信小程序端不能上传图片报错？](https://developers.weixin.qq.com/community/develop/doc/0004ac25b94a487b1e8d493da56800?highLine=ERR_INCOMPLETE_CHUNKED_ENCODING)
3. [关于ERR_INCOMPLETE_CHUNKED_ENCODING?](https://developers.weixin.qq.com/community/develop/doc/000628879b0dd048ba1cdcc6351c00?highLine=ERR_INCOMPLETE_CHUNKED_ENCODING)
4. [net_error_list:INCOMPLETE_CHUNKED_ENCODING](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/net/base/net_error_list.h#711)
5. [提示net::ERR_INCOMPLETE_CHUNKED_ENCODING](https://developers.weixin.qq.com/community/develop/doc/00040870b605b89edec748d3151000?highLine=ERR_INCOMPLETE_CHUNKED_ENCODING)

**服务端相关发现：**

1. 服务端框架中有记录生产环境日志
2. 服务端生产环境中并未发现该条报错信息日志
3. 原有的上传三张图片，服务端仅有两张上传图片日志

> **ℹ️注意：** 初步结论为，代理服务器因图片大小超过限制，拒绝图片上传请求



## 根本原因

- Request entity too large 请求实体太大
- 上传文件过大，服务器有使用 nginx 做反向代理



## 解决方案

**修改 nginx 配置文件，配置客户端请求体大小。**

```conf
client_max_body_size 8M;      ## (配置客户端请求体大小)
```

> [ngx_http_core_module#client_max_body_size](https://nginx.org/en/docs/http/ngx_http_core_module.html#client_max_body_size)

| 标题   | 内容                                   |
|--------|----------------------------------------|
| 语法   | client_max_body_size size;                             |
| 默认值 | client_max_body_size 1m;                             |
| 上下文 | http, server, location |

设置客户端请求体允许的最大值。如果请求大小超过配置值，则返回 `413 (Request Entity Too Large)` 错误到客户端。请注意浏览器无法正确展示该错误。把 `size` 设置为 0 则禁用客户端请求体大小检查

> **ℹ️注意：** 腾讯云负载均衡文档中 `client_max_body_size` 默认值 `60M`, 但实测默认值和 nginx 一致为 `1M`

> [腾讯云负载均衡 - 个性化配置](https://cloud.tencent.com/document/product/214/15171)



[multipart-boundary]: https://developers.weixin.qq.com/community/develop/doc/0004e8aef5cb4834b1ec992835b400
