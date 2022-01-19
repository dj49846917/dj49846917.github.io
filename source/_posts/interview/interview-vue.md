---
title: 面试——vue篇
date: 2020-12-28 15:06:19
tags: 
  - 面试
  - vue
type: 面试                                                                 # 标签、分类
description:  Vue是一套用于构建用户界面的渐进式框架。
top_img: /images/vue/vue.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 面试
  - vue                                                                 # 文章标签
cover: /images/vue/vue.jpg                 # 文章的缩略图（用在首页）
---

# 谈谈你对MVVM、MVC开发模式的理解
  1. MVVM分为Model、View、ViewModel三者。
    * Model 代表数据模型，数据和业务逻辑都在Model层中定义；
    * View 代表UI视图，负责数据的展示；
    * ViewModel 负责监听 Model 中数据的改变并且控制视图的更新，处理用户交互操作；

  2. Model 和 View 并无直接关联，而是通过 ViewModel 来进行联系的，Model 和 ViewModel 之间有着双向数据绑定的联系。因此当 Model 中的数据改变时会触发 View 层的刷新，View 中由于用户交互操作而改变的数据也会在 Model 中同步。这种模式实现了 Model 和 View 的数据自动同步，因此开发者只需要专注对数据的维护操作即可，而不需要自己操作 dom。

  3. 什么是MVC：
    * M（modal）：是应用程序中处理数据逻辑的部分。
    * V （view） ：是应用程序中数据显示的部分。
    * C（controller）：是应用程序中处理用户交互的地方
---

# 简述Vue的响应式原理
  * 当一个Vue实例创建时，vue会遍历data选项的属性，用 Object.defineProperty 将它们转为 getter/setter并且在内部追踪相关依赖，在属性被访问和修改时通知变化。每个组件实例都有相应的 watcher 程序实例，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的 setter 被调用时，会通知 watcher 重新计算，从而致使它关联的组件得以更新。
  * 简单版：数据劫持收集依赖派发更新

  * 缺点:
    1. 无法监测到对象属性的动态添加和删除
    2. 无法监测到数组的下标和length属性的变更
   
  * 解决方案：
    1. vue2提供vue.set方法用于动态给对象添加属性
      * 给对象新增一个属性，页面不会变化，需要使用 vm.$set(对象，key，值)
    2. vue2提供vue.delete方法用于动态删除对象的属性
      * 删除对象一个属性delete obj.a页面没有变化，需要使用vm.$delete(对象，key)
    3. 重写vue中数组的方法，用于监测数组的变更
      * 给数组适用下标方式新增一项（arr[3] = ‘哈哈’），页面无变化，需要使用 vm.$set(数组，3，‘哈哈’)或者使用push方法
---

# v-if 和 v-show 有什么区别？
  * v-show 仅仅控制元素的显示方式，将 display 属性在 block 和 none 来回切换；而v-if会控制这个 DOM 节点的存在与否
---

# Vue中如何监控某个属性值的变化？
    使用watch或者computed
---

# vue中给data中的对象属性添加一个新的属性时会发生什么，如何解决？
  * 直接通过methods给data里新增一个值，发现，值新增成功了，但是视图没有更新，原因在于在Vue实例创建时，新增的值并未声明，因此就没有被Vue转换为响应式的属性，自然就不会触发视图的更新，这时就需要使用Vue的全局api $set()。

  * $set()方法相当于手动的去把obj.b处理成一个响应式的属性
  * 在vue3.x中，将数据设置成proxy，可以更新页面
---

# delete和Vue.delete删除数组的区别
  1. delete只是被删除的元素变成了 empty/undefined 其他的元素的键值还是不变。
  2. Vue.delete直接删除了数组 改变了数组的键值。
---

# Vue的路由实现：hash模式 和 history模式的区别
  1. hash模式：在浏览器中符号“#”，#以及#后面的字符称之为hash，用window.location.hash读取；
	  - 特点：hash虽然在URL中，但不被包括在HTTP请求中；用来指导浏览器动作，对服务端安全无用，hash不会重加载页面。

  2. history模式：history采用HTML5的新特性；且提供了两个新方法：pushState（），replaceState（）可以对浏览器历史记录栈进行修改，以及popState事件的监听到状态变更。
---

# 谈谈Computed和Watch！vue中的computed和watch有什么区别？
  * Computed本质是一个具备缓存的watcher，依赖的属性发生变化就会更新视图。适用于计算比较消耗性能的计算场景。当表达式过于复杂时，在模板中放入过多逻辑会让模板难以维护，可以将复杂的逻辑放入计算属性中处理。
  * Watch没有缓存性，更多的是观察的作用，可以监听某些数据执行回调。当我们需要深度监听对象中的属性时，可以打开deep：true选项，这样便会对对象中的每一项进行监听。这样会带来性能问题，优化的话可以使用字符串形式监听，如果没有写到组件中，不要忘记使用unWatch手动注销哦。

  * 区别：在computed里面定义了的属性，在data里面不能再重复定义，computed依赖于data中的属性，而watch依赖于data中属性的改变，watch中可以进行异步操作。
---

# computed中的get作用是什么
  > 处理响应式的数据，并缓存
---

# computed实现原理
  > 在对computed的属性进行 watcher 的时候，传入一个 lazy 为 true 的参数，在 watcher 内部将 lazy 值赋值给 dirty 属性，在获取 computed 属性的时候，如果 dirty 为 true，则重新执行被 defineComputed 改写过的 get 方法，获取最新值，如果 dirty 为 false，则获取上一次 watcher 实例的 value 值，这里就实现了缓存。对于 dirty 值更改为 true 的时机，则是在 props 和 data 等被 defineProperty 劫持改写的 set 方法内，每当数据发生变化，则通过 defineReactive$$1 方法最后执行 update 方法更新 dirty 值。
---

# 如果要你实现computed的特性（缓存、响应），你会怎么做
---

# 初次加载页面computed会不会立即执行，watch会不会立即执行（watch的immediate设置为true就会立即执行）
---

# Vue.observable你有了解过吗？说说看
  * vue2.6发布一个新的API，可以处理一些简单的跨组件共享数据状态的问题。
---

# 你知道style加scoped属性的用途和原理吗？
  * 用途：防止全局同名CSS污染
  * 原理：在标签加上v-data-something属性，再在选择器时加上对应[v-data-something]，即CSS带属性选择器，以此完成类似作用域的选择方式
---

# 如何在子组件中访问父组件的实例？
    this.$parent
---

# watch，methods的属性用箭头函数定义结果会怎么样？
    因为箭头函数默绑定父级作用域的上下文，所以不会绑定vue实例，所以 this 是undefind
---

# 在vue项目中如何配置favicon？
    在index.html中加入meta标签，引入favicon
---

# 在vue事件中传入$event，使用e.target和e.currentTarget有什么区别？
  * currentTarget：事件绑定的元素
  * target:鼠标触发的元素
---

# 在.vue文件中style是必须的吗？那script是必须的吗？为什么？
    都不是必须的
---

# vue如果想扩展某个现有的组件时，怎么做呢？
  1. 使用Vue.extend直接扩展
  2. 使用Vue.mixin全局混入
  3. HOC封装
---

# 说说你对vue的表单修饰符.lazy的理解
  * input标签v-model用lazy修饰之后，vue并不会立即监听input Value的改变，会在input失去焦点之后，才会触发input Value的改变
---

# vue为什么要求组件模板只能有一个根元素？
  ```
    <1>.当我们实例化Vue的时候，填写一个el选项，来指定我们的SPA入口：
      let vm = new Vue({
              el:'#app'
      })

    <2>.同时我们也会在body里面新增一个id为app的div
      <body>
              <div id='app'></div>
      </body>
  ```
---

# vue组件里写的原生addEventListeners监听事件，要手动去销毁吗？为什么？
    肯定要，一方面是绑定多次，另一方面是函数没释放会内存溢出
---

# 使用vue渲染大量数据时应该怎么优化？说下你的思路！
  1. 异步渲染组件：因为组件渲染太多，影响页面的渲染时间，所有可以延迟组件渲染，可以考虑v-if处理
  2. 可以使用虚拟滚动的组件：参考使用这个插件
---

