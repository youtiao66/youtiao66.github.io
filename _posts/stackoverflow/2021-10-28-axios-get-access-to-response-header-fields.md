---
title: "Axios 在 CORS 跨域时未获取到部分 response headers"
categories:
  - StackOverflow
tags:
  - HTTP
---

当我使用 Axios 时，未能获取到 response headers 中的分页信息 Page 和 Total. 而通过浏览器开发者工具能够明确获取到该响应头信息，百思不得其解

<!--more-->

## 问题

当我使用 Axios 执行请求时，我想要获取 response headers 中的所有字段。在浏览器开发者工具中可以看到所有我需要的响应头信息。当我执行一下请求时：

```js
const request = axios.post(`${ROOT_URL}/auth/sign_in`, props);
request.then((response)=>{
  console.log(response.headers);
});
```

仅能获取到一下响应头信息：

```js
Object {content-type: "application/json; charset=utf-8", cache-control: "max-age=0, private, must-revalidate"}
```

这是我的浏览器网络选项卡，所有其他的响应头信息都展示出来了。

![浏览器响应头信息](https://i.loli.net/2021/10/28/ZVkeLgzf2QlBKSa.png)

## 答案

In case of CORS requests, browsers can only access the following response headers by default:
在 CORS 跨域请求时，浏览器环境中默认仅能够获取到以下 response headers:

- Cache-Control
- Content-Language
- Content-Type
- Expires
- Last-Modified
- Pragma

如果想要在客户端应用中获取到其他响应头信息，需要在服务端设置 `Access-Control-Expose-Headers`:

```
Access-Control-Expose-Headers: Access-Token, Uid, Page, Total
```

## 参考
> [Axios get access to response header fields](https://stackoverflow.com/questions/37897523/axios-get-access-to-response-header-fields)
