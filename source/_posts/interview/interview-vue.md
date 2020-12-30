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

# 谈谈你对MVVM开发模式的理解
  1. MVVM分为Model、View、ViewModel三者。
    * Model 代表数据模型，数据和业务逻辑都在Model层中定义；
    * View 代表UI视图，负责数据的展示；
    * ViewModel 负责监听 Model 中数据的改变并且控制视图的更新，处理用户交互操作；

  2. Model 和 View 并无直接关联，而是通过 ViewModel 来进行联系的，Model 和 ViewModel 之间有着双向数据绑定的联系。因此当 Model 中的数据改变时会触发 View 层的刷新，View 中由于用户交互操作而改变的数据也会在 Model 中同步。这种模式实现了 Model 和 View 的数据自动同步，因此开发者只需要专注对数据的维护操作即可，而不需要自己操作 dom。
---

# 简述Vue的响应式原理
  * 当一个Vue实例创建时，vue会遍历data选项的属性，用 Object.defineProperty 将它们转为 getter/setter并且在内部追踪相关依赖，在属性被访问和修改时通知变化。每个组件实例都有相应的 watcher 程序实例，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的 setter 被调用时，会通知 watcher 重新计算，从而致使它关联的组件得以更新。
  * 简单版：数据劫持收集依赖派发更新
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

# vue中的computed和watch有什么区别？
  * 在computed里面定义了的属性，在data里面不能再重复定义，computed依赖于data中的属性，而watch依赖于data中属性的改变，watch中可以进行异步操作。
---

# Vue.observable你有了解过吗？说说看
  * ue2.6发布一个新的API，可以处理一些简单的跨组件共享数据状态的问题。
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

# 说说你对slot的理解有多少？slot使用场景有哪些？
  * slot, 插槽, 在使用组件的时候, 在组建内部插入东西.
  * 场景：组件封装的时候最常使用到
---

# prop验证的type类型有哪几种？
    Number, String, Boolean, Array, Function, Object
---

# 你了解vue的diff算法吗？
  * 同级比较，如果它发现同级有不同，就比较完毕，将不同的进行替换。同级里面通过key值比较同级的元素差异，如果相同，继续往下比较，如果不同，将不同的进行替换。
---

# vue能监听到数组变化的方法有哪些？为什么？
    push(), pop(), shift(), unshift(), splice(), sort(), reverse()
---

# 你知道nextTick的原理吗？
  * 提到DOM的更新是异步执行的，只要数据发生变化，将会开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。如果同一个 watcher 被多次触发，只会被推入到队列中一次。简单来说，就是当数据发生变化时，视图不会立即更新，而是等到同一事件循环中所有数据变化完成之后，再统一更新视图。
---

# 怎么缓存当前的组件？缓存后怎么更新？
---

# vue生命周期总共有几个阶段？
```
答: 创建前/后， 挂载前/后，更新前/后，销毁前/后

    补充: 请描述下vue的生命周期是什么？

	答:
	生命周期就是vue从开始创建到销毁的过程，分为四大步（创建，挂载，更新，销毁），每一步又分为两小步，如beforeCreate，created。beforeCreate前，也就是new Vue的时候会初始化事件和生命周期；beforeCreate和created之间会挂载Data，绑定事件；接下来会根据el挂载页面元素，如果没有设置el则生命周期结束，直到手动挂载；el挂载结束后，根据templete/outerHTML(el)渲染页面；在beforeMount前虚拟DOM已经创建完成；之后在mounted前，将vm.$el替换掉页面元素el;mounted将虚拟dom挂载到真实页面（此时页面已经全部渲染完成）；之后发生数据变化时触发beforeUpdate和updated进行一些操作；最后主动调用销毁函数或者组件自动销毁时beforeDestroy，手动撤销监听事件，计时器等；destroyed时仅存在Dom节点，其他所有东西已自动销毁。这就是我所理解的vue的一个完整的生命周期；
```
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

# axios原理
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
  1. 如果是公共组件确实必须要返回一个函数,

  2. 但是如果不是可以直接一个对象，因为没有执行函数return之后返回的都是一个全新的对象(类似于工厂模式的设计思想),虽然我们一样, 但是我们又没有关系. 避免数据混淆
---

# v-for循环中key有什么作用？
    性能优化，让vue在更新数据的时候可以更有针对性
---

# 说说你对keep-alive的理解是什么？
    保留组件状态 避免重新渲染
---

# 什么是虚拟dom?
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

# vuex直接修改state 与 用commit提交mutation来修改state有什么区别?(mutation 和 action 有什么区别，怎么用？)
    相同点: 都可以修改state的值，并且是响应式的
    区别: 开启严格模式下，任何修改state的操作，只要不经过mutation的函数，就会报错
---

# vuex的store有几个属性值？分别讲讲它们的作用是什么？
  * state, 状态初始化, 并实施观察
  * getter, 获取数据用于view或data中使用
  * mutation: 内部处理state变化
  * action: 处理外部交互
  * module: 模块化以上四个
---

# 对 vuex 的理解，单向数据流
---

# vue3 的 类似 hooks 的原理是怎么样的
---

# 路由守卫哪些参数，怎么实现？
---

# vue3.0 有哪些改动和新特性？
---

# vue-lazyloader 的原理
  1. vue-lazyload是通过指令的方式实现的，定义的指令是v-lazy指令；

  2. 指令被bind时会创建一个listener，并将其添加到listener queue里面， 并且搜索target dom节点，为其注册dom事件(如scroll事件)

  3. 上面的dom事件回调中，会遍历 listener queue里的listener，判断此listener绑定的dom是否处于页面中perload的位置，如果处于则加载异步加载当前图片的资源；

  4. 同时listener会在当前图片加载的过程的loading，loaded，error三种状态触发当前dom渲染的函数，分别渲染三种状态下dom的内容；
---

# vue-router 的原理
---

# vue是怎么监听数组改变更新视图的
---


