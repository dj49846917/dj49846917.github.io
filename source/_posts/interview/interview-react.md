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
---

# redux的设计思想
  * 单向数据流
  * Store是唯一的数据源
  * Redux的三大原则
    1. 单一数据源
    2. State只读
    3. 使用纯函数来修改
---

# 单向数据流的好处
  * 数据流动方向可以追踪，流动单一，追查问题的时候可以更快捷
  * view发出action后不修改原有state，耳屎返回一个新的，可以保存state的历史记录，可以复现场景，易测试
---

# 列出Redux的组件
  * Action: 一个用来描述发生了什么事情的对象
  * Reducer: 一个确定状态将如何进行变换的地方
  * Store: 整个程序的状态，对象树保存在Store中
  * View: 只显示Store提供的数据
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

# React是如何将redux包装到组件上的？
> React的react-redux库，通过给根组件包装一层<Provider>，并将store绑定上去，需要进行状态管理的组件通过connect包装生成容器组件，并将状态和事件映射到props上。
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

# redux为什么是纯函数，不纯也不会报错，为什么开发要求要纯
---

# 请说下redux中你对reducers的理解,以及如何合并多个reducers
  * 在redux中reducer是一个纯函数，其中这个纯函数会接收2个参数 一个是state 一个是action。state用来保存公共的状态，state有一个特点就是只允许读不允许进行修改 。
  * 在实际开发中因为会涉及到多人协作开发所以每个模块都会有一个reducer。我们可以通过combineReducers来合并多个reducers
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

# Redux中异步的请求怎么处理
  * 利用中间件：redux-thunk、saga、
---

# react-redux 的原理，它是怎么跟 react 关联起来的？(实现原理、Redux如何实现多个组件之间的通信，多个组件使⽤相同状态如何进⾏管理)
  * react-redux 的核心组件只有两个，Provider 和 connect，Provider 存放 Redux 里 store 的数据到 context 里，通过 connect 从 context 拿数据，通过 props 传递给 connect 所包裹的组件。
---

# 接入Redux的过程？
  1. 安装redux
  2. 创建Store统一管理state
  3. 通过store，订阅state和分发更新state
---

# 绑定connect的过程？
  * connect的作用是连接Redux store和React组件: connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])
  * mapStateToProps: 将Redux store中的数据作为props绑定到组件上
  * mapDispatchToProps: 将action作为props绑定到组件上
  * mergeProps: mapStateToProps和mapDisPatchToProps得到的props和ownerProps合并
  * options: 定制connect的一些特殊行为
---

# connect原理
  1. Provider向子组件提供store的Context
  2. 接收Redux的store作为props，通过context对象传递给子组件上的connect
  3. connect连接Redux和React，通过高阶组件HOC包在我们的容器组件的外一层，它接收上面Provider提供的store，根据对应转换函数得到state和dispatch，并组成一个对象，以属性的形式传给我们的容器组件，并在内部订阅store的变化，根据变化更新state的值
---

# 多个组件之间如何拆分各⾃的state，每块⼩的组件有⾃⼰的状态，它们之间还有⼀些公共的状态需要维护，如何思考这块
  * 如果状态具有复杂的更新逻辑，则将该逻辑从组件中提取到自定义钩子中。
  * 保证唯一数据源和单向数据
  * 将复杂的状态逻辑提取到自定义钩子中(自定义useReducer)。
  * 对于组件的拆分要做到高内聚低耦合
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
  * 相同点：
    - 为了解决状态管理混乱，无法有效同步的问题统一维护管理应用状态;
    - 某一状态只有一个可信数据来源（通常命名为store，指状态容器）;
    - 操作更新状态方式统一，并且可控（通常以action方式提供更新状态的途径）;
    - 支持将store与React组件连接，如react-redux，mobx- react;
  
  * 不同点：
    - redux将数据保存在单一的store中，mobx将数据保存在分散的多个store中
    - redux使用plain object保存数据，需要手动处理变化后的操作;mobx适用observable保存数据，数据变化后自动处理响应的操作
    - redux使用不可变状态，这意味着状态是只读的，不能直接去修改它，而是应该返回一个新的状态，同时使用纯函数;mobx中的状态是可变的，可以直接对其进行修改
    - mobx相对来说比较简单，在其中有很多的抽象，mobx更多的使用面向对象的编程思维;redux会比较复杂，因为其中的函数式编程思想掌握起来不是那么容易，同时需要借助一系列的中间件来处理异步和副作用
    - mobx中有更多的抽象和封装，调试会比较困难，同时结果也难以预测;而redux提供能够进行时间回溯的开发工具，同时其纯函数以及更少的抽象，让调试变得更加的容易
---

# vuex, mobx, redux 各自的特点和区别。
  * vuex:
    1. 单向数据流。View 通过 store.dispatch() 调用 Action ，在 Action 执行完异步操作之后通过 store.commit() 调用 Mutation 更新 State ，通过 vue 的响应式机制进行视图更新。
    2. 单一数据源，和 Redux 一样全局只有一个 Store 实例。
    3. 可直接对 State 进行修改。

  * mobx: 
    1. 数据流流动不自然，只有用到的数据才会引发绑定，局部精确更新（细粒度控制）。
    2. 没有时间回溯能力，因为数据只有一份引用。
    3. 基于面向对象。
    4. 往往是多个 Store。
    5. 代码侵入性小。
    6. 简单可扩展。
    7. 大型项目使用 MobX 会使得代码难以维护。

  * redux:
    1. 单向数据流。View 发出 Action (store.dispatch(action))，Store 调用 Reducer 计算出新的 state
    2. 若 state 产生变化，则调用监听函数重新渲染 View （store.subscribe(render)）。
    3. 单一数据源，只有一个 Store。
    4. state 是只读的，每次状态更新之后只能返回一个新的 state。
    5. 没有 Dispatcher ，而是在 Store 中集成了 dispatch 方法，store.dispatch() 是 View 发出 Action 的唯一途径。

  * 区别：
    1. Redux 、Vuex 均为单向数据流。
    2. Redux 和 Vuex 是基于 Flux 的，Redux 较为泛用，Vuex 只能用于 vue。
    3. MobX 可以有多个 Store ，Redux 、Vuex 全局仅有一个 Store
    4. Redux 、Vuex 适用于大型项目的状态管理，MobX 在大型项目中应用会使代码可维护性变差。
    5. Redux 中引入了中间件，主要解决异步带来的副作用，可通过约定完成许多复杂工作。
    6. MobX 是状态管理库中代码侵入性最小的之一，具有细粒度控制、简单可扩展等优势，但是没有时间回溯能力，一般适合应用于中小型项目中。
---

# 说说你对 mobx 的理解
  * 概念：也是一个全局状态管理工具
  * 作用：可以让全局组件之间实现数据共享
  * 理解: 虽然框架一般都有组件通信的功能, 但是全局状态管理工具, 可以更人性的操作数据

