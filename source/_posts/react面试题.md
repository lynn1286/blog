---
title: react面试题
date: 2020-12-08 17:08:19
categories: 面试题
tags: react
---

## 为什么要使用pureComponent

  当使用component时，父组件的state或prop更新时，无论子组件的state、prop是否更新，都会触发子组件的更新，这会形成很多没必要的render，浪费很多性能；pureComponent的优点在于：pureComponent在shouldComponentUpdate只进行浅层的比较，只要外层对象没变化，就不会触发render,减少了不必要的render，当遇到复杂数据结构时，可以将一个组件拆分成多个pureComponent，以这种方式来实现复杂数据结构，以期达到节省不必要渲染的目的，如：表单、复杂列表、文本域等情况；

## purecomponent 和 component 区别

  1. pureComponent通过prop和state的浅比较（shallowEqual）来实现shouldComponentUpdate,
  2. component是需要开发者在shouldComponentUpdate钩子函数中自己写render逻辑的，在某些情况下可以使用pureComponent来提升性能。

  浅比较（shallowEqual）：是react源码中的一个函数，它代替了shouldComponentUpdate的工作, 只比较外层数据结构，只要外层相同，则认为没有变化，不会深层次比较数据。

## pureComponent的优缺点

  优点：
    不需要开发者使用shouldComponentUpdate就可使用简单的判断来提升性能；
  缺点：
    由于进行的是浅比较，可能由于深层的数据不一致导致而产生错误的否定判断，从而导致页面得不到更新；

## setState是同步还是异步的

  1. setState只在合成事件(例如： onClick、onChange )和钩子函数(生命周期)中是“异步”的，在原生事件(例如： addEventListener ， 或者原生js、jq直接 document.querySelector().onclick)和setTimeout 中都是同步的。
  2. setState 的“异步”并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形成了所谓的“异步”，当然可以通过第二个参数 setState(partialState, callback) 中的callback拿到更新后的结果。
  3. setState 的批量更新优化也是建立在“异步”（合成事件、钩子函数）之上的，在原生事件和setTimeout 中不会批量更新，在“异步”中如果对同一个值进行多次setState，setState的批量更新策略会对其进行覆盖，取最后一次的执行，如果是同时setState多个不同的值，在更新时会对其进行合并批量更新。

## React生命周期

  挂载过程， 当组件实例被创建并插入DOM中时，其生命周期调用顺序如下：
    1. constructor()
    2. static getDerivedStateFromProps()
    3. render()
    4. componentDidMount()

  注意： 在这个阶段的componentWillMount()生命周期即将过时，在新代码中应该避免使用。

  更新过程， 当组件的props或state发生变化时会触发更新，组件更新的生命周期调用顺序如下：
    1. static getDerivedStateFromProps()
    2. shouldComponentUpdate()
    3. render()
    4. getSnapshotBeforeUpdate()
    5. componentDidUpdate()
  
  注意： 在这个阶段的componentWillUpdate()、componentWillReceiveProps()生命周期即将过时，在新代码中应该避免使用。

  卸载过程， 当组件从DOM中移除时，组件更新的生命周期调用顺序如下：
    1. componentWillUnmount()

  错误处理， 当渲染过程，生命周期，或子组件的构造函数中抛出错误时，会调用如下方法：
    1. static getDerivedStateFromError()
    2. componentDidCatch()

## 常用React Hooks 方法

  1. useState
    useState相当于class的state的赋值操作
  2. useEffect
    useEffect Hook 可以看做 componentDidMount，componentDidUpdate 和 componentWillUnmount 这三个函数的组合。通常用于请求数据，事件处理，订阅等相关操作
  3. useLayoutEffect
    在大多数情况下，我们都可以使用useEffect处理副作用，但是，如果副作用是跟DOM相关的，就需要使用useLayoutEffect。useLayoutEffect中的副作用会在DOM更新之后同步执行。
  4. useReducer
    useState 的替代方案。它接收一个形如 (state, action) => newState 的 reducer，并返回当前的 state 以及与其配套的 dispatch 方法。
  5. useContext
    接收一个 context 对象（React.createContext 的返回值）并返回该 context 的当前值。当前的 context 值由上层组件中距离当前组件最近的 <MyContext.Provider> 的 value prop 决定。
  6. useRef
    相当于class中的this
  7. useMemo
    用于缓存计算结果。类似vue的计算属性，函数只依赖count的变化
  8. useCallback
    useCallback(fn, deps) 相当于 useMemo(() => fn, deps)

