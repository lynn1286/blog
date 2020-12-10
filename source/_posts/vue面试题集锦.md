---
title: vue面试题集锦
date: 2020-12-07 11:37:28
categories: 面试题
tags: vue
---

## 如何保留页面状态，例如列表跳转到详情回退还是在原来位置

  利用vue 内置组件 keep-alive 以及 vue-router实现

  具体做法： 在路由表中配置 meta 字段， 写入两个属性用来判断是否需要缓存并且记录 滚动条位置，在vue页面中， 使用 beforeRouteEnter 和 beforeRouteLeave两个api判断进入来源并判断meta标签的属性是否需要缓存并设置滚动条位置

  如果还需要更新列表页的数据， 那么还应该记录 用户点击的 列表id ， 并在返回的时候， 在 keep-alive 中的activated和deactivated生命周期中更新处理用户更改的那条数据

## 自己写过自定义指令吗

  自定义指令是 vue 提供给开发者对普通 DOM 元素进行底层操作api 。 它可以帮助我们实现一些通用的dom操作，如 focus 等。
  
  注册一个全局的自定义指令可以使用 Vue.directive()
  注册一个局部的自定义指令，vue文件的options还接受一个 directives 选项
  
  注册之后， 使用的方式是在任何元素上使用新的 v-xx 属性 。 这个 xx 是 directive 中的第一个参数名称， 例如 Vue.directive('focus' , {...})

  一个指令定义对象可以提供如下几个钩子函数 (均为可选)：
    1. bind：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
    2. inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
    3. update：所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 (详细的钩子函数参数见下)。
    4. componentUpdated：指令所在组件的 VNode 及其子 VNode 全部更新后调用。
    5. unbind：只调用一次，指令与元素解绑时调用。

  接下来我们来看一下钩子函数的参数 (即 el、binding、vnode 和 oldVnode)。
  
  钩子函数参数:
    1. el：指令所绑定的元素，可以用来直接操作 DOM 。
    2. binding：一个对象，包含以下属性：
      2.1 name：指令名，不包括 v- 前缀。
      2.2 value：指令的绑定值，例如：v-my-directive="1 + 1" 中，绑定值为 2。
      2.3 oldValue：指令绑定的前一个值，仅在 update 和 componentUpdated 钩子中可用。无论值是否改变都可用。
      2.4 expression：字符串形式的指令表达式。例如 v-my-directive="1 + 1" 中，表达式为 "1 + 1"。
      2.5 arg：传给指令的参数，可选。例如 v-my-directive:foo 中，参数为 "foo"。
      2.6 modifiers：一个包含修饰符的对象。例如：v-my-directive.foo.bar 中，修饰符对象为 { foo: true, bar: true }。
    3. vnode：Vue 编译生成的虚拟节点。移步 VNode API 来了解更多详情。
    4. oldVnode：上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。

  除了 el 之外，其它参数都应该是只读的，切勿进行修改。如果需要在钩子之间共享数据，建议通过元素的 dataset 来进行。
  记住，指令函数能够接受所有合法的 JavaScript 表达式。
  
## 如何强制更新例如像elementui里面的样式

  1. 新添加 style 标签 ，去除 scoped （ 慎用 ， 去污染全局 ）
  2. 深度作用选择器 ， 原生 css 你可以使用 >>> 操作符 ， 有些像 Sass 之类的预处理器无法正确解析 >>> ； 这种情况下你可以使用 /deep/ 或 ::v-deep 操作符取而代之——两者都是 >>> 的别名，同样可以正常工作。 （ scss 中使用 /deep/ 会报错的同学可以使用 ::v-deep , 具体原因是sass-loader 导致的， 感兴趣可自查 ）
  
## 为什么避免 v-if 和 v-for 用在一起，解决办法是什么

  如果直接 v-if 和 v-for 同时使用，在vue中会优先执行v-for, 当v-for把所有内容全部遍历之后 , v-if再对已经遍历的元素进行删除 , 造成了加载的浪费 。
  
  解决办法是： 使用计算属性将不需要的数据先过滤掉，再通过 v-for 显示在页面上。

