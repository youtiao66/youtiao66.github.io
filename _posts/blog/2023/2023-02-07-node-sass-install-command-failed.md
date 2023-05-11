---
title: "node-sass 安装失败 Command failed 报错"
categories:
  - Blog
tags:
  - Node.js
---

通过`yarn install`安装依赖包`node-sass`时总是报错，具体的错误原因为`Command failed`。应该如何解决这个问题呢？

## 报错信息

```javascript
D:\testspace\test\node_modules\node-sass: Command failed.
Exit code: 1
Command: node scripts/build.js
Arguments:
Directory: D:\testspace\test\node_modules\node-sass
Output:
Building: C:\Program Files\nodejs\node.exe D:\testspace\test\node_modules\node-gyp\bin\node-gyp.js rebuild --verbose --libsass_ext= --libsass_cflags= --libsass_ldflags= --libsass_library=
gyp info it worked if it ends with ok
gyp verb cli [
gyp verb cli   'C:\\Program Files\\nodejs\\node.exe',
gyp verb cli   'D:\\testspace\\test\\node_modules\\node-gyp\\bin\\node-gyp.js',
gyp verb cli   'rebuild',
gyp verb cli   '--verbose',
gyp verb cli   '--libsass_ext=',
gyp verb cli   '--libsass_cflags=',
gyp verb cli   '--libsass_ldflags=',
gyp verb cli   '--libsass_library='
gyp verb cli ]
gyp info using node-gyp@3.8.0
gyp info using node@16.14.2 | win32 | x64
gyp verb command rebuild []
gyp verb command clean []
gyp verb clean removing "build" directory
gyp verb command configure []
gyp verb download using dist-url https://npm.taobao.org/dist
gyp verb check python checking for Python executable "python2" in the PATH
gyp verb `which` failed Error: not found: python2
gyp verb `which` failed     at getNotFoundError (D:\testspace\test\node_modules\which\which.js:13:12)
gyp verb `which` failed     at F (D:\testspace\test\node_modules\which\which.js:68:19)
gyp verb `which` failed     at E (D:\testspace\test\node_modules\which\which.js:80:29)
gyp verb `which` failed     at D:\testspace\test\node_modules\which\which.js:89:16
gyp verb `which` failed     at D:\testspace\test\node_modules\isexe\index.js:42:5
gyp verb `which` failed     at D:\testspace\test\node_modules\isexe\windows.js:36:5
gyp verb `which` failed     at FSReqCallback.oncomplete (node:fs:198:21)
gyp verb `which` failed  python2 Error: not found: python2
gyp verb `which` failed     at getNotFoundError (D:\testspace\test\node_modules\which\which.js:13:12)
gyp verb `which` failed     at F (D:\testspace\test\node_modules\which\which.js:68:19)
gyp verb `which` failed     at E (D:\testspace\test\node_modules\which\which.js:80:29)
gyp verb `which` failed     at D:\testspace\test\node_modules\which\which.js:89:16
gyp verb `which` failed     at D:\testspace\test\node_modules\isexe\index.js:42:5
gyp verb `which` failed     at D:\testspace\test\node_modules\isexe\windows.js:36:5
gyp verb `which` failed     at FSReqCallback.oncomplete (node:fs:198:21) {
gyp verb `which` failed   code: 'ENOENT'
gyp verb `which` failed }
gyp verb check python checking for Python executable "python" in the PATH
gyp verb `which` succeeded python C:\Users\PC\AppData\Local\Programs\Python\Python36\python.EXE
gyp ERR! configure error
gyp ERR! stack Error: Command failed: C:\Users\PC\AppData\Local\Programs\Python\Python36\python.EXE -c import sys; print "%s.%s.%s" % sys.version_info[:3];
gyp ERR! stack   File "<string>", line 1
gyp ERR! stack     import sys; print "%s.%s.%s" % sys.version_info[:3];
gyp ERR! stack                                ^
gyp ERR! stack SyntaxError: invalid syntax
gyp ERR! stack
gyp ERR! stack     at ChildProcess.exithandler (node:child_process:399:12)
gyp ERR! stack     at ChildProcess.emit (node:events:526:28)
gyp ERR! stack     at maybeClose (node:internal/child_process:1092:16)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (node:internal/child_process:302:5)
gyp ERR! System Windows_NT 10.0.22621
gyp ERR! command "C:\\Program Files\\nodejs\\node.exe" "D:\\testspace\\test\\node_modules\\node-gyp\\bin\\node-gyp.js" "rebuild" "--verbose" "--libsass_ext=" "--libsass_cflags=" "--libsass_ldflags=" "--libsass_library="
gyp ERR! cwd D:\testspace\test\node_modules\node-sass
gyp ERR! node -v v16.14.2
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok
Build failed with error code: 1
info Visit https://yarnpkg.com/en/docs/cli/install for documentation about this command.
```

## 错误原因

我的当前环境是`node v16.14.2`，我安装的版本是`node-sass v4.14.1`。报错的根本原因是`node`和`node-sass`的版本不匹配。

## 指引

以下是`node-sass`支持最低到最高版本的快速指引：

| **NodeJS** | **Supported node-sass version** | **Node Module** |
| ---------- | ------------------------------- | --------------- |
| Node 19    | 8.0+                            | 111             |
| Node 18    | 8.0+                            | 108             |
| Node 17    | 7.0+, <8.0                      | 102             |
| Node 16    | 6.0+                            | 93              |
| Node 15    | 5.0+, <7.0                      | 88              |
| Node 14    | 4.14+                           | 83              |
| Node 13    | 4.13+, <5.0                     | 79              |
| Node 12    | 4.12+, <8.0                     | 72              |
| Node 11    | 4.10+, <5.0                     | 67              |
| Node 10    | 4.9+, <6.0                      | 64              |
| Node 8     | 4.5.3+, <5.0                    | 57              |
| Node <8    | <5.0                            | <57             |

## 其他问题

### node-sass 安装慢，安装不下来

**注意：** **安装慢，安装不下来**和本文中提到的**版本不匹配**是不同的问题，不可混淆

```javascript
npm install -g mirror-config-china --registry=https://registry.npmmirror.com
npm install node-sass
```

或者

```javascript
npm config set sass_binary_site=https://registry.npmmirror.com/binary.html?path=node-sass
npm install node-sass
```

`yarn`同理。(#^.^#)我在更新这部分内容的时候，用的是`npm`

### Vue Loader - Scoped CSS

> [Vue Loader - Scoped CSS](https://vue-loader.vuejs.org/zh/guide/scoped-css.html)

**深度作用选择器：**

| 操作符     | `css` | `sass (Dart Sass)` | `node-sass` | `less` |
| ---------- | ----- | ------------------ | ----------- | ------ |
| `>>>`      | 支持  | -                  | -           | -      |
| `::v-deep` | -     | 支持               | 支持        | 支持   |
| `/deep/`   | -     | -                  | 支持        | -      |

**结论：**

- 深度作用选择器在`css`中使用`>>>`，在`sass/less`之类的预处理器中使用`::v-deep`。不使用`/deep/`
- 使用`sass`预处理器时，建议用`sass`代替`node-sass`

## 参考

- [node-sass - npm](https://www.npmjs.com/package/node-sass)
- [node-sass - npmmirror](https://npmmirror.com/package/node-sass)
- [sass-loader - webpack](https://webpack.js.org/loaders/sass-loader/)
