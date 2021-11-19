---
title: "@alitajs/tabs-layout: 基于 umi + antd 的多标签页模式布局"
excerpt_separator: "<!--more-->"
categories:
  - GitHub
tags:
  - JavaScript
toc: true
toc_label: "目录"
toc_icon: "cog"
---

`@alitajs/tabs-layout` 是一款基于 `umi + antd` 的多标签页模式布局插件，具有**可插拔**，**无侵入**，**低耦合**的特点，**更专注**于多标签页模式布局

<!--more-->

## 安装

```
yarn add @alitajs/tabs-layout
```

## 配置

```js
export default {
  plugins: ['@alitajs/tabs-layout'],
  tabsLayout: [/./], // 这里表示所有的页面
};
```

`tabsLayout` 是一个数组，写明需要使用 tabs 的页面，支持字符串和正则表达式，如需要全部匹配，可以设置 `tabsLayout: [/./]`。

## 使用

需要使用 `TabsLayout` 替换 `children`。

```ts
import { TabsLayout } from 'umi';

const BasicLayout: React.FC = (props) => {
  return (
    <div>
      <TabsLayout {...props} />
    </div>
  );
};

export default BasicLayout;
```

## `TabsLayout` 组件属性

| 参数 | 类型 | 说明 |
| --- | --- | --- |
| menu | `Menu[]` | 默认值 `[]`, 默认情况下，标签页无名称，会展示 `path` |
| alias | `Alias` | 默认值 `{}`, 默认情况下，可以配置 `path` 和 `title` 字段别名 |

**Menu**

```ts
// 默认，可配置别名
interface Menu {
  path: string
  title?: string
  children?: Menu[]
}
```

**Alias**

```ts
interface Alias {
  path: string
  title: string
}
```

## 参考
> [@alitajs/tabs-layout](https://github.com/alitajs/alita/tree/master/packages/tabs-layout)

> [antd 多标签页模式讨论 issue](https://github.com/ant-design/ant-design-pro/issues/220)
