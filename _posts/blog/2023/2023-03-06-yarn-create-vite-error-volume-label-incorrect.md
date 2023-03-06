---
title: "yarn create vite 报错：文件名、目录名或卷标语法不正确"
categories:
  - Blog
tags:
  - Tool
---

在搭建第一个 Vite 项目时，命令行报错：The filename, directory name, or volume label syntax is incorrect. （文件名、目录名或卷标语法不正确）
## 具体错误
```javascript
D:\myspace> yarn create vite
yarn create v1.22.17
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Installed "create-vite@4.1.0" with binaries:
      - create-vite
      - cva
The filename, directory name, or volume label syntax is incorrect.
error Command failed.
Exit code: 1
Command: C:\Users\PC\AppData\Local\Yarn\bin\create-vite
Arguments:
Directory: D:\myspace
Output:

info Visit https://yarnpkg.com/en/docs/cli/create for documentation about this command.
```
## 错误原因
`Yarn`的默认安装路径在`C`盘，然而由于`C`盘空间太少，我已经把`Yarn`全局路径和缓存路径修改到`F`盘，但是我并没有修改全局可执行文件目录。由于三个路径磁盘卷标不匹配，所以会报这样的错误：`The filename, directory name, or volume label syntax is incorrect.`（文件名、目录名或卷标语法不正确）
使用一下命令可以查看全局路径、缓存路径和全局可执行文件目录：
```javascript
// 全局路径
D:\myspace> yarn global dir
F:\yarn\global
// 缓存路径
D:\myspace> yarn cache dir
F:\yarn\cache\v6
// 全局可执行文件目录
D:\myspace> yarn global bin
C:\Users\PC\AppData\Local\Yarn\bin
```
## 解决办法
把全局基础目录修改为`F:\yarn`
```javascript
yarn config set prefix 'F:\yarn'
```
然后全局可执行文件目录将自动变更。我们再一次通过命令查看全局可执行文件目录：
```javascript
D:\myspace> yarn global bin
F:\yarn\bin
```
最后，执行`yarn create vite`即可成功运行：
```javascript
D:\myspace> yarn create vite
yarn create v1.22.17
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Installed "create-vite@4.1.0" with binaries:
      - create-vite
      - cva
√ Project name: ... vite-project
√ Select a framework: » Vue
√ Select a variant: » TypeScript

Scaffolding project in D:\myspace\vite-project...

Done. Now run:

  cd vite-project
  yarn
  yarn dev

Done in 10.61s.
```
最终目录结构如下：
```javascript
F:.
└───yarn
    ├───bin
    ├───cache
    └───global
```
## 修改全局路径和缓存路径
```javascript
// 修改全局路径
yarn config set global-folder "F:\yarn\global"
// 修改缓存路径
yarn config set cache-folder "F:\yarn\cache"
```
把`F:\yarn\global\node_modules\.bin`添加系统环境变量到`Path`中。
## 参考文档

- [yarn global | Yarn](https://classic.yarnpkg.com/en/docs/cli/global)
- [yarn cache | Yarn](https://classic.yarnpkg.com/en/docs/cli/cache#toc-change-the-cache-path-for-yarn)
