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

***

## 适配
### uni-app 支持的通用 css 单位包括 px、rpx
  * px 即屏幕像素
  * rpx 即响应式px，一种根据屏幕宽度自适应的动态单位。以750宽的屏幕为基准，750rpx恰好为屏幕宽度。屏幕变宽，rpx 实际显示效果会等比放大，但在 App 端和 H5 端屏幕宽度达到 960px 时，默认将按照 375px 的屏幕宽度进行计算

### 设计稿 1px / 设计稿基准宽度 = 框架样式 1rpx / 750rpx
### 举例说明：
  * 若设计稿宽度为 750px，元素 A 在设计稿上的宽度为 100px，那么元素 A 在 uni-app 里面的宽度应该设为：750 * 100 / 750，结果为：100rpx。
  * 若设计稿宽度为 640px，元素 A 在设计稿上的宽度为 100px，那么元素 A 在 uni-app 里面的宽度应该设为：750 * 100 / 640，结果为：117rpx。
  * 若设计稿宽度为 375px，元素 B 在设计稿上的宽度为 200px，那么元素 B 在 uni-app 里面的宽度应该设为：750 * 200 / 375，结果为：400rpx。
  
{% note warning %}
注意：
  1. vue页面支持普通H5单位，但在nvue里不支持：
    * rem 根字体大小可以通过 page-meta 配置
    * vh viewpoint height，视窗高度，1vh等于视窗高度的1%
    * vw viewpoint width，视窗宽度，1vw等于视窗宽度的1%
  2. nvue还不支持百分比单位。
  3. App端，在 pages.json 里的 titleNView 或页面里写的 plus api 中涉及的单位，只支持 px。**注意此时不支持 rpx**
{% endnote %}