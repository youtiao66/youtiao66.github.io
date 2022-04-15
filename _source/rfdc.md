---
title: "从 rfdc - Really Fast Deep Clone 分析深度克隆原理"
permalink: /source/rfdc/
---

我比较倾向于一个依赖库只做一件事情，并且零依赖。这样有助于源码分析和学习。接下来从 rfdc - Really Fast Deep Clone 分析深度克隆原理

<!--more-->

## 思维导图

- 如果 `typeof` 类型不是 `object` 或者是 `null`, 直接返回
- 如果是日期，直接作为参数返回新日期
- 如果是数组，递归深度克隆每个元素
- 如果是对象，递归深度克隆每个存在的属性值

## 源码

```JavaScript
module.exports = function deepClone (target) {
  if (typeof target !== 'object' || target === null) return target
  if (target instanceof Date) return new Date(target)
  if (Array.isArray(target)) {
    const arr = []
    for (let i = 0; i < target.length; i++) {
      arr.push(deepClone(target[i]))
    }
    return arr
  }

  const result = {}
  for (let key in target) {
    if (!Object.prototype.hasOwnProperty.call(target, key)) continue
    result[key] = deepClone(target[key])
  }
  return result
}
```

## 参考

> [https://github.com/davidmarkclements/rfdc](https://github.com/davidmarkclements/rfdc)