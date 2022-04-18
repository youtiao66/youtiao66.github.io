---
title: "从 fast-deep-equal 分析深度相等原理"
permalink: /source/utils/fast-deep-equal/
---

从 fast-deep-equal 分析深度比较两个 JavaScript 对象的原理。通过思维导图分析深度相等原理，更清晰更形象，更容易理解源码并自己实现源码

## 思维导图
![](https:/youtiao66.github.io/assets/images/source/fast-deep-equal.jpeg)

## 源代码
```javascript
module.exports = function equal(a, b) {
  if (a === b) return true;

  if (a && b && typeof a == 'object' && typeof b == 'object') {
    if (a.constructor !== b.constructor) return false;

    var length, i, keys;
    if (Array.isArray(a)) {
      length = a.length;
      if (length != b.length) return false;
      for (i = length; i-- !== 0;)
        if (!equal(a[i], b[i])) return false;
      return true;
    }

    if (a.constructor === RegExp) return a.source === b.source && a.flags === b.flags;
    if (a.valueOf !== Object.prototype.valueOf) return a.valueOf() === b.valueOf();
    if (a.toString !== Object.prototype.toString) return a.toString() === b.toString();

    keys = Object.keys(a);
    length = keys.length;
    if (length !== Object.keys(b).length) return false;

    for (i = length; i-- !== 0;)
      if (!Object.prototype.hasOwnProperty.call(b, keys[i])) return false;

    for (i = length; i-- !== 0;) {
      var key = keys[i];
      if (!equal(a[key], b[key])) return false;
    }

    return true;
  }

  // true if both NaN, false otherwise
  return a!==a && b!==b;
};

```

## 参考
> [https://github.com/epoberezkin/fast-deep-equal](https://github.com/epoberezkin/fast-deep-equal)