## 组件传值有哪些方法

  父传子：
    1.属性props

      ```JavaScript
        // child
        props: { msg: String }

        // parent
        <HelloWorld msg="Welcome to Your Vue.js App"/>
      ```

    2. 引用refs

      ```JavaScript
        // parent

        <HelloWorld ref="hw"/>

        this.$refs.hw.xx = 'xxx' // 父组件可以读取子组件的值并修改
      ```

    3. 子组件chidren

      ```JavaScript
        // parent

        this.$children[0].xx = 'xxx' // 父组件可以通过$children或许子组件的值并修改
      ```

  子传父：
    4. 使用$emit

      ```JavaScript
      // child
      this.$emit('add', good)

      // parent
      <Cart @add="cartAdd($event)"></Cart>
      ```

  兄弟组件通信：
    5. 通过共同的祖辈组件搭桥，$parent或$root。

      ```JavaScript
        // Child2
        <h2 @click="$parent.$emit('foo',foo)">Child2</h2>

        // Child1
        this.$parent.$on('foo', (e) =>{
            console.log('1111111111111'+e);
        })
      ```

  祖先和后代通信：
    6. provide/inject：能够实现祖先给后代传值( 多个子代嵌套传值 )

      ```JavaScript
        //provide传值
        provide() {//多个子代嵌套传值
          return { myprovide: this }
        },

        // inject子组件接收
        inject:['myprovide'],
      ```

  任意两个组件通信：
    7. 事件总线：创建一个Bus类负责事件派发、监听和回调管理(单独创建一个vue实例做监听使用)

      ```JavaScript
        Vue.prototype.$bus = new Vue();

        <h3 @click="$bus.$emit('foo1')">{{msg}}</h3> // 创建响应

        this.$bus.$on('foo1', this.handle) // 收到响应
      ```

    8. vuex

## data返回为什么是返回函数

  JS中的实例是通过构造函数来创建的，每个构造函数可以new出很多个实例，那么每个实例都会继承原型上的方法或属性。
  vue的data数据其实是vue原型上的属性，数据存在于内存当中
  vue为了保证每个实例上的data数据的独立性，规定了必须使用函数，而不是对象。
  因为使用对象的话，每个实例（组件）上使用的data数据是相互影响的，这当然就不是我们想要的了。对象是对于内存地址的引用，直接定义个对象的话组件之间都会使用这个对象，这样会造成组件之间数据相互影响。

## 说说你对 SPA 单页面的理解，它的优缺点分别是什么

  SPA（ single-page application ）仅在 Web 页面初始化时加载相应的 HTML、JavaScript 和 CSS。一旦页面加载完成，SPA 不会因为用户的操作而进行页面的重新加载或跳转；取而代之的是利用路由机制实现 HTML 内容的变换，UI 与用户的交互，避免页面的重新加载。
  
  优点：
    1. 用户体验好、快，内容的改变不需要重新加载整个页面，避免了不必要的跳转和重复渲染；
    2. 基于上面一点，SPA 相对对服务器压力小；
    3. 前后端职责分离，架构清晰，前端进行交互逻辑，后端负责数据处理；

  缺点:
    1. 初次加载耗时多：为实现单页 Web 应用功能及显示效果，需要在加载页面的时候将 JavaScript、CSS 统一加载，部分页面按需加载；
    2. 前进后退路由管理：由于单页应用在一个页面中显示所有的内容，所以不能使用浏览器的前进后退功能，所有的页面切换需要自己建立堆栈管理；
    3. SEO 难度较大：由于所有的内容都在一个页面中动态替换显示，所以在 SEO 上其有着天然的弱势。

## v-show 与 v-if 有什么区别

  v-if 是真正的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建；也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

  v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 的 “display” 属性进行切换。
  
  所以，v-if 适用于在运行时很少改变条件，不需要频繁切换条件的场景；v-show 则适用于需要非常频繁切换条件的场景。

## 怎样理解 Vue 的单向数据流

  所有的 prop 都使得其父子 prop 之间形成了一个单向下行绑定: 父级 prop 的更新会向下流动到子组件中，但是反过来则不行。这样会防止从子组件意外改变父级组件的状态，从而导致你的应用的数据流向难以理解。
  
  额外的，每次父级组件发生更新时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你不应该在一个子组件内部改变 prop。如果你这样做了，Vue 会在浏览器的控制台中发出警告。子组件想修改时，只能通过 $emit 派发一个自定义事件，父组件接收到后，由父组件修改。
  
