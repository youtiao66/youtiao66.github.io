## 数据类型

### 6种原始类型（`primitive`）

- `Boolean`
- `Null`
- `Undefined`
- `Number`
- `String`
- `Symbol`

### 对象（`Object`）

## `typeof`

| 类型 | 结果 |
| --- | --- |
| `Boolean` | `"boolean"` |
| **`Null`** | **`"object"`** |
| `Undefined` | `"undefined"` |
| `Number` | `"number"` |
| `String` | `"string"` |
| `Symbol` | `"symbol"` |
| `Function` | `"function"` |
| 其他任何对象 | `"object"` |

``` js
  typeof null === "object"
  typeof function() {} === "function"
  typeof [] === "object"
```

``` js
  Object.prototype.toString.call(xx) === "[object Type]"
```

## 模块

### CommonJS

- 对于基本数据类型，属于复制。即会被模块缓存。同时，在另一个模块可以对该模块输出的变量重新赋值。
- 对于复杂数据类型，属于浅拷贝。由于两个模块引用的对象指向同一个内存空间，因此对该模块的值做修改时会影响另一个模块。
- 当使用require命令加载某个模块时，就会运行整个模块的代码。
- 当使用require命令加载同一个模块时，不会再执行该模块，而是取到缓存之中的值。也就是说，CommonJS模块无论加载多少次，都只会在第一次加载时运行一次，以后再加载，就返回第一次运行的结果，除非手动清除系统缓存。
- 循环加载时，属于加载时执行。即脚本代码在require的时候，就会全部执行。一旦出现某个模块被"循环加载"，就只输出已经执行的部分，还未执行的部分不会输出。

### ES6 Module

- 动态只读引用
- 对于只读来说，即不允许修改引入变量的值，import的变量是只读的，不论是基本数据类型还是复杂数据类型。当模块遇到import命令时，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。
- 对于动态来说，原始值发生变化，import加载的值也会发生变化。不论是基本数据类型还是复杂数据类型。
- 循环加载时，ES6模块是动态引用。只要两个模块之间存在某个引用，代码就能够执行。
