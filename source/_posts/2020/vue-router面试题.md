---
title: vue-router面试题
date: 2020-12-08 16:50:26
categories: 面试题
tags: vue
---

## Vue-router 导航守卫有哪些

  全局前置/钩子：beforeEach、beforeResolve、afterEach
  路由独享的守卫：beforeEnter
  组件内的守卫：beforeRouteEnter、beforeRouteUpdate、beforeRouteLeave

## vue-router hash 模式和 history 模式有什么区别

  区别：
    1. url 展示上，hash 模式有“#”，history 模式没有
    2. 刷新页面时，hash 模式可以正常加载到 hash 值对应的页面，而 history 没有处理的话，会返回 404，一般需要后端将所有页面都配置重定向到首页路由。
    3. 兼容性。hash 可以支持低版本浏览器和 IE。

## vue-router hash 模式和 history 模式是如何实现的

  hash 模式：
    #后面 hash 值的变化，不会导致浏览器向服务器发出请求，浏览器不发出请求，就不会刷新页面。同时通过监听 hashchange 事件可以知道 hash 发生了哪些变化，然后根据 hash 变化来实现更新页面部分内容的操作。

  history 模式：
    history 模式的实现，主要是 HTML5 标准发布的两个 API，pushState 和 replaceState，这两个 API 可以在改变 url，但是不会发送请求。这样就可以监听 url 变化来实现更新页面部分内容的操作。

## $route和 $router的区别是什么

  $router为VueRouter的实例，是一个全局路由对象，包含了路由跳转的方法、钩子函数等。
  $route 是路由信息对象||跳转的路由对象，每一个路由都会有一个route对象，是一个局部对象，包含path,params,hash,query,fullPath,matched,name等路由信息参数。

## vue-router 传参

  1.使用Params:
    1.1 只能使用name，不能使用path
    1.2 参数不会显示在路径上
    1.3 浏览器强制刷新参数会被清空

  2.使用Query:
    2.1 参数会显示在路径上，刷新不会被清空
    2.2 name 可以使用path路径
