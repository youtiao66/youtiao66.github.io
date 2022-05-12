---
title: "nginx 多个 location 配置不同的 root"
categories:
  - Blog
tags:
  - nginx
---

本地开发的时候常常使用 `nginx` 搭建静态资源服务器，那么如何使用多个 `location` 配置不同的 `root` ?

<!--more-->

## 当前目录设计

```
.
├── www
│   └── index.html
└── web
    └── index.html
```

想要通过一个服务器部署 `www` 和 `web` 两个静态资源目录。

- `localhost:8080      =>  www`
- `localhost:8080/web  =>  web`



## 可能应该这样？

```
# 🚫 Examples of incorrect code
http {
    server {
        location / {
            root    /html;
        }
        location /web {
            root    /web;    # 🚫 incorrect
        }
    }
}
```

> **ℹ️注意：** 我们首先想到的解决方法可能就是这样。但实际上，**这样并不能达到我们想要的效果**

为什么会这样呢？



## root

> [root 根目录官方文档](https://nginx.org/en/docs/http/ngx_http_core_module.html#root)

| 标题   | 内容                                   |
|--------|----------------------------------------|
| 语法   | root path;                             |
| 默认值 | root html;                             |
| 上下文 | http, server, location, if in location |

设置请求的根目录。例如，以下配置

```
location /i/ {
    root /data/w3;
}
```

`/data/w3/i/top.gif` 文件将被作为 `/i/top.gif` 请求的响应。

`path` 值可以包含除 `$document_root` 和 `$realpath_root` 以外的变量。

**文件路径纯粹是由 `URI` 添加到根目录组合而成的**。如果 `URI` 被重写就应当使用 [`alias`][alias] 指令。



## 正确的做法

- 首先，更改目录设计。分别添加项目子目录

  ```
  .
  ├── www
  │   └── index.html
  └── web
      └── project
      │   └── index.html
      └── project2
          └── index.html
  ```

- 其次，更改 nginx 配置

  ```
  worker_processes  1;

  error_log  logs/error.log;
  error_log  logs/error.log  notice;
  error_log  logs/error.log  info;

  pid        logs/nginx.pid;

  events {
      worker_connections  1024;
  }

  http {
      include             mime.types;
      default_type        application/octet-stream;

      sendfile            on;
      keepalive_timeout   65;

      server {
          listen          8080;
          server_name     localhost;

          location / {
              root        /html;
          }

          location /project {
              root        /web;
          }

          location /project2 {
              root        /web;
          }
      }
  }
  ```

- 最后，实际的访问路径
  - `localhost:8080           =>  /www`
  - `localhost:8080/project   =>  /web/project`
  - `localhost:8080/project2  =>  /web/project2`

诚然，最后的实现和最开始的期望有所不同，但我想这是很多人想要达到的效果, `web` 目录本身不是项目目录而是根目录

这个问题困扰我有一段时间了，认真学习官方文档才是解决问题的最好的方式之一



## 常用 nginx 命令

| 命令              | 描述                     |
|-------------------|--------------------------|
| `start nginx`     | 开启 nginx               |
| `nginx -t`        | 测试当前配置文件是否可用 |
| `nginx -s reload` | 重启                     |
| `nginx -s stop`   | 停止                     |

### Windows 相关命令

`netstat -an | findstr 8080` 查看端口占用情况 

| 命令                                    | 描述                |
|-----------------------------------------|---------------------|
| `tasklist /fi "imagename eq nginx.exe"` | 查看`nginx`进程     |
| `taskkill /T /F /PID 18980`             | 终止指定`pid`的进程 |



[alias]: https://nginx.org/en/docs/http/ngx_http_core_module.html#alias
