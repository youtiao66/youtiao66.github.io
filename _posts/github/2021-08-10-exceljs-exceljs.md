---
title: "ExcelJS: 读取，操作并写入电子表格数据和样式到 XLSX 和 JSON 文件"
excerpt_separator: "<!--more-->"
categories:
  - GitHub
tags:
  - Node.js
toc: true
toc_label: "目录"
toc_icon: "cog"
---

读取，操作并写入电子表格数据和样式到 XLSX, CSV 和 JSON 文件。一个 Excel 电子表格文件逆向工程项目。

<!--more-->

## 安装

```shell
npm install exceljs
```

## 使用

```js
// 导入
const ExcelJS = require('exceljs');
// 创建工作簿
const workbook = new ExcelJS.Workbook();
// 添加工作表
const worksheet = workbook.addWorksheet('My Sheet');
// 添加列标题并定义列键
worksheet.columns = [
  { header: 'Id', key: 'id'},
  { header: 'Name', key: 'name'},
  { header: 'D.O.B.', key: 'dob'}
];
// 添加多行数据
worksheet.addRows([
  {id:5, name: 'Bob', dob: new Date()}
  {id:6, name: 'Barbara', dob: new Date()}
]);
// 写入 buffer
const buffer = await workbook.xlsx.writeBuffer();
// 写入文件（写入文件和写入 buffer 二选一即可）
await workbook.xlsx.writeFile(filename);
```

## 功能
- 功能更齐全
- [中文文档](https://github.com/exceljs/exceljs/blob/master/README_zh.md)
- 使用更简洁，可读性更强，可维护性更高

## 参考
> [exceljs/exceljs](https://github.com/exceljs/exceljs)
