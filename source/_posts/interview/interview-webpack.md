---
title: 面试——webpack篇
date: 2020-12-29 10:50:01
tags: 
  - 面试
  - webpack
type: 面试                                                                 # 标签、分类
description:  Webpack 是一个前端资源加载/打包工具。它将根据模块的依赖关系进行静态分析,然后将这些模块按照指定的规则生成对应的静态资源。
top_img: https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=3813469802,1665117316&fm=26&gp=0.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 面试
  - webpack                                                                 # 文章标签
cover: https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=3813469802,1665117316&fm=26&gp=0.jpg                 # 文章的缩略图（用在首页）
---

# webpack 如何实现动态加载

---

# React.lazy 的原理是什么？
  1. React.lazy的用法
    * React.lazy方法可以异步加载组件文件。
      ```
        const Foo = React.lazy(() => import('../componets/Foo));
      ```
    
    * React.lazy不能单独使用，需要配合React.suspense，suspence是用来包裹异步组件，添加loading效果等。 
      ```
        <React.Suspense fallback={<div>loading...</div>}>
          <Foo/>
        </React.Suspense>
      ```

  2. React.lazy原理
    * React.lazy使用import来懒加载组件，import在webpack中最终会调用requireEnsure方法，动态插入script来请求js文件，类似jsonp的形式。
---

# webpack 能动态加载 require 引入的模块吗？
---

# require 引入的模块 webpack 能做 Tree Shaking 吗？
---

# webpack的plugins和loaders的实现原理。
---

# webpack如何优化编译速度?
---

# webpack 热更新原理
  * 大概是我们用 webpack-dev-server启动一个服务之后，浏览器和服务端是通过 websocket进行长连接， webpack内部实现的 watch就会监听文件修改，只要有修改就 webpack会重新打包编译到内存中，然后 webpack-dev-server依赖中间件 webpack-dev-middleware和 webpack之间进行交互，每次热更新都会请求一个携带 hash值的 json文件和一个 js， websocker传递的也是 hash值，内部机制通过 hash值检查进行热更新
---

# webpack原理
  1. 初始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数；
  2. 开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译；
  3. 确定入口：根据配置中的 entry 找出所有的入口文件；
  4. 编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理；
  5. 完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系；
  6. 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会；
  7. 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。
  8. 在以上过程中，Webpack 会在特定的时间点广播出特定的事件，插件在监听到感兴趣的事件后会执行特定的逻辑，并且插件可以调用 Webpack 提供的 API 改变 Webpack 的运行结果。
---

# babel原理
  * babel的转译过程分为三个阶段：parsing、transforming、generating，以ES6代码转译为ES5代码为例，babel转译的具体过程如下：
    - ES6代码输入
    - babylon 进行解析得到 AST
    - plugin 用 babel-traverse 对 AST 树进行遍历转译,得到新的AST树
    - 用 babel-generator 通过 AST 树生成 ES5 代码
---

# webpack怎么配置多入口
---

# webpack的loader的加载顺序
---