# mobx实现原理
  1. 三个基本概念：
    * Observable  //被观察者
    * Observer    //观察者
    * Reaction    //响应
  2. 在被观察者和观察者之间建立依赖关系
    * 通过一个Reaction来track一个函数，该函数中访问了Observable变量，Observable变量的get方法会被执行，此时可以进行依赖收集，将此函数加入到该Observable变量的依赖中。
  3. 触发依赖函数
    * 上一步中Observable中，已经收集到了该函数。一旦Observable被修改，会调用set方法，就是依次执行该Observable之前收集的依赖函数，当前函数就会自动执行。
    * mobx底层对数据的观察，是使用Object.defineProperty(Mobx4)或Proxy(Mobx5)，Mobx4中Array是用类数组对象来模拟的，原始值是装箱为一个对象。
  4. mobx-react更新react状态
    * observer这个装饰器，对React组件的render方法进行track。将render方法，加入到各个observable的依赖中。当observable发生变化，track方法就会执行。
    * track中，还是先进行依赖收集，调用forceUpdate去更新组件，一个mobx添加的周期componentWillReact，也会在此时执行。然后结束依赖收集。
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
  * 
---

# 说说redux存储数据和本地存储数据有什么区别？
1. redux中的数据，在刷新（手动或者js触发）页面时，就会消失（或者说被初始化），无法持久化。
2. sessionStorage中的数据，关闭页面消失（会话结束）。
3. localStorage中的数据，永不消失（持久化在硬盘）。
4. redux中的数据发生变化，相关页面（connect），会自动变化，其他两者无此功能，这是主要区别。
5. 最重要的区别：redux存储在内存，localstorage则以文件的方式存储在本地
6. 应用场景：redux用于组件之间的传值，localstorage则主要用于不同页面之间的传值。
7. 永久性：当刷新页面时redux存储的值会丢失，localstorage不会。
---

# React数据持久化有什么实践吗？
  1. 数据持久化本质是利用localstorage，同一个域名共享同一个，且存储量不能超过5M，存储量太大会影响性能，建议不超过2.5M
  2. 如果要控制过期时间，可以在set数据时加个时间戳
  3. 注意JSON.stringify,有一些对象无法存储，例如：function、正则等
  4. 若业务线众多，建议划分域名使用localstorage
  5. 跨页面传输数据，优先建议url传输数据
---

# 使用redux时，不能在componentWillReceiveProps方法中使用action
原因：有时候一个action改变数据后，我们希望拿到改变后的数据做另外一个action，比如初始化action读取硬盘中的数据到内存，然后用该参数进行请求网络数据action。此时我们可以在componentWillReceiveProps方法中拿到参数，若此时发出再发出action，则数据返回后改变reducer会再次进入componentWillReceiveProps方法，又继续发出action，陷入死循环。
---

# Redux中间件是什么？接收几个参数？柯里化函数两端的参数具体是什么？
  * Redux中间件：提供的是位于action被发起之后，到达reducer之前的扩展点，也就是说，原本 view => action => reducer => store的数据流加上中间件后变成了 view => action => 中间件 => reducer => store，在这一环节我们可以做一些"副作用"的操作，比如异步请求，打印日志等

  * 接收一个对象做参数，对象的参数上有两个字段dispatch和getState, 分别代表着Redux Store上的两个同名函数

  * 柯里化函数两端一个是中间件，一个是store.dispatch
---

# redux中间件原理
  * 中间件提供第三方插件的模式，自定义拦截 action -> reducer 的过程。变为 action -> middlewares -> reducer 。这种机制可以让我们改变数据流，实现如异步 action ，action 过滤，日志输出，异常报告等功能。

  * 常见的中间件：
    - redux-logger：提供日志输出
    - redux-thunk：处理异步操作
    - redux-promise：处理异步操作，actionCreator的返回值是promise
---

# Redux 中间件是怎么拿到store 和 action? 然后怎么处理?
  > redux中间件本质就是一个函数柯里化。redux applyMiddleware Api 源码中每个middleware 接受2个参数， Store 的getState 函数和dispatch 函数，分别获得store和action，最终返回一个函数。该函数会被传入 next 的下一个 middleware 的 dispatch 方法，并返回一个接收 action 的新函数，这个函数可以直接调用 next（action），或者在其他需要的时刻调用，甚至根本不去调用它。调用链中最后一个 middleware 会接受真实的 store的 dispatch 方法作为 next 参数，并借此结束调用链。所以，middleware 的函数签名是（{ getState，dispatch })=> next => action。
---

# Redux中的connect有什么作用
connect负责连接React和Redux
  1. 获取state
    - connect 通过 context获取 Provider 中的 store，通过store.getState() 获取整个store tree 上所有state
  2. 包装原组件
    - 将state和action通过props的方式传入到原组件内部 wrapWithConnect 返回—个 ReactComponent 对 象 Connect，Connect 重 新 render 外部传入的原组件 WrappedComponent ，并把 connect 中传入的 mapStateToProps，mapDispatchToProps与组件上原有的 props合并后，通过属性的方式传给WrappedComponent
  3. 监听store tree变化
    - connect缓存了store tree中state的状态，通过当前state状态 和变更前 state 状态进行比较，从而确定是否调用 this.setState()方法触发Connect及其子组件的重新渲染
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

# React 的原生事件与它的合成事件的区别
---

# React 事件机制；React 为什么要用合成事件？
合成事件是围绕浏览器原生事件充当跨浏览器包装器的对象。它们将不同浏览器的行为合并为一个 API。这样做是为了确保事件在不同浏览器中显示一致的属性。
---

# React 优化
  * 在constructor改变this指向代替箭头函数和render内绑定this，避免函数作为props带来不必要的rerender
  * shouldComponentUpdate和PureComponent，减少不不必要的render
  * 使用唯一key优化list diff
  * 合并setState操作，减少虚拟dom对比频率
  * React路由动态加载react-loadable
  * 合理使用useMemo和useCallBack
  * 合理拆分组件
---

# shouldComponentUpdate是为了解决什么问题？
  > 组件重复渲染
---

# 在react中如何解决单页面开发首次加载白屏现象
1. 通过路由懒加载的方式 reactloadable
2. 首屏服务端渲染
---

# 什么是 Pure Components?
  * pureComponent表示一个纯组件，可以用来优化react程序。减少render函数渲染的次数。提高性能
  * pureComponent进行的是浅比较，也就是说如果是引用数据类型的数据，只会比较不是同一个地址，而不会比较这个地址里面的数据是否一致
  * 好处:当组件更新时，如果组件的props或者state都没有改变，render函数就不 会触发。省去虚拟DOM的生成和对比过程，达到提升性能的目的。具体原因 是因为react自动帮我们做了一层浅比较
---

# pureComponent和FunctionComponent区别？
  > Component需要自己实现shouldComponentUpdate
  > 但是在shouldComponentUpdate实现中PureComponent使用了props和state的浅比较。
---

# pureComponent执行的是浅比较还是深比较？
浅比较
---

# redux执行的是浅比较还是深比较？
浅比较
---

# 什么是纯组件？
纯（Pure） 组件是可以编写的最简单、最快的组件。它们可以替换任何只有 render() 的组件。这些组件增强了代码的简单性和应用的性能。
---

# 什么是受控组件?什么是非受控组件?
  * 受控: 在随后的用户输入中，能够控制表单中输入元素的组件被称为受控组件
  * 非受控: 非受控组件是在内部存储其自身状态的组件，当需要时，可以使用 ref 查询 DOM 并查找其当前值。
---

# 构造函数使用带 props 参数的目的是什么?
  * 为了在子构造函数中访问this.props。
---