## 为什么使用key

  react的diff算法是把key当成唯一id然后比对组件的value来确定是否需要更新的，所以如果没有key，react将不会知道该如何更新组件。
  不传默认使用数组的索引作为key。
  react根据key来决定是销毁重新创建组件还是更新组件，原则是：
    1. key相同，组件有所变化，react会只更新组件对应变化的属性。
    2. key不同，组件会销毁之前的组件，将整个组件重新渲染。

## Redux及其工作原理

    Redux 是 React的一个状态管理库，它基于flux。 Redux简化了React中的单向数据流。 Redux将状态管理完全从React中抽象出来。

    工作原理： 
      1. 组件连接到 redux ，如果要访问 redux，需要派出一个包含 id和负载(payload) 的 action。action 中的 payload 是可选的，action 将其转发给 Reducer。
      2. 当reducer收到action时，通过 swithc…case 语法比较 action 中type。 匹配时，更新对应的内容返回新的 state。
      3. 当Redux状态更改时，连接到Redux的组件将接收新的状态作为props。当组件接收到这些props时，它将进入更新阶段并重新渲染 UI。

## 什么是 Fragments

  React 中一个常见模式是为一个组件返回多个元素。Fragments 可以让你聚合一个子元素列表，并且不在DOM中增加额外节点。
  
## 什么是传送门(Portals)

  和vue3新增的teleport一样，是 Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案。

## 如何提高性能

  1. 减少渲染的节点/降低渲染计算量(复杂度)
    1.1 不要在渲染函数都进行不必要的计算
    1.2 减少不必要的嵌套
    1.3 虚拟列表只渲染当前视口可见元素:
    1.4 惰性渲染的初衷本质上和虚表一样，也就是说我们只在必要时才去渲染对应的节点。
    1.5 选择合适的样式方案
  2. 避免重新渲染
    减少不必要的重新渲染也是 React 组件性能优化的重要方向. 为了避免不必要的组件重新渲染需要在做到两点:
    2.1 保证组件纯粹性。即控制组件的副作用，如果组件有副作用则无法安全地缓存渲染结果
    2.2 通过shouldComponentUpdate生命周期函数来比对 state 和 props, 确定是否要重新渲染。对于函数组件可以使用React.memo包装
  3. 简化 props ， 如果一个组件的 props 太复杂一般意味着这个组件已经违背了‘单一职责’，首先应该尝试对组件进行拆解. 另外复杂的 props 也会变得难以维护, 比如会影响shallowCompare效率, 还会让组件的变动变得难以预测和调试. 简化的 props 更容易理解, 且可以提高组件缓存的命中率
  4. 不变的事件处理器
    4.1 避免使用箭头函数形式的事件处理器(假设组件是一个复杂的 PureComponent, 这里使用箭头函数，其实每次渲染时都会创建一个新的事件处理器，这会导致组件始终会被重新渲染.)
    4.2 使用useCallback来包装事件处理器，尽量给下级组件暴露一个静态的函数
    4.3 设计更方便处理的 Event Props
  5. 不可变数据： 不可变数据可以让状态变得可预测，也让 shouldComponentUpdate '浅比较'变得更可靠和高效.
  6. 简化 state： 不是所有状态都应该放在组件的 state 中. 例如缓存数据。
  7. 使用 recompose 精细化比对
  8. 精细化渲染
    8.1 响应式数据的精细化渲染： 大部分情况下，响应式数据可以实现视图精细化的渲染, 但它还是不能避免开发者写出低效的程序. 本质上还是因为组件违背‘单一职责’.
    8.2 不要滥用 Context： 首先要理解 Context API 的更新特点，它是可以穿透React.memo或者shouldComponentUpdate的比对的，也就是说，一旦 Context 的 Value 变动，所有依赖该 Context 的组件会全部 forceUpdate.使用 Context API 要遵循一下原则:明确状态作用域, Context 只放置必要的，关键的，被大多数组件所共享的状态。比较典型的是鉴权状态

