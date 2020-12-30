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

# 有用过状态管理吗？说说redux 的理念；
```
  概念: Redux 是 React的一个状态管理库

  工作原理；
    <1>.在React中，组件连接到 redux ，如果要访问 redux，需要派出一个包含 id和负载(payload) 的 action。action 中的 payload 是可选的，action 将其转发给 Reducer。

    <2>.当reducer收到action时，通过 swithc...case 语法比较 action 中type。 匹配时，更新对应的内容返回新的 state。

    <3>.当Redux状态更改时，连接到Redux的组件将接收新的状态作为props。当组件接收到这些props时，它将进入更新阶段并重新渲染 UI。
```

# redux 做状态管理和发布订阅模式有什么区别？
    redux 其实也是一个发布订阅，但是 redux 可以做到数据的可预测和可回溯。
---

# react-redux 的原理，它是怎么跟 react 关联起来的？
  * react-redux 的核心组件只有两个，Provider 和 connect，Provider 存放 Redux 里 store 的数据到 context 里，通过 connect 从 context 拿数据，通过 props 传递给 connect 所包裹的组件。
---

# react 16 生命周期有什么改变？
---

# class 组件与函数式组件的区别
  * 函数或无状态组件是一个纯函数，它可接受接受参数，并返回react元素。这些都是没有任何副作用的纯函数。这些组件没有状态或生命周期方法
  * 类或有状态组件具有状态和生命周期方可能通过setState()方法更改组件的状态。类组件是通过扩展React创建的。它在构造函数中初始化，也可能有子组件
---

# React 事件机制；React 为什么要用合成事件？
---

# 聊聊 React 的 diff
---

# React 优化
  * 适当地使用shouldComponentUpdate生命周期方法。 它避免了子组件的不必要的渲染。 如果树中有100个组件，则不重新渲染整个组件树来提高应用程序性能。
  * 不可变性是提高性能的关键。不要对数据进行修改，而是始终在现有集合的基础上创建新的集合，以保持尽可能少的复制，从而提高性能。
  * 在显示列表或表格时始终使用 Keys，这会让 React 的更新速度更快
  * 代码分离是将代码插入到单独的文件中，只加载模块或部分所需的文件的技术。
---

# 什么是Virtual DOM及其工作原理
  * 概念：虚拟DOM可以看做一棵模拟了DOM树的JavaScript对象树。
  * 原理: React将整个DOM副本保存为虚拟DOM,每当有更新时，它都会维护两个虚拟DOM，以比较之前的状态和当前状态，并确定哪些对象已被更改。 它通过比较两个虚拟DOM 差异，并将这些变化更新到实际DOM,一旦真正的DOM更新，它也会更新UI 
---

# 什么是 JSX?
  * JSX是javascript的语法扩展。它就像一个拥有javascript全部功能的模板语言。它生成React元素，这些元素将在DOM中呈现。
---

# 什么是 Pure Components?
  * React.PureComponent 与 React.Component 完全相同，只是它为你处理了 shouldComponentUpdate() 方法。当属性或状态发生变化时，PureComponent 将对属性和状态进行浅比较。另一方面，一般的组件不会将当前的属性和状态与新的属性和状态进行比较。因此，在默认情况下，每当调用 shouldComponentUpdate 时，默认返回 true，所以组件都将重新渲染。
---

# 状态和属性有什么区别?
  * Props 以类似于函数参数的方式传递给组件，而状态则类似于在函数内声明变量并对它进行管理。
  * 区别：
| 条件                 | States | Props |
| -------------------- | ------ | ----- |
| 可从父组件接收初始值 | 是     | 是    |
| 可在父组件中改变其值 | 否     | 是    |
| 在组件内设置默认值   | 是     | 是    |
| 在组件内可改变       | 是     | 否    |
| 可作为子组件的初始值 | 是     | 是    |

---

# 我们为什么不能直接更新状态?
  * 如果你尝试直接改变状态，那么组件将不会重新渲染。正确方法应该是使用 setState() 方法。它调度组件状态对象的更新。当状态更改时，组件通将会重新渲染。
---

# 回调函数作为 setState() 参数的目的是什么?
  * 当 setState 完成和组件渲染后，回调函数将会被调用。由于 setState() 是异步的，回调函数用于任何后续的操作。
---

# 什么是受控组件?什么是非受控组件?
  * 受控: 在随后的用户输入中，能够控制表单中输入元素的组件被称为受控组件
  * 非受控: 非受控组件是在内部存储其自身状态的组件，当需要时，可以使用 ref 查询 DOM 并查找其当前值。
---

# 组件生命周期的不同阶段是什么?
  * **初始化**：在组件初始化阶段会执行
    1. constructor （用来定义状态，或者是用来放一些this的方法的）
    2. static getDerivedStateFromProps() ----将来会使用（定义一个状态，这个状态将来会使用）
    3. componentWillMount() / UNSAFE_componentWillMount() ----带有 UNSAFE属于过时的钩子函数（老版本的）componentWillMount()会在17版本后启用，使用static getDerivedStateFromProps()
    4. render()
    5. componentDidMount()（组件挂载结束）

  * **更新阶段**: props或state的改变可能会引起组件的更新，组件重新渲染的过程中会调用以下方法：
    1. componentWillReceiveProps() / UNSAFE_componentWillReceiveProps() （属性发生改变）
    2. static getDerivedStateFromProps()（属性发生改变触发）
    3. shouldComponentUpdate() // react性能优化第二方案 （组件应该更新么）
    4. componentWillUpdate() / UNSAFE_componentWillUpdate()
    5. render()
    6. getSnapshotBeforeUpdate() （快照）
    7. componentDidUpdate()（组件更新结束）

  * **卸载阶段**
    1. componentWillUnmount()

  * **错误处理**
    1. componentDidCatch() --16.3版本之后出现的 
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

# 执行setState发生了什么？
  * setState() 是一个异步操作。出于性能原因，React 会对状态更改进行批处理，因此在调用 setState() 方法之后，状态可能不会立即更改。
---

# forceUpdate经历了哪些生命周期，子组件呢?
  * 都有render
---

# hooks 原理
  1. 单向链表通过next把hooks串联起来
  2. memoizedState存在fiber node上，组件之间不会相互影响
  3. useState和useReducer中通过dispatchAction调度更新任务
---

# hooks和class Component的区别\
---

# 了解过 react-router 内部实现机制吗？
---

# React Native 的运行机制？
---