# 为什么 React 使用 className 而不是 class 属性?
  * class 是 JavaScript 中的关键字，而 JSX 是 JavaScript 的扩展。
---

# React如何进行组件/逻辑复用?
  * 属性代理
  * 反向继承
  * 渲染属性
  * react-hooks
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

  4. 具体原理：React会维护两个链表，一个是CurrentHook,一个是WorkInProgressHook,每一个节点类型都是hook，每当调用useXXX的时候，React都会创建一个hook对象，并挂载到链表的尾部，函数式组件之所以可以做一些class才能做得事情，就是因为Hook对象，函数式组件的状态、计算值、缓存的函数都是交由hook进行管理的，需要和当前调用他的组件通过fiber.memoizedState关联起来，组件对应的fiber会维护一个memoizedState属性，它永远指向hook链表的头部
---

# React Hooks 如何更新状态(useState原理)
当我们在每次调用 dispatcher 时，并不会立刻对状态值进行修改，而是创建一条修改操作——在对应 Hook 对象的queue属性挂载的链表上加一个新节点，在下次执行函数组件，再次调用 useState 时， React 才会根据每个 Hook 上挂载的更新操作链表来计算最新的状态值。
---

# useEffect原理
  * useEffect 的保存方式与 useState / useReducer 类似，也是以链表的形式挂载在FiberNode.updateQueue中。
  * 在挂载阶段：
    1. 根据函数组件函数体中依次调用的 useEffect 语句，构建成一个链表并挂载在FiberNode.updateQueue中
    2. 组件完成渲染后，遍历链表执行。
  * 更新阶段：
    1. 同样在依次调用 useEffect 语句时，判断此时传入的依赖列表，与链表节点Effect.deps中保存的是否一致。如果一致，则在Effect.tag标记上NoHookEffect。
  * 在每次组件渲染完成后，就会进入 useEffect 的执行阶段：
    1. 遍历链表
    2. 如果遇到Effect.tag被标记上NoHookEffect的节点则跳过。
    3. 如果Effect.destroy为函数类型，则需要执行该清除副作用的函数（至于这Effect.destroy是从哪里来的，下面马上说到）
    4. 执行Effect.create，并将执行结果保存到Effect.destroy（如果开发者没有配置return，那得到的自然是undefined了，也就是说，开发者认为对于当前 useEffect 代码段，不存在需要清除的副作用）；注意由于闭包的缘故，Effect.destroy实际上可以访问到本次Effect.create函数作用域内的变量。
---

# 如何用 useEffect 模拟生命周期?
  ```
    useEffect(()=>{},[]) // 模拟 componentDidMount
    useEffect(()=>{},[xxx]) // 模拟 componentDidUpdate
  ```
---

# useEffect中的依赖是浅比较还是深比较
---

# 浅比较是怎么个比较法？
---

# useEffect不加数组和加数组的区别
---

# useEffect里面如何进行依赖项比较
---

# useEffect和useLayoutEffect区别？
  * useEffect是异步，useLayoutEffect是同步的
  * useEffect是render结束后，callback函数执行，但是不会阻断浏览器的渲染，算是某种异步的方式吧。但是class的componentDidMount 和componentDidUpdate是同步的,在render结束后就运行,useEffect在大部分场景下都比class的方式性能更好.
  * useLayoutEffect里面的callback函数会在DOM更新完成后立即执行,但是会在浏览器进行任何绘制之前运行完成,阻塞了浏览器的绘制.
---

# useEffect怎么知道关联的变量变化了
---

# 函数可不可以当 useEffect 的依赖?
可以当, 但是得在函数外包一层 useCallback, 这样函数就被缓存, 只有内部依赖改变时才会变化函数
---

# 为什么使用 useEffect 的时候会出现死循环？
如果是无限循环请求, 则代表你依赖的数据一直在变化, 比如说请求数据时一直设置引用类变量或者利用函数作为依赖
---

# 如果在 useEffect 的依赖项加入引用类型，会导致 useEffect 执行死循环，如何解决呢？
---

# useeffect第二个参数不加会怎样
---

# useEffect和闭包有什么关系，会产生什么问题
---

# useEffect的触发时机（无参数、`[]`,`[a,b]`），在render前还是render后
---

# 为什么函数组件要用hook
---

# useReducer与redux的区别
  1. useContext + useReducer可以一定程度上替代Redux
  2. useReducer的store是独立分开的，并非一个独立的大状态，Redux是一个统一的状态。
  3. useReducer的dispatch每个都是独立的，Redux是统一的对象。
  4. useContext使得数据流不只是至上而下，也能往上传递。
---

# useCallback与useMemo的区别
  * 相同点：useCallback 和 useMemo基本是一样的，都是对组件进行性能优化的，应用场景：父组件向子组件传值时，父组件render会导致子组件也会跟着render，不管传的值是否发生改变，子组件都会render，造成资源的浪费。用useCallback 和 useMemo可以监控传值情况，对其进行缓存，传值没有改变时，会阻止子组件render。
  
  * 不同点：useCallback直接缓存函数，useMemo缓存函数的返回值
---

# react hooks性能优化
  1. useMemo缓存数据
  2. useCallBack缓存函数
---

# 简单介绍下什么是hooks，hooks产生的背景？hooks的优点？
  * 概念：
    - Hooks是 React 16.8 中的新添加内容。它们允许在不编写类的情况下使用state和其他 React 特性。使用 Hooks，可以从组件中提取有状态逻辑，这样就可以独立地测试和重用它。Hooks 允许咱们在不改变组件层次结构的情况下重用有状态逻辑，这样在许多组件之间或与社区共享 Hooks 变得很容易。
  * 背景：
    -  组件之间复用状态逻辑很难，在hooks之前，实现组件复用，一般采用高阶组件和 Render Props，它们本质是将复用逻辑提升到父组件中，很容易产生很多包装组件，带来嵌套地域。
    - 组件逻辑变得越来越复杂，尤其是生命周期函数中常常包含一些不相关的逻辑，完全不相关的代码却在同一个方法中组合在一起。如此很容易产生 bug，并且导致逻辑不一致。
    - 复杂的class组件，使用class组件，需要理解 JavaScript 中 this 的工作方式，不能忘记绑定事件处理器等操作，代码复杂且冗余。除此之外，class组件也会让一些react优化措施失效。
    - 难以记忆的生命周期
  * 作用：
    - 用于在函数组件中引入状态管理和生命周期方法
    - 取代高阶组件和render props来实现抽象和可重用性
  * 优点也很明显：
    - 使用直观
    - 解决hoc的prop 重名问题
    - 解决render props 因共享数据 而出现嵌套地狱的问题
    - 能在return之外使用数据的问题
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