## 如何在重新加载页面时保留数据

  每当重新加载应用程序时，我们使用浏览器localstorage来保存应用程序的状态。我们将整个存储数据保存在localstorage中，每当有页面刷新或重新加载时，我们从localstorage加载状态。

## 什么是虚拟DOM

  虚拟DOM（VDOM）它是真实DOM的内存表示,一种编程概念，一种模式。它会和真实的DOM同步，比如通过ReactDOM这种库，这个同步的过程叫做调和(reconcilation)。
  虚拟DOM更多是一种模式，不是一种特定的技术。

## 类组件和函数组件之间有什么区别

  1. 类组件（Class components）
    1.1 无论是使用函数或是类来声明一个组件，它决不能修改它自己的 props ， 所有 React 组件都必须是纯函数，并禁止修改其自身 props。
    1.2 React是单项数据流，父组件改变了属性，那么子组件视图会更新， 属性 props是外界传递过来的，状态 state是组件本身的，状态可以在组件中任意修改，组件的属性和状态改变都会更新视图。
  2. 函数组件（functional component）
    2.1 函数组件接收一个单一的 props 对象并返回了一个React元素

  区别：
    函数组件和类组件当然是有区别的，而且函数组件的性能比类组件的性能要高，因为类组件使用的时候要实例化，而函数组件直接执行函数取返回结果即可。为了提高性能，尽量使用函数组件。

  |  区别   | 函数组件  | 类组件 |
  |  ----  | ----  | ---- |
  | 是否有 this  | 没有 | 有 |
  | 是否有生命周期  | 没有 | 有 |
  | 是否有状态 state  | 没有 | 有 |

## React中的refs作用是什么

  Refs 是 React 提供给我们的安全访问 DOM 元素或者某个组件实例的句柄。
  
## 描述React事件处理

  为了解决跨浏览器兼容性问题，React中的事件处理程序将传递SyntheticEvent实例，该实例是React跨浏览器本机事件的跨浏览器包装器。这些综合事件具有与您惯用的本机事件相同的界面，除了它们在所有浏览器中的工作方式相同。
  有点有趣的是，React实际上并未将事件附加到子节点本身。React将使用单个事件侦听器在顶层侦听所有事件。这对性能有好处，也意味着React在更新DOM时无需担心跟踪事件监听器。

## state 和 props有什么区别

  state 和 props都是普通的JavaScript对象。尽管它们两者都具有影响渲染输出的信息，但它们在组件方面的功能不同。即：
    1. props是一个从外部传进组件的参数，主要作为就是从父组件向子组件传递数据，它具有可读性和不变性，只能通过外部组件主动传入新的props来重新渲染子组件，否则子组件的props以及展现形式不会改变。
    2. state的主要作用是用于组件保存、控制以及修改自己的状态，它只能在constructor中初始化，它算是组件的私有属性，不可通过外部访问和修改，只能通过组件内部的this.setState来修改，修改state属性会导致组件的重新渲染。

## 如何创建refs

  Refs 是使用 React.createRef() 方法创建的，并通过 ref 属性添加到 React 元素上。为了在整个组件中使用refs，只需将 ref 分配给构造函数中的实例属性

## 什么是高阶组件

  高阶组件就是一个函数，且该函数接受一个组件作为参数，并返回一个新的组件。基本上，这是从React的组成性质派生的一种模式，我们称它们为“纯”组件， 因为它们可以接受任何动态提供的子组件，但它们不会修改或复制其输入组件的任何行为。
    1. 高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧
    2. 高阶组件的参数为一个组件返回一个新的组件
    3. 组件是将 props 转换为 UI，而高阶组件是将组件转换为另一个组件

