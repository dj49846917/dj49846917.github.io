---
title: 微信小程序开发总结
date: 2021-09-25 07:13:39
tags: 
  - 小程序
type: 小程序                                                                 # 标签、分类
description:  小程序是一种新的开放能力，开发者可以快速地开发一个小程序。
top_img: /images/weapp/logo.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 小程序                                                                 # 文章标签
cover: /images/weapp/logo.jpg                 # 文章的缩略图（用在首页）
---

# 小程序代码规范
  1. 书写的class类名用中划线`-`，不要用驼峰或者下划线`_`命名
  2. 自定义组件的wxss文件中尽量不要使用后代选择器、id选择器、标签选择器、带驼峰或者下划线类名的选择器，最好使用class选择器。
  3. 在页面级的wxss文件中，可以使用后代选择器，和class选择器，其他避免使用，以免出现警告
  4. 在app.wxss中，不要使用标签选择器，不然也会出现警告

# 在小程序中使用tabbar
  1. 找到app.json，和window，pages属性同级的地方添加tabBar属性
    ```
      {
        "pages":[
          "pages/index/index",
          "pages/category/category",
          "pages/cart/cart",
          "pages/user/user"
        ],
        "tabBar": {
          "selectedColor": "#f77127",
          "color": "#a8a8a8",
          "list": [
            {
              "pagePath": "pages/index/index",
              "text": "首页",
              "iconPath": "/assets/images/home.png",
              "selectedIconPath": "/assets/images/home_select.png"
            },
            {
              "pagePath": "pages/category/category",
              "text": "分类",
              "iconPath": "/assets/images/category.png",
              "selectedIconPath": "/assets/images/category_select.png"
            },
            {
              "pagePath": "pages/cart/cart",
              "text": "购物车",
              "iconPath": "/assets/images/cart.png",
              "selectedIconPath": "/assets/images/cart_select.png"
            },
            {
              "pagePath": "pages/user/user",
              "text": "我的",
              "iconPath": "/assets/images/user.png",
              "selectedIconPath": "/assets/images/user_select.png"
            }
          ]
        },
        ...
      }
    ```
  2. 项目结构：![项目结构](/images/weapp/结构.jpg)
  3. 展示效果：![展示效果](/images/weapp/效果.jpg)
{% note info %}
  * 注意：
    - iconPath和selectedIconPath图标的路径是相对于根路径下的，在根路径下新建了assets/images文件夹， pages也是在根路径下
{% endnote %}

# 在wxss中引入全局变量
  1. 在app.wxss中申明变量，使用--开头
    ```
      /* 定义变量 */
      .container{
        --mainColor--: #ff0
      }
    ```
  2. 在各个页面的wxss中使用var关键字引入变量
    ```
      # index.wxml中：
      
      <view class="container">
        <text class="box">pages/index/index.wxml</text>
      </view>

      # index.wxss中：

      .box {
        color: var(--mainColor--)
      }
    ```
{% note info %}
  * 注意：
    - 使用变量时用var关键字包裹，格式：--变量名--: 变量值
    - 颜色不用引号，并且只能用16进制，rgba和单词(red、yellow等)都不能用
{% endnote %}

# 如何使用外部js的方法
  1. 在外部js文件(如：utils/util.js)中，写入一个方法parseDate,用module.exports暴露出去
    ```
      const parseData = () => {
        return "123"
      }

      module.exports = {
        parseData
      }
    ```

  2. 在需要调用的js文件中，通过require引入
    ```
      const utils = require("../../utils/util")

      onReady: function () {
        console.log(utils.parseData())
      },
    ```

# js中如何使用全局变量
  1. 在app.js中，定义globalData属性
    ```
      App({
        globalData: {
          user: "张三"
        }
      })
    ```

  2. 在其他页面js中使用这个user
    ```
      const app = getApp();

      onReady: function () {
        console.log(app.globalData.user)
      },
    ```

# 变量单独提取到一个文件
  1. 在根路径下新建constant/constant.js，专门用来放置全局变量
    ```
      const Constant = {
        url: "abcde"
      }
      module.exports = Constant
    ```

  2. 在需要用到的地方(js)使用
    ```
      const a = require("../../constant/constant)

      onReady: function () {
        console.log(a.url)
      },
    ```

# WXS文件的使用方法
  1. 当wxml文件中有渲染的模板数据需要做处理的时候，就会用到wxs文件
  2. 举例：在index.wxml模板中的值create_time进行时间转换（将时间戳转换为`yyyy-mm-dd`）
    ```
      # 在index.js中

        Page({
          data: {
            create_time: 1632527653186
          },
          ......
        })

      # 在index.wxml中

        <view>{{ create_time }}</view>
    ```
  3. 在index文件夹下新建index.wxs文件
    ```
      function parseDate(date) {
        var originDate = getDate(date);
        var year = originDate.getFullYear()
        var originMonth = originDate.getMonth() + 1
        var month = originMonth < 10 ? ("0" + originMonth) : originMonth
        var originDay = originDate.getDate()
        var day = originDay < 10 ? ("0" + originDay) : originDay
        return year + "-" + month + "-" + day
      }

      module.exports = {
        parseDate: parseDate
      }
    ```
  4. 修改index.wxml中的模板
    ```
      <!-- 引入wxs文件 -->
      <wxs src="./index.wxs" module="index" />
      <view>{{ index.parseDate(create_time) }}</view>
    ```

