---
title: 面试——笔试篇
date: 2020-12-30 15:51:46
tags: 
  - 面试
  - 笔试
type: 面试                                                                 # 标签、分类
description:  JavaScript（简称“JS”） 是一种具有函数优先的轻量级，解释型或即时编译型的编程语言。
top_img: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=955487690,3458128037&fm=26&gp=0.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 面试
  - 笔试                                                                 # 文章标签
cover: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=955487690,3458128037&fm=26&gp=0.jpg                 # 文章的缩略图（用在首页）
---

# 手写函数防抖和节流
---

# 写一个 es6 的继承过程
---

# 手写深浅拷贝
---

# 数组去重；(所有方法)
---

# 如何手写实现一个 Promise？
---

# 手写实现 jsonp
---

# 手写快排，时间复杂度，优化
---

# 手写冒泡排序。快速排序
---

# 实现一个事件发布订阅类，其实就是 eventEmitter
---

# 手写 vue 的 mixin 方法
---

# 手写 promise 的 all 方法
---

# 手写 vue 双向绑定；
---

# 写一个方法找出字符串中出现次数最多的字母
  ```
    function fn(str) {
      var arr = str.split("");
      var result = {};
      var char = "";
      var max = 0;
      for (var i = 0; i < arr.length; i++) {
        if (!result[arr[i]]) {
          result[arr[i]] = 1;
        } else {
          result[arr[i]]++;
        }
      }
      for (let item in result) {
        if (result[item] > max) {
          char = item;
          max = result[item];
        }
      }
      return char;
    }
  ```
---

# 写一个方法获取URL中的查询字符串的参数
---

# vue原理, 手写实现数据劫持
---

# 实现洋葱模型
---

# 实现一个中间件
---

# Nodejs实现一个从大JSON文件取到对应值的方法
---

# 实现一个视频对话网页
---

# 手写算法 遍历一个树，找出树的叶子节点上某一个值，找到后返回查找的路径
---

# 手写算法 求一个数的平方根，需考虑无限除不尽的小数以及精度问题
---

# 手写题 写一个数组的原型函数，返回出现次数大于输入n的数组项
---

# 写出遍历二叉树，深度遍历与广度遍历
---

# 给你一个数组，数组的长度为n, 请找出数组中第k大的数
![算法](/images/math/算法.jpg)
---

# 手写 new
---

# 手写 apply
---

# 手写代码，两个有序数组的合并
---