## computed 和 watch 的区别和运用的场景

  computed： 是计算属性，依赖其它属性值，并且 computed 的值有缓存，只有它依赖的属性值发生改变，下一次获取 computed 的值时才会重新计算 computed 的值；
  watch： 更多的是「观察」的作用，类似于某些数据的监听回调 ，每当监听的数据变化时都会执行回调进行后续操作；

  运用场景：
    1. 当我们需要进行数值计算，并且依赖于其它数据时，应该使用 computed，因为可以利用 computed 的缓存特性，避免每次获取值时，都要重新计算；
    2. 当我们需要在数据变化时执行异步或开销较大的操作时，应该使用 watch，使用 watch 选项允许我们执行异步操作 ( 访问一个 API )，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。这些都是计算属性无法做到的。

## 直接给一个数组项赋值，Vue 能检测到变化吗

  由于 JavaScript 的限制，Vue 不能检测到以下数组的变动：
    1. 当你利用索引直接设置一个数组项时，例如：vm.items[indexOfItem] = newValue
    2. 当你修改数组的长度时，例如：vm.items.length = newLength

  为了解决第一个问题，Vue 提供了以下操作方法：

    ```JavaScript
        // Vue.set
        Vue.set(vm.items, indexOfItem, newValue)
        // vm.$set，Vue.set的一个别名
        vm.$set(vm.items, indexOfItem, newValue)
        // Array.prototype.splice
        vm.items.splice(indexOfItem, 1, newValue)
    ```

  为了解决第二个问题，Vue 提供了以下操作方法：

    ```JavaScript
      // Array.prototype.splice
      vm.items.splice(newLength)
    ```

## 谈谈你对 Vue 生命周期的理解

  1. 生命周期是什么?
    Vue 实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模版、挂载 Dom -> 渲染、更新 -> 渲染、卸载等一系列过程，我们称这是 Vue 的生命周期。
  
  2. 各个生命周期的作用
    Vue 的生命周期总共有 8 个 , 分别是：

        |  生命周期   | 描述  |
        |  ----  | ----  |
        | beforeCreate  | 创建前 ， 此时data对象和 $el 都为 undefined ，一般此处可以加 loading 事件 |
        | created  | 创建后，此时data对象已经有了， 但是 $el 还没有 ， 此处可以做些 关闭 loading 事件 ，异步请求等 |
        | beforeMount  | 渲染前 ， 此时data对象和$el都初始化了，但还是虚拟的dom节点 |
        | mounted  | 渲染后 ， 实例挂载完毕，此阶段可以做的事情： 配合路由钩子使用 |
        | beforeUpdate  | 更新前， data更新时触发 |
        | update  | 更新后， data更新时触发 ， 此处可以在数据更新时做些处理， 也可以使用 watch 进行观测 |
        | beforeDestory  | 销毁前 ， 组件销毁时触发， 此阶段可做的事情：询问用户是否销毁 |
        | destoryed  | 销毁后， 组件销毁时触发，此时 实例解除了事件监听以及dom的绑定，但是dom节点依旧存在，此阶段可以做的事情： 组件销毁时进行提示 |

        补充两个 keep-alive 专属生命周期：

        |  keep-alive 专属生命周期   | 描述  |
        |  ----  | ----  |
        | activited  | 组件被激活时调用 |
        | deactivated  | 组件被销毁时调用 |

## Vue 的父组件和子组件生命周期钩子函数执行顺序

  Vue 的父组件和子组件生命周期钩子函数执行顺序可以归类为以下 4 部分：
    1. 加载渲染过程： 父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted
    2. 子组件更新过程： 父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父
    3. 父组件更新过程： 父 beforeUpdate -> 父 updated
    4. 销毁过程： 父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed

## 在哪个生命周期内调用异步请求

  可以在钩子函数 created、beforeMount、mounted 中进行调用，因为在这三个钩子函数中，data 已经创建，可以将服务端端返回的数据进行赋值。但是本人推荐在 created 钩子函数中调用异步请求，因为在 created 钩子函数中调用异步请求有以下优点：
    1. 能更快获取到服务端数据，减少页面 loading 时间；
    2. ssr 不支持 beforeMount 、mounted 钩子函数，所以放在 created 中有助于一致性；

## 在什么阶段才能访问操作DOM

  在钩子函数 mounted 被调用前，Vue 已经将编译好的模板挂载到页面上，所以在 mounted 中可以访问操作 DOM。
  
## 父组件可以监听到子组件的生命周期吗

  可以 子组件在对应的生命周期内 $emit 告诉 父组件
  