# Vue性能优化方法
  1. 编码阶段
    * 尽量减少data中的数据，data中的数据都会增加getter和setter，会收集对应的watcher；
    * 如果需要使用v-for给每项元素绑定事件时使用事件代理；
    * SPA 页面采用keep-alive缓存组件；
    * 在更多的情况下，使用v-if替代v-show；
    * key保证唯一；
    * 使用路由懒加载、异步组件；
    * 防抖、节流；
    * 第三方模块按需导入；
    * 长列表滚动到可视区域动态加载；
    * 图片懒加载；
  2. 用户体验：
    * 骨架屏；
    * PWA；
    * 还可以使用缓存(客户端缓存、服务端缓存)优化、服务端开启gzip压缩等。
  3. SEO优化
    * 预渲染；
    * 服务端渲染SSR；
  4. 打包优化
    * 压缩代码；
    * Tree Shaking/Scope Hoisting；
    * 使用cdn加载第三方模块；
    * 多线程打包happypack；
    * splitChunks抽离公共文件；
    * sourceMap优化；
---

# 在vue中使用this应该注意哪些问题？
    vue中使用匿名函数，会出现this指针改变。

    解决方法:
    1).使用箭头函数
    2).定义变量绑定this至vue对象
---

# 组件中写name选项有什么作用？ 
  1. 项目使用keep-alive时，可搭配组件name进行缓存过滤
  2. DOM做递归组件时需要调用自身name
  3. vue-devtools调试工具里显示的组见名称是由vue中组件name决定的
  4. 路由跳转用params传参时，用到的是name
---

# 说说你对slot的理解有多少？slot使用场景有哪些？有什么作用？原理是什么？
  * slot又名插槽，是Vue的内容分发机制，组件内部的模板引擎使用slot元素作为承载分发内容的出口。插槽slot是子组件的一个模板标签元素，而这一个标签元素是否显示，以及怎么显示是由父组件决定的。
  * 场景：组件封装的时候最常使用到
  * slot又分三类，默认插槽，具名插槽和作用域插槽。
    - 默认插槽：又名匿名查抄，当slot没有指定name属性值的时候一个默认显示插槽，一个组件内只有有一个匿名插槽。
    - 具名插槽：带有具体名字的插槽，也就是带有name属性的slot，一个组件可以出现多个具名插槽。
    - 作用域插槽：默认插槽、具名插槽的一个变体，可以是匿名插槽，也可以是具名插槽，该插槽的不同点是在子组件渲染作用域插槽时，可以将子组件内部的数据传递给父组件，让父组件根据子组件的传递过来的数据决定如何渲染该插槽。
  * 实现原理：当子组件vm实例化时，获取到父组件传入的slot标签的内容，存放在vm.$slot中，默认插槽为vm.$slot.default，具名插槽为vm.$slot.xxx，xxx 为插槽名，当组件执行渲染函数时候，遇到slot标签，使用$slot中的内容进行替换，此时可以为插槽传递数据，若存在数据，则可称该插槽为作用域插槽。
---

# prop验证的type类型有哪几种？
    Number, String, Boolean, Array, Function, Object
---

# 你了解vue的diff算法吗？(Vue2.x和Vue3.x渲染器的diff算法分别说一下)
  * 简单来说，diff算法有以下过程
    1. 同级比较，再比较子节点
    2. 先判断一方有子节点一方没有子节点的情况(如果新的children没有子节点，将旧的子节点移除)
    3. 比较都有子节点的情况(核心diff)
    4. 递归比较子节点
  
  * 正常Diff两个树的时间复杂度是O(n^3)，但实际情况下我们很少会进行跨层级的移动DOM，所以Vue将Diff进行了优化，从O(n^3) -> O(n)，只有当新旧children都为多个子节点时才需要用核心的Diff算法进行同层级比较。

  * Vue2的核心Diff算法采用了双端比较的算法，同时从新旧children的两端开始进行比较，借助key值找到可复用的节点，再进行相关操作。相比React的Diff算法，同样情况下可以减少移动节点次数，减少不必要的性能损耗，更加的优雅。

  * Vue3.x借鉴了 ivi算法和 inferno算法

  * 在创建VNode时就确定其类型，以及在mount/patch的过程中采用位运算来判断一个VNode的类型，在这个基础之上再配合核心的Diff算法，使得性能上较Vue2.x有了提升。(实际的实现可以结合Vue3.x源码看。)

  * 该算法中还运用了动态规划的思想求解最长递归子序列。
---

# diff一定可以节约性能吗？
由于Vue的diff算法是同层级比较的，所以如果是不同层级出现可复用的组件就不能被复用，所以此时性能会低
---

# 你知道nextTick的原理吗？如何知道DOM渲染结束？
  * 提到DOM的更新是异步执行的，只要数据发生变化，将会开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。如果同一个 watcher 被多次触发，只会被推入到队列中一次。简单来说，就是当数据发生变化时，视图不会立即更新，而是等到同一事件循环中所有数据变化完成之后，再统一更新视图。

  * 如何知道：通过异步队列的方式来控制dom更新和nextTick的回调先后执行，保证了dom更新后再执行回调
---

# 在Vue生命周期的created()钩子函数进行的DOM操作一定要放在Vue.nextTick() 回调函数中。原因是什么呢
  * 在 created() 钩子函数执行的时候DOM其实并未进行任何渲染，而此时进行 DOM 操作无异于徒劳，
  * 所以在数据变化后要执行的某个操作，而这个操作需要使用随数据改变而改变的DOM结构的时候，这个操作都应该放进Vue.nextTick() 的回调函数中
---

# vue页面跳转的时候会触发哪些生命周期，destroy一定会被触发吗？为什么？
---

# 怎么缓存当前的组件？缓存后怎么更新？
---

# vue生命周期
  * 4个阶段：创建前/后， 挂载前/后， 更新前/后， 销毁前/后

  * **beforeCreate**：是new Vue()之后触发的第一个钩子，在当前阶段data、methods、computed以及watch上的数据和方法都不能被访问。可以根据路由信息进行重定向等操作

  * **created**： 在实例创建完成后发生，当前阶段已经完成了数据观测，也就是可以使用数据，更改数据，在这里更改数据不会触发updated函数。可以做一些初始数据的获取，在当前阶段无法与Dom进行交互，如果非要想，可以通过vm.$nextTick来访问Dom。可以进行数据、资源请求；

  * **beforeMount**：发生在挂载之前，在这之前template模板已导入渲染函数编译。而当前阶段虚拟Dom已经创建完成，即将开始渲染。在此时也可以对数据进行更改，不会触发updated。

  * **mounted**： 在挂载完成后发生，在当前阶段，真实的Dom挂载完毕，数据完成双向绑定，可以访问到Dom节点，使用$refs属性对Dom进行操作。可以进行一些dom操作；

  * **beforeUpdate**： 发生在更新之前，也就是响应式数据发生更新，虚拟dom重新渲染之前被触发，你可以在当前阶段进行更改数据，不会造成重渲染。

  * **updated**： 发生在更新完成之后，当前阶段组件Dom已完成更新。要注意的是避免在此期间更改数据，因为这可能会导致无限循环的更新。

  * **beforeDestroy**： 发生在实例销毁之前，在当前阶段实例完全可以被使用，我们可以在这时进行善后收尾工作，比如清除计时器。

  * **destroyed**： 发生在实例销毁之后，这个时候只剩下了dom空壳。组件已被拆解，数据绑定被卸除，监听被移出，子实例也统统被销毁。
---

# 第一次页面加载会触发哪几个钩子
---

# vue获取数据在哪个周期函数
---

# Vue中组件生命周期调用顺序说一下
  * 组件的调用顺序都是先父后子,渲染完成的顺序是先子后父。
  * 组件的销毁操作是先父后子，销毁完成的顺序是先子后父。

  * 加载渲染过程
    - 父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount- >子mounted->父mounted
  * 子组件更新过程
    - 父beforeUpdate->子beforeUpdate->子updated->父updated
---

# 生命周期钩子是如何实现的?
  * 简单说就是vue的生命周期钩子就是回调函数而已，当创建组件实例的过程中会调用对应的钩子方法。内部主要是使用callHook方法来调用对应的方法。核心是一个发布订阅模式，将钩子订阅好(内部采用数组的方式存储)，在对应的阶段进行发布。
---

# 如何中断axios的请求？
    使用cancelToken
---

# axios的特点有哪些？
  1. Axios 是一个基于 promise 的 HTTP 库，支持promise所有的API
  2. 它可以拦截请求和响应
  3. 它可以转换请求数据和响应数据，并对响应回来的内容自动转换成 JSON类型的数据
  4. 安全性更高，客户端支持防御 XSRF
