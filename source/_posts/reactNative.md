---
title: react-native开发总结
date: 2020-06-26 09:59:45
tags: 
  - react-native
  - 适配
type: hexo                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: 快速、简洁且高效的博客框架                                                            # 描述
keywords: hexo的搭建                                                                       # 关键词，便于搜索
top_img: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1593147695326&di=534e080eedb76452ae129c102cd04798&imgtype=0&src=http%3A%2F%2Fimg.mp.itc.cn%2Fq_mini%2Cc_zoom%2Cw_640%2Fupload%2F20161228%2Fb2da1d0b3e1247d9b570eea5a531fa52_th.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - react-native                                                                 # 文章标签
  - 适配
cover: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1593147695326&di=534e080eedb76452ae129c102cd04798&imgtype=0&src=http%3A%2F%2Fimg.mp.itc.cn%2Fq_mini%2Cc_zoom%2Cw_640%2Fupload%2F20161228%2Fb2da1d0b3e1247d9b570eea5a531fa52_th.jpg                 # 文章的缩略图（用在首页）
---

# 开发环境的搭建
## 下载jdk
  * 下载地址: https://www.oracle.com/java/technologies/javase-downloads.html
  * ![jdk的安装与配置](/images/reactNative/images/jdk安装及配置.jpg)
  {% note primary %}
    注意：推荐下载jdk1.8及以上的版本，下载之后配置环境变量
  {% endnote %}

## 下载android studo
  * 下载地址: https://developer.android.google.cn/studio/
  {% note primary %}
    android studo推荐使用3.5以上的版本，然后下载android sdk，选择8.1以上的版本
  {% endnote %}
  * ![android studio和android sdk下载安装](/images/reactNative/images/androidstudio环境搭建.jpg)
  * ![安卓环境变量](/images/reactNative/images/android环境变量.jpg)
  
## 创建react-native项目
  * 输入命令：npx react-native init myproject

***

# 分辨率的适配， Dimensions
  * 详细代码如下：
  ```
      import { Dimensions, View } from 'react-native'

      // 适配方案
      export const sumPx = {
        dpi: (w) => { // 自适应px, 750为设计稿的宽度
          return width / 750 * w
        },
        h: height,
        w: width
      }

      使用方法
      <View style={{width: sumPx.dpi(300), height: sumPx.dpi(300), backgroundColor: 'red'}}></View>
  ```
  * [详细代码请看:https://github.com/dj49846917/react-native-study/blob/master/docs/example/%E9%80%82%E9%85%8D/UnitConvert.js](https://github.com/dj49846917/react-native-study/blob/master/docs/example/%E9%80%82%E9%85%8D/UnitConvert.js)

# 代码调试
  * 输入命令： adb shell input keyevent 82， 唤起调试菜单
    
  * 唤起之后，选择debug，会弹出谷歌浏览器，然后console里面就能打印你代码中的console.log
    ![代码调试](/images/reactNative/images/代码调试.jpg)
    
  * 取消debug只需要再次唤起菜单，点击stop
    ![取消代码调试](/images/reactNative/images/取消代码调试.jpg)

# 获取导航栏的高度
  * 要先下载react-navigation

```
  import { Header } from 'react-navigation';

  const navHeight = Header.HEIGHT;
  console.log(navHeight)
```
