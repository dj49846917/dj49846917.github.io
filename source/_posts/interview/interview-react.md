---
title: 面试——react篇
date: 2020-12-28 15:24:26
tags: 
  - 面试
  - react
type: 面试                                                                 # 标签、分类
description:  React 是一个用于构建用户界面的 JavaScript 库。
top_img: https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=1452433621,3510720345&fm=26&gp=0.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 面试
  - react                                                                 # 文章标签
cover: https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=1452433621,3510720345&fm=26&gp=0.jpg                 # 文章的缩略图（用在首页）
---

# 解释一下 Flux
  * Flux 是一种强制单向数据流的架构模式。它控制派生数据，并使用具有所有数据权限的中心 store 实现多个组件之间的通信。整个应用中的数据更新必须只能在此处进行。 Flux 为应用提供稳定性并减少运行时的错误。
  * Flux 的最大特点，就是数据的"单向流动"。
  * 过程：
    1. 用户访问 View
    2. View 发出用户的 Action
    3. Dispatcher 收到 Action，要求 Store 进行相应的更新
    4. Store 更新后，发出一个"change"事件
    5. View 收到"change"事件后，更新页面
--- 

# 了解redux么？说一下redux
  * redux 是一个应用数据流框架，主要是解决了组件间状态共享的问题，原理是集中式管理，
  * 主要有三个核心方法，action，store，reducer。
  * 工作流程是 view 调用 store 的 dispatch 接收 action 传入 store，reducer 进行 state 操作，view 通过 store 提供的 getState 获取最新的数据，flux 也是用来进行数据操作的，有四个组成部分 action，dispatch，view，store，工作流程是 view 发出一个 action，派发器接收 action，让 store 进行数据更新，更新完成以后 store 发出 change，view 接受 change 更新视图。
  * Redux 和 Flux 很像。主要区别在于 Flux 有多个可以改变应用状态的 store，在 Flux 中 dispatcher 被用来传递数据到注册的回调事件，但是在 redux 中只能定义一个可更新状态的 store，redux 把 store 和 Dispatcher 合并,结构更加简单清晰
  * 新增 state,对状态的管理更加明确，通过 redux，流程更加规范了，减少手动编码量，提高了编码效率，同时缺点时当数据更新时有时候组件不需要，但是也要重新绘制，有些影响效率。一般情况下，我们在构建多交互，多数据流的复杂项目应用时才会使用它们
---

# 有用过状态管理吗？说说redux 的理念；
```
  概念: Redux 是 React的一个状态管理库

  工作原理；
    <1>.在React中，组件连接到 redux ，如果要访问 redux，需要派出一个包含 id和负载(payload) 的 action。action 中的 payload 是可选的，action 将其转发给 Reducer。
    <2>.当reducer收到action时，通过 swithc...case 语法比较 action 中type。 匹配时，更新对应的内容返回新的 state。
    <3>.当Redux状态更改时，连接到Redux的组件将接收新的状态作为props。当组件接收到这些props时，它将进入更新阶段并重新渲染 UI。
```

# 说明Flux和Redux的处理流程
  * Flux：
    1. 用户通过与 view 交互或者外部产生一个 Action，Dispatcher 接收到 Action 并执行那些已经注册的回调，向所有 Store 分发 Action。
    2. 通过注册的回调，Store 响应那些与他们所保存的状态有关的 Action。
    3. 然后 Store 会触发一个 change 事件，来提醒 controller-views 数据已经发生了改变。
    4. Controller-views 监听这些事件并重新从 Store 中获取数据。
    5. 这些 controller-views 调用他们自己的 setState() 方法，重新渲染自身以及组件树上的所有后代组件。
    
  * Redux：
    1. store通过reducer创建了初始状态
    2. view通过store.getState()获取到了store中保存的state挂载在了自己的状态上
    3. 用户产生了操作，调用了actions 的方法
    4. actions的方法被调用，创建了带有标示性信息的action
    5. actions将action通过调用store.dispatch方法发送到了reducer中
    6. reducer接收到action并根据标识信息判断之后返回了新的state
    7. store的state被reducer更改为新state的时候，store.subscribe方法里的回调函数会执行，此时就可以通知view去重新获取state
---

# Redux遵循的三个原则是什么？
  * 单一事实来源：整个应用的状态存储在单个 store 中的对象/状态树里。单一状态树可以更容易地跟踪随时间的变化，并调试或检查应用程序。
  * 状态是只读的：改变状态的唯一方法是去触发一个动作。动作是描述变化的普通 JS 对象。就像 state 是数据的最小表示一样，该操作是对数据更改的最小表示。
  * 使用纯函数进行更改：为了指定状态树如何通过操作进行转换，你需要纯函数。纯函数是那些返回值仅取决于其参数值的函数。
---

# 你对“单一事实来源”有什么理解？
Redux 使用 “Store” 将程序的整个状态存储在同一个地方。因此所有组件的状态都存储在 Store 中，并且它们从 Store 本身接收更新。单一状态树可以更容易地跟踪随时间的变化，并调试或检查程序。
---

# 解释 Reducer 的作用。
Reducers 是纯函数，它规定应用程序的状态怎样因响应 ACTION 而改变。Reducers 通过接受先前的状态和 action 来工作，然后它返回一个新的状态。它根据操作的类型确定需要执行哪种更新，然后返回新的值。如果不需要完成任务，它会返回原来的状态。
---

# Redux中同步 action 与异步 action 最大的区别是什么
同步只返回一个普通 action 对象。而异步操作中途会返回一个 promise 函数。当然在 promise 函数处理完毕后也会返回一个普通 action 对象。thunk 中间件就是判断如果返回的是函数，则不传导给 reducer，直到检测到是普通 action 对象，才交由 reducer 处理。
---