## constructor中super与props参数一起使用的目的是什么

  在调用方法之前，子类构造函数无法使用this引用super()。
  在ES6中，在子类的constructor中必须先调用super才能引用this。
  在constructor中可以使用this.props
  
## 什么是受控组件

  在HTML当中，像 input,textarea , 和  select 这类表单元素会维持自身状态，并根据用户输入进行更新。但在React中，可变的状态通常保存在组件的状态属性中，并且只能用 setState() 方法进行更新。

## 非受控组件

  非受控组件，即组件的状态不受React控制的组件
  
## 什么是JSX

  JSX即JavaScript XML。一种在React组件内部构建标签的类XML语法。JSX为react.js开发的一套语法糖，也是react.js的使用基础。React在不使用JSX的情况下一样可以工作，然而使用JSX可以提高组件的可读性，因此推荐使用JSX。
  
  优点：
    1. 允许使用熟悉的语法来定义 HTML 元素树；
    2. 提供更加语义化且移动的标签；
    3. 程序结构更容易被直观化；
    4. 抽象了 React Element 的创建过程；
    5. 可以随时掌控 HTML 标签以及生成这些标签的代码；
    6. 是原生的 JavaScript。

## 为什么不直接更新state状态

  如果进行直接赋值更新状态，那么它将不会重新渲染组件。
  而是使用setState()方法。它计划对组件状态对象的更新。状态改变时，组件通过重新渲染做出响应
  注意：可以分配状态的唯一位置是构造函数。
  
## 使用React Hooks有什么优势

  hooks 是react 16.8 引入的特性，他允许你在不写class的情况下操作state 和react的其他特性。
  hooks 只是多了一种写组件的方法，使编写一个组件更简单更方便，同时可以自定义hook把公共的逻辑提取出来，让逻辑在多个组件之间共享。

  Hook 是什么：
    Hook 是一个特殊的函数，它可以让你“钩入” React 的特性。例如，useState 是允许你在 React 函数组件中添加 state 的 Hook。

  ReactHooks的优点：
    1. 无需复杂的DOM结构
    2. 简洁易懂

## React中的StrictMode是什么

  React的StrictMode是一种帮助程序组件，可以帮助您编写更好的react组件，您可以使用包装一些组件，StrictMode 并且基本上可以：
    1. 验证内部组件是否遵循某些推荐做法，如果不在控制台中，则会发出警告。
    2. 验证不赞成使用的方法，如果使用了严格模式，则会在控制台中警告您。
    3. 通过识别潜在风险来帮助您预防某些副作用。

## 为什么类方法需要绑定

  在JavaScript中，this的值取决于当前上下文。在React类的组件方法中，开发人员通常希望它引用组件的当前实例，因此有必要将这些方法绑定到该实例。
  通常，这是在构造函数中完成的

## 描述Flux与MVC

  传统的MVC模式在分离数据（模型），UI（视图）和逻辑（控制器）的关注方面效果很好，但是MVC架构经常遇到两个主要问题：
    1. 数据流定义不佳：跨视图进行的级联更新通常会导致纠结的事件网，难以调试。
    2. 缺乏数据完整性：可以从任何地方对模型数据进行突变，从而在整个UI上产生不可预测的结果。

  使用Flux模式，复杂的UI不再受到级联更新的困扰。任何给定的React组件都将能够根据商店提供的数据重建其状态。Flux模式还通过限制对共享数据的直接访问来增强数据完整性。

## React context是什么

  使用Context，可以跨越组件进行数据传递。
  
## React Fiber是什么

  React Fiber 并不是所谓的纤程（微线程、协程），而是一种基于浏览器的单线程调度算法，背后的支持 API 是大名鼎鼎的：requestIdleCallback。
  Fiberl是一种将 recocilation （递归 diff），拆分成无数个小任务的算法；它随时能够停止，恢复。停止恢复的时机取决于当前的一帧（16ms）内，还有没有足够的时间允许计算。
