---
title: "Exists 查询"
excerpt: "返回包含指定字段索引值的文档"
permalink: /esguidecn/query-dsl-exists-query/
---

返回包含指定字段索引值的文档

文档不存在指定字段索引值的原因如下：

- 该字段在源 JSON 中是 `null` 或者 `[]`
- 该字段在映射中设置为 `"index" : false`
- 该字段的值长度超过在映射中设置的 `ignore_above`
- 该字段的值已被格式化，并且属于在映射中设置的 `ignore_malformed`

## 请求示例

```js
GET /_search
{
  "query": {
    "exists": {
      "field": "user"
    }
  }
}
```

## `exists` 顶层参数

`field`

(必选，字符串) 想要搜索的字段名称

如果 JSON 值为 `null` 或者 `[]`, 那么该字段视为不存在的，以下值表明相应字段是存在的：

- 空字符串，例如 `""` or `"-"`
- 包含 `null` 或其他值的数组，例如 `[null, "foo"]`
- 字段映射中自定义的 `null-value`

## 说明

### 查找不存在字段索引值的文档

为了查找不存在字段索引值的文档，使用 `must_not` **布尔查询** 配合 `exists` 查询实现

以下搜索返回不存在 `user.id` 字段所以值的文档

```js
GET /_search
{
  "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "user.id"
        }
      }
    }
  }
}
```
