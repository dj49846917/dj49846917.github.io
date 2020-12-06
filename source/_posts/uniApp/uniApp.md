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

### 详细文档请看
  * 详细文档请看: https://uniapp.dcloud.io/frame?id=%e5%b0%ba%e5%af%b8%e5%8d%95%e4%bd%8d

*** 

# uni-app设置底部导航栏
  * 详细文档请看: https://uniapp.dcloud.io/collocation/pages?id=tabbar
  * 代码配置如下:
    ```
      "tabBar": {
          "color": "#000000",
          "selectedColor": "#07c261",
          "backgroundColor": "#ffffff",
          "list": [{
              "pagePath": "pages/home/home",
              "iconPath": "static/images/index.png",
              "selectedIconPath": "static/images/index-select.png",
              "text": "微信"
          }, 
        {
              "pagePath": "pages/chat/chat",
              "iconPath": "static/images/mail.png",
              "selectedIconPath": "static/images/mail-select.png",
              "text": "通讯录"
          },
        {
            "pagePath": "pages/look/look",
            "iconPath": "static/images/find.png",
            "selectedIconPath": "static/images/find-select.png",
            "text": "发现"
        },
        {
            "pagePath": "pages/account/account",
            "iconPath": "static/images/my.png",
            "selectedIconPath": "static/images/my-select.png",
            "text": "我"
        }]
      }
    ```

# uni-app隐藏顶部导航栏
  * 在pages.json中，设置globalStyle的navigationStyle: custom
    ```
      // pages.json

      "globalStyle": {
        ...
        "navigationStyle":"custom",
      },
    ```

  * 在pages.json中，设置globalStyle的app-plus
    ```
      // pages.json
    
      "globalStyle": {
        ...
        "app-plus": {
          "titleNView": false,
          "scrollIndicator": "none"
        }
      },
    ```

{% note warning %}
  navigationStyle设置成custom之后，所有端的原生导航栏都没有了，包括app和小程序，如果只想app端去掉导航栏，使用app-plus
{% endnote %}

# uni-app中获取状态栏的高度
  1. 使用plus.navigator.getStatusbarHeight()
    * **注意，该api只在nvue里有效，一定要用条件编译**
      ```
        <view class="tipBar" :style="'height'+ tipBarHeight + 'px'">
          <text>{{ tipBarHeight }}</text>
        </view>

        export default {
          data() {
            return {
              tipBarHeight: 0
            }
          },
          onLoad() {
            // #ifdef APP-PLUS-NVUE
              this.tipBarHeight = plus.navigator.getStatusbarHeight()   // 获取状态栏的高度
            // #endif
          },
        }
      ```

  2. 使用uni.getSystemInfoSync().statusBarHeight
    * 该api可以在所有端都有用
      ```
        <view class="tipBar" :style="'height'+ tipBarHeight + 'px'">
          <text>{{ tipBarHeight }}</text>
        </view>

        export default {
          data() {
            return {
              tipBarHeight: 0
            }
          },
          onLoad() {
            this.tipBarHeight = uni.getSystemInfoSync().statusBarHeight // 获取状态栏高度
          },
        }
      ```

*** 

# uni-app集成vuex
## 集成步骤
  1. 在根路径下创建store文件夹，并创建modules文件夹和index.js
  2. 在modules文件夹下随意新建一个js文件，写入以下代码
      ```
        const home = {
          state: {
            user: "张三"
          },
          mutations: {
            
          },
          actions: {
            
          }
        }

        export default home;
      ```
  3. index.js中写入以下代码:
      ```
        import Vue from 'vue'
        import Vuex from 'vuex'

        Vue.use(Vuex)

        import home from "./modules/home.js"

        export default new Vuex.Store({
          modules:{
            home
          }
        })
      ```

  4. 在main.js中引入store
      ```
        import store from "./store/index.js"

        Vue.prototype.$store = store
      ```

  5. 使用：通过this.$store.state就能获取store里的值

## mapState的使用
  * 通过vuex存储和获取状态栏的高度
    ```
      // 在App.vue中：
      
      onLaunch: function() {
        // #ifdef APP-PLUS-NVUE
       
        // 获取状态栏的高度
        this.$store.commit("app/setTipBarHeight", plus.navigator.getStatusbarHeight())
        // #endif
      },

      
      // 在store/app.js中：
      
      const App = {
        namespaced: true,
        state: {
          tipBarHeight: 0
        },
        mutations: {
          setTipBarHeight: (state, payload) => {
            state.tipBarHeight = payload
          }
        },
      }
      export default App


      // 调用，在home.nvue中：

      <view class="tipBar" :style="getStatusBarHeight"></view>

      computed: {
        ...mapState({
          getStatusBarHeight: (state) => {
            return `height: ${state.app.tipBarHeight}px`
          }
        }),
      },
    ```

# uni-app 在js中rpx转换为px
  * 使用uni.upx2px(value)

# uni-app 跳转页面并传递参数
## uni.navigateTo(OBJECT)
  * 保留当前页面，跳转到应用内的某个页面，使用uni.navigateBack可以返回到原页面。
  * 举例：
    ```
      //在起始页面跳转到test.vue页面并传递参数
      uni.navigateTo({
          url: 'test?id=1&name=uniapp'
      });
    ```

  * 接收参数：
    ```
      // 在test.vue页面接受参数
      export default {
          onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
              console.log(option.id); //打印出上个页面传递的参数。
              console.log(option.name); //打印出上个页面传递的参数。
          }
      }
    ```

  * url有长度限制，太长的字符串会传递失败，可使用窗体通信、全局变量，或encodeURIComponent等多种方式解决，如下为encodeURIComponent示例。
    ```
      <navigator :url="'/pages/test/test?item='+ encodeURIComponent(JSON.stringify(item))"></navigator>  


      // 在test.vue页面接受参数
      onLoad: function (option) {
          const item = JSON.parse(decodeURIComponent(option.item));
      }
    ```