## 谈谈你对 keep-alive 的了解

  keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，避免重新渲染 ，其有以下特性：
    1. 一般结合路由和动态组件一起使用，用于缓存组件；
    2. 提供 include 和 exclude 属性，两者都支持字符串或正则表达式， include 表示只有名称匹配的组件会被缓存，exclude 表示任何名称匹配的组件都不会被缓存 ，其中 exclude 的优先级比 include 高；
    3. 对应两个钩子函数 activated 和 deactivated ，当组件被激活时，触发钩子函数 activated，当组件被移除时，触发钩子函数 deactivated。

## v-model 的原理

  我们在 vue 项目中主要使用 v-model 指令在表单 input、textarea、select 等元素上创建双向数据绑定，我们知道 v-model 本质上不过是语法糖，v-model 在内部为不同的输入元素使用不同的属性并抛出不同的事件：
    1. text 和 textarea 元素使用 value 属性和 input 事件；
    2. checkbox 和 radio 使用 checked 属性和 change 事件；
    3. select 字段将 value 作为 prop 并将 change 作为事件。

        ```JavaScript
          <input v-model='something'>

          // 相当于

          <input v-bind:value="something" v-on:input="something = $event.target.value">
        ```

  如果在自定义组件中，v-model 默认会利用名为 value 的 prop 和名为 input 的事件，如下所示：

      ```JavaScript
        // 父组件：
        <ModelChild v-model="message"></ModelChild>

        // 子组件：
        <div>{{value}}</div>

        props:{
          value: String
        },
        methods: {
          test1(){
            this.$emit('input', '小红')
          },
        },
      ```

## 你使用过 Vuex 吗

  Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。每一个 Vuex 应用的核心就是 store（仓库）。“store” 基本上就是一个容器，它包含着你的应用中大部分的状态 ( state )。
    1. Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
    2. 改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation。这样使得我们可以方便地跟踪每一个状态的变化。

  主要包括以下几个模块：
    1. State：定义了应用状态的数据结构，可以在这里设置默认的初始状态。
    2. Getter：允许组件从 Store 中获取数据，mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性。
    3. Mutation：是唯一更改 store 中状态的方法，且必须是同步函数。
    4. Action：用于提交 mutation，而不是直接变更状态，可以包含任意异步操作。
    5. Module：允许将单一的 Store 拆分为多个 store 且同时保存在单一的状态树中。

## 使用过 Vue SSR 吗？说说 SSR

  SSR大致的意思就是vue在客户端将标签渲染成的整个 html 片段的工作在服务端完成，服务端形成的html 片段直接返回给客户端这个过程就叫做服务端渲染。

  服务端渲染的优点：
    1. 更好的 SEO
    2. 更快的内容到达时间（首屏加载更快）

  服务端渲染的缺点：
    1. 更多的开发条件限制：  例如服务端渲染只支持 beforCreate 和 created 两个钩子函数，这会导致一些外部扩展库需要特殊处理，才能在服务端渲染应用程序中运行；并且与可以部署在任何静态文件服务器上的完全静态单页面应用程序 SPA 不同，服务端渲染应用程序，需要处于 Node.js server 运行环境；
    2. 更多的服务器负载：在 Node.js  中渲染完整的应用程序，显然会比仅仅提供静态文件的  server 更加大量占用CPU 资源 (CPU-intensive - CPU 密集)，因此如果你预料在高流量环境 ( high traffic ) 下使用，请准备相应的服务器负载，并明智地采用缓存策略。

## vue-router 路由模式有几种

  vue-router 有 3 种路由模式：hash、history、abstract：
    1. hash: 使用 URL hash 值来作路由。支持所有浏览器，包括不支持 HTML5 History Api 的浏览器；
    2. history : 依赖 HTML5 History API 和服务器配置。具体可以查看 HTML5 History 模式；
    3. abstract : 支持所有 JavaScript 运行环境，如 Node.js 服务器端。如果发现没有浏览器的 API，路由会自动强制进入这个模式.

## Vue 是如何实现数据双向绑定的

  Vue 数据双向绑定主要是指：数据变化更新视图，视图变化更新数据。

  主要通过以下 4 个步骤来实现数据双向绑定的：
    1. 实现一个监听器 Observer：对数据对象进行遍历，包括子属性对象的属性，利用 Object.defineProperty() 对属性都加上 setter 和 getter。这样的话，给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化。
    2. 实现一个解析器 Compile：解析 Vue 模板指令，将模板中的变量都替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，调用更新函数进行数据更新。
    3. 实现一个订阅者 Watcher：Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁 ，主要的任务是订阅 Observer 中的属性值变化的消息，当收到属性值变化的消息时，触发解析器 Compile 中对应的更新函数。
    4. 实现一个订阅器 Dep：订阅器采用 发布-订阅 设计模式，用来收集订阅者 Watcher，对监听器 Observer 和 订阅者 Watcher 进行统一管理。