# 介绍下常用的hooks？
  * useState()，状态钩子。为函数组建提供内部状态
  * useContext()，共享钩子。该钩子的作用是，在组件之间共享状态。 可以解决react逐层通过Porps传递数据，它接受React.createContext()的返回结果作为参数，使用useContext将不再需要Provider 和 Consumer。
  * useReducer()，Action 钩子。useReducer() 提供了状态管理，其基本原理是通过用户在页面中发起action, 从而通过reducer方法来改变state, 从而实现页面和状态的通信。使用很像redux
  * useEffect()，副作用钩子。它接收两个参数， 第一个是进行的异步操作， 第二个是数组，用来给出Effect的依赖项
  * useRef()，获取组件的实例；渲染周期之间共享数据的存储(state不能存储跨渲染周期的数据，因为state的保存会触发组件重渲染）useRef传入一个参数initValue，并创建一个对象{ current: initValue }给函数组件使用，在整个生命周期中该对象保持不变。
  * useMemo和useCallback：可缓存函数的引用或值，useMemo用在计算值的缓存，注意不用滥用。经常用在下面两种场景（要保持引用相等；对于组件内部用到的 object、array、函数等，如果用在了其他 Hook 的依赖数组中，或者作为 props 传递给了下游组件，应该使用 useMemo/useCallback）
  * useLayoutEffect：会在所有的 DOM 变更之后同步调用 effect，可以使用它来读取 DOM 布局并同步触发重渲染
---

# React Hooks最佳实践
---

# hook中useReducer,useRef的介绍
---

# 描述下hooks下怎么模拟生命周期函数，模拟的生命周期和class中的生命周期有什么区别吗？
  ```
    // componentDidMount，必须加[],不然会默认每次渲染都执行
    useEffect(()=>{
    }, [])

    // componentDidUpdate
    useEffect(()=>{
    document.title = `You clicked ${count} times`;
    return()=>{
    // 以及 componentWillUnmount 执行的内容       
    }
    }, [count])

    //  shouldComponentUpdate, 只有 Parent 组件中的 count state 更新了，Child 才会重新渲染，否则不会。
    function Parent() {
        const [count,setCount] = useState(0);
        const child = useMemo(()=> <Child count={count} />, [count]);
        return <>{count}</>
    }

    function Child(props) {
        return <div>Count:{props.count}</div>
    }
  ```
  * 这里有一个点需要注意，就是默认的useEffect（不带[]）中return的清理函数，它和componentWillUnmount有本质区别的，默认情况下return，在每次useEffect执行前都会执行，并不是只有组件卸载的时候执行。
  * useEffect在副作用结束之后，会延迟一段时间执行，并非同步执行，和compontDidMount有本质区别。遇到dom操作，最好使用useLayoutEffect。
---

# hooks的出现给状态管理器带来的变化
---

# 怎么设计一个状态管理器
---

# 类组件和hook组件的区别
  * 类组件需要继承 class，函数组件不需要；
  * 类组件可以访问生命周期方法，函数组件不能；
  * 类组件中可以获取到实例化后的 this，并基于这个 this 做各种各样的事情，而函数组件不可以；
  * 类组件中可以定义并维护 state（状态），而函数组件不可以；
---

# 为什么 useState 要使用数组而不是对象
  * 如果 useState 返回的是数组，那么使用者可以对数组中的元素命名，代码看起来也比较干净
  * 如果 useState 返回的是对象，在解构对象的时候必须要和 useState 内部实现返回的对象同名，想要使用多次的话，必须得设置别名才能使用返回值
  * useState 返回的是 array 而不是 object 的原因就是为了降低使用的复杂度，返回数组的话可以直接根据顺序解构，而返回对象的话要想使用多次就需要定义别名了。
---

# React Hook 的使用限制有哪些？
  1. 不要在循环、条件或嵌套函数中调用 Hook；必须始终在 React函数的顶层使用Hook
    * 这是因为React需要利用调用顺序来正确更新相应的状态，以及调用相应的钩子函数。一旦在循环或条件分支语句中调用Hook，就容易导致调用顺序的不一致性，从而产生难以预料到的后果。
  2. 使用useState时候，使用push，pop，splice等直接更改数组对象,无法获取到新值,应该采用...方式
  3. useState设置状态的时候，只有第一次生效，后期需要更新状态，必须通过useEffect
  4. 善用useCallback，父组件传递给子组件事件句柄时，如果我们没有任何参数变动可能会选用useMemo。但是每一次父组件渲染子组件即使没变化也会跟着渲染一次。

# React Hooks 和生命周期的关系？
  * Hooks 组件有生命周期，而函数组件是没有生命周期的。
  * 具体关系：
    1. constructor：函数组件不需要构造函数，可以通过调用 useState 来初始化 state。如果计算的代价比较昂贵，也可以传一个函数给 useState。
    2. getDerivedStateFromProps：一般情况下，我们不需要使用它，可以在渲染过程中更新 state，以达到实现 getDerivedStateFromProps 的目的。
    3. shouldComponentUpdate：可以用 React.memo 包裹一个组件来对它的 props 进行浅比较
    4. 注意：React.memo 等效于 PureComponent，它只浅比较 props。这里也可以使用 useMemo 优化每一个节点。
    5. render：这是函数组件体本身。
    6. componentDidMount, componentDidUpdate： useLayoutEffect 与它们两的调用阶段是一样的。但是，我们推荐你一开始先用 useEffect，只有当它出问题的时候再尝试使用 useLayoutEffect。useEffect 可以表达所有这些的组合。
    7. componentWillUnmount：相当于 useEffect里面返回的 cleanup 函数
    8. componentDidCatch and getDerivedStateFromError：目前还没有这些方法的 Hook 等价写法，但很快会加上。
---

# 什么是高阶组件（HOC）? 作用是什么?
  * 概念: hoc是 React 中用于重用组件逻辑的高级技术，它是一个函数，能够接受一个组件并返回一个新的组件。
  * 作用:
    1. 代码复用，逻辑抽象化
    2. 渲染劫持
    3. 抽象化和操作状态（state）
    4. 操作属性（props）
  * 实现高阶组件的两种方式：
    - 属性代理。高阶组件通过包裹的React组件来操作props
    - 反向继承。高阶组件继承于被包裹的React组件
---

# React高阶组件，和普通组件有什么区别
---

# 什么是属性代理?
  * 属性代理组件继承自React.Component，通过传递给被包装的组件props得名
  * 属性代理的用途:
    - 更改 props，可以对传递的包裹组件的WrappedComponent的props进行控制
    - 通过 refs 获取组件实例
    - 把 WrappedComponent 与其它 elements 包装在一起
---

# 什么是反向继承？
反向继承是继承自传递过来的组件，反向继承允许高阶组件通过 this 关键词获取 WrappedComponent，意味着它可以获取到 state，props，组件生命周期（component lifecycle）钩子，以及渲染方法（render），所以我们主要用它来做渲染劫持，比如在渲染方法中读取或更改 React Elements tree，或者有条件的渲染等。
---

# 你的项目中怎么使用的高阶组件
  1. 项目中经常存在在配置系统中配置开关/全局常量，然后在页面需要请求配置来做控制，如果在每个需要调用全局设置的地方都去请求一下接口，就会有一种不优雅的感觉，这个时候我就想到利用高阶组件抽象一下。
  2. 项目开发过程中，经常也会遇到需要对当前页面的一些事件的默认执行做阻止，我们也可以写一个高阶组件等。
---

# hooks和hoc和render props有什么不同？
它们之间最大的不同在于，后两者仅仅是一种开发模式，而自定义的hooks是react提供的API模式，它既能更加自然的融入到react的渲染过程也更加符合react的函数编程理念。
---

# 了解过 react-router 内部实现机制吗？(实现原理)
  * 当用户点击页面跳转时，react-router阻止了浏览器的默认跳转行为，而改用history模块的pushState方法去触发url更新，当执行history.push时，执行了注册的listener函数，listener中的setState函数也被执行，将当前url地址栏对应的url传递下去，当Route组件匹配到该地址栏的时候，就会渲染该组件，如果匹配不到，Route组件就返回null
  * react-router依赖基础是history库：
    - 老浏览器的history: 主要通过hash来实现，对应createHashHistory
    - 高版本浏览器: 通过html5里面的history，对应createBrowserHistory
    - node环境下: 主要存储在memeory里面，对应createMemoryHistory
---

# react-router的原理
  * hashHistory：使用hash的方式，由于hash值改变时浏览器不会发送请求，实现了单页应用的功能。
     - 一开始将所有页面对应的hash值储存到一个数组中，并存入对应的回调函数。
     - 监听hashChange事件，当hash值改变时执行对应的回调函数，实现组件的切换。
     - 通过location.hash获取当前页面的hash值，执行对应回调

  * BroswerHistory(不能刷新,浏览器会自动发送请求，导致错误。可以通过在服务器配置，当收到不符合要求的url就返回index.html页面) 利用H5的history新特性实现，当使用history.pushState()/history.replaceState()改变url时，页面不会发送请求，只会改变url的值。但由于不能监听history的改变，所以不能监听到对应的url而执行对应回调，所以需要手动监听。
     - 一开始注册所有对应url的回调函数，存入数组。
     - 监听popState事件，当点击前进后退按钮时，会触发，然后执行对应的回调。
     - 当发生点击跳转时，取消点击事件的默认请求操作，调用pushState()改变url，执行对应的回调函数。
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

# react中常用的路由配置项？
  1. BrowserRouter 路由根组件，路径不带#号history路由
  2. HashRouter    路由根组件，路径带#号hash路由
  3. withRouter 对非路由渲染的组件组件进行包裹，让其具备三个属性
  4. Route 定义路由
  5. Link 路由跳转，没有动态属性，使用场景，非tabBar
  6. NavLink 路由跳转，有动态属性，使用场景，tabBar
  7. Switch 路由渲染，被其包裹的组件只会被渲染一个，包裹时最好将子组件 放在这个标签之外，父组件放在内部
  8. Redirect 路由重定向
---

# hashrouter通过什么来感知hash的变化
---

# 谈谈你对react中withRouter的理解
默认情况下必须是经过路由匹配渲染的组件在this.props上拥有路由参数，才能使用编程式导航的写法，withRouter是把不是通过路由切换过来的组件，将reactrouter 的 history、location、match 三个对象传入props对象上
---

# 如何控制路由的路径完全匹配？
  在NavLink或者Route标签中添加exact属性，是路径完全匹配
---

# react-router怎么实现路由切换
  1. Router组件使用Provider向下提供history对象，以及location的变更监听
  2. Route接收loaction(props || context)，Router尝试其属性path与location进行匹配，匹配检测(children > component > render)，内容渲染
  3. Link 跳转链接，生成一个a标签，禁用默认事件，js处理点击事件跳转(history.push) -> Link与直接使用a标签的区别是a页面会重新加载一下，Link只重新渲染Route组件部分
  4. Redirect to
  5. 从上下文中获取history this.context.router.history.push…
---

# react-router里的<Link>标签和<a>标签有什么区别
  1. 有onclick那就执行onclick
  2. click的时候阻止a标签默认事件
  3. 根据跳转href,用history跳转，此时只是连接变了，并没有刷新页面
---

# react router和常规路由有何不同？
  * 参与的页面：
   ​ reactRouter：只涉及单个HTML页面；常规路由：每个视图对应一个新文件。
  * URL更改：
    ​reactRouter：仅更改历史记录属性。常规路由：http请求被发送到服务器并且接收相应的html页面
  * 体验：
    ​reactRouter：用户认为自己正在不同的页面间切换。常规路由：用户实际在每个视图的不同页面切换
---

# router和router和route的区别
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

# react中的JSX原理
React.js 就把 JavaScript 的语法扩展了一下，让 JavaScript 语言能够支持这种直接在 JavaScript 代码里面编写类似 HTML 标签结构的语法，这样写起来就方便很多了。编译的过程会把类似 HTML 的 JSX 结构转换成 JavaScript 的对象结构。
React.createElement 会构建一个 JavaScript 对象来描述你 HTML 结构的信息，包括标签名、属性、还有子元素等。
jsx实际上就是一种javascript对象，他经过react语法的构造，还有编译转化，最后得到dom元素，可以插到页面中：所谓的 JSX 其实就是 JavaScript 对象，所以使用 React 和 JSX 的时候一定要经过编译的过程:
JSX —使用react构造组件，bable进行编译—> JavaScript对象 — ReactDOM.render()—>DOM元素 —>插入页面
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

# 虚拟dom真的比真实dom性能好吗？
  1. 首次渲染大量dom时，由于多了一层虚拟dom的计算，会比innerHTML插入强
  2. 正如它能保证性能下限，在真实dom操作的时候进行针对性的优化时，还是更快的
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

# 解释下React的Props?
  * Props 是 React 中属性的简写。它们是只读组件，不可变。它们总是在整个应用中从父组件传递到子组件。子组件永远不能将 prop 送回父组件。这有助于维护单向数据流，通常用于呈现动态生成的数据。
---

# React中的props为什么是只读的？
  因为react的特点是单向数据流，只能从父组件流向子组件
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
  * 简单说：将传递给 setState 的对象合并到组件的当前状态，这将启动一个和解的过程，构建一个新的 react 元素树，与上一个元素树进行对比（diff），从而进行最小化的重渲染。

  * 详细步骤：
    1. 将setState传入的partialState参数存储在当前组件实例的state暂存队列中。
    2. 判断当前React是否处于批量更新状态，如果是，将当前组件加入待更新的组件队列中。
    3. 如果未处于批量更新状态，将批量更新状态标识设置为true，用事务再次调用前一步方法，保证当前组件加入到了待更新组件队列中。
    4. 调用事务的waper方法，遍历待更新组件队列依次执行更新。
    5. 执行生命周期getDerivedStateFromProps。
    6. 将组件的state暂存队列中的state进行合并，获得最终要更新的state对象，并将队列置为空。
    7. 执行生命周期componentShouldUpdate，根据返回值判断是否要继续更新。
    8. 执行生命周期getSnapshotBeforeUpdate。
    9. 执行真正的更新，render。
    10. 执行生命周期componentDidUpdate。
---

# setState原理（同上）
  1. react的类组件通过state管理内部状态，而唯一改变状态更新的视图只能通过setState和forceUpdate。setState最终也会走forceUpdate。每个类组件都有一个updater对象用于管理state的变化。
  2. 当我们调用setState传入partialState时，会将partialState存入updater中的pendingState中
  3. 此时updater又会调用emitUpdate来决定当前是否立即更新，判断条件简单来说是否有nextProps,或者updateQueue的isPending是否开启。（这个updateQueue用于批量管理updater。）如果updateQueue的isPending为true，那么就将当前update直接加入到updateQueue的队列中。（开启isPending的方式可以是自定义方法和生命周期等。）
  4. 当这些方法执行完毕更新update,调用update的componentUpdate, 安段组件的shouldComponentUpdate决定是否调用forceUpdate进行更新
---

# state是怎么注入到组件的，从reducer到组件经历了什么样的过程
  * 通过connect和mapStateToProps将state注入到组件中
---

# React中setState的第二个参数作用是什么？
setState 的第二个参数是一个可选的回调函数。这个回调函数将在组件重新渲染后执行。等价于在 componentDidUpdate 生命周期内执行。通常建议使用 componentDidUpdate 来代替此方式。在这个回调函数中你可以拿到更新后 state 的值：
---

# 在React中组件的this.state和setState有什么区别？
this.state通常是用来初始化state的，this.setState是用来修改state值的。如果初始化了state之后再使用this.state，之前的state会被覆盖掉，如果使用this.setState，只会替换掉相应的state值。所以，如果想要修改state的值，就需要使用setState，而不能直接修改state，直接修改state之后页面是不会更新的。
---

# react进行setState后触发了哪些钩子（state和props触发更新的生命周期分别有什么区别）
  * setState的改变会触发4个生命周期钩子
    - shouldComponentUpdate(getDerivedStateFromProps)
    - componentWillUpdate(getSnapshotBeforeUpdate)
    - render
    - componentDidUpdate

  * props的改变会触发5个生命周期钩子
    - componentWillReveiceProps
    - shouldComponentUpdate
    - componentWillUpdate
    - render
    - componentDidUpdate
  
  * 区别：
    - 在getDerivedStateFromProps、shouldComponentUpdate、getSnapshotBeforeUpdate这几个生命周期中，如果是更新state,参数prevState会有值，nextProps是一个空对象；如果是更新props，参数nextProps会有值，prevState是一个空对象
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
---

# setState为什么默认是异步
  > 假如所有的setState是同步的，意味着每执行一次setState时都重新vnode diff + dom修改，这对性能来说是极为不好的，如果是异步，则可以把一个同步代码的多个setState合并成一次组件更新
---

# 回调函数作为 setState() 参数的目的是什么?
  * 当 setState 完成和组件渲染后，回调函数将会被调用。由于 setState() 是异步的，回调函数用于任何后续的操作。
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

# 在 componentWillMount() 方法中使用 setState() 好吗?
建议避免在 componentWillMount() 生命周期方法中执行异步初始化。在 mounting 发生之前会立即调用 componentWillMount()，且它在 render() 之前被调用，因此在此方法中更新状态将不会触发重新渲染。应避免在此方法中引入任何副作用或订阅操作。我们需要确保对组件初始化的异步调用发生在 componentDidMount() 中，而不是在 componentWillMount() 中。
---

# 什么是调解?
当组件的props或state发生更改时，React 通过将新返回的元素与先前呈现的元素进行比较来确定是否需要实际的 DOM 更新。当它们不相等时，React 将更新 DOM 。此过程称为reconciliation。
---

# 我们为什么不能直接更新状态?
  * 如果你尝试直接改变状态，那么组件将不会重新渲染。正确方法应该是使用 setState() 方法。它调度组件状态对象的更新。当状态更改时，组件通将会重新渲染。
---

# 聊聊 React 的 diff(原理)
当你使用react的时候，在某个时间点render函数创建了一颗react元素树，在下一个state或者props更新的时候,react函数将创建一颗新的react树,react将对比两棵树的不同之处，计算出如何高效的更新UI
---

# react diff和vue diff的差别
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

# 为什么要改变生命周期
  * Fiber 架构下，reconciler 会进行多次，reconciler 过程又会调用多次之前的 willxxx ，造成了语意不明确，因此干掉
  * 多次调用 willxxx 会导致一些性能安全/数据错乱等问题，因此 Unsafe
  * 静态函数 getDerivedStateFromProps ，直接将其函数内的用户逻辑降低几个数量级，减少用户出错，提高性能，符合语意
  * getSnapshotBeforeUpdate 替换之前 willxxxx，给想读取 dom 的用户一些空间，强逼用户到 mount 阶段才能操作 dom
  * 提高性能，减少 try catch 的使用
---

# 重新渲染界面这个过程在生命周期中哪几步？
static getDerivedStateFromProps() > shouldComponentUpdate > getSnapshotBeforeUpdate > render > componentDidUpdate
---

# 哪些方法会触发react重新渲染？重新渲染render会做些什么？
  * 方法：
    1. setState调用
    2. 父组件重新渲染
  * 做些什么：
    1. 会对新旧vNode进行对比
    2. 对新旧两棵树进行一个深度优先遍历，这样每一个节点都会游一个标记，在到深度遍历的时候，每遍历一个节点，就把该节点和新的节点树进行对比，如果有差异就放到一个对象里面
    3. 遍历差异对象，根据差异的类型，根据对应规则进行更新vNode
---

# 错误处理期间调用哪些方法?
static getDerivedStateFromError() > componentDidCatch()
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

# 在⽣命周期中的哪⼀步你应该发起 AJAX 请求
  * componentDidMount
  * React 下⼀代调和算法 Fiber 会通过开始或停⽌渲染的⽅式优化应⽤性能，其会影响 到 componentWillMount 的触发次数。对于 componentWillMount 这个⽣命周期函数 的调⽤次数会变得不确定， React 可能会多次频繁调⽤ componentWillMount 。
  * 如果我 们将 AJAX 请求放到 componentWillMount 函数中，那么显⽽易⻅其会被触发多次，⾃ 然也就不是好的选择。 如果我们将 AJAX 请求放置在⽣命周期的其他函数中，我们并不能保证请求仅在组件挂载 完毕后才会要求响应。如果我们的数据请求在组件挂载之前就完成，并且调⽤了setState 函数将数据添加到组件状态中，对于未挂载的组件则会报错。⽽在componentDidMount 函数中进⾏ AJAX 请求则能有效避免这个问题
---

# 哪些生命周期会执行多次
render、componentWillReceiveProps、shouldComponentUpdate、componentWillUpdate、componentDidUpdate
---

# 生命周期方法 getDerivedStateFromProps() 的目的是什么?
  * 新的静态 getDerivedStateFromProps() 生命周期方法在实例化组件之后以及重新渲染组件之前调用。它可以返回一个对象用于更新状态，或者返回 null 指示新的属性不需要任何状态更新。此生命周期方法与 componentDidUpdate() 一起涵盖了 componentWillReceiveProps() 的所有用例。
---

# 生命周期方法 getSnapshotBeforeUpdate() 的目的是什么?
  * 新的 getSnapshotBeforeUpdate() 生命周期方法在 DOM 更新之前被调用。此方法的返回值将作为第三个参数传递给componentDidUpdate()。此生命周期方法与 componentDidUpdate() 一起涵盖了 componentWillUpdate() 的所有用例。
---

# forceUpdate经历了哪些生命周期，子组件呢?
  shouldComponentUpdate、componentWillUpdate、render、componentDidUpdate
---

# this.setState执行后会执行哪些生命周期函数？
  shouldComponentUpdate、componentWillUpdate、render、componentDidUpdate
---

# fiber带来的新的生命周期是什么
  ![react面试题](/images/vue/vue面试5.png)
---

# 渲染一个react组件的过程
   1. 在组件渲染过程中，会先走constructor处理里面的逻辑，如：设置初始的state
   2. 再执行getDeriveStateFromProps钩子，return prevProps的操作就是将props的内容映射到state中，使用getDeriveStateFromProps钩子，需要先使用this.state
   3. 组件渲染的过程中都会走一遍componentDidMount钩子
   4. 如果有触发state更新，钩子会从getDeriveStateFromProps开始处理
   5. 再执行shouldComponentUpdate钩子，满足条件后返回true触发更新走render函数
   6. render之后还要看是否有进一步的更新操作
   7. 看是否有getSnapshotBeforeUpdate钩子拦截 它的返回值是componentDidupdate钩子的第三个参数snapshot
   8. 不管有咩有getSnapshotBeforeUpdate，都会走到componentDidUpdate钩子，如果再这个过程中还有操作引起state的内容变化，会再次从getDerivedStateFromProps --- shouldComponentUpdate --- render ---getSnapshotBeforeUpdate --- componentDidUpdate

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

# 什么是React.forwardRef？它有什么作用？
  * React.forwardRef 会创建一个React组件，这个组件能够将其接受的 ref 属性转发到其组件树下的另一个组件中。
  * 作用：
    - 转发 refs 到 DOM 组件
    - 在高阶组件中转发 refs
---

# react key是干嘛用的，为什么要加？key主要是解决哪一类问题
key 用于识别唯一的 Virtual DOM 元素及其驱动 UI 的相应数据。它们通过回收 DOM 中当前所有的元素来帮助 React 优化渲染。这些 key 必须是唯一的数字或字符串，React 只是重新排序元素而不是重新渲染它们。这可以提高应用程序的性能。
---

# createElement 和 cloneElement 有什么区别?
JSX 元素将被转换为 React.createElement() 函数来创建 React 元素，这些对象将用于表示 UI 对象。而 cloneElement 用于克隆元素并传递新的属性
---

# 什么是上下文（Context）?
Context 通过组件树提供了一个传递数据的方法，从而避免了在每一个层级手动的传递props。比如，需要在应用中许多组件需要访问登录用户信息、地区偏好、UI主题等。
---

# 谈谈你对context的理解
  > 当你不想在组件树中通过逐层传递props或者state的方式来传递数据时，可以使用Context来实现跨层级的组件数据传递。Context对象就好比一个直通给子组件访问的作用域，而Context对象的属性可以看成作用域上的活动对象。由于组件的Context由其父节点链上所有组件通过getChildContext()返回的Context对象组合而成，所以，组件通过Context是可以访问到其父组件链上所有节点组件提供的Context的属性
---

# react中context作用和使用场景，在之前版本的react中类组件怎么获得context呢
---

# children 属性是什么?
Children 是一个属性（this.props.chldren），它允许你将组件作为数据传递给其他组件，就像你使用的任何其他组件一样。在组件的开始和结束标记之间放置的组件树将作为children属性传递给该组件。
---

# React.Children.map和js的map有什么区别？
JavaScript中的map不会对为null或者undefined的数据进行处理，而React.Children.map中的map可以处理React.Children为null或者undefined的情况。
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

# Fiber算法原理
  0. 总结：更新任务分成俩个阶段，Reconcilition Phase(调和阶段)和Commit Phase(交付阶段)。Reconciliation Phase的任务干的事情是，找出要做的更新工作(Diff Fiber Tree),就是一个计算阶段，计算结果可以被缓存，也就可以被打断；Commit Phase需要提交所有更新并渲染，为了防止页面抖动，被设置为不能打断。
  1. 用户操作引起setState被调用以后，先调用enqueueSetState方法，该方法可以划分成俩个阶段（个人理解），第一阶段Data Preparation，是初始化一些数据结构，比如fiber，updateQueue，update。
  2. 新的update会通过insertUpdateIntoQueue方法，根据优先级插入到队列的对应位置，ensureUpdateQueues方法初始化俩个更新队列，queue1和current.updateQueue对应，queue2和current.alternate.updateQueue对应。
  3. 第二阶段，Fiber Reconciler，就开始进行任务分片调度，scheduleWork首先更新每个fiber的优先级，这里并没有updatePriority这个方法，但是干了这件事。当fiber.return === null，找到父节点，把所有diff出的变化(side effect)归结到root上。
  4. requestWork，首先把当前的更新添加到schedule list中(addRootToSchedule),然后根据当前是否为异步渲染(isAsync参数)，异步渲染调用。scheduleCallbackWithExpriation方法，下一步高能！！
  5. scheduleCallbackWithExpriation这个方法在不同环境，实现不一样，chrome等浏览器中使用requestIdleCallback API，没有这个API的浏览器中，通过requestAnimationFrame模拟一个requestIdCallback，来在浏览器空闲时，完成下一个分片的工作，注意，这个函数会传入一个expirationTime，超过这个时间活没干完，就放弃了。
  6. 执行到performWorkOnRoot，就是fiber文档中提到的Commit Phase和Reconciliation Phase俩阶段。
  7. 第一阶段Reconciliation Phase,在workLoop中，通过一个while循环，完成每个分片任务。
  8. performUnitOfWork也可以分成俩阶段，蓝色框表示。beginWork是一个入口函数，根据workInProgress的类型去实例化不同的react element class。workInProgress是通过alternate挂载一些新属性获得的。
  9. 实例化不同的react element class时候会调用和will有关的生命周期方法。
  10. completeUnitOfWork是进行一些收尾工作，diff完一个节点以后，更新props和调用生命周期方法等。
  11. 然后进入Commit Phase阶段，这个阶段不能被打断。
---

# 为什么 Fiber 双向链表的结构可以解决递归慢的问题
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

# 请解释下React的事件？
  * 在React中，事件是对鼠标悬停，淡季，按键等特定操作的触发反应，类似于处理dom元素的事件
  * 与处理dom元素的事件的区别：
    1. 驼峰命名法对事件命名而不是仅适用于小写字母
    2. 事件作为函数而不是字符串传递，时间参数包含一组特定于事件的属性，每个事件类型都包含自己的属性和行为，只能通过其事件处理程序访问
---

# React 事件绑定原理
  * React并不是将click事件绑定到了div的真实DOM上，而是在document处监听了所有的事件，当事件发生并且冒泡到document处的时候，React将事件内容封装并交由真正的处理函数运行。这样的方式不仅仅减少了内存的消耗，还能在组件挂在销毁时统一订阅和移除事件。

  * 除此之外，冒泡到document上的事件也不是原生的浏览器事件，而是由react自己实现的合成事件（SyntheticEvent）。因此如果不想要是事件冒泡的话应该调用event.preventDefault()方法，而不是调用event.stopProppagation()方法。
---

# React组件中怎么做事件代理
  > React基于Virtual DOM实现了一个SyntheticEvent层（合成事件层），定义的事件处理器会接收到一个合成事件对象的实例，它符合W3C标准，且与原生的浏览器事件拥有同样的接口，支持冒泡机制，所有的事件都自动绑定在最外层上。
---

# React组件事件代理的原理
  * 在React底层，主要对合成事件做了：事件委派和自动绑定。
    - 事件委派： React会把所有的事件绑定到结构的最外层，使用统一的事件监听器，这个事件监听器上维持了一个映射来保存所有组件内部事件监听和处理函数。
    - 自动绑定： React组件中，每个方法的上下文都会指向该组件的实例，即自动绑定this为当前组件。
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

# React如何判断什么时候重新渲染组件？
  * 组件状态的改变可以因为props的改变，或者直接通过setState方法改变。组件获得新的状态然后React决定是否应该重新渲染组件。只要组件的state发生变化，React就会对组件进行重新渲染。这是因为React中的shouldComponentUpdate方法默认返回true，这就是导致每次更新都重新渲染的原因。

  * 当React将要渲染组件时会执行shouldComponentUpdate方法来看它是否返回true（组件应该更新，也就是重新渲染）。所以需要重写shouldComponentUpdate方法让它根据情况返回true或者false来告诉React什么时候重新渲染什么时候跳过重新渲染。
---

# 对React SSR的理解
  * 服务端渲染是数据与模版组成的html，即 html= 数据 ＋ 模版。将组件或页面通过服务器生成html字符串，再发送到浏览器，最后将静态标记"混合"为客户端上完全交互的应用程序。页面没使用服务渲染，当请求页面时，返回的body里为空，之后执行js将html结构注入到body里，结合css显示出来;
  * 优势：
    - 对SEO友好
    - 所有的模版、图片等资源都存在服务器端
    - 一个html返回所有数据
    - 减少HTTP请求
    - 响应快、用户体验好、首屏渲染快
  * 劣势:
    - 服务端压力较大
      - 本来是通过客户端完成渲染，现在统一到服务端node服务去做。尤其是高并发访问的情况，会大量占用服务端CPU资源;
    - 开发条件受限
      - 在服务端渲染中，只会执行到componentDidMount之前的生命周期钩子，因此项目引用的第三方的库也不可用其它生命周期钩子，这对引用库的选择产生了很大的限制;
    - 学习成本相对较高
      - 除了对webpack、MVVM框架要熟悉，还需要掌握node、 Koa2等相关技术。相对于客户端渲染，项目构建、部署过程更加复杂。
---

# 什么是纯函数？
纯函数是不依赖并且不会在其作用域之外修改变量状态的函数。本质上，纯函数始终在给定相同参数的情况下返回相同结果。
---

# 纯函数的概念，好处和用途
---

# React/Redux中哪些功能用到了哪些设计模式
---

# Class中static的属性和普通属性的区别，从继承的角度来说呢？
---

# dva的数据流是怎么样的？同步情况和异步情况？
---

# 讲一下 DVA 的思想和数据流向 effects
  1. 一个基于 redux 和 redux-saga 的数据流方案，然后为了简化开发体验，dva 还额外内置了 react-router 和 fetch，所以也可以理解为一个轻量级的应用框架。
  2. （举个例子）输入url渲染对应的组件，该组件通过dispatch去出发action里面的函数，如果是同步的就去进入model的ruducer去修改state，如果是异步比如fetch获取数据就会被effect拦截通过server交互获取数据进而修改state，同样state通过connect将model、状态数据与组件相连
---

# react父组件更新会导致子组件更新吗？为什么？如何优化？
---

# React Native是如何渲染的
---

# React Native长列表优化
---

# 知道 React Native 加载 bundle 的机制吗？
要实现 RN 的脚本热更新，我们要搞明白 RN 是如何去加载脚本的。 在编写业务逻辑的时候，我们会有许多个 js 文件，打包的时候 RN 会将这些个 js 文件打包成一个叫 index.android.bundle(ios 的是 index.ios.bundle)的文件，所有的 js 代码(包括 rn 源代码、第三方库、业务逻辑的代码)都在这一个文件里，启动 App 时会第一时间加载 bundle 文件，所以脚本热更新要做的事情就是替换掉这个 bundle 文件。
---

# React Native和Flutter 的区别？谁的操作效率更高？更快？
---

# RN混原生和原生混RN有什么不同？
  1. RN混原生
    * RN混原生，工程中大部分是RN，调用部分原生模块。可以实现调用设备API，实现原生功能
  2. 原生混RN
    * 原生混RN，RN的js代码最后都是打包为jsbundle文件，所以其实就是如何使用这个文件来渲染一个原生组件和处理逻辑。
    * 原生混用RN可以实现多业务拆分，rn实现的不同业务代码打包多个jsbundle文件，再由原生调用，可以业务间的独立性
---

# react-native的原理
  > React Native 渲染 在 React 框架中，JSX 源码通过 React 框架最终渲染到了浏览器的真实 DOM 中，而在 React Native 框架中，JSX 源码通过 React Native 框架编译后，通过对应平台的 Bridge 实现了与原生框架的通信。如果我们在程序中调用了 React Native 提供的 API，那么 React Native 框架就通过 Bridge 调用原生框架中的方法。 因为 React Native 的底层为 React 框架，所以如果是 UI 层的变更，那么就映射为虚拟 DOM 后进行 diff 算法，diff 算法计算出变动后的 JSON 映射文件，最终由 Native 层将此 JSON 文件映射渲染到原生 App 的页面元素上，最终实现了在项目中只需要控制 state 以及 props 的变更来引起 iOS 与 Android 平台的 UI 变更。 编写的 React Native代码最终会打包生成一个 main.bundle.js 文件供 App 加载，此文件可以在 App 设备本地，也可以存放于服务器上供 App 下载更新
---

# RN如何调用原生的一些功能？
  > 使用原生暴露的模块来使用原生的方法或者view，js层通过NativeModules.模块名.方法名来调用
---

# RN如何和原生进行通信？
  * 通过原生Module进行交互
  * 通过原生 View 进行交互
  * 通过发送事件 Event 进行交互
---

# 如何做RN在安卓和IOS端的适配
  1. 布局：由于RN组件的尺寸是不用传单位的，而设计稿一般标记的是px，所以要进行转换，规则是：`px * 设备分辨率 / 设计稿的基准宽度`
  2. 图片：需要准备1倍，2倍，3倍3种尺寸，rn会根据不同的分辨率选择不同的倍图
  3. 组件：在RN自带组件有些带有平台标记的组件，比如说ImagePickerIOS,表明该组件是用于IOS平台。所以如果安卓和ios效果不一样的时候，使用Platform.OS来区分平台，进行不同的布局
---

# RN为什么能在原生中绘制成原生组件
  > RN写的代码最终会打包成jsbundle并被原生加载解析，在打包之前会经过babel把jsx解析为js纯函数，RN上的组件现在都是ReactElement对象。在原生创建好了js线程和加载完jsbundle之后开始执行js代码，而原生到rn的入口是appResgistry.registerComponent,在这个方法里一路走可以看到几个主要函数调用是renderApplication => React.createElement，RN上的ReactElement调用React.createElement渲染，最终调用的是原生模块RCTUIManager的createView方法来创建原生组件，通过yoga引擎解析计算组件的布局，最终渲染出对应的原生组件。
---

# Native提供了什么能力给RN？
  1. 扩展原生功能给到RN使用，比如获取设备信息，使用设备硬件。
  2. 创建原生view给的RN使用，如果官方demo中的原生封装地图组件给到rn使用，使其保持原生良好的性能。
---

# 如何解决props层级过深的问题
  1. ContextAPI: 提供一种组件间的共享状态，而不必通过显示组件树逐层传递props
  2. Redux等应用状态管理库
---

# 使用 React 时候遇到的难点？
---

# 用过哪些自定义 hooks，第三方 hooks ？
---

# 写react hooks时需要注意的问题，为什么不能在条件判断循环里写，array是全局的吗，useEffect有什么问题
---

# React Context 和 redux mobx的区别
---