{% note info %}
  * 注意：
    - 在wxs文件中，定义变量只能用var，使用const或者let会报错
    - 也不能使用模板字符串
    - 在模板中使用wxs文件时，一定要加上module属性
    - 字符串拼接时，如果运算复杂，一定要用()包裹
      ```
        function parseDate(date) {
          var originDate = getDate(date);
          var year = originDate.getFullYear()
          var month = originDate.getMonth() + 1
          var day = originDate.getDate()
          return year + "-" + (month < 10 ? ("0" + month) : month) + "-" + (day < 10 ? ("0" + day) : day)
        }
      ```
{% endnote %}

# 小程序获取顶部状态栏的高度
  * 通过wx.getSystemInfoSync()获取设备信息
    ```
      onLaunch() {
        const res = wx.getSystemInfoSync()
        const statusBarHeight = res.statusBarHeight
      }
    ```

# 在微信小程序中引入iconfont
  1. 在iconfont官网中选择你需要的图片，添加至项目
  2. 选择font class按钮，点击下方的链接，把该css文件保存到你的项目中 ![引入iconfont](/images/weapp/iconfont.jpg)
  3. 在app.wxss中引入该文件
    ```
      @import "../../assets/style/iconfont.css"
    ```
  4. 在页面中使用
    ```
      <text class="iconfont icon-sousuo search_icon"></text>
    ```

# 小程序中使用scroll-view(横向滑动)
  * 代码详解
    ```
      # wxml部分

      <view class="cate">
        <scroll-view class="cate_scroll" scroll-x>
          <view class="cate_item" wx:for="{{dataSource.category_info}}" wx:key="cate_id">
            <view class="cate_item_box">
              <image src="{{item.img_url}}" class="cate_img" mode="widthFix" />
            <text class="cate_text">{{ item.name }}</text>
            </view>
          </view>
        </scroll-view>
      </view>

      # wxss部分

      .cate {
        width: 750rpx;
        height: 200rpx;
      }
      .cate_scroll{
        white-space: nowrap;
        height: 100%;
      }
      .cate_item{
        display: inline-block;
        margin: 0 20rpx;
        height: 100%;
      }
      .cate_item_box {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: 100%;
      }
      .cate_img {
        width: 96rpx;
        height: 96rpx;
        margin-bottom: 6rpx;
        border-radius: 18rpx;
      }
      .cate_text {
        font-size: 28rpx;
      }
    ```
  * 展示效果：![scrollview](/images/weapp/scrollview.jpg)

{% note info %}
  * 注意：
    - scrollview外面一定要包裹一层view做最外层，并且一定要指定宽高
    - scrollview里面最近的一层盒子(.cate_item)使用display:flex是没有效果的，只能设置display: inline-block
    - 要想在scrollview里面使用flex，只能在scrollview里面第二层盒子开始使用（.cate_item_box）
{% endnote %}

# 微信小程序事件传递参数
  * 以bindtap点击事件为例，bindtap的方法名不能使用括号，只能在标签上写data-xx属性，通过e.currentTarget.dataset.name获取值
    ```
      # wxml部分

      <view class="list_item_box center {{ item.tab_name === tabActiveIndex ? 'active' : '' }}" bindtap="handleSelect" data-name="{{ item.tab_name }}">

      # js部分

      data: {
        tabActiveIndex: "热销好货",
      },

      handleSelect: function(event) {
          this.setData({
              tabActiveIndex: event.currentTarget.dataset.name
          })
      }
    ```

# 小程序中如何自定义组件
  1. 在根路径下新建components文件夹，写入对应的组件
  2. 找到需要使用该组件的文件夹下面的json文件，配置usingComponents
    ```
      {
        "usingComponents": {
          "statusBar": "/components/statusBar/statusBar"
        }
      }
    ```
  3. 在wxml中使用`<statusBar />`即可

# 小程序组件传值
## 父传子
  * 和vue完全一样
    ```
      # 父组件 将list传递给子组件

      <goods-list 
        list="{{ dataSource.recommend_info[tabActiveIndex].tab_list }}" 
        wx:if="{{dataSource.recommend_info[tabActiveIndex].tab_list}}"
        bind:setCart="handleAddCart"
      />

      # 子组件

      <view class="tp-list verticalCenter" wx:for="{{ list }}" wx:key="index"></view>
    ```

## 子传父
  * 使用this.triggerEvent方法， 父组件先传递一个setCart属性
    ```
      # 子组件

        <button class="cart center" bindtap="addCart" data-id="{{ item.goods_id }}" wx:if="{{ item.cart_num === 0 }}">加入购物车</button>

        addCart: function(event) {
            console.log("event", event.currentTarget.dataset.id)
            this.triggerEvent("setCart", event.currentTarget.dataset.id)
        }

      # 父组件
      
        <goods-list 
          list="{{ dataSource.recommend_info[tabActiveIndex].tab_list }}" 
          wx:if="{{dataSource.recommend_info[tabActiveIndex].tab_list}}"
          bind:setCart="handleAddCart"
        />
    ```

# 小程序解决sitemap警告提示
  * `[sitemap 索引情况提示] 根据 sitemap 的规则[0]，当前页面 [pages/index/index] 将被索引`
  * 解决办法：修改project.config.json,将checkSiteMap改为false

# 小程序获取dom的高度
  ```
    onLoad: function () {
        const query = wx.createSelectorQuery()
        const that = this
        query.selectAll('.header').boundingClientRect(function (obj) {
            console.log("obj", obj);
            console.log(obj[0].height + 'px')
            that.setData({
                contentHeight: "calc(100% - " + obj[0].height + "px - 80rpx)"
            })
        }).exec();
    },
  ```