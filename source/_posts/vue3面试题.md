---
title: vue3面试题
date: 2020-12-10 12:12:17
categories: 面试题
tags: vue
---

## Vue3.0里有哪些是值得我们重点关注的点

  1. 性能：
    Vue.js的发展，向来都是以提高开发与构建的速度为驱动，对比3.0和此前的Vue版本，这一点尤为明显。由于虚拟DOM已被完全重写，因此这个新版本将比以往更快。
    对于服务器端渲染，Vue.js 3.0.0的性能提高了2倍，速度提高了3倍。同时，组件的初始化现在也更加高效，甚至具有了编译器通知的快速执行路径。
  2. 代码优化（Tree-shaking）：
    在Vue.js 3.0.0中，提供了“摇树”支持，即通过"摇"我们的JS文件，将其中用不到的代码"摇"掉。具体来说，在 webpack 项目中，有一个入口文件，相当于一棵树的主干，入口文件有很多依赖的模块，相当于树枝。在实际情况中，虽然依赖了某个模块，但其实只使用其中的某些功能。通过 tree-shaking，便可将没有使用的模块摇掉，这样来达到代码优化的目的。现在，Vue中可选的大多数功能都支持“摇树”，例如过渡和v模型。这极大地减小了Vue应用程序的大小，例如一个标准HelloWorld系统现在的文件大小仅为13.5kb（通过使用composition API，它的大小甚至可以降到11.75kb）。“摇树”的出现，允许一个包括了所有运行时功能的项目大小可缩至22.5kb。这意味着即使增加了更多功能，Vue 3.0.0仍然比任何2.x版本都轻盈。
  3. Composition API
    Composition API 是一种全新的逻辑重用和代码组织方法。Vue团队主要对当前的Composition API进行了添加和改进，而不是进行重大更新，这让已经熟悉了Vue2语法的人可以更容易上手。此前，我们经常使用“options”API （如data、methods、computed等属性）来构建组件，目的就是为了将逻辑添加到Vue组件中。这种方法最大的缺点是：它本身并不是一个标准的JavaScript代码。因此，您需要确切地知道模板中可以访问哪些属性以及this关键字的行为。在底层，Vue编译器需要将此属性转换为标准代码。正因为如此，我们无法从自动建议或类型检查中获益。所以，Vue团队推出了composition API来解决这些问题，它具备了在Vue组件中使用和重用纯JS函数的灵活性和自由度。使用composition API并不意味着不能使用“options”API。相反，我们可以将 composition API与options API一起使用。（就像在React的钩子中那样）
  4. Fragments
    Vue JS将在 3.0.0版本中引入类似React Fragments的功能，该功能的主要需求是因为在之前的版本中Vue模板只能拥有一个根节点，任何Vue组件都需要绑定在单个根节点中，在3.0中将内置允许模板组件拥有多个根节点功能，这一点和React的功能类似。
  5. Teleport
    Teleport（以前称为Portal）是将子节点渲染到DOM谱系之外的DOM节点中的安全通道，例如弹出窗口甚至是模式。在此之前，使用CSS通常会遇到很多麻烦，现在Vue允许您使用Teleport 在模板部分中进行处理。我相信Teleport受到React门户的启发，并将随Vue JS的3.0.0版本一起提供。
  6. Suspense
    Suspense的提供可以让我们在应用延迟加载一些内容的同时，使加载过程可视化，这个过程可以是一个加载动画或是一个占位符，这样无疑会使用户体验更流畅，也会让程序的性能从感知层面上有一些提升。
  7. 更好的TypeScript支持
    Vue 3.0版本已经使用了TypeScript重写，对于终端用户来讲，不论用户使用的是TS还是JS，都会获得更好的编程体验，包括静态检查等。即使你用的是JS，你仍然可以得到参数的提示、类型声明，甚至可以跳进类型声明中去看源码， TS与JS在代码和API之间没有太大区别。并且，目前如果你喜欢使用Class组件，它仍受支持。
  
