# Vue

## 生命周期图示
![生命周期图示](https://cn.vuejs.org/images/lifecycle.png)

# Vuex
Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。

## State
单一状态树，用一个对象就包含了全部的应用层级状态。

## Getter
从 state 中派生出一些状态。类似于计算属性，返回值会根据它的依赖缓存起来。

## Mutation
更改状态的唯一方法是提交 mutation。

## Action
Action 类似于 mutation，不同在于：
- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。

## Vuex 和全局对象
- Vuex 的状态存储是响应式的。
- 不能直接改变 store 中的状态。