---

# axios原理(axios和express的http是如何实现请求拦截器的，说下原理)
  1. axios 原理还是属于 XMLHttpRequest， 因此需要实现一个ajax。 
  2. 还需要但会一个promise对象来对结果进行处理。
---

# vue常用的修饰符有哪些？列举并说明
    v-model: .trim .number
    v-on: .stop .prevent
---

# 你认为vue的核心是什么？
    数据改变驱动视图层的改变
---

# 为什么data属性必须声明为返回一个初始数据对应的函数呢？
  * 一个组件被复用多次的话，也就会创建多个实例。本质上，这些实例用的都是同一个构造函数。如果data是对象的话，对象属于引用类型，会影响到所有的实例。所以为了保证组件不同的实例之间data不冲突，data必须是一个函数。
---

# v-for循环中key有什么作用？
    性能优化，让vue在更新数据的时候可以更有针对性
---

# v-if 与 v-for的优先级?
  1. v-for优先于v-if被解析 
  2. 如果同时出现，每次渲染都会先执行循环再判断条件，无论如何循环都不可避免，浪费了性能 
  3. 要避免出现这种情况，则在外层嵌套template，在这一层进行v-if判断，然后在内部进行v-for循环 
  4. 如果条件出现在循环内部，可通过计算属性提前过滤掉那些不需要显示的项
---

# 说说你对keep-alive的理解是什么？
  * 简单说：保留组件状态 避免重新渲染

  * keep-alive可以实现组件缓存，当组件切换时不会对当前组件进行卸载。
  * 常用的两个属性include/exclude，允许组件有条件的进行缓存。
  * 两个生命周期activated/deactivated，用来得知当前组件是否处于活跃状态。
---

# 如果两个组件被keep-alive 缓存，切换时候还会执行 create 吗
  > 不会
---

# keep-alive缓存的是什么？
* 缓存的是被包含在 keep-alive 中创建的组件的状态避免组件重新渲染，
---

# keep-alive的原理是什么？
* 它是一个动态组件，同时也是vue的内置组件，当组件在切换的过程中，keep-alive可以将状态保存在内存中，防止重复渲染dom。会增加两个生命周期：组件激活时：actived() 该钩子在服务器渲染期间不能被调用
  组件停用时：deactivated() 该钩子在服务器渲染期间不能被调用。
---

# 怎么监听组件是否处于激活或失活
---

# 什么是虚拟dom?
  * Virtual DOM本质就是用一个原生的JS对象去描述一个DOM节点。是对真实DOM的一层抽象。(也就是源码中的VNode类，它定义在src/core/vdom/vnode.js中。)

  * VirtualDOM映射到真实DOM要经历VNode的create、diff、patch等阶段。
---

# 为什么使用虚拟DOM
  1. 手动操作 DOM 比较麻烦，还需要考虑浏览器兼容性问题，虽然有 jQuery等简化 DOM 操作，但是随着项目的复杂 DOM操作复杂提升
  2. 为了简化 DOM 的复杂操作于是出现来各种 MVVM框架，MVVM框架解决来视图和状态的同步问题
  3. 为了简化视图的操作我们可以使用模版引擎，但是模版引擎没有解决跟踪状态变化的问题，于是 Virtual DOM 出现了；Virtual DOM的好处是当状态改变时不需要立即更新DOM，只需要创建一个虚拟树来描述 DOM，Virtual DOM 内部将弄清楚如果有效(diff) 的更新DOM
  4. 虚拟DOM可以维护程序的状态，跟踪上一次的状态,通过比较前后两次状态的差异更新真实DOM
---

# vue和react有什么不同？使用场景是什么？ 
  1. 监听数据变化的实现原理不同：Vue通过 getter/setter以及一些函数的劫持，能精确知道数据变化；React默认是通过比较引用的方式（diff）进行的，如果不优化可能导致大量不必要的VDOM的重新渲染。React不精确监听数据变化。

  2. 数据流的不同：Vue1.0中可以实现两种双向绑定；React一直不支持双向绑定，提倡的是单向数据流。

  3. 组件通信的区别：React 本身并不支持自定义事件，而Vue中子组件向父组件传递消息有两种方式：事件和回调函数，但Vue更倾向于使用事件。在React中我们都是使用回调函数的。

  4. 模板渲染方式的不同：在表层上，模板的语法不同，React是通过JSX渲染模板。而Vue是通过一种拓展的HTML语法进行渲染，但其实这只是表面现象，毕竟React并不必须依赖JSX；在深层上，模板的原理不同。React是在组件JS代码中，通过原生JS实现模板中的常见语法，比如插值，条件，循环等，都是通过JS语法实现的，更加纯粹更加原生。而Vue是在和组件JS代码分离的单独的模板中，通过指令来实现的，比如条件语句就需要 v-if 来实现对这一点，这样的做法显得有些独特，会把HTML弄得很乱。
---

# 说说你对vue的理解。
  1. 数据驱动
	2. 模块化
	3. 轻量级
	4. 单页应用
---

# vue-router有哪几种导航钩子（ 导航守卫 ）？
  1. 全局的：beforeEach, afterEach
    ```
      const router = new VueRouter({ ... });
      router.beforeEach((to, from, next) => {
          // do someting
      });
    ```

  2. 单个路由独享的: beforeEnter
    ```
      cont router = new VueRouter({
          routes: [
              {
                  path: '/file',
                  component: File,
                  beforeEnter: (to, from ,next) => {
                      // do someting
                  }
              }
          ]
      });
    ```

  3. 组件内的: beforeRouteEnter, beforeRouteUpdate, beforeRouteLeave

# 切换到新路由时，页面要滚动到顶部或保持原先的滚动位置怎么做呢？
    在那个页面的路由里配置:
    scrollBehavior(to, from, savedPosition) {
        return { x: 0, y: 0 }
    }
---

# 如何获取路由传过来的参数？
  * 如果使用query方式传入的参数使用this.$route.query 接收
  * 如果使用params方式传入的参数使用this.$router.params接收
---

# 说说active-class是哪个组件的属性？
    router-link
---

# vue-router内部做了什么？
  * 实现并声明了两个组件router-view、router-link
  * 实现install方法，及this.$router.push()
---

# vue定义响应式数据的几种方式
  1. defineProperty
  2. 利用vue2的Vue.util.defineReactive
  3. proxy
---

# 为什么要将router添加到main.js的配置项中
  > 因为要在vue插件vue-router的install方法中去使用，而VueRouter.install方法是谁在调用呢？是Vue.use在调用，也就是说这个router插件执行的时刻是非常早的，在执行Vue.use(VueRouter)的时候，会调用install,所以他执行的时刻会比创建实例newVueRouter要早，所以在Vue.install方法里面，没有办法拿到vue实例this,所以我们才要把这个router写到main.js的new Vue里面
---

# router-view是如何实现的
  * dom实现：因为在路由文件里定义了path和component，可以通过js的onHashChange事件，监听当前页的hash地址值，或者history的变化popstate事件，当事件发生变化的时候，我就可以拿到当前的地址，当我拿到这个地址之后，就可以根据路由文件path与这个地址匹配，拿到对应的component,然后就可以把这个component变成真实的dom,再追加到router-view里面去，可以把之前的router-view清空掉，然后把这些内容创建完之后再追加进去
  * 利用vue响应式：及创建一个响应式的数据，这个值可能是当前的地址，可能是current，如果将来这个地址发生变化，router-view里面的相关组件可以重新渲染（核心思路）
---

# router-link原理
  > 通过定义router-link自定义标签映射到a标签上，绑定click事件，然后执行对应的VueRouter实例的push方法实现的，会执行preventDefault()函数通知浏览器不要执行与事件相关的动作，所有的动作比如设置hash都是通过VueRouter的内置代码来实现。
  > 详细代码请看：`/images/vueRource/vue-router简易版源码.zip`
---

# router-view原理
  > 创建一个响应式的数据，这个值可能是当前的地址，可能是current，监听这个地址的变化，如果有变化，赋值给这个current，然后通过router文件的path与这个current进行比较，拿到对应的component,在进行重新渲染
  > 详细代码请看：`/images/vueRource/vue-router简易版源码.zip`
---