## Vue3.0 里为什么要用 Proxy API 替代 defineProperty API？

  1. Vue2.x中使用 defineProperty 对 data 中的属性做了遍历 + 递归，为每个属性设置了 getter、setter，这也就是为什么 Vue 只能对 data 中预定义过的属性做出响应的原因，在Vue中使用下标的方式直接修改属性的值或者添加一个预先不存在的对象属性是无法做到setter监听的，这是defineProperty的局限性。
  2. Proxy API的监听是针对一个对象的，那么对这个对象的所有操作会进入监听操作， 这就完全可以代理所有属性，将会带来很大的性能提升和更优的代码。
  3. 在 Vue.js 2.x 中，对于一个深层属性嵌套的对象，要劫持它内部深层次的变化，就需要递归遍历这个对象，执行 Object.defineProperty 把每一层对象数据都变成响应式的，这无疑会有很大的性能消耗。
  4. 在 Vue.js 3.0 中，使用 Proxy API 并不能监听到对象内部深层次的属性变化，因此它的处理方式是在 getter 中去递归响应式，这样的好处是真正访问到的内部属性才会变成响应式，简单的可以说是按需实现响应式，减少性能消耗。

## Vue3.0 编译做了哪些优化？（底层，源码）

  1. 模版渲染方面：
    在2.0里，渲染效率的快慢与组件大小成正相关：组件越大，渲染效率越慢。并且，对于一些静态节点，又无数据更新，这些遍历都是性能浪费。
    在3.0里，渲染效率不再与模板大小成正相关，而是与模板中动态节点的数量成正相关。
  2. slot 编译优化
    Vue.js 2.x 中，如果有一个组件传入了slot，那么每次父组件更新的时候，会强制使子组件update，造成性能的浪费。
    Vue.js 3.0 优化了slot的生成，使得非动态slot中属性的更新只会触发子组件的更新。
    动态slot指的是在slot上面使用v-if，v-for，动态slot名字等会导致slot产生运行时动态变化但是又无法被子组件track的操作。
  3. diff算法优化

## Vue3.0新特性 —— Composition API 与 React.js 中 Hooks的异同点

  1. React hook 底层是基于链表实现，调用的条件是每次组件被render的时候都会顺序执行所有的hooks。
  2. vue hook 只会被注册调用一次，vue 能避开这些麻烦的问题，原因在于它对数据的响应是基于proxy的，对数据直接代理观察。
  3. react 中，数据更改的时候，会导致重新render，重新render又会重新把hooks重新注册一次，所以react复杂程度会高一些。
  
## Vue3.0是如何变得更快的？

  1. diff方法优化
    Vue2.x 中的虚拟dom是进行全量的对比。
    Vue3.0 中新增了静态标记（PatchFlag）：在与上次虚拟结点进行对比的时候，值对比带有patch flag的节点，并且可以通过flag 的信息得知当前节点要对比的具体内容化。
  2. hoistStatic 静态提升
    Vue2.x : 无论元素是否参与更新，每次都会重新创建。
    Vue3.0 : 对不参与更新的元素，只会被创建一次，之后会在每次渲染时候被不停的复用。
  3. cacheHandlers 事件侦听器缓存
    默认情况下onClick会被视为动态绑定，所以每次都会去追踪它的变化但是因为是同一个函数，所以没有追踪变化，直接缓存起来复用即可。

## 如何获取当前实例

  getCurrentInstance()

## vue3.0 路由跳转的方式

  ```javascript
    //方式1
    import { useRouter } from 'vue-router';
    export default {
      setup() {
        const router = useRouter();
        function goto(){
          router.push("/about");
        }
        return{
          goto  //一定要要放在return里才能在模板上面使用
        }
      }
    }

    // 方式2
    import router from "../../router/index.js";
    router.push("/");
  ```
