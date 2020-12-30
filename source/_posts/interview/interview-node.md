---
title: 面试——node篇
date: 2020-12-30 16:25:28
tags: 
  - 面试
  - node
type: 面试                                                                 # 标签、分类
description:  Vue是一套用于构建用户界面的渐进式框架。
top_img: https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=4104387483,4024841696&fm=26&gp=0.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 面试
  - node                                                                 # 文章标签
cover: https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=4104387483,4024841696&fm=26&gp=0.jpg                 # 文章的缩略图（用在首页）
---

# Node 异步单线程原理？
---

# Node 如何多进程？
---

# express实现原理
  * express的所有服务端逻辑处理都是通过中间件来实现的，中间件是一个函数，而app.use（）方法就是去装载这些函数,并放入一个数组中。

  * 当前端一个请求传到服务器的时候，首先会经过request，然后是一系列的服务端处理，也就是中间件处理，存放于数组中的中间件采用后进先出的栈模式处理请求，最先入栈的中间件处理完请求之后，通过next将执行权交给第二个入栈的中间件，依次类推，直到数组末尾或者中间某个中间件没有调用next()函数，最后再将处理完的结果response回前端。
---

# node的事件循环和浏览器的有什么区别吗？
---

# express 框架的核心特性
  1. 可以设置中间件来响应 HTTP 请求。
  2. 定义了路由表用于执行不同的 HTTP 请求动作。
  3. 可以通过向模板传递参数来动态渲染 HTML页面
---

# 介绍一下你对中间件的理解
---