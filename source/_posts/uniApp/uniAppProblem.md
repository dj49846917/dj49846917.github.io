---
title: uni-app问题收集及解决
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

# css部分
## nvue支持sass但是不支持sass的嵌套
  * 变量使用(**支持**)
    ```
      <style lang="scss">
        $lineBorderColor: yellow;
        .icon_right{
          font-size: 50px;
          color: $lineBorderColor;
        }
      </style>
    ```

  * 外部scss文件引入(**支持**)
    ```
      <style lang="scss">
        @import "../../constant/css/constant.scss";
        .icon_right{
          font-size: 50px;
          color: $lineBorderColor;
        }
      </style>
    ```

  * nvue不支持sass嵌套
  
  * nvue支持sass的所有其他属性，举例mixin(混合)
    ```
      // constant.scss文件

      $lineBorderColor: #dfdfdf;
      @mixin my {
        justify-content: center;
        align-items: center;
        height: 200rpx;
        border-color: red;
        border-width: 2px;
        border-style: solid;
        width: 750rpx;
        background-color: pink;
      }

      // home.nvue中

      <style lang="scss">
        @import "../../constant/css/constant.scss";
        .icon_right{
          font-size: 50px;
          color: $lineBorderColor;
        }
        .list {
          @include my;
        }
      </style>
    ```

## 不可用css收集
### display
  * 在nvue中，默认就是display: flex,所以在nvue中，不支持display属性，写了在app端不支持，其他端可以

### border
  * 在vue中，border的3个属性需要分开写，不能像vue那样
  * 举例：
    ```
      # 在vue中：
        border: 1px solid #ccc;

      # 在nvue中:
        border-color: #ccc;
        border-width: 1px;
        border-style: solid;
    ```
