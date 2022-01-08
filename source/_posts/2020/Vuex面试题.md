---
title: Vuex面试题
date: 2020-12-07 18:28:27
categories: 面试题
tags: vue
---

## Vuex和单纯的全局对象有什么区别

  Vuex和全局对象主要有两大区别：
    1. Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
    2. 不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

## 为什么 Vuex 的 mutation 中不能做异步操作

  Vuex中所有的状态更新的唯一途径都是mutation，异步操作通过 Action 来提交 mutation实现，这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。
  每个mutation执行完成后都会对应到一个新的状态变更，这样devtools就可以打个快照存下来，然后就可以实现 time-travel 了。如果mutation支持异步操作，就没有办法知道状态是何时更新的，无法很好的进行状态的追踪，给调试带来困难

## vuex有哪几种属性

  有五种，分别是 State、 Getter、Mutation 、Action、 Module。

## vuex的State特性是
  
  1. Vuex就是一个仓库，仓库里面放了很多对象。其中state就是数据源存放地，对应于与一般Vue对象里面的data
  2. state里面存放的数据是响应式的，Vue组件从store中读取数据，若是store中的数据发生改变，依赖这个数据的组件也会发生更新
  3. 它通过mapState把全局的 state 和 getters 映射到当前组件的 computed 计算属性中

## vuex的Getter特性是

  1. getters 可以对State进行计算操作，它就是Store的计算属性
  2. 虽然在组件内也可以做计算属性，但是getters 可以在多组件之间复用
  3. 如果一个状态只在一个组件内使用，是可以不用getters

## vuex的Mutation , Action特性是

  1. Action 类似于 mutation，不同在于: Action 提交的是 mutation，而不是直接变更状态
  2. Action 可以包含任意异步操作
