---
title: "fastjson-1.1.24 生成的JSON字符串由于包含 \\t (tab)特殊字符导致浏览器无法解析"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - JavaScript
toc: true
toc_label: "目录"
toc_icon: "cog"
---

刚参加工作时遇到一个服务端返回的 `JSON` 字符串无法解析的问题。当时研究了很久都没有找到原因，最后一步步抽丝剥茧发现是由于该字符串中包含 `\t` (tab)制表符的原因。

<!--more-->

## 问题
服务端返回的 `JSON` 字符串浏览器无法解析

![SyntaxError](https://i.loli.net/2021/05/17/ZqvXs4g8ubAImpd.png)

```js
JSON.parse("{\"a\":\"\t\"}")
// => SyntaxError: Unexpected token    in JSON
JSON.parse("{\"a\":\"\\t\"}")
// => { a: '\t' }
```

## 原因
fastjson-1.1.24 未对特殊字符 `\t` 转义。不符合[RFC 4627 | "JSON 标准"](http://www.json.org.cn/standard.htm)

```java
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;

public class HelloWorld
{
	public static void main (String[] args)
	{
		JSONObject test = new JSONObject();
		test.put("key", "before\tafter");
		String jsonString = JSON.toJSONString(test);
		System.out.println(jsonString);
		// fastjson-1.1.24 => {"key":"before  after"}
		// fastjson-1.1.28 => {"key":"before\tafter"}
	}
}
```

## 解决方案

### 该`BUG`已经在`fastjson-1.1.28`中修复

### 通过客户端字符串替换解决该问题
```js
jsonString = jsonString.replace(/\t/g, "\\t");
```

## 如何在 `Java` 代码中导入 `jar` 包并执行
为了复现该`BUG`，我可是折腾了好久。由于并没有准备`Java Web`环境，想要运行起来简直是一波三折。这是我的源代码，`jar`包、源代码以及构建目录均直接使用项目根目录。

![Import-jar-File-Java-Code](https://i.loli.net/2021/05/17/rW1VySui2cbNv4a.png)

```bash
# 编译

# 错误写法
javac HelloWorld.java
# => elloWorld.java:1: 错误: 程序包com.alibaba.fastjson不存在
# => import com.alibaba.fastjson.JSON;

# 正确写法
javac -cp fastjson-1.1.24.jar HelloWorld.java
```

```bash
# 运行

# 错误写法
java -cp fastjson-1.1.24.jar HelloWorld
# => 错误: 找不到或无法加载主类 HelloWorld.java 

# 正确写法
# Unix
java -cp .:fastjson-1.1.24.jar HelloWorld
# Windows
java -cp .;fastjson-1.1.24.jar HelloWorld
```

## 参考
> [Import-jar-File-Java-Code](https://coderanch.com/t/411406/java/Import-jar-File-Java-Code)

> You need to specify the classpath for compilation and at runtime: `java -classpath .;swt.jar SWTHelloWorld`. Note that the dot is important, otherwise the SWTHelloWorld class won't get found. (This assumes that you're using Windows; if you're on Unix, use a colon instead of a semicolon.)

## P.S.
> 这本该是一篇 2016 年左右写出来的博客(\*T_T\*) 