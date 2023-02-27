---
title: "TypeScript 如何把数组转换为字符串字面量联合类型？"
categories:
  - Blog
tags:
  - JavaScript
---

想要把字符串数组转换为字符串字面量联合类型，可以先使用`as const`关键字定义只读字符串数组，然后对数组中的全部值使用`typeof`操作符。
```javascript
// 只读的字符串数组
const namesArr = ["John", "Lily", "Roy"] as const;

// 把数组转换为字符串字面量联合类型
type Names = typeof namesArr[number]; // "John" | "Lily" | "Roy"
```
这可能需要一些时间去理解发生了什么。因此让我带你一步一步地分析：
第一步，初始化字符串数组
```javascript
// 字符串数组
const namesArr = ["John", "Lily", "Roy"];
```
我们的目标是把它转换为字符串字面量联合类型
接着，在`TypeScript`中必须使用`as const`关键字才能把数组转换为只读的在各个方面都是常量的数组。
就像这样做：
```javascript
// 只读的字符串数组
const namesArr = ["John", "Lily", "Roy"] as const;
```
把字符串数组转换为只读模式之后，就不能在使用常规的`push`, `pop`, `slice`等数组方法。它在各个方面都是一个常数
然后，我们需要把只读的数组转换为字符串字面量联合类型。
为此我们定义一个叫做`Names`的新类型，然后对`namesArr`数组的全部值都是用`typeof`操作符。
如下所示：
```javascript
// 只读的字符串数组
const namesArr = ["John", "Lily", "Roy"] as const;

// 把数组转换为字符串字面量联合类型
type Names = typeof namesArr[number]; // "John" | "Lily" | "Roy"
```
现在你可能会问我们为什么对`namesArr`使用`number`类型，这就是告诉`TypeScript`编译器去获取来自于`namesArr`的所有编号的索引值，然后通过这些值去创建一个类型。
因此，当`TypeScript`试图推断`namesArr`数组中包含的所有值的类型时，它将创建一种字符串字面量联合类型，因为数组中的所有值都是常量和只读的。
最后，`Names`类型将包含如下内容：
```javascript
type Names = "John" | "Lily" | "Roy";
```
这就是我们想要的，现在就可以在`TypeScript`中使用`Names`类型

> [How to convert an array into string literal union type in TypeScript?](https://melvingeorge.me/blog/convert-array-into-string-literal-union-type-typescript)
