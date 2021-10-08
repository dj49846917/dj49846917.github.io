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
  * 防抖：
    ```
      //防抖debounce代码：
      function debounce(fn,delay) {
          var timeout = null; // 创建一个标记用来存放定时器的返回值
          return function (e) {
              // 每当用户输入的时候把前一个 setTimeout clear 掉
              clearTimeout(timeout); 
              // 然后又创建一个新的 setTimeout, 这样就能保证interval 间隔内如果时间持续触发，就不会执行 fn 函数
              timeout = setTimeout(() => {
                  fn.apply(this, arguments);
              }, delay);
          };
      }
      // 处理函数
      function handle() {
          console.log('防抖：', Math.random());
      }
              
      //滚动事件
      window.addEventListener('scroll', debounce(handle,500));
    ```
  * 节流：
    ```
      //节流throttle代码：
      function throttle(fn,delay) {
          let canRun = true; // 通过闭包保存一个标记
          return function () {
              // 在函数开头判断标记是否为true，不为true则return
              if (!canRun) return;
              // 立即设置为false
              canRun = false;
              // 将外部传入的函数的执行放在setTimeout中
              setTimeout(() => { 
              // 最后在setTimeout执行完毕后再把标记设置为true(关键)表示可以执行下一次循环了。
              // 当定时器没有执行的时候标记永远是false，在开头被return掉
                  fn.apply(this, arguments);
                  canRun = true;
              }, delay);
          };
      }
      
      function sayHi(e) {
          console.log('节流：', e.target.innerWidth, e.target.innerHeight);
      }
      window.addEventListener('resize', throttle(sayHi,500));
    ```
---

# 写一个 es6 的继承过程
---

# 手写继承
---

# 手写深浅拷贝
---

# 数组去重；(所有方法)
---

# 如何手写实现一个 Promise？
---

# 编程题：实现promiseAll
---

# 手写实现 jsonp
---

# 手写快排，时间复杂度，优化
---

# 快排的原理
---

# 手写冒泡排序。快速排序
  * 冒泡：
    ```
      var arr = [12,3,44,343,55,1,23];
      for(var i=0;i<arr.length-1;i++){
          for(var j=0;j<arr.length-i-1;j++){
              if(arr[j]>arr[j+1]){
                  var current = arr[j];
                  arr[j] = arr[j+1];
                  arr[j+1] = current;
              }
          }
      }
      console.log(arr);
    ```
  * 快排：
    ```
      /*
      * 利用递归函数实现排序
      * 每次获取数组中间元素cItem
      * 把大于和小于cItem的元素分别放置在arrGt和arrLt两个数组中,
      * 利用concat组合递归调用函数返回的值
      * 直到数组的长度等于1时，直接返回元素调出递归
      */
      var arr = [10, 8, 20, 5, 6, 30, 11, 9]；
      function fastSort(arr){
          //6. 递归退出条件
          if(arr.length<=1){
            return arr;
          }
          //1. 找出数组中间位置元素
          var cIdx = parseInt(arr.length/2);

          //2.删除中间元素，避免与自己本身进行对比而造成死循环
          var cItem = arr.splice(cIdx,1);//[6],arr=[10, 8, 20, 5, 30, 11, 9]

          //3. 创建两个空数组用于保存大于或小于cItem的数字
          var arrLt = [];//[5]
          var arrGt = [];//[10,8,20,30,11,9]

          // 4.遍历数组，分别与cItem进行对比
          // 把小于cItem的数写入arrLt
          // 把大于cItem的数写入arrGt
          for(var i=0;i<arr.length;i++){
              if(arr[i]<cItem[0]){
                arrLt.push(arr[i])
              }else{
                arrGt.push(arr[i]);
              }
          }
          // 5.组合排序后的数组
          return fastSort([5]).concat(cItem,fastSort(arrGt));
      }
      console.log(fastSort(arr));
    ```
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

# 如果让你去实现一个 symbol 你会怎么实现
---

# 计算两个数组的交集和并集。
---

# 手写bind
---

# 数组反转
---

# Array的map、filter、reduce挑一个手写实现
---

# 手写ajax
  * 要求：
    1. 封装promise形式
    2. 根据封装后的promise形式写出扩展函数
      - 请求成功则返回值
      - 不成功到达3次则取消请求

# 手写axios（用promise封装ajax）,只考虑get,post请求
---

# 手写JSON.stringify(),传参类型为数组或对象，返回一个JSON字符串
---

# 手写一个函数实现const的功能
---

# 获取一个字母数组混合的字符串中最大的数字
```
    function solution(arr){
      let max=-1;
      let tmp=0;
      for(let i = 0;i<arr.length;i++){
          if(arr[i]>='0'&&arr[i]<='9'){
            let cur = parseInt(arr[i]);
            tmp*=10;
            tmp+=cur;
            if(max<tmp){
              max=tmp;
            }                  
          }else{
            tmp=0;
          }
      }
      return max;
  }
  console.log(solution("055e" ))
```
---

# 手写实现instance
```
  function myInstanceOf(left, right) {
    let prototype = right.prototype;
    left = left.proto;
    while(true) {
      if(!left) {
        return false;
      }
      if(left === prototype) {
        return true;
      }
      left = left.proto;
    }
  }
```

# 编程题：实现trim
---

# 实现一个once函数，传入函数参数只执行一次
```
  function ones(func){
      var tag=true;
      return function(){
          if(tag==true){
              func.apply(null,arguments);
              tag=false;
          }
          return undefined
      }
  }
```
---