# Store 在 Redux 中的意义是什么？
Store 是一个 JavaScript 对象，它可以保存程序的状态，并提供一些方法来访问状态、调度操作和注册侦听器。应用程序的整个状态/对象树保存在单一存储中。因此，Redux 非常简单且是可预测的。我们可以将中间件传递到 store 来处理数据，并记录改变存储状态的各种操作。所有操作都通过 reducer 返回一个新状态。
---

# redux 做状态管理和发布订阅模式有什么区别？
    redux 其实也是一个发布订阅，但是 redux 可以做到数据的可预测和可回溯。
---

# react-redux 的原理，它是怎么跟 react 关联起来的？(实现原理、Redux如何实现多个组件之间的通信，多个组件使⽤相同状态如何进⾏管理)
  * react-redux 的核心组件只有两个，Provider 和 connect，Provider 存放 Redux 里 store 的数据到 context 里，通过 connect 从 context 拿数据，通过 props 传递给 connect 所包裹的组件。
---

# 多个组件之间如何拆分各⾃的state，每块⼩的组件有⾃⼰的状态，它们之间还有⼀些公共的状态需要维护，如何思考这块
每块小组件自己的状态放到私有state中，公共的才用Redux管理
---

# React-Redux中展示组件和容器组件之间的区别是什么?
  * 展示组件是一个类或功能组件，用于描述应用程序的展示部分。
  * 容器组件是连接到 Redux Store的组件的非正式术语。容器组件订阅 Redux 状态更新和dispatch操作，它们通常不呈现 DOM 元素；他们将渲染委托给展示性的子组件。
---

# 我可以在 reducer 中触发一个 Action 吗?
  可以。在 reducer 中触发 Action 是反模式。您的 reducer 应该没有副作用，只是接收 Action 并返回一个新的状态对象。在 reducer 中添加侦听器和调度操作可能会导致链接的 Action 和其他副作用。
---

# 如何在组件外部访问 Redux 存储的对象?
吧store暴露出来，引入即可
---

# 什么是 redux-saga?
redux-saga是一个库，旨在使 React/Redux 项目中的副作用（数据获取等异步操作和访问浏览器缓存等可能产生副作用的动作）更容易，更好。
---

# 在 redux-saga 中 call() 和 put() 之间有什么区别?
all()和put()都是 Effect 创建函数。 call()函数用于创建 Effect 描述，指示中间件调用 promise。put()函数创建一个 Effect，指示中间件将一个 Action 分派给 Store。
---

# 什么是 Redux Thunk?
Redux Thunk中间件允许您编写返回函数而不是 Action 的创建者。 thunk 可用于延迟 Action 的发送，或仅在满足某个条件时发送。内部函数接收 Store 的方法dispatch()和getState()作为参数。
---

# redux-saga 和 redux-thunk 之间有什么区别?
Redux Thunk和Redux Saga都负责处理副作用。在大多数场景中，Thunk 使用Promises来处理它们，而 Saga 使用Generators。Thunk 易于使用，因为许多开发人员都熟悉 Promise，Sagas/Generators 功能更强大，但您需要学习它们。但是这两个中间件可以共存，所以你可以从 Thunks 开始，并在需要时引入 Sagas。
---

# Redux与Flux的区别
Redux是基于Flux实现的，Vuex是Redux的基础上进行改变，在Redux中我们只能定义一个store，在Flux中我们可以定义多个，在Redux中，store和dispatch都放到了store，在redux中本身就内置State对象
---

# Redux与mobx的区别
---

# vuex, mobx, redux 各自的特点和区别。
---

# 说说对Vuex，Flux和Redux的理解
  * Vuex是一个专为Vue.js应用程序开发的状态管理模式，它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化

  * Flux和Redux都不是必须和React搭配使用的，因为Flux和Redux是完整的架构，在学习React的时候，只是将React的组件作为Redux中的视图层去使用了

  * Flux和Redux区别：Redux是基于Flux实现的，Vuex是Redux的基础上进行改变，在Redux中我们只能定义一个store，在Flux中我们可以定义多个，在Redux中，store和dispatch都放到了store，在redux中本身就内置State对象

  * Redux和Vuex的区别：Vuex是在Redux的基础上进行改变，Vuex使用mutation来替换Redux中的reducer，Vuex有自动渲染的功能

  * 一个Flux应用包含四个部分：
    1. Dispatcher，处理动作分发，维持 Store 之间的依赖关系
    2. Store，负责存储数据和处理数据相关逻辑
    3. Action，触发 Dispatcher
    4. View，视图，负责显示用户界面

  * Redux分为三部分：
    1. Action，就是一个单纯的包含 { type, payload } 的对象，type 是一个常量用来标示动作类型，payload 是这个动作携带的数据。
    2. Reducer 用来处理 Action 触发的对状态树的更改。
    3. Store 的作用就是连接上两者
  
  * vuex核心：
    1. state：存放多个组件共享的状态（数据）
    2. mutations：存放更改state里状态的方法，用于变更状态，是唯一一个更改状态的属性
    3. getters：将state中某个状态进行过滤，然后获取新的状态，类似于vue中的computed
    4. actions：用于调用事件动作，并传递给mutation
    5. modules：主要用来拆分state
---

# redux为什么每次reducer要返回一个新对象，面对大量节点如何优化
---

# 使用redux时，不能在componentWillReceiveProps方法中使用action
原因：有时候一个action改变数据后，我们希望拿到改变后的数据做另外一个action，比如初始化action读取硬盘中的数据到内存，然后用该参数进行请求网络数据action。此时我们可以在componentWillReceiveProps方法中拿到参数，若此时发出再发出action，则数据返回后改变reducer会再次进入componentWillReceiveProps方法，又继续发出action，陷入死循环。
---

