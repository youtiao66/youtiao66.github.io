---
title: "pkg: 把你的 Node.js 项目打包为可执行文件"
categories:
  - GitHub
tags:
  - Node.js
---

pkg: 把你的 Node.js 项目打包为可执行文件。这个命令行界面工具可以让你把你的 Node.js 项目打包为适用于 Linux, macOS 和 Windows 等未安装 Node.js 设备上的可执行文件。

<!--more-->

## 安装

```sh
npm install -g pkg
```



## 使用
安装以后，执行`pkg --help`查看选项列表：

```console
pkg [options] <input>

  Options:

    -h, --help           output usage information
                            输出使用帮助信息
    -v, --version        output pkg version
                            输出 pkg 版本信息
    -t, --targets        comma-separated list of targets (see examples)
                            以逗号分隔的目标列表（参考示例）
    -c, --config         package.json or any json file with top-level config  
                            package.json 或者任何 json 文件顶层配置
    --options            bake v8 options into executable to run with them on
                            将 v8 选项打包到可执行文件中，以便它们一起运行
    -o, --output         output file name or template for several files
                            输出文件名或者多个文件的输出模板
    --out-path           path to save output one or more executables
                            保存输出可执行文件的路径
    -d, --debug          show more information during packaging process [off]
                            在打包过程中展示更多信息，默认关闭
    -b, --build          don't download prebuilt base binaries, build them
                            不下载预构建的基础二进制文件，而是构建它们
    --public             speed up and disclose the sources of top-level project
                            加速和公开顶级项目的源代码
    --public-packages    force specified packages to be considered public
                            强制指定包被认定为公开的
    --no-bytecode        skip bytecode generation and include source files as plain js
                            跳过字节码生成阶段，直接打包源文件为普通 js
    --no-native-build    skip native addons build
                            跳过原生插件构建
    --no-dict            comma-separated list of packages names to ignore dictionaries. Use --no-dict * to disable all dictionaries
                            以逗号分隔的包名列表忽略字典，使用 --no-dict * 禁用所有字典
    -C, --compress       [default=None] compression algorithm = Brotli or GZip
                            压缩算法 Brotli 或者 GZip. 默认关闭

  Examples:

  – Makes executables for Linux, macOS and Windows                 打包 Linux, macOS 或者 Windows 的可执行文件
    $ pkg index.js
  – Takes package.json from cwd and follows 'bin' entry            通过当前目录下的 package.json 配置的 bin 入口打包
    $ pkg .
  – Makes executable for particular target machine                 指定目标设备
    $ pkg -t node14-win-arm64 index.js
  – Makes executables for target machines of your choice           指定多个设备和 node 版本
    $ pkg -t node12-linux,node14-linux,node14-win index.js
  – Bakes '--expose-gc' and '--max-heap-size=34' into executable
    $ pkg --options "expose-gc,max-heap-size=34" index.js
  – Consider packageA and packageB to be public
    $ pkg --public-packages "packageA,packageB" index.js
  – Consider all packages to be public
    $ pkg --public-packages "*" index.js
  – Bakes '--expose-gc' into executable
    $ pkg --options expose-gc index.js
  – reduce size of the data packed inside the executable with GZip 通过 GZip 算法压缩可执行文件中的数据包大小
    $ pkg --compress GZip index.js
```

### Targets 目标

pkg 可以同时生成多个目标设备的可执行文件。你可以通过`--targets`选项并以逗号分隔的列表来指定。一个规范的目标由以中横线连接的 3 部分组成，例如`node12-macos-x64`或者`node14-linux-arm64`:

- **node版本** (node8), node10, node12, node14, node16 or latest
- **平台** alpine, linux, linuxstatic, win, macos, (freebsd)
- **架构** x64, arm64, (armv6, armv7)



## 已打包应用的使用方法
已打包应用当前目录下执行命令`./app a b`等于`node app.js a b`

### 快照文件系统
打包过程中`pkg`把项目文件打包到可执行文件中。这就叫做快照。在运行时已打包的应用程序有权限获取快照文件系统中存在的文件

已打包的文件路径中有`/snapshot/`前缀（或者在Windows中`C:\snapshot\`）。如果使用`pkg /path/app.js`命令，然后`__filename`的值在运行时将是`/snapshot/path/app.js`. `__dirname`的值将是`/snapshot/path`. 以下是相对路径值的比较表格：

| value                         | with `node`     | packaged                 | comments                       |
|-------------------------------|-----------------|--------------------------|--------------------------------|
| \_\_filename                  | /project/app.js | /snapshot/project/app.js |                                |
| \_\_dirname                   | /project        | /snapshot/project        |                                |
| process.cwd()                 | /project        | /deploy                  | suppose the app is called ...  |
| process.execPath              | /usr/bin/nodejs | /deploy/app-x64          | `app-x64` and run in `/deploy` |
| process.argv[0]               | /usr/bin/nodejs | /deploy/app-x64          |                                |
| process.argv[1]               | /project/app.js | /snapshot/project/app.js |                                |
| process.pkg.entrypoint        | undefined       | /snapshot/project/app.js |                                |
| process.pkg.defaultEntrypoint | undefined       | /snapshot/project/app.js |                                |
| require.main.filename         | /project/app.js | /snapshot/project/app.js |                                |

### 真实文件系统路径
获取可执行文件的真实文件系统路径（外部路径，存储可执行文件的目标设备路径）。在不同设备和环境下得到的路径支持情况不同

`process.cwd()`

| value | with `node` | packaged |
|-------|-------------|----------|
| macos | ✅           | 🚫       |
| win   | -           | -        |

`process.execPath`

| value | with `node` | packaged |
|-------|-------------|----------|
| macos | 🚫          | ✅        |
| win   | -           | ✅        |

推荐在本地 Node 环境执行时通过`cross-env CURRENT=node node index.js`, 采用`process.cwd()`. 在可执行文件执行时采用 `process.execPath`

```js
// 真实文件系统目录
const realFileSystemDir = path.dirname(process.env.CURRENT === 'node' ? process.cwd() : process.execPath)
// 真实文件系统目录下所有文件
const currentDirFiles = fs.readdirSync(realFileSystemDir)
```



## 参考
> [pkg - Package your Node.js project into an executable](https://github.com/vercel/pkg)
