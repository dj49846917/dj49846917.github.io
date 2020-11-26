---
title: uni-app中nvue的开发心得
date: 2020-11-26 14:56:43
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

# 介绍
  * uni-app App端内置了一个基于 weex 改进的原生渲染引擎，提供了原生渲染能力。
  * 在App端，如果使用vue页面，则使用webview渲染；如果使用nvue页面(native vue的缩写)，则使用原生渲染。一个App中可以同时使用两种页面，比如首页使用nvue，二级页使用vue页面，hello uni-app示例就是如此。
  * 虽然nvue也可以多端编译，输出H5和小程序，但nvue的css写法受限，所以如果你不开发App，那么不需要使用nvue。

***

# 使用场景
  1. 需要高性能的区域长列表或瀑布流滚动。webview的页面级长列表滚动时没有性能问题的（就是滚动条覆盖webview整体高度），但页面中某个区域做长列表滚动，则需要使用nvue的list、recycle-list、waterfall等组件。这些组件的性能要高于vue页面里的区域滚动组件scroll-view。
  2. 复杂高性能的自定义下拉刷新。uni-app的pages.json里可以配置原生下拉刷新，但引擎内置的下拉刷新样式只有雪花和circle圈2种样式。如果你需要自己做复杂的下拉刷新，推荐使用nvue的refresh组件。当然插件市场里也有很多vue下的自定义下拉刷新插件，只要是基于renderjs或wxs的，性能也可以商用，只是没有nvue的refresh组件更极致。
  3. 左右拖动的长列表。在webview里，通过swiper+scroll-view实现左右拖动的长列表，前端模拟下拉刷新，这套方案的性能不好。此时推荐使用nvue，比如新建uni-app项目时的新闻示例模板，就采用了nvue，切换很流畅。
  4. 实现区域滚动长列表+左右拖动列表+吸顶的复杂排版效果，效果可参考hello uni-app模板里的swiper-list
  5. 如需要将软键盘右下角按钮文字改为“发送”，则需要使用nvue。比如聊天场景，除了软键盘右下角的按钮文字处理外，还涉及聊天记录区域长列表滚动，适合nvue来做。
  6. 解决前端控件无法覆盖原生控件的层级问题。当你使用map、video、live-pusher等原生组件时，会发现前端写的view等组件无法覆盖原生组件，层级问题处理比较麻烦，此时使用nvue更好。或者在vue页面上也可以覆盖一个subnvue（一种非全屏的nvue页面覆盖在webview上），以解决App上的原生控件层级问题。
  7. 如深度使用map组件，建议使用nvue。除了层级问题，App端nvue文件的map功能更完善，和小程序拉齐度更高，多端一致性更好。
  8. 如深度使用video，建议使用nvue。比如如下2个场景：video内嵌到swiper中，以实现抖音式视频滑动切换，例子见插件市场；nvue的视频全屏后，通过cover-view实现内容覆盖，比如增加文字标题、分享按钮。
  9. 直播推流：nvue下有live-pusher组件，和小程序对齐，功能更完善，也没有层级问题。
  10. 对App启动速度要求极致化。App端v3编译器模式下，如果首页使用nvue且在manifest里配置fast模式，那么App的启动速度可以控制在1秒左右。而使用vue页面的话，App的启动速度一般是3秒起，取决于你的代码性能和体积。

# 详细文档
  * https://uniapp.dcloud.io/use-weex

***

# 日常nvue开发总结
## 开启纯原生渲染模式
  * 启用纯原生渲染模式，可以减少App端的包体积、减少使用时的内存占用。因为webview渲染模式的相关模块将被移除。
  * 在manifest.json源码视图的"app-plus"下配置"renderer":"native"，即代表App端启用纯原生渲染模式。此时pages.json注册的vue页面将被忽略，vue组件也将被原生渲染引擎来渲染。写入以下代码：
    ```
      // manifest.json    
      {    
          // ...    
          /* App平台特有配置 */    
          "app-plus": {    
              "renderer": "native", //App端纯原生渲染模式
          }    
      }
    ```

## 设置编译模式为uni-app
  * 在 manifest.json 中修改2种编译模式，manifest.json -> app-plus -> nvueCompiler 切换编译模式。
    ```
      // manifest.json    
      {    
          // ...    
          /* App平台特有配置 */    
          "app-plus": {    
              "nvueCompiler":"uni-app" //是否启用 uni-app 模式  
          }    
      }
    ```

***

# 在uni-app中使用iconfont
## nvue运行在app中
  1. iconfont官网：https://www.iconfont.cn/
  2. 登录
  3. 选择你要的图标添加到项目中
    * ![添加图标到项目](/images/uniApp/添加到项目.jpg)
    * 点击查看在线链接按钮，看到一串代码, ![添加图标到项目2](/images/uniApp/添加到项目2.jpg)
  4. 在需要用到的nvue页面或者在App.vue中的onLaunch事件中写入以下代码:
    ```
      // App.vue中

      onLaunch: function() {
			  // #ifdef APP-PLUS-NVUE
			  // 引入iconfont
			  const domModule = weex.requireModule('dom');
        domModule.addRule('fontFace', {
          fontFamily: 'iconfont',
          src: "url('https://at.alicdn.com/t/font_2199095_voz8fueyab.ttf')"
        });
        // #endif
      },
    ```

{% note warning %}
  代码中的src的路径就是你项目中iconfont的ttf文件路径
{% endnote %}

  5. 在代码中用text组件
    ```
      <text class="iconfont">
        &#xe649;
      </text>
    ```
{% note warning %}
  * class的内容必须是iconfont，和步骤4的fontFamily保持一致
  * 必须要text标签，其他标签都没有效果
  * text的内容不能有任何的空格，或者换行
  * 可以通过css控制样式
{% endnote %}

## 兼容其他端
  1. iconfont官网：https://www.iconfont.cn/
  2. 登录
  3. 选择你要的图标添加到项目中
    * ![添加图标到项目](/images/uniApp/添加到项目.jpg)
    * 点击查看在线链接按钮，看到一串代码, ![添加图标到项目2](/images/uniApp/添加到项目2.jpg)
  4. 新建icon.css文件，然后在App.vue中引入,用到条件编译
    ```
      /* #ifndef APP-PLUS-NVUE */
      @import "./common/css/icon.css";
      /* #endif */
    ```
  5. 回到iconfont,点击下载至本地，然后拿到iconfont.css,把里面的内容复制到新建的icon.css中
    * ![添加图标到项目](/images/uniApp/添加到项目3.jpg)
    * 在icon.css中只留下以下代码即可:
      ``` 
        @font-face {font-family: "iconfont";
          src: url('//at.alicdn.com/t/font_2199095_voz8fueyab.ttf?t=1606396700079') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+ */
        }

        .iconfont {
          font-family: iconfont !important;
          font-size: 16px;
          font-style: normal;
          -webkit-font-smoothing: antialiased;
          -moz-osx-font-smoothing: grayscale;
        }
      ```
    * 将app.vue中的ttf路径放到icon.css中
      ```
        @font-face {font-family: "iconfont";
          src: url('https://at.alicdn.com/t/font_2199095_voz8fueyab.ttf') format('truetype'),
        }
      ```