# 说说vue-router完整的导航解析流程是什么？
  1. 导航被触发；
  2. 在失活的组件里调用beforeRouteLeave守卫；
  3. 调用全局beforeEach守卫；
  4. 在复用组件里调用beforeRouteUpdate守卫；
  5. 调用路由配置里的beforeEnter守卫；
  6. 解析异步路由组件；
  7. 在被激活的组件里调用beforeRouteEnter守卫；
  8. 调用全局beforeResolve守卫；
  9. 导航被确认；
  10. 调用全局的afterEach钩子；
  11. DOM更新；
  12. 用创建好的实例调用beforeRouteEnter守卫中传给next的回调函数。
---

# vue-router 的原理（核心：更新视图而不重新请求页面）
  ![原理](/images/vue/vue面试题3.jpg)
  > 当url发生改变的时候，首先判断是history路由还是hash路由，触发监听事件，如果是hash路由，使用onhashchange方法监听到路由地址的改变，如果是history路由，通过popstate监听，然后去改变vue-router里面的当前地址current，vue组件会监听current的变化，根据变化获取新的组件，再用render去渲染
---

# vue-router有几种路由模式，说说你对他们的理解
  * hash
  * history
  * abstract: 适用于所有JavaScript环境，例如服务器端和Node.js. 如果没有浏览器API，路由器将自动强制进入此模式。
---

# 路由守卫哪些参数，怎么实现？
  有to（去的那个路由）、from（离开的路由）、next（一定要用这个函数才能去到下一个路由，如果不用就拦截）
---

# vuex直接修改state 与 用commit提交mutation来修改state有什么区别?(mutation 和 action 有什么区别，怎么用？)
  * 相同点: 都可以修改state的值，并且是响应式的
  * 区别: 
    1. action 提交的是 mutation，而不是直接变更状态。mutation可以直接变更状态
    2. action 可以包含任意异步操作。mutation只能是同步操作
    3. 提交方式不同
      * action 是用this.store.dispatch('ACTION_NAME',data)来提交。
      * mutation是用this.$store.commit('SET_NUMBER',10)来提交

4.接收参数不同，mutation第一个参数是state，而action第一个参数是context.
---

# vue3中vue-router的变化
  1. history选项代替了mode选项
    * history：createWebHistory（）
    * hash：createWebHashHistory（）
    * abstract：createMemoryHistory（）
  2. base选项移至createWebHistory等方法中，作为参数传入
  3. 现在keep-alive和transition 必须用在router-view内部
---

# Vue 的 $store 是怎么绑定到 Vue 实例上的?
  > $store是vuex实现的一个属性，由于在main.js里我们导入了store之后，挂载到了vue实例上，通过翻看vuex源码，在vuex中实现的install方法里，通过定义$store全局属性，赋值给this.$options.store这样就跟绑定上去了
---

# 给 Vue 全局绑定一个变量有什么方式? 为什么 $store 不直接绑定到原型
* 方式：
  - 专门定义一个全局变量的文件，暴露出去给需要用的地方引入
  - 全局变量模块挂载到Vue.prototype里，通过vue.prototype.xxx定义
* 原因：不想改变vue的原型，通过vue.mixin加入
---

# 为什么 vue 不建议在 action 中修改 state 而是在 mutation 中修改？有什么好处？
  * 当某种类型的action只有一个声明时，action的回调会被当作普通函数执行，而当如果有多个声明时，它们是被视为Promise实例，并且用Promise.all执行，总所周知，Promise.all在执行Promise时是不保证顺序的，也就是说，假如有3个Promise实例：P1、P2、P3，它们3个之中不一定哪个先有返回结果,那么我们仔细思考一下：如果同时在多个action中修改了同一个state，那会有什么样的结果？
  * 其实很简单，我们在多个action中修改同一个state，因为很有可能每个action赋给state的新值都有所不同，并且不能保证最后一个有返回结果action是哪一个action，所以最后赋予state的值可能是错误的
  * 那么Vuex为什么要使用Promise.all执行action呢？其实也是出于性能考虑，这样我们就可以最大限度进行异步操作并发
---

# vuex的store有几个属性值？分别讲讲它们的作用是什么？
  * state, 状态初始化, 并实施观察
  * getter, 获取数据用于view或data中使用
  * mutation: 内部处理state变化
  * action: 处理外部交互
  * module: 模块化以上四个
---

# 如何在vuex中使用异步修改?
  > 在调用vuex中的方法action的时候，用promise实现异步修改
---

# vuex 的状态规则 modules 作用
  > 那么Vuex为什么要使用Promise.all执行action呢？其实也是出于性能考虑，这样我们就可以最大限度进行异步操作并发
---

# vuex 和 eventbus 优缺点有什么不一样
  > vuex 和传统的 event-bus 的不同点除了 vuex 实现了更加友好的响应式状态之外，还禁止了 vuex 里面数据的直接修改，大大增强了信任度（有点像promise的status），通过增加 mutations 和 actions 这种“中间层”，它能更好的控制中间的变化，比如实现时间旅行、状态回退和状态保存的功能。
---

# eventbus的emit，on和子到父传值的emit，on有啥区别？
---

# this.$emit原理
  > 采用发布订阅模式，根据传入的事件名从当前实例的_events属性（即事件中心）中获取到该事件名所对应的回调函数cbs，然后再获取传入的附加参数args，由于cbs是一个数组，所以遍历该数组，拿到每一个回调函数，执行回调函数并将附加参数args传给该回调。
---

# $on怎么监听
  > $on采用发布订阅模式，首先定义一个事件中心，通过$on订阅事件，将事件存储在事件中心里面，然后通过$emit触发事件中心里面存储的订阅事件。
  > $on函数接收俩个参数，第一个是订阅的事件名，可以是多个，如果是多个就传入一个事件名数组。另一个是回调函数。 首先判断传入的事件是不是一个数组，如果是，那么遍历这个数组，将数组中的每一个事件都递归调用$on方法将其作为单个事件订阅。如过不是数组，那就当做单个事件名来处理，以该事件名作为key，先尝试在当前实例的_events属性中获取其对应的事件列表，如果获取不到就给其赋空数组为默认值，并将第二个参数回调函数添加进去。
---

# 对 vuex 的理解，单向数据流
  * Vuex 是适用于 Vue.js 应用的状态管理库，为应用中的所有组件提供集中式的状态存储与操作，保证了所有状态以可预测的方式进行修改。

  * 五大核心属性:
    1. state：存储数据，存储状态；在根实例中注册了store 后，用 this.$store.state 来访问；对应vue里面的data；存放数据方式为响应式，vue组件从store中读取数据，如数据发生变化，组件也会对应的更新。
    2. getters：可以认为是 store 的计算属性，相当于 vue中的 computed，依赖于 state里面的值。它的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。
    3. mutations：用于修改状态，store里面的数仅能通过mutations里面的方法改变,但是必须是同步的。更改 vuex 的 store 中的状态的唯一方法是提交 mutation，也就是$store.commit。
    4. actions：包含任意异步操作，用它处理完后再触发mutations来改变状态。
    5. module：将 store 分割成模块，每个模块都具有state、mutation、action、getter、甚至是嵌套子模块。
  
  * vuex的数据传递流程:
    - 当组件进行数据修改的时候我们需要调用Dispatch来触发Actions里面的方法。
    - actions里面的每个方法中都会有一个commit方法，当方法执行的时候会通过commit来触发mutations里面的方法进行数据的修改。
    - mutations里面的每个函数都会有一个state参数，这样就可以在mutations里面进行state的数据修改，当数据修改完毕后，会传导给页面。页面的数据也会发生改变。
---

# actions与mutations的区别?
  > commit是提交执行mutations中的方法，Mutations 是修改数据的，必须同步。
  > dispatch是提交执行actions中的方法，action 提交的是Mutations,可以是异步操作。action不可以修改store中的数据，需要commit mutation中的方法进行数据修改
---

# 在Vuex中使用mutation要注意什么?
  > mutation 必须是同步函数
---

# 说一下vuex的思想
  * Vuex 的思想是 当我们在页面上点击一个按钮，它会触发(dispatch)一个action, action 随后会执行(commit)一个mutation, mutation 立即会改变state,
  * state 改变以后,我们的页面会state 获取数据，页面发生了变化。 Store 对象，包含了我们谈到的所有内容，action, state, mutation
---