# redux中间件原理
  * 中间件提供第三方插件的模式，自定义拦截 action -> reducer 的过程。变为 action -> middlewares -> reducer 。这种机制可以让我们改变数据流，实现如异步 action ，action 过滤，日志输出，异常报告等功能。

  * 常见的中间件：
    - redux-logger：提供日志输出
    - redux-thunk：处理异步操作
    - redux-promise：处理异步操作，actionCreator的返回值是promise
---

# class 组件与函数式组件的区别
  * 函数或无状态组件是一个纯函数，它可接受接受参数，并返回react元素。这些都是没有任何副作用的纯函数。这些组件没有状态或生命周期方法
  * 类或有状态组件具有状态和生命周期方可能通过setState()方法更改组件的状态。类组件是通过扩展React创建的。它在构造函数中初始化，也可能有子组件
  * 如果组件需要使用状态或生命周期方法，那么使用类组件，否则使用函数组件。
---

# 什么是无状态组件?
如果行为独立于其状态，则它可以是无状态组件。你可以使用函数或类来创建无状态组件。但除非你需要在组件中使用生命周期钩子，否则你应该选择函数组件。无状态组件有很多好处： 它们易于编写，理解和测试，速度更快，而且你可以完全避免使用this关键字。
---

# 什么是有状态组件?
当应用程序以开发模式运行的时，React 将会自动检查我们在组件上设置的所有属性，以确保它们具有正确的类型。如果类型不正确，React 将在控制台中生成警告信息。由于性能影响，它在生产模式下被禁用。使用 isRequired 定义必填属性。
---

# HTML 和 React 事件处理有什么区别?
  1. 在 HTML 中事件名必须小写，而在React中它遵循驼峰
  2. 在 HTML 中你可以返回 false 以阻止默认的行为，而在React中你必须地明确地调用preventDefault()
---

# React 事件机制；React 为什么要用合成事件？
合成事件是围绕浏览器原生事件充当跨浏览器包装器的对象。它们将不同浏览器的行为合并为一个 API。这样做是为了确保事件在不同浏览器中显示一致的属性。
---

# React 优化
  * 适当地使用shouldComponentUpdate生命周期方法。 它避免了子组件的不必要的渲染。 如果树中有100个组件，则不重新渲染整个组件树来提高应用程序性能。
  * 不可变性是提高性能的关键。不要对数据进行修改，而是始终在现有集合的基础上创建新的集合，以保持尽可能少的复制，从而提高性能。
  * 在显示列表或表格时始终使用 Keys，这会让 React 的更新速度更快
  * 代码分离是将代码插入到单独的文件中，只加载模块或部分所需的文件的技术。
---

# 什么是 Pure Components?
  * React.PureComponent 与 React.Component 完全相同，只是它为你处理了 shouldComponentUpdate() 方法。当属性或状态发生变化时，PureComponent 将对属性和状态进行浅比较。另一方面，一般的组件不会将当前的属性和状态与新的属性和状态进行比较。因此，在默认情况下，每当调用 shouldComponentUpdate 时，默认返回 true，所以组件都将重新渲染。
---

# 什么是纯组件？
纯（Pure） 组件是可以编写的最简单、最快的组件。它们可以替换任何只有 render() 的组件。这些组件增强了代码的简单性和应用的性能。
---

# 什么是受控组件?什么是非受控组件?
  * 受控: 在随后的用户输入中，能够控制表单中输入元素的组件被称为受控组件
  * 非受控: 非受控组件是在内部存储其自身状态的组件，当需要时，可以使用 ref 查询 DOM 并查找其当前值。
---

# 什么是高阶组件（HOC）? 作用是什么?
  * 概念: 高阶组件(HOC) 就是一个函数，且该函数接受一个组件作为参数，并返回一个新的组件，我们将它们称为纯组件，因为它们可以接受任何动态提供的子组件，但它们不会修改或复制其输入组件中的任何行为。
  * 作用:
    1. 代码复用，逻辑抽象化
    2. 渲染劫持
    3. 抽象化和操作状态（state）
    4. 操作属性（props）
---

# 构造函数使用带 props 参数的目的是什么?
  * 为了在子构造函数中访问this.props。
---

# 为什么 React 使用 className 而不是 class 属性?
  * class 是 JavaScript 中的关键字，而 JSX 是 JavaScript 的扩展。
---

# 什么是 Fragments ?为什么使用 Fragments 比使用容器 div 更好?
  * 概念:  它是 React 中的常见模式，用于组件返回多个元素。Fragments 可以让你聚合一个子元素列表，而无需向 DOM 添加额外节点。
  * 好处:
    1. 通过不创建额外的 DOM 节点，Fragments 更快并且使用更少的内存。这在非常大而深的节点树时很有好处。
    2. 一些 CSS 机制如Flexbox和CSS Grid具有特殊的父子关系，如果在中间添加 div 将使得很难保持所需的结构。
    3. 在 DOM 审查器中不会那么的杂乱。
---

# hooks 原理
  1. 单向链表通过next把hooks串联起来
  2. memoizedState存在fiber node上，组件之间不会相互影响
  3. useState和useReducer中通过dispatchAction调度更新任务
---

# hooks和class Component的区别
---

# 什么是 React Hooks？
Hooks是 React 16.8 中的新添加内容。它们允许在不编写类的情况下使用state和其他 React 特性。使用 Hooks，可以从组件中提取有状态逻辑，这样就可以独立地测试和重用它。Hooks 允许咱们在不改变组件层次结构的情况下重用有状态逻辑，这样在许多组件之间或与社区共享 Hooks 变得很容易。
---

