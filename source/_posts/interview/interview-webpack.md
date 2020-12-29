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