# vuex是怎么实现的（vuex的原理3.x）
  * vuex是利用vue的mixin混入机制，在beforeCreate钩子前混入vuexInit方法，vuexInit方法实现了store注入vue组件实例，并注册了vuex store的引用属性$store。
  * Vuex的state状态是响应式，是借助vue的data是响应式，将state存入vue实例组件的data中；Vuex的getters则是借助vue的计算属性computed实现数据实时监听。
  * 在4.x版本中，通过provide、inject方式，通过根组件的install方法，把store实例用provide注册到根实例中，在子组件通过定义useStore函数中用inject接收这个store实例，同时也通过调用vue3.x版本提供的app.config.globalProperties.$store赋值给store实例this, state状态的响应式通过reactive的方式
---

# createStore是怎么实现的，做了哪些事情
  > new一个store实例，实现传入的state,getters,mutations,actions等方法
---

# useStore是怎么实现的，做了什么事情
  > provide加inject实现的，通过根组件的install方法，把store实例用provide注册到根实例中，在子组件通过定义useStore函数中用inject接收这个store实例。
  > 作用就是在每个子组件中拿到store实例
---

# 如何在composition api中使用vuex
  > 主要是获取Store实例，通过vuex提供的useStore方法
---

# vue的插件机制
  * 通过在插件中定义install方法，在vue2.x版本中，通过vue.mixin，在每个组件的beforeCreate中设置$store
  * 3.x版本中通过provide + inject方式，在install方法中通过provide把实例挂载到根组件中，通过inject在useStore函数中接收这个store实例
  * nstall方法的参数有2个，第一个是app实例，也就是vue实例，第二个是这个插件的名称
---

# 页面刷新后vuex的state数据丢失怎么解决
  1. 使用Localstorage、sessionStorage
  2. 使用一些插件：vuex-persistedstate
---

# vue能监听到数组变化的方法有哪些？为什么？(Vue里面数组为什么直接修改不能触发数据更新，vue重写了哪些数组方法)
    push(), pop(), shift(), unshift(), splice(), sort(), reverse()
---

# vue内部是怎么实现数组的响应式的。为什么不直接去更改数组原型上的方法？改了会有什么问题
  * 数组则通过重写数组方法来实现的。扩展它的 7 个变更⽅法，通过监听这些方法可以做到依赖收集和派发更新；
---

# 请说一下响应式数据的理解？
  1. 对象内部通过defineReactive方法，使用 Object.defineProperty() 监听数据属性的 get 来进行数据依赖收集，再通过 set 来完成数据更新的派发；
  2. 数组则通过重写数组方法来实现的。扩展它的 7 个变更⽅法，通过监听这些方法可以做到依赖收集和派发更新；
---

# Vue.set 方法是如何实现的？（$set的原理）
  * 为什么$set可以触发更新，我们给对象和数组本身都增加了dep属性，当给对象新增不存在的属性则触发对象依赖的watcher去更新，当修改数组索引时我们调用数组本身的splice方法去更新数组。
  * 如果是数组，调用重写的splice方法。
  * 如果不是响应式的也不需要将其定义成响应式属性。
  * 如果是对象，将属性定义成响应式的
---

# vue-lazyloader 的原理
  1. vue-lazyload是通过指令的方式实现的，定义的指令是v-lazy指令；

  2. 指令被bind时会创建一个listener，并将其添加到listener queue里面， 并且搜索target dom节点，为其注册dom事件(如scroll事件)

  3. 上面的dom事件回调中，会遍历 listener queue里的listener，判断此listener绑定的dom是否处于页面中perload的位置，如果处于则加载异步加载当前图片的资源；

  4. 同时listener会在当前图片加载的过程的loading，loaded，error三种状态触发当前dom渲染的函数，分别渲染三种状态下dom的内容；
---

# vue是怎么监听数组改变更新视图的
  * 使用了函数劫持的方式，重写了数组的方法，Vue将data中的数组进行了原型链重写，指向了自己定义的数组原型方法。这样当调用数组api时，可以通知依赖更新。如果数组中包含着引用类型，会对数组中的引用类型再次递归遍历进行监控。这样就实现了监测数组变化。
---

# 问了数据劫持，收集依赖的过程 ，实现的方式
---

# vue从data改变到页面渲染的过程。
  1. new Vue，执行初始化。
  2. 挂载$mount方法，通过自定义Render方法、template、el等生成Render函数。
  3. 通过Watcher监听数据的变化。
  4. 当数据发生变化时，Render函数执行生成VNode对象。
  5. 通过patch方法，对比新旧VNode对象，通过DOM Diff算法，添加、修改、删除真正的DOM元素；
---

# 说一下v-model的原理
  * v-model本质就是一个语法糖，可以看成是value + input方法的语法糖。 可以通过model属性的prop和event属性来进行自定义。原生的v-model，会根据标签的不同生成不同的事件和属性。
---

# 在v-model上怎么用Vuex中state的值？
  > 需要通过computed计算属性来转换。
---

# Vue事件绑定原理说一下
    原生事件绑定是通过addEventListener绑定给真实元素的，组件事件绑定是通过Vue自定义的$on实现的。
---

# Vue模版编译原理知道吗，能简单说一下吗？(介绍vue template到render的过程)
  * 简单说，Vue的编译过程就是将template转化为render函数的过程。模板编译又分三个阶段，解析parse，优化optimize，生成generate，最终生成可执行函数render。
    1. parse阶段：使用大量的正则表达式对template字符串进行解析，将标签、指令、属性等转化为抽象语法树AST。
    2. optimize阶段：遍历AST，找到其中的一些静态节点并进行标记，方便在页面重渲染的时候进行diff比较时，直接跳过这一些静态节点，优化runtime的性能。
    3. generate阶段：将最终的AST转化为render函数字符串。

  * 首先解析模版，生成AST语法树(一种用JavaScript对象的形式来描述整个模板)。 使用大量的正则表达式对模板进行解析，遇到标签、文本的时候都会执行对应的钩子进行相关处理。
 
  * Vue的数据是响应式的，但其实模板中并不是所有的数据都是响应式的。有一些数据首次渲染后就不会再变化，对应的DOM也不会变化。那么优化过程就是深度遍历AST树，按照相关条件对树节点进行标记。这些被标记的节点(静态节点)我们就可以跳过对它们的比对，对运行时的模板起到很大的优化作用。

  * 编译的最后一步是将优化后的AST树转换为可执行的代码。 
---

# vue3.0在编译方面有哪些优化？
  * vue.js 3.x中标记和提升所有的静态节点，diff的时候只需要对比动态节点内容；
  * Fragments（升级vetur插件): template中不需要唯一根节点，可以直接放文本或者同级标签
  * 静态提升(hoistStatic),当使用 hoistStatic 时,所有静态的节点都被提升到 render 方法之外.只会在应用启动的时候被创建一次,之后使用只需要应用提取的静态节点，随着每次的渲染被不停的复用。
  * patch flag, 在动态标签末尾加上相应的标记,只能带 patchFlag 的节点才被认为是动态的元素,会被追踪属性的修改,能快速的找到动态节点,而不用逐个逐层遍历，提高了虚拟dom diff的性能。
  * 缓存事件处理函数cacheHandler,避免每次触发都要重新生成全新的function去更新之前的函数 tree shaking 通过摇树优化核心库体积,减少不必要的代码量
---

# assets和static的区别
  * assets中的文件在运行npm run build的时候会打包，简单来说就是会被压缩体积，代码格式化之类的。打包之后也会放到static中。
  * static中的文件则不会被打包。
  * 建议：将图片等未处理的文件放在assets中，打包减少体积。而对于第三方引入的一些资源文件如iconfont.css等可以放在static中，因为这些文件已经经过处理了。
---

# $route和$router的区别
  * $router 为 VueRouter 实例，想要导航到不同 URL，则使用 $router.push 方法
  * $route 为当前 router 跳转对象里面可以获取 name 、 path 、 query 、 params 等
---

# 父组件和子组件生命周期执行的顺序？
  1. 加载渲染过程：
  父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted

  2. 子组件更新过程：
  父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父 updated

  3. 父组件更新过程：
  父 beforeUpdate -> 父 updated

  4. 销毁过程：
  父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed
---

# vue-loader是什么？使用它的用途有哪些？
  * 解析.vue文件的一个加载器，跟template/js/style转换成js模块。
  * 用途：js可以写es6、style样式可以scss或less、template可以加jade等
---

