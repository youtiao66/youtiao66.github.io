# JavaScript

## 深浅拷贝

快速实现深拷贝的方法，有局限性
``` js
JSON.parse(JSON.stringify(object))
```

## 闭包

参考：
- [JS | InterviewMap | 闭包](https://github.com/InterviewMap/CS-Interview-Knowledge-Map/blob/master/JS/JS-ch.md#%E9%97%AD%E5%8C%85)
- [闭包 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)

# 算法

## 冒泡排序

``` js
function sort(arr) {
  let len = arr.length
  for (let i = len - 1; i > 0; i--) {
    for (let j = 0; j < i; j++) {
      if (arr[j] > arr[j + 1]) {
        let tmp = arr[j]
        arr[j] = arr[j + 1]
        arr[j + 1] = tmp
      }
    }
  }
  return arr
}
```

## 插入排序

``` js
function sort(arr) {
  let len = arr.length
  for (let i = 1; i < len; i++) {
    let preIndex = i - 1
    let current = arr[i]
    while(preIndex >= 0 && arr[preIndex] > current) {
      arr[preIndex + 1] = arr[preIndex]
      preIndex--
    }
    arr[preIndex + 1] = current
  }
  return arr
}
```

## 选择排序

``` js
function sort(arr) {
  let len = arr.length
  for (let i = 0; i < len - 1; i++) {
    let minIndex = i
    for (let j = i + 1; j < len; j++) {
      if (arr[minIndex] > arr[j]) {
        minIndex = j
      }
    }
    let tmp = arr[i]
    arr[i] = arr[minIndex]
    arr[minIndex] = tmp
  }
  return arr
}
```

## 数组去重

- 利用ES6的Array.from()或者扩展运算符 以及 Set
``` js
Array.from(new Set(arr))
```
``` js
[...new Set(arr)]
```

- 遍历数组，建立新数组，利用indexOf判断是否存在于新数组中
``` js
function unique(arr) {
  let newArr = []
  for (let i in arr) {
    if (newArr.indexOf(arr[i]) === -1) {
      newArr.push(arr[i])
    }
  }
  return newArr
}
```

- 遍历数组，利用Object对象的key值保存数组值，key值唯一
``` js
function unique(arr) {
  let newArr = []
  let obj = {}
  for (let i in arr) {
    let item = arr[i]
    if (obj[item] !== 1) {
      obj[item] = 1
      newArr.push(item)
    }
  }
  return newArr
}
```

- 先排序，然后遍历排序后的数组并push到新数组，判断最后一个元素是否相同
``` js
function unique(arr) {
  let newArr = []
  arr.sort()
  for (let i in arr) {
    let item = arr[i]
    if (newArr[newArr.length - 1] !== item) {
      newArr.push(item)
    }
  }
  return newArr
}
```

## 获取字符串中第一个不重复的字母
``` js
function getFirstSingle(str) {
  if (str.length === 1) return str
  for (let i = 0; i < str.length; i++) {
    for (let j = 0; j < str.length; j++) {
      if (i != j) {
        if (str[i] === str[j]) {
          break
        }
      }
      if (j === str.length - 1) {
        return str[i]
      }
    }
  }
}
```