## Vue 框架怎么实现对象和数组的监听

  如果被问到 Vue 怎么实现数据双向绑定，大家肯定都会回答 通过 Object.defineProperty() 对数据进行劫持，但是  Object.defineProperty() 只能对属性进行数据劫持，不能对整个对象进行劫持，同理无法对数组进行劫持，但是我们在使用 Vue 框架中都知道，Vue 能检测到对象和数组（部分方法的操作）的变化，那它是怎么实现的呢？我们查看相关代码如下：

    ```JavaScript
    /**
    * Observe a list of Array items.
    */
    observeArray (items: Array<any>) {
      for (let i = 0, l = items.length; i < l; i++) {
        observe(items[i])  // observe 功能为监测数据的变化 
      }
    }

    /**
    * 对属性进行递归遍历
    */
    let childOb = !shallow && observe(val) // observe 功能为监测数据的变化
    ```

  通过以上 Vue 源码部分查看，我们就能知道 Vue 框架是通过遍历数组 和递归遍历对象，从而达到利用 Object.defineProperty() 也能对对象和数组（部分方法的操作）进行监听。

  拓展： 在Vue2.x中数组变化监听的问题，其实不是Object.definePropertype方法监听不到，而是为了性能和收益比例综合考虑之下，改变了监听方式 , 从原本的直接监听结果变化这种思路变换到监听会导致结果变化的方法上（重写数组的部分方法（七种： push，pop，shift，unshift，splice, sort，reverse））。
  Vue3.0中利用Proxy的方式则完美解决了2.0中出现的问题

## Proxy 与 Object.defineProperty 优劣对比

  Proxy 的优势如下:
    1. Proxy 可以直接监听对象而非属性；
    2. Proxy 可以直接监听数组的变化；
    3. Proxy 有多达 13 种拦截方法,不限于 apply、ownKeys、deleteProperty、has 等等是 Object.defineProperty 不具备的；
    4. Proxy 返回的是一个新对象,我们可以只操作新的对象达到目的,而 Object.defineProperty 只能遍历对象属性直接修改；
    5. Proxy 作为新标准将受到浏览器厂商重点持续的性能优化，也就是传说中的新标准的性能红利；

  Object.defineProperty 的优势如下:
    兼容性好，支持 IE9，而 Proxy 的存在浏览器兼容性问题,而且无法用 polyfill 磨平，因此 Vue 的作者才声明需要等到下个大版本( 3.0 )才能用 Proxy 重写。

## Vue 怎么用 vm.$set() 解决对象新增属性不能响应的问题

  受现代 JavaScript 的限制 ，Vue 无法检测到对象属性的添加或删除。由于 Vue 会在初始化实例时对属性执行 getter/setter 转化，所以属性必须在 data 对象上存在才能让 Vue 将它转换为响应式的。但是 Vue 提供了 Vue.set (object, propertyName, value) / vm.$set (object, propertyName, value)  来实现为对象添加响应式属性，那框架本身是如何实现的呢？

  我们查看对应的 Vue 源码：vue/src/core/instance/index.js

    ```JavaScript
      export function set (target: Array<any> | Object, key: any, val: any): any {
        // target 为数组  
        if (Array.isArray(target) && isValidArrayIndex(key)) {
          // 修改数组的长度, 避免索引>数组长度导致splcie()执行有误
          target.length = Math.max(target.length, key)
          // 利用数组的splice变异方法触发响应式  
          target.splice(key, 1, val)
          return val
        }
        // key 已经存在，直接修改属性值  
        if (key in target && !(key in Object.prototype)) {
          target[key] = val
          return val
        }
        const ob = (target: any).__ob__
        // target 本身就不是响应式数据, 直接赋值
        if (!ob) {
          target[key] = val
          return val
        }
        // 对属性进行响应式处理
        defineReactive(ob.value, key, val)
        ob.dep.notify()
        return val
      }
    ```

  我们阅读以上源码可知，vm.$set 的实现原理是：
    1. 如果目标是数组，直接使用数组的 splice 方法触发相应式；
    2. 如果目标是对象，会先判读属性是否存在、对象是否是响应式，最终如果要对属性进行响应式处理，则是通过调用   defineReactive 方法进行响应式处理（ defineReactive 方法就是  Vue 在初始化对象时，给对象属性采用 Object.defineProperty 动态添加 getter 和 setter 的功能所调用的方法）