# template预编译是什么？
  * 对于 Vue 组件来说，模板编译只会在组件实例化的时候编译一次，生成渲染函数之后在也不会进行编译。因此，编译对组件的 runtime 是一种性能损耗。

  * 而模板编译的目的仅仅是将template转化为render function，这个过程，正好可以在项目构建的过程中完成，这样可以让实际组件在 runtime 时直接跳过模板渲染，进而提升性能，这个在项目构建的编译template的过程，就是预编译。
---

# template和jsx的有什么分别？
  * 对于 runtime 来说，只需要保证组件存在 render 函数即可，而我们有了预编译之后，我们只需要保证构建过程中生成 render 函数就可以。
  * 在 webpack 中，我们使用vue-loader编译.vue文件，内部依赖的vue-template-compiler模块，在 webpack 构建过程中，将template预编译成 render 函数。
  * 与 react 类似，在添加了jsx的语法糖解析器babel-plugin-transform-vue-jsx之后，就可以直接手写render函数。
  * 所以，template和jsx的都是render的一种表现形式，不同的是：JSX相对于template而言，具有更高的灵活性，在复杂的组件中，更具有优势，而 template 虽然显得有些呆滞。但是 template 在代码结构上更符合视图与逻辑分离的习惯，更简单、更直观、更好维护。
---

# Proxy 相对于 Object.defineProperty有哪些优点？
  * proxy的性能本来比defineproperty好，proxy可以拦截属性的访问、赋值、删除等操作，不需要初始化的时候遍历所有属性，另外有多层属性嵌套的话，只有访问某个属性的时候，才会递归处理下一级的属性。

  * 可以* 监听数组变化
  * 可以劫持整个对象
  * 操作时不是对原对象操作,是 new Proxy 返回的一个新对象
  * 可以劫持的操作有 13 种
  
# vue3.0 有哪些改动和新特性？
## 改动
  * 速度更快
    - 重写了虚拟Dom实现
    - 编译模板的优化
    - 更高效的组件初始化
    - undate性能提高1.3~2倍
    - SSR速度提高了2~3倍
  
  * 体积减少
    - 通过webpack的tree-shaking功能，可以将无用模块“剪辑”，仅打包需要的
    - 能够tree-shaking，有两大好处：
      1. 对开发人员，能够对vue实现更多其他的功能，而不必担忧整体体积过大
      2. 对使用者，打包出来的包体积变小了
    - vue可以开发出更多其他的功能，而不必担忧vue打包出来的整体体积过多

  * 更易维护
    * compositon Api
      - 可与现有的Options API一起使用
      - 灵活的逻辑组合与复用
      - Vue3模块可以和其他框架搭配使用
    * 更好的Typescript支持
      - VUE3是基于typescipt编写的，可以享受到自动的类型定义提示 
    * 编译器重写
  
  * 更接近原生
    - 可以自定义渲染 API

  * 更易使用
    - 响应式 Api 暴露出来  

## 新特性
  * ![vue面试](/images/vue/vue面试2.png)
---

# vue3生命周期新特性
  * ![vue面试](/images/vue/vue面试.png)
---

# vue3 的 类似 hooks 的原理是怎么样的
  * 数据绑定建立响应式大致分为三个阶段: 
    1. 初始化阶段： 初始化阶段通过组件初始化方法形成对应的proxy对象，然后形成一个负责渲染的effect。

    2. get依赖收集阶段：通过解析template，替换真实data属性，来触发get,然后通过stack方法，通过proxy对象和key形成对应的deps，将负责渲染的effect存入deps。（这个过程还有其他的effect，比如watchEffect存入deps中 ）。

    3. set派发更新阶段：当我们 this[key] = value 改变属性的时候，首先通过trigger方法，通过proxy对象和key找到对应的deps，然后给deps分类分成computedRunners和effect,然后依次执行，如果需要调度的，直接放入调度。

  * 一句话描述：其实就是在 Proxy 第二个参数 handler，拦截各种取值、赋值操作，依托 track 和 trigger 两个函数进行依赖收集和派发更新。

  * 优点：
    1. 可以监测到代理对象属性的动态添加和删除
    2. 可以监测到数组的下标和length属性的变更
  * 缺点：
    1. ES6的proxy语法对于低版本浏览器不支持，ie11
    2. vue3针对ie11出了一个特殊的版本做兼容
---

# Vue3.x响应式数据原理?
  > Vue3.x改用Proxy替代Object.defineProperty。
  > 因为Proxy可以直接监听对象和数组的变化，并且有多达13种拦截方法。并且作为新标准将受到浏览器厂商重点持续的性能优化。
  > Proxy只会代理对象的第一层，那么Vue3又是怎样处理这个问题的呢？ 判断当前Reflect.get的返回值是否为Object，如果是则再通过reactive方法做代理， 这样就实现了深度观测。
  > 监测数组的时候可能触发多次get/set，那么如何防止触发多次呢？ 我们可以判断key是否为当前被代理对象target自身属性，也可以判断旧值与新值是否相等，只有满足以上两个条件之一时，才有可能执行trigger。
---

# 都说Composition API与React Hook很像，说说区别
  1. 从React Hook的实现角度看，React Hook是根据useState调用的顺序来确定下一次重渲染时的state是来源于哪个useState，所以出现了以下限制
    * 不能在循环、条件、嵌套函数中调用Hook
    * 必须确保总是在你的React函数的顶层调用Hook
    * useEffect、useMemo等函数必须手动确定依赖关系
  
  2. 而Composition API是基于Vue的响应式系统实现的，与React Hook的相比
    * 声明在setup函数内，一次组件实例化只调用一次setup，而React Hook每次重渲染都需要调用Hook，使得React的GC比Vue更有压力，性能也相对于Vue来说也较慢
    * Compositon API的调用不需要顾虑调用顺序，也可以在循环、条件、嵌套函数中使用
    * 响应式系统自动实现了依赖收集，进而组件的部分的性能优化由Vue内部自己完成，而React Hook需要手动传入依赖，而且必须必须保证依赖的顺序，让useEffect、useMemo等函数正确的捕获依赖变量，否则会由于依赖不正确使得组件性能下降。

  3. 原理
    * React hook 底层是基于链表实现，调用的条件是每次组件被render的时候都会顺序执行所有的hooks。
    * vue hook 只会被注册调用一次，vue 能避开这些麻烦的问题，原因在于它对数据的响应是基于proxy的，对数据直接代理观察。（这种场景下，只要任何一个更改data的地方，相关的function或者template都会被重新计算，因此避开了react可能遇到的性能上的问题）。
    * react 中，数据更改的时候，会导致重新render，重新render又会重新把hooks重新注册一次，所以react复杂程度会高一些。
---

# Vue 3.0 性能提升主要是通过哪几方面体现的？(vue2.0与vue3.0的区别)
  1. 重构响应式系统，使用Proxy替换Object.defineProperty，使用Proxy优势：
    * 可直接监听数组类型的数据变化
    * 监听的目标为对象本身，不需要像Object.defineProperty一样遍历每个属性，有一定的性能提升
    * 可拦截apply、ownKeys、has等13种方法，而Object.defineProperty不行
    * 直接实现对象属性的新增/删除
  2. 新增Composition API，更好的逻辑复用和代码组织
  3. 重构 Virtual DOM
    * 模板编译时的优化，将一些静态节点编译成常量
    * slot优化，将slot编译为lazy函数，将slot的渲染的决定权交给子组件
    * 模板中内联事件的提取并重用（原本每次渲染都重新生成内联函数）
  4. 代码结构调整，更便于Tree shaking，使得体积更小
  5. 使用Typescript替换Flow
---

# Vue 3为什么要用 Proxy API 替代 DefineProperty API？
  1. defineProperty API 的局限性最大原因是它只能针对单例属性做监听。
    - Vue2.x中的响应式实现正是基于defineProperty中的descriptor，对 data 中的属性做了遍历 + 递归，为每个属性设置了 getter、setter。       
    - 这也就是为什么 Vue 只能对 data 中预定义过的属性做出响应的原因，在Vue中使用下标的方式直接修改属性的值或者添加一个预先不存在的对象属性是无法做到setter监听的，这是defineProperty的局限性。

  2. Proxy API的监听是针对一个对象的，那么对这个对象的所有操作会进入监听操作， 这就完全可以代理所有属性，将会带来很大的性能提升和更优的代码。
    - Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。

  3. 响应式是惰性的
    - 在 Vue.js 2.x 中，对于一个深层属性嵌套的对象，要劫持它内部深层次的变化，就需要递归遍历这个对象，执行 Object.defineProperty 把每一层对象数据都变成响应式的，这无疑会有很大的性能消耗。
    - 在 Vue.js 3.0 中，使用 Proxy API 并不能监听到对象内部深层次的属性变化，因此它的处理方式是在 getter 中去递归响应式，这样的好处是真正访问到的内部属性才会变成响应式，简单的可以说是按需实现响应式，减少性能消耗。
