---
title: "PHP 和 CryptoJS AES 加解密互通"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - Node.js
  - PHP
toc: true
toc_label: "目录"
toc_icon: "cog"
---

PHP 和 CryptoJS AES 加解密互通

<!--more-->

## 使用

| 名称 | 值 |
| --- | --- |
| 模式 | `CBC` |
| 填充 | `Pkcs7` |
| 长度 | `128`，`key` 和 `iv` 的长度均为 `16` |
| 输出 | `Base64` |

```js
import * as CryptoJS from 'crypto-js'

/**
 * AES 加密
 * @param plaintext 明文
 * @param key 加密 key
 * @param iv 偏移量
 * @returns 密文
 */
export const AESencrypt = (plaintext, key, iv) => {
  key = CryptoJS.enc.Utf8.parse(key)
  iv = CryptoJS.enc.Utf8.parse(iv)
  const encryptedData = CryptoJS.AES.encrypt(plaintext, key, {
    iv,
    mode: CryptoJS.mode.CBC,
    padding: CryptoJS.pad.Pkcs7
  })
  return encryptedData.toString()
}

/**
 * AES 解密
 * @param ciphertext 密文
 * @param key 加密 key
 * @param iv 偏移量
 * @returns 明文
 */
export const AESdecrypt = (ciphertext, key, iv) => {
  key = CryptoJS.enc.Utf8.parse(key)
  iv = CryptoJS.enc.Utf8.parse(iv)
  const decryptedData = CryptoJS.AES.decrypt(ciphertext, key, {
    iv,
    padding: CryptoJS.pad.Pkcs7
  })
  return decryptedData.toString(CryptoJS.enc.Utf8)
}
```

```php
class AESHelper
{
  public static function encrypt($data, $key, $iv)
  {
    return base64_encode(openssl_encrypt($data, 'AES-128-CBC', $key, 1, $iv));
  }
  public static function decrypt($data, $key, $iv)
  {
    return openssl_decrypt(base64_decode($data), 'AES-128-CBC', $key, 1, $iv);
  }
}
```

## 常见问题

- `CryptoJS` 解密时不需要设置模式
- 解密失败，`Error: Malformed UTF-8 data`
- 解密失败，解密结果为空字符串或者乱码

## 检查方法

- 模式设置不正确
- 填充设置不正确
- `key` 和 `iv` 不正确
- `key` 和 `iv` 未处理，`const key = CryptoJS.enc.Utf8.parse(key)`

```js
// `key` 和 `iv` 正确处理方式，明文和密文不需要
const key = CryptoJS.enc.Utf8.parse(key)
const iv = CryptoJS.enc.Utf8.parse(iv)
```

> CryptoJS supports AES-128, AES-192, and AES-256. It will pick the variant by the size of the key you pass in. If you use a passphrase, then it will generate a 256-bit key

## CryptoJS - JavaScript 加解密标准库
> [brix/crypto-js](https://github.com/brix/crypto-js)
