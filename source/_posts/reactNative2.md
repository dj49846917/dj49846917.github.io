---
title: react-native开发总结——react-navigation
date: 2020-06-26 11:37:45
tags: 
  - react-native
  - react-navigation
  - 适配
type: hexo                                                                         # 标签、分类和友情链接三個页面需要配置(必填)
description: 快速、简洁且高效的博客框架                                                            # 描述
keywords: hexo的搭建                                                                       # 关键词，便于搜索
top_img: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1593147695326&di=534e080eedb76452ae129c102cd04798&imgtype=0&src=http%3A%2F%2Fimg.mp.itc.cn%2Fq_mini%2Cc_zoom%2Cw_640%2Fupload%2F20161228%2Fb2da1d0b3e1247d9b570eea5a531fa52_th.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 教程
  - react-native                                                                 # 文章标签
  - react-navigation
  - 适配
cover: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1593147695326&di=534e080eedb76452ae129c102cd04798&imgtype=0&src=http%3A%2F%2Fimg.mp.itc.cn%2Fq_mini%2Cc_zoom%2Cw_640%2Fupload%2F20161228%2Fb2da1d0b3e1247d9b570eea5a531fa52_th.jpg                 # 文章的缩略图（用在首页）
---

# 介绍
> react-navigation 路由导航(用于页面跳转, 4.x的版本)
> 包括：
> createStackNavigator：页面跳转的路由导航
> createSwitchNavigator：底部tabbar
> createDrawerNavigator：抽屉侧边栏导航

# createStackNavigator：页面跳转的路由导航

## 使用步骤
### 核心包：react-navigation
  ```
    npm install react-navigation
  ```

### 依赖项目：
  ```
    npm install react-native-reanimated 
                react-native-gesture-handler 
                react-native-screens 
                react-native-safe-area-context 
                @react-native-community/masked-view
  ```

### 使用createStackNavigator 基础导航器 前置安装
  ```
    npm install react-navigation-stack 
                @react-native-community/masked-view
  ```

### 使用createStackNavigator 具体
  1. 导入 createAppContainer createStackNavigator
  2. 定义导航
  3. createAppContainer 函数对createStackNavigator 进行包裹
  4. 导出createAppContainer 创建的组件 作为应用程序的根组件
  5. [详细代码请看:https://github.com/dj49846917/react-native-study/blob/master/docs/example/%E8%B7%AF%E7%94%B1%E8%B7%B3%E8%BD%AC/createStackNavigator/App.js](https://github.com/dj49846917/react-native-study/blob/master/docs/example/%E8%B7%AF%E7%94%B1%E8%B7%B3%E8%BD%AC/createStackNavigator/App.js)
  6. 展示效果: ![展示效果](/images/reactNative/images/普通导航.gif)


{% note warning %}
注意：
  可以使用headerMode: 'none'关闭默认导航栏

  参数传递是放到navigate的第二个参数里，获取参数通过navigate.state

  ```
    传递参数
    onPress={() => {
      const params = {
        name: '张三',
        age: 16,
      };
      navigation.navigate('Home', params);
    }}

    获取参数
    const {navigation} = this.props;
    console.log(navigation.state.params);
  ```
{% endnote %}

***

# createMaterialTopTabNavigator: 顶部选项卡

## 使用步骤
### 核心包：react-navigation
  ```
    npm install react-navigation
  ```

### 依赖项目：
  ```
    npm install react-native-reanimated 
                react-native-gesture-handler 
                react-native-screens 
                react-native-safe-area-context 
                @react-native-community/masked-view
  ```

### 使用createMaterialTopTabNavigator 顶部选项卡 前置安装
  ```
    npm install --save react-navigation-tabs
  ```

### 使用createMaterialTopTabNavigator 具体
  1. 导入 createAppContainer createMaterialTopTabNavigator
  2. 定义导航
  3. createAppContainer 函数对createMaterialTopTabNavigator 进行包裹
  4. 导出createAppContainer 创建的组件 作为应用程序的根组件
  5. [详细代码请看:https://github.com/dj49846917/react-native-study/blob/master/docs/example/%E8%B7%AF%E7%94%B1%E8%B7%B3%E8%BD%AC/createMaterialTopTabNavigator/App.js](https://github.com/dj49846917/react-native-study/blob/master/docs/example/%E8%B7%AF%E7%94%B1%E8%B7%B3%E8%BD%AC/createMaterialTopTabNavigator/App.js)
  6. 展示效果: ![展示效果](/images/reactNative/images/顶部导航栏效果.jpg)
***

# createBottomTabNavigator: 底部导航栏

## 使用步骤
### 核心包：react-navigation
  ```
    npm install react-navigation
  ```

### 依赖项目：
  ```
    npm install react-native-reanimated 
                react-native-gesture-handler 
                react-native-screens 
                react-native-safe-area-context 
                @react-native-community/masked-view
  ```

### 使用createBottomTabNavigator 顶部选项卡 前置安装
  ```
    npm install --save react-navigation-tabs
  ```

### 使用createBottomTabNavigator 具体
  1. 导入 createAppContainer createBottomTabNavigator
  2. 定义导航
  3. createAppContainer 函数对createBottomTabNavigator 进行包裹
  4. 导出createAppContainer 创建的组件 作为应用程序的根组件
  5. [详细代码请看:https://github.com/dj49846917/react-native-study/blob/master/docs/example/%E8%B7%AF%E7%94%B1%E8%B7%B3%E8%BD%AC/createBottomTabNavigator/App.js](https://github.com/dj49846917/react-native-study/blob/master/docs/example/%E8%B7%AF%E7%94%B1%E8%B7%B3%E8%BD%AC/createBottomTabNavigator/App.js)
  6. 展示效果: ![展示效果](/images/reactNative/images/底部导航栏效果.gif)