## 虚拟 DOM 的优缺点

  优点：
    1. 保证性能下限
    2. 无需手动操作 DOM
    3. 跨平台
  
  缺点:
    1. 无法进行极致优化

## 虚拟 DOM 实现原理

  虚拟 DOM 的实现原理主要包括以下 3 部分:
    1. 用 JavaScript 对象模拟真实 DOM 树，对真实 DOM 进行抽象；
    2. diff 算法 — 比较两棵虚拟 DOM 树的差异；
    3. pach 算法 — 将两个虚拟 DOM 对象的差异应用到真正的 DOM 树。

## Vue 中的 key 有什么作用

  key 是为 Vue 中 vnode 的唯一标记，通过这个 key，我们的 diff 操作可以更准确、更快速。
  
## 你有对 Vue 项目进行哪些优化

  1. 代码层面的优化
    v-if 和 v-show 区分使用场景
    computed 和 watch  区分使用场景
    v-for 遍历必须为 item 添加 key，且避免同时使用 v-if
    长列表性能优化
    事件的销毁
    图片资源懒加载
    路由懒加载
    第三方插件的按需引入
    优化无限列表性能
    服务端渲染 SSR or 预渲染
  2. Webpack 层面的优化
    Webpack 对图片进行压缩
    减少 ES6 转为 ES5 的冗余代码
    提取公共代码
    模板预编译
    提取组件的 CSS
    优化 SourceMap
    构建结果输出分析
    Vue 项目的编译优化
  3. 基础的 Web 技术的优化
    开启 gzip 压缩
    浏览器缓存
    CDN 的使用
    使用 Chrome Performance 查找性能瓶颈

## Vue 中 v-html 会导致什么问题

  在网站上动态渲染任意 HTML，很容易导致 XSS 攻击。所以只能在可信内容上使用 v-html，且永远不能用于用户提交的内容上。

## nextTick是做什么用的，其原理是什么

  定义：在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。
  理解：nextTick()，是将回调函数延迟在下一次dom更新数据后调用，简单的理解是：当数据更新了，在dom中渲染后，自动执行该函数

  原理：
    nextTick() 涉及到js线程中的微任务和宏任务.在一次事件循环中,先执行微任务,再执行宏任务。
    查询浏览器支持的程度,先后执行
      1. promise
      2. MutationObserve
      3. setTimeout
    nextTick好处: 碰到太频繁的js操作,只需要显示最后一次的数据的视图,如果每次都实时更新视图,会消耗太多性能

## Vue 的模板编译原理

  vue模板的编译过程分为3个阶段：
    1. 第一步：解析。 将模板字符串解析生成 AST，生成的AST 元素节点总共有 3 种类型，1 为普通元素， 2 为表达式，3为纯文本。
    2. 第二步：优化语法树。 Vue 模板中并不是所有数据都是响应式的，有很多数据是首次渲染后就永远不会变化的，那么这部分数据生成的 DOM 也不会变化，我们可以在 patch 的过程跳过对他们的比对。
    此阶段会深度遍历生成的 AST 树，检测它的每一颗子树是不是静态节点，如果是静态节点则它们生成 DOM 永远不需要改变，这对运行时对模板的更新起到极大的优化作用。
    3. 生成代码： 通过 generate 方法，将ast生成 render 函数。

## 为什么Vue 采用异步渲染

  因为如果不采用异步更新，那么每次更新数据都会对当前组件进行重新渲染，所以为了性能考虑，
  Vue 会在本轮数据更新后，再去异步更新数据；

## vue常用的修饰符

  1. 按键修饰符: 如：.delete（捕获“删除”和”退格“键）
    用法上和事件修饰符一样，挂载在v-on:后面，语法：v-on:keyup.xxx=’yyy’

          ```html
            <inputclass = 'aaa' v-model="inputValue" @keyup.delete="onKey"/>
          ```

  2. 系统修饰符
    可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器:
      2.1 .ctrl
      2.2 .alt
      2.3 .shift
      2.4 .meta

  3. 鼠标按钮修饰符
    .left
    .right
    .middle

  4. 其他修饰符
    .lazy
    .number
    .trim