# 为什么使用hooks
  * 只能在顶层调用Hooks? Hooks是使用数组或单链表串联起来，Hooks顺序修改会打乱执行结果
  * hook之前的函数组件时无状态、无副作用，只作单纯的展示组件
  * class组件的弊端，为什么要引入hook？
    - 组件之间复用逻辑难，比如使用Context要引入Provider、Consumer，一层套一层
    - 复杂组件变得难以理解：hook将组件中相互关联的部分拆成更小的函数
    - 难以理解的class：比如js中的this问题、构建时的一些问题
  * 引入hook之后函数组件发生了哪些变化？
    - 函数组件可以存储和改变状态值：useState、useReducer
    - 可以执行副作用：useEffect
    - 复用状态逻辑： 自定义hook
---

# React Hook 比起 Class 有什么优缺点？
  * 优点
    1. 解决 class 的 this 和 生命周期两大痛点
    2. 更好地拆分逻辑代码，每个 Hook 就做一件事
    3. 更好地复用代码，不会跟 HOC 一样出现嵌套地狱
  * 缺点
    1. 缺少 getSnapshotBeforeUpdate 这类生命周期的实现
    2. 在过度复杂或者过度拆分方面摇摆不定，对开发者的水平提出了更高的要求。
    3. Hooks 在使用层面有着严格的规则约束（比如不要在循环、条件或嵌套函数中调用 Hook。）
---

# 了解过 react-router 内部实现机制吗？(实现原理)
  * 当用户点击页面跳转时，react-router阻止了浏览器的默认跳转行为，而改用history模块的pushState方法去触发url更新，当执行history.push时，执行了注册的listener函数，listener中的setState函数也被执行，将当前url地址栏对应的url传递下去，当Route组件匹配到该地址栏的时候，就会渲染该组件，如果匹配不到，Route组件就返回null
  * react-router依赖基础是history库：
    - 老浏览器的history: 主要通过hash来实现，对应createHashHistory
    - 高版本浏览器: 通过html5里面的history，对应createBrowserHistory
    - node环境下: 主要存储在memeory里面，对应createMemoryHistory
---

# 如何配置React-Router
  1. npm install react-router-dom
  2. router基本组件
    * Router 路由器
    * Link 连接
    * Route 路由
    * Switch 独占
    * Redirect 重定向
---

# react-router怎么实现路由切换
  1. Router组件使用Provider向下提供history对象，以及location的变更监听
  2. Route接收loaction(props || context)，Router尝试其属性path与location进行匹配，匹配检测(children > component > render)，内容渲染
  3. Link 跳转链接，生成一个a标签，禁用默认事件，js处理点击事件跳转(history.push) -> Link与直接使用a标签的区别是a页面会重新加载一下，Link只重新渲染Route组件部分
  4. Redirect to
  5. 从上下文中获取history this.context.router.history.push…
---

# 路由的动态加载模块
  1. 使用react-loadable包实现懒加载
  2. ()=>import(…)
---

# 介绍路由的history
---

# 什么是 JSX?
  * JSX是javascript的语法扩展。它就像一个拥有javascript全部功能的模板语言。它生成React元素，这些元素将在DOM中呈现。
---

# 为什么浏览器无法读取JSX？
  * 浏览器只能处理 JavaScript 对象，而不能读取常规 JavaScript 对象中的 JSX。所以为了使浏览器能够读取 JSX，首先，需要用像 Babel 这样的 JSX 转换器将 JSX 文件转换为 JavaScript 对象，然后再将其传给浏览器。
---

# 什么是Virtual DOM及其工作原理
  * 概念：虚拟DOM可以看做一棵模拟了DOM树的JavaScript对象树。
  * 原理: React将整个DOM副本保存为虚拟DOM,每当有更新时，它都会维护两个虚拟DOM，以比较之前的状态和当前状态，并确定哪些对象已被更改。 它通过比较两个虚拟DOM 差异，并将这些变化更新到实际DOM,一旦真正的DOM更新，它也会更新UI 
---

# 为什么虚拟 dom 会提高性能?
虚拟 dom 相当于在 js 和真实 dom 中间加了一个缓存，利用 dom diff 算法避免了没有必要的 dom 操作，从而提高性能。用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异把 2 所记录的差异应用到步骤 1 所构建的真正的 DOM 树上，视图就更新了。
---

# 虚拟Dom中的$$typeof属性的作用是什么？
  ```
    $$typeof是一个 Symbol类型的变量，这个变量可以防止 XSS JSON中不能存储 Symbol类型的变量。
  ```
---

# Real DOM和Virtual DOM的区别
  * Real DOM
    1. 更新缓慢。
    2. 可以直接更新 HTML。
    3. 如果元素更新，则创建新DOM。
    4. DOM操作代价很高。
    5. 消耗的内存较多。
  * Virtual DOM
    1. 更新更快。
    2. 无法直接更新 HTML。
    3. 如果元素更新，则更新 JSX 。
    4. DOM 操作非常简单。
    5. 很少的内存消耗。
---

# 什么是 Props?
  * Props 是 React 中属性的简写。它们是只读组件，必须保持纯，即不可变。它们总是在整个应用中从父组件传递到子组件。子组件永远不能将 prop 送回父组件。这有助于维护单向数据流，通常用于呈现动态生成的数据。
---

# React中的状态是什么？它是如何使用的？
  * 状态是 React 组件的核心，是数据的来源，必须尽可能简单。基本上状态是确定组件呈现和行为的对象。与props 不同，它们是可变的，并创建动态和交互式组件。可以通过 this.state() 访问它们。
---

