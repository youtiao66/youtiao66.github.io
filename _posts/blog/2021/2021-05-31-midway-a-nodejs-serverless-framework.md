---
title: "Midway - 一个面向未来的云端一体 `Node.js` 框架"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - Node.js
toc: true
toc_label: "目录"
toc_icon: "cog"
---

Midway 是一个适用于构建 Serverless 服务，传统应用、微服务，小程序后端的 Node.js 框架

<!--more-->

## 基础介绍
Midway (中途岛) 是淘系架构团队（前淘宝UED）研发的一款面向未来的的 Node.js 框架。在大规模编程和 Serverless 等多种场景中，Midway 通过 TypeScript 和完全自研的**依赖注入**能力，将用户体验打造到极致。
Midway 2.0 集成了 `Serverless` 能力，同时扩展了 RPC、Socket、**微服务**、前后端一体化研发等能力，不同的场景之间可以组合、协作，给用户提供相对灵活又可靠的使用体验。

## 概述
### Web 框架选择
Midway 设计之初就可以兼容多种上层框架，在 Web 场景默认封装了 Egg.js 作为上层的 Web 框架，同时，Midway 也提供了其他的 Web 框架选择，比如常见的 `Express` 和 `Koa` 。

### 控制器（Controller）
在常见的 MVC 架构中，C 即代表控制器，控制器用于负责**解析用户的输入，处理后返回相应的结果。**
![MVC Controller](https://cdn.nlark.com/yuque/0/2020/png/501408/1600592027849-679b4cfc-cf11-466a-a467-403907bd6a3e.png?x-oss-process=image%2Fresize%2Cw_1492)

> **装饰器**（Decorator）模式 的定义：指在不改变现有对象结构的情况下，动态地给该对象增加一些职责（即增加其额外功能）的模式，它属于对象结构型模式。

- 路由
- 路由方法
- 请求参数
- 状态码
- 响应头
- 重定向
- 响应类型
- 优先级

### 服务和注入
在业务中，只有控制器（Controller）的代码是不够的，一般来说会有一些业务逻辑被抽象到一个特定的逻辑单元中，我们一般称为服务（Service）。
![MVC Service](https://cdn.nlark.com/yuque/0/2020/png/501408/1600604974682-f5309741-dda9-484b-bcf3-ac054f98fe78.png?x-oss-process=image%2Fresize%2Cw_1492)

- 创建服务
- 使用服务
- 注入行为描述
- 注入参数

#### 使用服务
在 Controller 处，我们需要来调用这个服务。传统的代码写法，我们需要初始化这个 Class（new），然后将实例放在需要调用的地方。在 Midway 中，你**不需要这么做**，只需要编写我们提供的 "**依赖注入**" 的代码写法。

```ts
import { Inject, Controller, Post, Provide, Query } from '@midwayjs/decorator';
import { UserService } from '../service/user';

@Provide()
@Controller('/api/user')
export class APIController {

  @Inject()
  userService: UserService;

  @Get('/')
  async getUser(@Query('id') uid) {
    const user = await this.userService.getUser(uid);
    return {success: true, message: 'OK', data: user};
  }
}
```

Midway 的核心 “依赖注入” 容器会**自动关联**你的控制器（Controller） 和服务（Service），在运行过程中**会自动初始化**所有的代码，你**无需手动初始化**这些 Class。

## 请求、响应和应用
Midway 框架会根据不同的场景来启动不同的应用，前文提到，我们默认选用 EggJS 作为我们的 Web 框架，也可以使用 Express 或者 Koa。

每个使用的 Web 框架会提供自己独特的能力，这些独特的能力都会体现在各自的 **请求和响应**（Context）和 **应用**（Application）之上。

- 上下文和应用定义约定
- 请求和响应
- 应用实例

## Web 中间件
Web 中间件是在控制器调用 **之前** 和 **之后（部分）** 调用的函数。 中间件函数可以访问请求和响应对象。
![Web 中间件](https://cdn.nlark.com/yuque/0/2020/png/501408/1600592120947-c000a3a8-5da1-4a8d-839a-c6f81b771577.png?x-oss-process=image%2Fresize%2Cw_1492)

[洋葱圈模型](https://eggjs.org/zh-cn/intro/egg-and-koa.html#midlleware)

## 启动和部署
### 使用 Docker 部署

## 基础能力
### 依赖注入
### 运行环境
### 多环境配置
### 参数校验和转换
### 生命周期
### 使用组件
### 日志
### 本地调试
### 测试

## 增强
### 代码流程控制
### 方法拦截器（切面）
### 多框架研发
### 缓存（Cache）
### Database（TypeORM）（暂时不用）
### MongoDB（暂时不用）
### Swagger（暂时不用）
### Redis

## EggJS

## Web 技术
### Cookies
### Session
### BodyParser
### 静态资源（Static File）
### 跨域 CORS

## 微服务
### gRPC
### RabbitMQ（暂时不用）
### Consul

## WebSocket - SocketIO

## Serverless

