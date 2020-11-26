---
title: uni-app开发总结
date: 2020-11-26 14:38:48
tags: 
  - uni-app
type: uni-app                                                                 # 标签、分类
description:  是一个使用 Vue.js 开发所有前端应用的框架，开发者编写一套代码，可发布到iOS、Android、Web（响应式）、以及各种小程序（微信/支付宝/百度/头条/QQ/钉钉/淘宝）、快应用等多个平台。
top_img: /images/uniApp/logo.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - uni-app                                                                 # 文章标签
cover: /images/uniApp/logo.jpg                 # 文章的缩略图（用在首页）
---

# 开发环境搭建
## 下载HBuilderX
  * 下载地址：https://www.dcloud.io/hbuilderx.html
## 安装插件
  * 打开菜单： 工具 > 插件安装，安装以下插件
    * App真机运行
    * es6编译
    * eslint-js
    * eslint-plugin-vue
    * less编译
    * scss/sass编译
    * stylus编译
    * uni-app App调试
    * uni-app编译
***

# uni-app的开发总结
## 条件编译
### 写法：以 #ifdef 或 #ifndef 加 %PLATFORM% 开头，以 #endif 结尾。
  * #ifdef：if defined 仅在某平台存在
  * #ifndef：if not defined 除了某平台均存在
  * %PLATFORM%：平台名称
  * 详细文档请看：https://uniapp.dcloud.io/platform?id=%e6%9d%a1%e4%bb%b6%e7%bc%96%e8%af%91