# 状态和属性有什么区别?
  * props是一个从外部传进组件的参数，由于React具有单向数据流，所以它的主要作用是从父组件向子组件传递数据，它是不可改变的。如果想要改变它，只能通过外部组件传入新的props来重新渲染子组件，否则子组件的props以及展现形式不会改变。 props除了可以传字符串、数字，还可以传数组，对象、甚至是回调函数。

  * state的主要作用是用于组件保存、控制以及修改自己的状态，它只能在constructor中初始化，state是可以被改变的。state放改动的一些属性，比如点击选中，再点击取消。类似的这种属性就可以放到state里。 没有state的叫做无状态组件，多用props少用state，多写无状态组件。 修改state的值时，必须通过setState()方法。当我们调用this.setState方法时，React会更新组件的数据状态state，并且重新调用render方法
  
  * 相同点：
    1. props和state都是导出HTML的原始数据
    2. props和state都是确定性的，如果我们写的组件为同一props和state的组合生成了不同的输出，那么我们肯定在哪里做错了
    3. props和state更改都会触发渲染更新，这里讨论同一个组件内的props和state，即props是从外层组件获取的，而state是当前组件自己维护的（这里可以看做是共同点也可看做是不同点，因为虽然都是会触发渲染更新，但是如何更改的机制不一样）
    4. props和state都是纯JS对象（对象字面量，{}，我们会简称为对象；对于[]，我们会简称为数组），我们可以用typeof来判断他们，结果都是object
    5. 可以从父组件得到初始值props和state的初始值
  * 不同点：
    1. 可以从父组件修改自组件的props，而不能从父组件修改自组件的state
    2. 可以在组件内部分别对state和props设置初始值
    3. props不可以在组件内部修改，但state可以在内部修改
    4. state是组件自己管理数据，控制自己的状态，值是可以改变的；props是外部传入的数据参数，不可变；
---

# 执行setState发生了什么？(react的setstate过程)
将传递给 setState 的对象合并到组件的当前状态，这将启动一个和解的过程，构建一个新的 react 元素树，与上一个元素树进行对比（diff），从而进行最小化的重渲染。
---

# react进行setState后触发了哪些钩子
  * setState的改变会触发4个生命周期钩子
    - shouldComponentUpdate
    - componentWillUpdate
    - render
    - componentDidUpdate

  * props的改变会触发5个生命周期钩子
    - componentWillReveiceProps
    - shouldComponentUpdate
    - componentWillUpdate
    - render
    - componentDidUpdate
---

# 当调用`setState`时，React `render` 是如何工作的？
  1. 虚拟 DOM 渲染:当render方法被调用时，它返回一个新的组件的虚拟 DOM 结构。当调用setState()时，render会被再次调用，因为默认情况下shouldComponentUpdate总是返回true，所以默认情况下 React 是没有优化的。
  2. 原生 DOM 渲染:React 只会在虚拟DOM中修改真实DOM节点，而且修改的次数非常少——这是很棒的React特性，它优化了真实DOM的变化，使React变得更快。
---

# setState 和 replaceState 的区别
setState 是修改其中的部分状态，相当于 Object.assign，只是覆盖，不会减少原来的状态 replaceState 是完全替换原来的状态，相当于赋值，将原来的 state 替换为另一个对象，如果新状态属性减少，那么 state 中就没有这个状态了
---

# setState是同步还是异步
  * 结论：
    - 由React控制的事件处理程序，以及生命周期函数调用setState不会同步更新state
    - React控制之外的事件中调用setState是同步更新的。比如原生js绑定的事件、setTimeout、setInterval等。
  * 原因：React批量更新
    - 源码中根据isBatchingUpdates(默认false，同步更新)判断立即更新，还是放到队列中延迟更新
    - batchedUpdates方法会修改isBatchingUpdates为true，引起setState的异步更新
    - React调用事件处理函数前会先调用batchedUpdates方法，所以由React处理的事件setState为异步。
---

# 在 componentWillMount() 方法中使用 setState() 好吗?
建议避免在 componentWillMount() 生命周期方法中执行异步初始化。在 mounting 发生之前会立即调用 componentWillMount()，且它在 render() 之前被调用，因此在此方法中更新状态将不会触发重新渲染。应避免在此方法中引入任何副作用或订阅操作。我们需要确保对组件初始化的异步调用发生在 componentDidMount() 中，而不是在 componentWillMount() 中。
---

# 为什么我们需要将函数传递给 setState() 方法?(为什么函数比对象更适合于 setState()?)
  * 这背后的原因是 setState() 是一个异步操作。出于性能原因，React 会对状态更改进行批处理，因此在调用 setState() 方法之后，状态可能不会立即更改。这意味着当你调用 setState() 方法时，你不应该依赖当前状态，因为你不能确定当前状态应该是什么。这个问题的解决方案是将一个函数传递给 setState()，该函数会以上一个状态作为参数。通过这样做，你可以避免由于 setState() 的异步性质而导致用户在访问时获取旧状态值的问题。

  * 假设初始计数值为零。在连续三次增加操作之后，该值将只增加一个。
    ```
      // assuming this.state.count === 0
      this.setState({ count: this.state.count + 1 })
      this.setState({ count: this.state.count + 1 })
      this.setState({ count: this.state.count + 1 })
      // this.state.count === 1, not 3
    ```

  * 如果将函数传递给 setState()，则 count 将正确递增。
    ```
      this.setState((prevState, props) => ({
        count: prevState.count + props.increment
      }))
      // this.state.count === 3 as expected
    ```
---

# 什么是调解?
当组件的props或state发生更改时，React 通过将新返回的元素与先前呈现的元素进行比较来确定是否需要实际的 DOM 更新。当它们不相等时，React 将更新 DOM 。此过程称为reconciliation。
---

# 我们为什么不能直接更新状态?
  * 如果你尝试直接改变状态，那么组件将不会重新渲染。正确方法应该是使用 setState() 方法。它调度组件状态对象的更新。当状态更改时，组件通将会重新渲染。
