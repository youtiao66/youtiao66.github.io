---
title: "img 图片标签给 src 请求添加 headers 参数"
categories:
  - StackOverflow
tags:
  - HTTP
---

当我想要通过 img 图片标签展示图片时，服务端要求 src 请求必须添加 headers 参数。如何实现？

<!--more-->

## 答案
无法直接通过图片标签设置头信息参数，以下有几种方案：

### `fetch + blob`
使用 [fetch][fetch] 方法添加 headers 然后把结果加载到 `<img>`

```js
const src = 'https://api.mywebsite.com/profiles/123/avatar';
const options = {
  headers: {
    'Some-Header': '...'
  }
};

fetch(src, options)
  .then(res => res.blob())
  .then(blob => {
    imgElement.src = URL.createObjectURL(blob);
  });
```

### `ajax + base64`

1. 需要通过异步请求设置请求头信息
2. 使用 HTML5 APIs 把二进制数据转换为`base64`
3. 把图片标签的`src`设置为`data:`协议和`base64`数据

```js
var oReq = new XMLHttpRequest();
oReq.open("GET", "yourpage.jsp", true);
oReq.setRequestHeader("Your-Header-Here", "Value");
oReq.responseType = "arraybuffer";
oReq.onload = function (oEvent) {
  var arrayBuffer = oReq.response;
  if (arrayBuffer) {
    var u8 = new Uint8Array(arrayBuffer);
    var b64encoded = btoa(String.fromCharCode.apply(null, u8));
    var mimetype="image/png";
    document.getElementById("yourimageidhere").src="data:"+mimetype+";base64,"+b64encoded;
  }
};
oReq.send(null);
```

```html
<img src="data:image/png;base64,[CODE-OF-THE-IMAHE]">
```

> [https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/Sending_and_Receiving_Binary_Data][Sending_and_Receiving_Binary_Data]

> [https://stackoverflow.com/questions/12710001/how-to-convert-uint8-array-to-base64-encoded-string][convert-uint8-array-to-base64]

### GET 查询参数
使用 GET 查询参数代替请求头信息可以完成同样的功能

```html
<img src="controller?token=[TOKEN]">
```


## 参考
> [https://stackoverflow.com/questions/23609946/img-src-path-with-header-params-to-pass][img-src-path-with-header-params]

[fetch]: https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch
[Sending_and_Receiving_Binary_Data]: https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/Sending_and_Receiving_Binary_Data
[convert-uint8-array-to-base64]: https://stackoverflow.com/questions/12710001/how-to-convert-uint8-array-to-base64-encoded-string
[img-src-path-with-header-params]: https://stackoverflow.com/questions/23609946/img-src-path-with-header-params-to-pass