---

# Composition Api 与 Vue 2.x使用的Options Api 有什么区别？
  * Options Api
    - 包含一个描述组件选项（data、methods、props等）的对象 options；
    - API开发复杂组件，同一个功能逻辑的代码被拆分到不同选项 ；
    - 使用mixin重用公用代码，也有问题：命名冲突，数据来源不清晰；

  * composition Api
    - vue3 新增的一组 api，它是基于函数的 api，可以更灵活的组织组件的逻辑。
    - 解决options api在大型项目中，options api不好拆分和重用的问题。
---

# Proxy只会代理对象的第一层，Vue3是怎样处理这个问题的呢？
  判断当前Reflect.get的返回值是否为Object，如果是则再通过reactive方法做代理， 这样就实现了深度观测。
监测数组的时候可能触发多次get/set，那么如何防止触发多次呢？我们可以判断key是否为当前被代理对象target自身属性，也可以判断旧值与新值是否相等，只有满足以上两个条件之一时，才有可能执行trigger。
---

# vue3的toRefs作用
  > toRefs() 函数可以将 reactive() 创建出来的响应式对象, 转换为普通对象, 只不过这个普通对象上的而每一个属性都是响应式的,这样可以在不丢失响应式的情况下对返回的对象解构使用
---

# ref 和 reactive的区别？
  * 语法上：ref定义的响应式数据需要用`[data].value`的方式进行更改数据；reactive定义的数据需要`[data].[prpoerty]`的方式更改数据。
  * 应用上：ref定义基本类型，reactive什么类型都可以
---

# vue3中的teleport
  * teleport是一种能够将我们的模板移动到 DOM 中 Vue app 之外的其他位置的技术
  * 像 modals,toast 等这样的元素，很多情况下，我们将它完全的和我们的 Vue 应用的 DOM 完全剥离，管理起来反而会方便容易很多,原因在于如果我们嵌套在 Vue 的某个组件内部，那么处理嵌套组件的定位、z-index 和样式就会变得很困难
  * 使用方法就是一个teleport标签，有个to属性，绑定你要挂载的位置
---

# vue首次渲染的过程
  ![vue面试](/images/vue/vue面试题4.jpg)
  ![vue面试](/images/vue/vue面试5.png)
---

# 为什么vue2里不能监听数组
---

# 直接操作dom和虚拟dom的场景
---

# vue懒加载原理
---

# vue可以拿index当key吗，为什么
* 不建议，
---

# .vue文件怎么在浏览器上显示的，这个过程和原理
---

# vue模板解析是用正则匹配的吗？还是别的？
---

# vue在render的时候内部做什么处理
---

# 用了mixins,这是什么，优缺点？
* 混合 (mixins) 是一种分发 Vue 组件中可复用功能的非常灵活的方式。混合对象可以包含任意组件选项。当组件使用混合对象时，所有混合对象的选项将被混入该组件本身的选项。
* 优缺点：Mixin的优点是不需要传递状态，组件复用。但缺点不可知，不易维护。因为你可以在mixins里几乎可以加任何代码，props、data、methods、各种东西，就导致如果不了解mixins封装的代码的话，是很难维护的
---

# mixins和vuex的区别?
  * vuex公共状态管理，在一个组件被引入后，如果该组件改变了vuex里面的数据状态，其他引入vuex数据的组件也会对应修改，所有的vue组件应用的都是同一份vuex数据。（在js中，有点类似于浅拷贝）
  * vue引入mixins数据，mixins数据或方法，在每一个组件中都是独立的，互不干扰的，都属于vue组件自身。（在js中，有点类似于深度拷贝）
---

# 简述mixin、extends的覆盖逻辑
  * mixin和extends均是用于合并、拓展组件的，两者均通过mergeOptions方法进行合并
  * mixins接收一个混入对象的数组，其中混入对象可以像正常的实例对象一样包含实例选项，这些选项会被合并到最终的选项中，Mixin钩子按照传入顺序依次调用，并在调用组件自身的钩子之前被调用
  * extends主要是便于拓展单文件组件，接收一个对象或者构造函数
  * mergeOptions的执行过程
    1. 规范化选项
    2. 对未合并的选项，进行判断
    3. 合并处理。根据一个通用vue实例所包含的选项进行分类逐一判断合并，如：props、data、methods、watch、computed、生命周期等，将合并结果储存在新定义的options对象里。
    4. 返回合并结果options
---

# this.$ref原理
* 是一个对象，持有已注册过的所有子组件。ref为子组件指定一个名称，通过this.$refs.ref指定的子组件名称 即可获得对该子组件的操作（包括data中定义的数据和methods中定义的方法）
---

# mounted和watch里面监听props 哪个先执行
* watch先于mounted，props传过去的值，第一时间在mounted是获取不到的。因为是异步传值的。解决方法：结合watch和mounted同时渲染
   export default {
		name: 'zblist',
		props: {
			lists:Array
		},
		data() {
			return {
				show: true,
				Tlist: this.lists,
				list_today:[]
			}
		},
		watch:{
			lists:function(newVal,oldVal){
				this.Tlist = newVal
				this.getData()
			}
		},
		mounted() {
			this.getData()
		},
		methods: {
			getData() {
				console.log(this.Tlist)
				this.list_today = this.Tlist[0].today
			}
		},
	}
---

# vue中怎么重置data
  > 使用object.assign()，vm.$ data可以获取当前状态下的data，vm.$option.data(this)，可以获取到组件初始化状态下data
object.assgin(this. $data,this. $option.data(this))
---

# Vue SSR渲染原理？
  * 优点：
    - 更利于SEO
      - 不同爬虫工作原理类似，只会爬取源码，不会执行网站的任何脚本（Google除外，据说Googlebot可以运行javaScript）。使用了Vue或者其它MVVM框架之后，页面大多数DOM元素都是在客户端根据js动态生成，可供爬虫抓取分析的内容大大减少。另外，浏览器爬虫不会等待我们的数据完成之后再去抓取我们的页面数据。服务端渲染返回给客户端的是已经获取了异步数据并执行JavaScript脚本的最终HTML，网络爬中就可以抓取到完整页面的信息。
    - 更利于首屏渲染
      - 首屏的渲染是node发送过来的html字符串，并不依赖于js文件了，这就会使用户更快的看到页面的内容。尤其是针对大型单页应用，打包后文件体积比较大，普通客户端渲染加载所有所需文件时间较长，首页就会有一个很长的白屏等待时间。
  * 场景：交互少，数据多，例如新闻，博客，论坛类等
  * 原理：
    - 相当于服务端前面加了一层url分配，可以假想为服务端的中间层，
    - 当地址栏url改变或者直接刷新，其实直接从服务器返回内容，是一个包含内容部分的html模板，是服务端渲染
    - 而在交互过程中则是ajax处理操作，局部刷新，首先是在history模式下，通过history. pushState方式进而url改变，然后请求后台数据服务，拿到真正的数据，做到局部刷新，这时候接收的是数据而不是模板
  * 缺点：
    - 服务端压力较大
      - 本来是通过客户端完成渲染，现在统一到服务端node服务去做。尤其是高并发访问的情况，会大量占用服务端CPU资源；
    - 开发条件受限
      - 在服务端渲染中，created和beforeCreate之外的生命周期钩子不可用，因此项目引用的第三方的库也不可用其它生命周期钩子，这对引用库的选择产生了很大的限制；
    - 安全问题
      - 因为做了node服务，因此安全方面也需要考虑DDOS攻击和sql注入
    - 学习成本相对较高
      - 除了对webpack、Vue要熟悉，还需要掌握node、Express相关技术。相对于客户端渲染，项目构建、部署过程更加复杂。
---

# vue的全局组件有哪些
  * 全局：transition、keep-alive、transition-group
---