---

# 回调函数作为 setState() 参数的目的是什么?
  * 当 setState 完成和组件渲染后，回调函数将会被调用。由于 setState() 是异步的，回调函数用于任何后续的操作。
---

# 聊聊 React 的 diff(原理)
  1. 把树形结构按照层级分解，只比较同级元素。
  2. 给列表结构的每个单元添加唯一的 key 属性，方便比较。
  3. React 只会匹配相同 class 的 component（这里面的 class 指的是组件的名字）。
  4. 合并操作，调用 component 的 setState 方法的时候, React 将其标记为 dirty.到每一个事件循环结束, React 检查所有标记 dirty 的 component 重新绘制.
  5. 选择性子树渲染。开发人员可以重写 shouldComponentUpdate 提高 diff 的性能。
---

# 组件生命周期的不同阶段是什么?（分为三个阶段，初始，运行中，销毁）
  * **初始化**：在组件初始化阶段会执行
    1. 执行getDefaultProps钩子函数，执行一次，挂载属性props（无Dom元素，有组件相关的this但是无法获取数据，组件想要拥有默认属性可以通过这个钩子函数设置）
    2. 执行getInitialState钩子函数，初始化自身状态state（同上，无Dom元素，有组件相关的this，但是无法获取数据，组件想要拥有状态只能通过这个钩子函数）
    3. static getDerivedStateFromProps() 挂载前（类似于Vue的created加beforeMount阶段，可以进行数据请求（ajax），做一些初始数据的获取和设置，并且在这里更改数据不会触发运行阶段的钩子函数，在这里还可以更改this的指向问题）
    4. render(构建组件的虚拟DOM结构进行编译)
    5. componentDidMount（）挂载完成（有Dom元素，数据准备完毕，这里可以操作DOM，并且可以访问已经渲染的DOM，在这个钩子函数里面也可以进行对数据的获取）

  * **更新阶段**: props或state的改变可能会引起组件的更新，组件重新渲染的过程中会调用以下方法：
    1. static getDerivedStateFromProps()函数：当props发生变化时调用（当接收到的属性发生变化时触发，可以在这里更改改变后的属性去做一些事情，比如更改自己的状态，在这里this上的属性还没有更新，要想使用新的数据需要从参数中得到）
    2. shouldComponentUpdate函数：主要做效率优化，控制组件是否随之更新，函数返回的true或false表示视图是否渲染，如：在函数中比较this.props.name（数据更新前）和props.name（数据更新后）对比，二者是否相同，从而避免重复渲染，加强优化
    3. getSnapshotBeforeUpdate函数：准备工作，多做一些调试工作，在props和state发生改变的时候执行，并且在render方法之前执行，但是你在这个钩子函数里不能更改状态，否则会造成死循环，类似Vue中的beforeUpdate render：重新渲染Dom
    4. componentDidUpdate：页面更新渲染完成，组件的更新结束后执行,在这里可以操作更新完成后的dom，类似Vue的updated
  * **卸载阶段**
    1. componentWillUnmount：组件将要销毁，可以将定时器，事件等取消或结束 （ReactDOM.unmountComponentAtNode(node) 销毁节点中的组件）

  * **错误处理**
    1. componentDidCatch() --16.3版本之后出现的 
---

# react 16 生命周期有什么改变？(在 React v16 中，哪些生命周期方法将被弃用?)
  * react 16.3以前： 
    - componentWillMount – 使用constructor或componentDidMount代替
    - componentWillUpdate – 使用componentDidUpdate代替
    - componentWillReceiveProps – 使用一个新的方法：static getDerivedStateFromProps来代替
  
  * React 16.3版本中：
    - componentWillMount，componentWillUpdate，componentWillReceiveProps还能用
  * React 16.x版本中：
    - 启用弃用警告，三个方法变为：UNSAFE_componentWillMount，UNSAFE_componentWillReceiveProps，UNSAFE_componentWillUpdate
  * 在React17.0版本中：
    - 这三个生命周期被移除
---

# 重新渲染界面这个过程在生命周期中哪几步？
componentwillReceiveProps > shouldComponentUpdate > componentWillUpdate > render > componentDidUpdate
---

# static getDerivedStateFromProps什么时候用
  1. 组件实例化。
  2. 组件的props发生变化。
  3. 父组件重新渲染。
  4. this.setState()不会触发getDerivedStateFromProps()，但是this.forceUpdate()会。在update阶段也会调用一次这个方法。
---

# 生命周期方法 getDerivedStateFromProps() 的目的是什么?
  * 新的静态 getDerivedStateFromProps() 生命周期方法在实例化组件之后以及重新渲染组件之前调用。它可以返回一个对象用于更新状态，或者返回 null 指示新的属性不需要任何状态更新。
  * 此生命周期方法与 componentDidUpdate() 一起涵盖了 componentWillReceiveProps() 的所有用例。
---

# 生命周期方法 getSnapshotBeforeUpdate() 的目的是什么?
  * 新的 getSnapshotBeforeUpdate() 生命周期方法在 DOM 更新之前被调用。此方法的返回值将作为第三个参数传递给componentDidUpdate()。
  * 此生命周期方法与 componentDidUpdate() 一起涵盖了 componentWillUpdate() 的所有用例。
---

# 为什么两个will生命周期要被标记为danger(为什么弃用)
  * 比如在 componentWillMount 里发起异步请求，以为可以让页面早点显示出来，但异步请求再怎么快也快不过（React 15 下）同步的生命周期。componentWillMount 结束后，render 会迅速地被触发，所以说首次渲染依然会在数据返回之前执行。这样做不仅没有达到你预想的目的，还会导致服务端渲染场景下的冗余请求等额外问题，得不偿失。
  * Fiber 架构下会因为中断和重启导致多次调用，如果内部有付款类的接口请求则会出现大问题
  * 防止各种骚操作，比如在 componentWillReceiveProps 中使用 setState 直接爆栈
