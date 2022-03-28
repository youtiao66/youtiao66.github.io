---
title: "JavaScript 中你不知道的 5 个 JSON 隐秘特性"
categories:
  - Blog
tags:
  - JavaScript
---

我非常确信你已经使用全局 `JSON` 对象完成各种各样的事情，比如异步请求和避免可怕的 `[object Object]`. 我打赌你仍然不知道 `JSON` 提供的其他不广为人知的特性。

<!--more-->

`JSON` 可以完成许多很酷的工作，例如恢复数据，自定义格式编码或解码数据，转换为字符串的时候隐藏属性和格式化 JSON 等



## 1. 格式化
默认的转换为字符串方法会压缩 JSON, 这看起来很丑

```js
const user = {
  name: 'John',
  age: 30,
  isAdmin: true,
  friends: ['Bob', 'Jane'],
  address: {
    city: 'New York',
    country: 'USA'
  }
};

console.log(JSON.stringify(user));
//=> {"name":"John","age":30,"isAdmin":true,"friends":["Bob","Jane"],"address":{"city":"New York","country":"USA"}}
```

`JSON.stringify` 可以指定美化格式

```js
console.log(JSON.stringify(user, null, 2));
// {
//   "name": "John",
//   "age": 30,
//   "isAdmin": true,
//   "friends": [
//     "Bob",
//     "Jane"
//   ],
//   "address": {
//     "city": "New York",
//     "country": "USA"
//   }
// }
```

（如果你想知道 null 的作用，稍后将为你解答）

在这个示例当中, JSON 以两个空格的缩进格式化。

我们也可以指定一个自定义字符用于缩进

```js
console.log(JSON.stringify(user, null, 'lol'));
// {
// lol"name": "John",
// lol"age": 30,
// lol"isAdmin": true,
// lol"friends": [
// lollol"Bob",
// lollol"Jane"
// lol],
// lol"address": {
// lollol"city": "New York",
// lollol"country": "USA"
// lol}
// }
```



## 2. 在转换为字符串的时候隐藏某个属性
`JSON.stringify` 第二个参数基本没人知道，它叫做 `replacer`, 它是个用于决定哪些数据输出哪些数据不输出的函数或者数组

以下示例可以隐藏用户的 `password` 字段：

```js
const user = {
  name: 'John',
  password: '12345',
  age: 30
};

console.log(JSON.stringify(user, (key, value) => {
    if (key === 'password') {
            return;
    }

    return value;
}));
```

输出是：

```JSON
{"name":"John","age":30}
```

我们还可以重构它：

```js
function stripKeys(...keys) {
    return (key, value) => {
        if (keys.includes(key)) {
            return;
        }

        return value;
    };
}

const user = {
  name: 'John',
  password: '12345',
  age: 30,
  gender: 'male'
};

console.log(JSON.stringify(user, stripKeys('password', 'gender')))
```

输出是：

```JSON
{"name":"John","age":30}
```

也可以传一个数组用于只展示指定字段

```js
const user = {
    name: 'John',
    password: '12345',
    age: 30
}

console.log(JSON.stringify(user, ['name', 'age']))
```

这可以达到同样的效果

很棒的是这对数组也是同样有效的哦，如果有很长的蛋糕数组：

```js
const cakes = [
    {
        name: 'Chocolate Cake',
        recipe: [
            'Mix flour, sugar, cocoa powder, baking powder, eggs, vanilla, and butter',
            'Mix in milk',
            'Bake at 350 degrees for 1 hour',
            // ...
        ],
        ingredients: ['flour', 'sugar', 'cocoa powder', 'baking powder', 'eggs', 'vanilla', 'butter']
    },
    // tons of these
];
```

我们可以简单地达到同样的目的, `replacer` 可以应用到数组的每一个元素

结果将是：

```JSON
[{"name":"Chocolate Cake"},{"name":"Vanilla Cake"},...]
```

太棒了！



## 3. 使用 `toJSON` 创建自定义输出格式
如果对象已经实现了 `toJSON` 方法, `JSON.stringify` 将使用该方法格式化数据

思考一下这个例子：

```js
class Fraction {
  constructor(n, d) {
    this.numerator = n;
    this.denominator = d;
  }
}

console.log(JSON.stringify(new Fraction(1, 2)))
```

这将输出 `{"numerator":1,"denominator":2}`. 但我们想要的是字符串 `1/2`

添加 `toJSON` 方法：

```js
class Fraction {
  constructor(n, d) {
    this.numerator = n;
    this.denominator = d;
  }
}

console.log(JSON.stringify(new Fraction(1, 2)))
```

`JSON.stringify` 将遵循 `toJSON` 属性并输出字符串 `"1/2"`.



## 4. 恢复数据
以上分数示例起作用了。但是如果我们想恢复数据怎么办呢？当我们再次解析 JSON 的时候这个分数又能够神奇的恢复会不会很绝妙。当然是可以的

添加恢复方法：

```js
class Fraction {
  constructor(n, d) {
    this.numerator = n;
    this.denominator = d;
  }

  toJSON() {
      return `${this.numerator}/${this.denominator}`
  }

  static fromJSON(key, value) {
    if (typeof value === 'string') {
        const parts = value.split('/').map(Number);
        if (parts.length === 2) return new Fraction(parts);
    }

    return value;
  }
}

const fraction = new Fraction(1, 2);
const stringified = JSON.stringify(fraction);
console.log(stringified);
// "1/2"
const revived = JSON.parse(stringified, Fraction.fromJSON);
console.log(revived);
// Fraction { numerator: 1, denominator: 2 }
```

我可以传递第二个参数给 `JSON.parse` 去指定恢复函数。恢复函数的工作是恢复已转换为字符串的数据为原始格式。



## 5. 通过转换器隐藏数据
和替换器类似，转换器也可以用于隐藏数据，方法是一样的

示例如下：

```js
const user = JSON.stringify({
  name: 'John',
  password: '12345',
  age: 30
});

console.log(JSON.parse(user, (key, value) => {
    if (key === 'password') {
            return;
    }

    return value;
}));
```

输出结果如下：

```js
{ name: 'John', age: 30 }
```



## 最后
你知道哪些其他奇妙的 `JSON` 技巧👀



> 本文译自 [5 Secret features of JSON in JavaScript you didn't know about][5-secret-features-of-json]



[5-secret-features-of-json]: https://dev.to/siddharthshyniben/5-secret-features-of-json-you-didnt-know-about-5bbg