# transition原理
  * transition是vue内置的一个抽象组件，在transition组件渲染时会将绑定在transition自身的属性和事件组合成一个对象，并将该对象绑定在transition子组件的data.transition属性上。
  * 有6个类名：
    - v-enter：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。
    - v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。
    - v-enter-to: 2.1.8版及以上 定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter 被移除)，在过渡/动画完成之后移除。
    - v-leave: 定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。
    - v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
    - v-leave-to: 2.1.8版及以上 定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave 被删除)，在过渡/动画完成之后移除。
  * 原理：
    - 动画流程出现：
      - 开始前一帧： 点击出现动画，元素由none变为block，动画开始前一帧，插入opacity:0属性 “1”，和监听opacity属性变化时间为3s “2”
      - 动画第二帧：opacity:0，属性 “1” 去除，引起“2”监听执行时间变化
      - 动画最后一帧： 动画结束，去除所有
    - 动画流程消失：
      - 开始前一帧： 点击消失动画，元素由block变为none，动画开始前一帧，只插入监听opacity属性变化时间为3s “4”
      - 动画第二帧： 插入，“3” 属性opacity:0引起 “2” 监听执行事件变化
      - 动画最后一帧： 动画结束，去除所有
---

# vue的全局指令有哪些
  * 全局：model、show
---

# 如何自定义全局组件
  1. 使用Vue.component(组件名，方法)
  2. 添加install方法
  3. 在main.js中引入之后，通过Vue.use挂载
---

# 如何定义局部组件
  1. 编写组件并导出
  2. 在需要用到组件的地方import引入
  3. 在components属性中写入这个组件
---

# vue设计思想的理解
  * 核心思想：数据驱动，组件化。三要素是数据响应式、模板引擎以及渲染
  * 数据响应式： 监听数据变化并在视图中更新
    - Object.defineProperty()
    - Proxy
  * 模板引擎： 提供描述视图的模板语法
    - 插值：两个花括号
    - 指令：v-bind, v-on, v-model, v-for, v-if
  * 渲染：如何将模板转换为html
    - 模板=> vdom => dom
  * 数据响应式原理
    - vue2中利用Object.defineProperty()实现变更检测。
---

# new Vue() 发生了什么？
  1. 首先在new Vue之后，对生命周期、事件中心、data、props等进行了初始化。
  2. 然后调用$mount函数对$el节点进行渲染。渲染的过程中，Vue只认render函数，那么就会先将template转换为render函数(mountComponent函数的功能)。
  3. 然后调用render函数生成vnode。怎么生成？主要是利用createELement函数，先将vnode的children处理为一维数组，然后通过判断tag来生成vnode。
  4. 生成vnode之后，就是通过patch函数转换为dom节点，渲染在视图上。那么，怎么将vnode转换为dom节点？在vnode中，有个elm用来保存对应的dom节点，因此不用再额外生成dom（除了文本节点以外）。只是需要不深度遍历children，将children中的dom节点添加到父节点中。完成dom树的构建。
---

# Vue.use是干什么的？原理是什么？
  * vue.use 是用来使用插件的，我们可以在插件中扩展全局组件、指令、原型方法等。

  * 原理：
    1. 检查插件是否注册，若已注册，则直接跳出；
    2. 处理入参，将第一个参数之后的参数归集，并在首部塞入 this 上下文；
    3. 执行注册方法，调用定义好的 install 方法，传入处理的参数，若没有 install 方法并且插件本身为 function 则直接进行注册；
---

# 如何理解自定义指令？
  1. 在生成 ast 语法树时，遇到指令会给当前元素添加directives属性
  2. 通过 genDirectives 生成指令代码
  3. 在patch前将指令的钩子提取到 cbs中，在patch过程中调用对应的钩子。
  4. 当执行指令对应钩子函数时，调用对应指令定义的方法
---

# 怎么给vue定义全局方法
  1. 将方法挂载到vue.prototype上。缺点：调用这个方法的时候没有提示
  2. 你用全局混入mixin。优点：因为mixin里面的methods会和创建的每个单个文件组件合并。这样调用的时候就有提示
  3. 使用plugin方式：vue.use的实现没有挂载的功能，只是触发插件的install方法，本质还是用的vue.prototype
  4. 任意vue文件中写全局函数
---

# vue中父组件可以监听到子组件的生命周期吗？
  * 可以。使用以下几种方式
    1. 使用on和emit
    2. 使用hook钩子函数
---

# vue-cli默认是单页面的，如果要开发多页面怎么办
  * 可以在vue-cli中配置vue.config.js的pages选项，它是一个对象，对象的key是入口的名字，value是具体的路径
---

# $mount的原理及作用(vue实例挂载的实现过程)
  * 作用：生成真实dom
  * 在执行$mount的时候，vue会先判断render函数是否存在，若存在，则执行render编译模板，若不存在，则判断template，若template存在，则将template通过render函数编译成Vnode，然后调用mountComponent函数，该函数主要调用的是updateComponent函数。mountComponent作用：把虚拟dom转换为真实dom。最后更新DOM，在这过程中，初始化时调用了Watcher类来充当观察者模式，一旦vm实例中的数据发生变化，Watcher就执行回调函数
---

# render方法的实现原理
  > Vue 的 _render 方法是实例的一个私有方法，它用来把实例渲染成一个虚拟 Node,执行 createElement 方法并返回的是 vnode
---

# update方法的实现原理
  > 私有化方法_update主要是调用了patch函数，patch函数的主要功能将vnode转换为dom节点然后渲染在视图中。因此，为了生成dom节点，还需要判断vnode是否有子节点，一直递归到没有子节点时。开始创建子节点并插入在父节点中。最后再将原来定义的根节点移除，因为已经重新建立了新的节点替换原来的根节点。
---

# createElement的实现过程
  0. 总结：createElement函数主要是通过对children进行扁平化处理之后，通过对标签（tag）的判断，使用VNode类来生成vnode，并将其返回。
  1. 在createElement中，首先检测data的类型，通过判断data是不是数组，以及是不是基本类型，来判断data是否传入。如果没有传入，则将所有的参数向前赋值，且data=undifined。 然后，判断判断传入的Normalize参数是否为真，为真的话，赋值给alwaysNormalize常量。
  2. 在_createElement中，首先判断data是不是响应式的，vnode中的data不能是响应式的。如果是，则Vue抛出警告
  3. 最重要的部分是将children转换为1维数组。在normalizeChildren函数中，主要是调用normalizeArrayChildren函数去处理children类数组
  4. 在normalizeArrayChildren函数中，主要完成的功能是判断children中的元素是不是数组，如果是的话，就递归调用数组，并将每个元素保存在数组中返回。首先判断children中的元素是不是数组，是的话递归调用函数。如果第一个和最后一个都是文本节点的话，将其合并，优化。然后，判断该元素是不是基本类型。如果是，在判断最后一个结点是不是文本节点，是的话将其与该元素合并为一个文本节点。否则，把这个基本类型转换为文本节点(VNode)。最后一种情况，该元素是一个VNode，先同样进行优化（合并第一个和最后一个节点），然后判断该节点的属性，最后将该节点加入到结果中。
  5. 回到creat-element.js中，在将children拍平为一维数组后，接着判断标签（tag）是不是字符串，是的话，则判断该标签是不是平台内建的标签（如：‘div’），是的话则创建该VNode。然后判断该标签是不是组件，是的话，创建一个组件VNode。如果都不是，则按照该标签名创建一个VNode。如果标签（tag）不是字符串，则创建组件VNode。
---

# 思考题
  ```
    <div id="app></div>

    const app = new Vue({
      el: "#app",
      data: {
        obj: {
          foo: 'foo'
        }
      }
    })
  ```
  * 问：会有几个Observer? Dep? Watcher?
  * 答：2 4 1
  * 解析：有1个对象1个Oberver, 有1个key就会产生1个dep(2大2小), 1个组件一个watcher
---

# Vue响应式原理Observer、Dep、Watcher理解
---

# $mount 和 el的区别
  > 两者在使用效果上没有任何区别，都是为了将实例化后的vue挂载到指定的dom元素中。如果在实例化vue的时候指定el，则该vue将会渲染在此el对应的dom中，反之，若没有指定el，则vue实例会处于一种“未挂载”的状态，此时可以通过$mount来手动执行挂载。
---

# setup()是如何生效的？如何和之前的版本兼容？

---

# vue3初始化过程有哪些变化？
  > 首先执行createApp函数，这个方法会返回一个app实例，会执行mount函数，mount函数内部会执行render函数，render函数内部又执行patch,执行完patch之后，会处理根组件processComponent函数，它的内部会执行mountComponent
---