---

# 在 React v16 中，哪些生命周期方法将被弃用?
  * componentWillMount(), componentWillReceiveProps(), componentWillUpdate()  
---

# 在 mounting 阶段生命周期方法的执行顺序是什么?
  * constructor() => static getDerivedStateFromProps() => render() =>componentDidMount()
---

# 生命周期方法 getDerivedStateFromProps() 的目的是什么?
  * 新的静态 getDerivedStateFromProps() 生命周期方法在实例化组件之后以及重新渲染组件之前调用。它可以返回一个对象用于更新状态，或者返回 null 指示新的属性不需要任何状态更新。此生命周期方法与 componentDidUpdate() 一起涵盖了 componentWillReceiveProps() 的所有用例。
---

# 生命周期方法 getSnapshotBeforeUpdate() 的目的是什么?
  * 新的 getSnapshotBeforeUpdate() 生命周期方法在 DOM 更新之前被调用。此方法的返回值将作为第三个参数传递给componentDidUpdate()。此生命周期方法与 componentDidUpdate() 一起涵盖了 componentWillUpdate() 的所有用例。
---

# forceUpdate经历了哪些生命周期，子组件呢?
  * 都有render
---

# fiber带来的新的生命周期是什么
  ![react面试题](/images/vue/vue面试5.png)
---

# 是否可以在不调用 setState 方法的情况下，强制组件重新渲染?
  component.forceUpdate(callback)
---

# 你对 React 的 refs 有什么了解？
  * Refs 是 React 中引用的简写。它是一个有助于存储对特定的 React 元素或组件的引用的属性，它将由组件渲染配置函数返回。用于对 render() 返回的特定元素或组件的引用。当需要进行 DOM 测量或向组件添加方法时，它们会派上用场。
---

# refs 有什么用?
ref 用于返回对元素的引用。但在大多数情况下，应该避免使用它们。当你需要直接访问 DOM 元素或组件的实例时，它们可能非常有用
---

# 如何创建 refs?
  1. Refs 是使用 React.createRef() 方法创建的，并通过 ref 属性添加到 React 元素上。为了在整个组件中使用refs，只需将 ref 分配给构造函数中的实例属性。
    ```
      class MyComponent extends React.Component {
        constructor(props) {
          super(props)
          this.myRef = React.createRef()
        }
        render() {
          return <div ref={this.myRef} />
        }
      }
    ```
  2. 你也可以使用 ref 回调函数的方案，而不用考虑 React 版本。
    ```
      class SearchBar extends Component {
        constructor(props) {
            super(props);
            this.txtSearch = null;
            this.state = { term: '' };
            this.setInputSearchRef = e => {
              this.txtSearch = e;
            }
        }

        onInputChange(event) {
            this.setState({ term: this.txtSearch.value });
        }

        render() {
            return (
              <input
                  value={this.state.term}
                  onChange={this.onInputChange.bind(this)}
                  ref={this.setInputSearchRef} />
            );
        }
      }
    ```
---

# 为什么 String Refs 被弃用?
  1. 它们强制 React 跟踪当前执行的组件。这是有问题的，因为它使 React 模块有状态，这会导致在 bundle 中复制 React 模块时会导致奇怪的错误。
  2. 它们是不可组合的 - 如果一个库把一个 ref 传给子元素，则用户无法对其设置另一个引用。
  3. 它们不能与静态分析工具一起使用，如 Flow。Flow 无法猜测出 this.refs 上的字符串引用的作用及其类型。Callback refs 对静态分析更友好。
  4. 使用 “render callback” 模式（比如： ），它无法像大多数人预期的那样工作。
---

# 请说一说Forwarding Refs有什么用
是父组件用来获取子组件的dom元素的
---

# React 中 key 的重要性是什么？
key 用于识别唯一的 Virtual DOM 元素及其驱动 UI 的相应数据。它们通过回收 DOM 中当前所有的元素来帮助 React 优化渲染。这些 key 必须是唯一的数字或字符串，React 只是重新排序元素而不是重新渲染它们。这可以提高应用程序的性能。
---

# createElement 和 cloneElement 有什么区别?
JSX 元素将被转换为 React.createElement() 函数来创建 React 元素，这些对象将用于表示 UI 对象。而 cloneElement 用于克隆元素并传递新的属性
---

# 什么是上下文（Context）?
Context 通过组件树提供了一个传递数据的方法，从而避免了在每一个层级手动的传递props。比如，需要在应用中许多组件需要访问登录用户信息、地区偏好、UI主题等。
---

# children 属性是什么?
Children 是一个属性（this.props.chldren），它允许你将组件作为数据传递给其他组件，就像你使用的任何其他组件一样。在组件的开始和结束标记之间放置的组件树将作为children属性传递给该组件。
---

# react-dom 中 render 方法的目的是什么?
  * 此方法用于将 React 元素渲染到所提供容器中的 DOM 结构中，并返回对组件的引用。如果 React 元素之前已被渲染到容器中，它将对其执行更新，并且只在需要时改变 DOM 以反映最新的更改。
  * 如果提供了可选的回调函数，该函数将在组件被渲染或更新后执行。
---

# 在 React 中如何使用 innerHTML?
**dangerouslySetInnerHTML** 属性是 React 用来替代在浏览器 DOM 中使用 **innerHTML**。与 innerHTML 一样，考虑到跨站脚本攻击（XSS），使用此属性也是有风险的。使用时，你只需传递以 __html 作为键，而 HTML 文本作为对应值的对象。
```
  function createMarkup() {
    return { __html: 'First &middot; Second' }
  }

  function MyComponent() {
    return <div dangerouslySetInnerHTML={createMarkup()} />
  }
```

# React 的工作原理(React组件的渲染流程)
React 会创建一个虚拟 DOM(virtual DOM)。当一个组件中的状态改变时，React 首先会通过 "diffing" 算法来标记虚拟 DOM 中的改变；第二步是调节(reconciliation)，会用 diff 的结果来更新 DOM。
---

# React中Dom结构发生变化后内部经历了哪些变化？
  1. setState引起状态变化
  2. React重新构建vnode树
  3. 和旧vnode树对比，得出变化部分Patches(diff算法)
  4. 将Patches更新到真实dom上
---

# React中元素与组件的区别
  * 元素：
    - 它是 React 中最小基本单位，React 元素不是真实的 DOM 元素，它仅仅是 js 的普通对象（plain objects），所以也没办法直接调用 DOM 原生的 API。
  * 组件:
    - React 中有三种构建组件的方式。React.createClass()、ES6 class和无状态函数。
  * 组件是由元素构成的。元素数据结构是普通对象，而组件数据结构是类或纯函数。
---

# 你对Fiber架构了解吗
  * Fiber 是 React 16 中新的协调引擎或重新实现核心算法。它的主要目标是支持虚拟DOM的增量渲染。React Fiber 的目标是提高其在动画、布局、手势、暂停、中止或重用等方面的适用性，并为不同类型的更新分配优先级，以及新的并发原语。
  * React Fiber 的目标是增强其在动画、布局和手势等领域的适用性。它的主要特性是增量渲染:能够将渲染工作分割成块，并将其分散到多个帧中。
---

# Fiber架构相对于以前的递归更新组件有有什么优势
  * 原因是递归更新组件会让JS调用栈占用很长时间。
  * 因为浏览器是单线程的，它将GUI渲染，事件处理,js执行等等放在了一起，只有将它做完才能做下一件事，如果有足够的时间，浏览器是会对我们的代码进行编译优化（JIT）及进行热代码优化。
  * Fiber架构正是利用这个原理将组件渲染分段执行，提高这样浏览器就有时间优化JS代码与修正reflow！
---

# 既然你说Fiber是将组件分段渲染，那第一段渲染之后，怎么知道下一段从哪个组件开始渲染呢？
Fiber节点拥有return, child, sibling三个属性，分别对应父节点， 第一个孩子， 它右边的兄弟， 有了它们就足够将一棵树变成一个链表， 实现深度优化遍历。
---

# 怎么决定每次更新的数量
  * React16则是需要将虚拟DOM转换为Fiber节点，首先它规定一个时间段内，然后在这个时间段能转换多少个FiberNode，就更新多少个。
  * 因此我们需要将我们的更新逻辑分成两个阶段，第一个阶段是将虚拟DOM转换成Fiber, Fiber转换成组件实例或真实DOM（不插入DOM树，插入DOM树会reflow）。Fiber转换成后两者明显会耗时，需要计算还剩下多少时间。
  * 比如，可以记录开始更新视图的时间var now = new Date - 0，假如我们更新试图自定义需要100毫秒，那么定义结束时间是var deadline = new Date + 100 ,所以每次更新一部分视图，就去拿当前时间new Date<1deadline>做判断，如果没有超过deadline就更新视图，超过了，就进入下一个更新阶段
---

# 如何调度时间才能保证流畅
  * 使用浏览器自带的api - requestIdleCallback，
  * 它的第一个参数是一个回调，回调有一个参数对象，对象有一个timeRemaining方法，就相当于new Date - deadline，并且它是一个高精度数据， 比毫秒更准确
  * 这个因为浏览器兼容性问题，react团队自己实现了requestIdleCallback
---

# 什么是状态提升
使用 react 经常会遇到几个组件需要共用状态数据的情况。这种情况下，我们最好将这部分共享的状态提升至他们最近的父组件当中进行管理。
---

# 谈谈对react的理解
  * react是基于mvvm（视图层）层的一款框架，虚拟dom和diff算法
  * react特点：
    1. 声明式设计
    2. 高效，其中高效以现在虚拟dom，最大限度减少与dom的交互和diff算法
    3. 灵活，体现在可以与已知的框架或库很好的配合
    4. JSX，是js语法的扩展
    5. 组件化，构建组件，是代码的更容易得到复用，比较建议在大型项目的开发
    6. 单项数据，实现单项数流，从而减少代码复用
---

# React组件中怎么做事件代理
---

# React组件事件代理的原理
  * 事件委托：事件注册到document上
    - 统一管理卸载
  * e.stopPropagation阻止冒泡不可达到document层，解决：e.nativeEvent.stopImmediatePropagation()
  * react源码通过一个合成事件映射表判断是否是合成事件
---

# 在 React 中使用构造函数和 getInitialState 有什么区别？
构造函数和getInitialState之间的区别就是ES6和ES5本身的区别。在使用ES6类时，应该在构造函数中初始化state，并在使用React.createClass时定义getInitialState方法。
---

# React怎么做数据的检查和变化
  * props:组件属性，专门用来连接父子组件间通信，父组件传输父类成员，子组件可以利用但不能编辑父类成员；
  * state：专门负责保存和改变组件内部的状态；
---

# 哪些方法会触发react重新渲染
  1. 初始化
  2. 更新
    * state发生改变
    * props发送改变
  3. 强制更新
    * this.forceUpdate
---

# React 事件和 DOM 事件的区别
  1. 所有事件挂载到 document上
  2. event 不是原生的，是 SyntheticEvent合成事件对象
  3. dispatchEvent
---