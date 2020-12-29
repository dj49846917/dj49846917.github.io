---
title: 面试——js篇
date: 2020-12-28 15:24:57
tags: 
  - 面试
  - js
type: 面试                                                                 # 标签、分类
description:  JavaScript（简称“JS”） 是一种具有函数优先的轻量级，解释型或即时编译型的编程语言。
top_img: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=955487690,3458128037&fm=26&gp=0.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 面试
  - js                                                                 # 文章标签
cover: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=955487690,3458128037&fm=26&gp=0.jpg                 # 文章的缩略图（用在首页）
---

# import 和 require 导入的区别
  * require 是 AMD规范引入方式；import是es6的一个语法标准，如果要兼容浏览器的话必须转化成es5的语法
  * require是运行时调用，所以require理论上可以运用在代码的任何地方；import是编译时调用，所以必须放在文件开头
  * 本质上，require是赋值过程，其实require的结果就是对象、数字、字符串、函数等，再把require的结果赋值给某个变量；而import是解构过程，但是目前所有的引擎都还没有实现import，我们在node中使用babel支持ES6，也仅仅是将ES6转码为ES5再执行，import语***被转码为require；
---

# 介绍js的基本数据类型。
  * 答: Undefined、Null、Boolean、Number、String、Symbol(创建后独一无二且不可变的数据类型 )
---

# JavaScript有几种类型的值？有什么区别？
  * 类型
    1. 栈：原始数据类型（Undefined，Null，Boolean，Number、String）
    2. 堆：引用数据类型（对象、数组和函数）

  * 区别
    1. 两种类型的区别是：存储位置不同；
    2. 原始数据类型直接存储在栈(stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；
    3. 引用数据类型存储在堆(heap)中的对象,占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体

# 介绍js有哪些内置对象？
  * Object 是 JavaScript 中所有对象的父对象
  * 数据封装类对象：Object、Array、Boolean、Number 和 String
  * 其他对象：Function、Arguments、Math、Date、RegExp、Error
---

# JavaScript原型，原型链 ? 有什么特点？
  * 原型: 每个对象都会在其内部初始化一个属性，就是prototype(原型)
  * 当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么他就会去prototype里找这个属性，这个prototype又会有自己的prototype，于是就这样一直找下去，也就是我们平时所说的原型链的概念。
  * 关系：instance.constructor.prototype = instance.__proto__
  * 特点：JavaScript对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。
---

# JavaScript继承的几种实现方式？
  1. 原型链继承
    * 优点：
      1. 父类新增原型方法/原型属性，子类都能访问到
      2. 父类新增原型方法/原型属性，子类都能访问到

    * 缺点：
      1. 无法实现多继承
      2. 来自原型对象的所有属性被所有实例共享
      3. 创建子类实例时，无法向父类构造函数传参
      4. 要想为子类新增属性和方法，必须要在Student.prototype = new Person() 之后执行，不能放到构造器中

    * 示例：
      ```
        //父类型
        function Person(name, age) {
          this.name = name,
          this.age = age,
          this.play = [1, 2, 3]
          this.setName = function () {}
        }
        Person.prototype.setAge = function () {}
        //子类型
        function Student(price) {
          this.price = price
          this.setScore = function () {}
        }
        Student.prototype = new Person() // 子类型的原型为父类型的一个实例对象
        var s1 = new Student(15000)
        var s2 = new Student(14000)
        console.log(s1,s2)
      ```

  2. 借用构造函数继承
    * 优点:
      1. 解决了原型链继承中子类实例共享父类引用属性的问题
      2. 创建子类实例时，可以向父类传递参数
      3. 可以实现多继承(call多个父类对象)

	  * 缺点:
      1. 实例并不是父类的实例，只是子类的实例
      2. 只能继承父类的实例属性和方法，不能继承原型属性和方法
      3. 无法实现函数复用，每个子类都有父类实例函数的副本，影响性能

    * 示例：
      ```
        function Person(name, age) {
          this.name = name,
          this.age = age,
          this.setName = function () {}
        }
        Person.prototype.setAge = function () {}
        function Student(name, age, price) {
          Person.call(this, name, age)  // 相当于: this.Person(name, age)
          /*this.name = name
          this.age = age*/
          this.price = price
        }
        var s1 = new Student('Tom', 20, 15000)
      ```

  3. 组合继承(原型+借用构造)
    * 优点:
      1. 可以继承实例属性/方法，也可以继承原型属性/方法
      2. 不存在引用属性共享问题
      3. 可传参
      4. 函数可复用
  
    * 缺点: 完美的，没有缺点。

    * 示例：
      ```
        function Person (name, age) {
          this.name = name,
          this.age = age,
          this.setAge = function () { }
        }
        Person.prototype.setAge = function () {
          console.log("111")
        }
        function Student (name, age, price) {
          Person.call(this, name, age)
          this.price = price
          this.setScore = function () { }
        }
        Student.prototype = new Person()
        Student.prototype.constructor = Student//组合继承也是需要修复构造函数指向的
        Student.prototype.sayHello = function () { }
        var s1 = new Student('Tom', 20, 15000)
        var s2 = new Student('Jack', 22, 14000)
        console.log(s1)
        console.log(s1.constructor) //Student
        console.log(s2.constructor) //Person  
      ```

  4. 原型式继承：就是 ES5 Object.create 的模拟实现，将传入的对象作为创建的对象的原型。    
    * 缺点: 包含引用类型的属性值始终都会共享相应的值，这点跟原型链继承一样。
    
    * 示例：
      ```
        function createObj(o) {
          function F(){}
          F.prototype = o;
          return new F();
        }
      ```

  5. 寄生式继承
    * 缺点：跟借用构造函数模式一样，每次创建对象都会创建一遍方法。
    
    * 示例：
      ```
        function createObj (o) {
          var clone = Object.create(o);
          clone.sayName = function () {
              console.log('hi');
          }
          return clone;
        }
      ```

  6. 寄生组合式继承
    * 缺点：组合继承最大的缺点是会调用两次父构造函数。
    
    * 示例：
      ```
        function Parent (name) {
          this.name = name;
          this.colors = ['red', 'blue', 'green'];
        }

        Parent.prototype.getName = function () {
          console.log(this.name)
        }

        function Child (name, age) {
          Parent.call(this, name);
          this.age = age;
        }

        Child.prototype = new Parent();

        var child1 = new Child('kevin', '18');
        console.log(child1)
      ```
---

# 什么是Javascript作用域链?
  * 全局函数无法查看局部函数的内部细节，但局部函数可以查看其上层的函数细节，直至全局细节。
  * 当需要从局部函数查找某一属性或方法时，如果当前作用域没有找到，就会上溯到上层作用域查找，直至全局函数，这种组织形式就是作用域链。
---

# 谈谈你对作用域，作用域链的理解
  * 作用域：
    1. 一个变量的作用域（scope）是程序源代码中定义的这个变量的区域。
    2. 不在任何函数内声明的变量（函数内省略var的也算全局）称作全局变量（global scope）
    3. 在函数内声明的变量具有函数作用域（function scope），属于局部变量
  * 作用域同上
---

# 谈谈This对象的理解。
  * this总是指向函数的直接调用者（而非间接调用者）；
  * 如果有new关键字，this指向new出来的那个对象；
  * 在事件中，this指向触发这个事件的对象，特殊的是，IE中的attachEvent中的this总是指向全局对象Window；
  * 如果apply、call或bind方法用于调用、创建一个函数，函数内的 this 就是作为参数传入这些方法的对象。
  * 如果该函数是箭头函数，this被设置为它被创建时的上下文。
---

# null，undefined 的区别？
  ```
    答:
        <1>.null 	表示一个对象是“没有值”的值，也就是值为“空”；

            undefined 	表示一个变量声明了没有初始化(赋值)；

        <2>.undefined不是一个有效的JSON，而null是；

            undefined的类型(typeof)是undefined；

            null的类型(typeof)是object；

        <3>.Javascript将未赋值的变量默认值设为undefined；

            Javascript从来不会将变量设为null。它是用来让程序员表明某个用var声明的变量时没有值的。

        <4>.typeof undefined
            //"undefined"
            undefined :是一个表示"无"的原始值或者说表示"缺少值"，就是此处应该有一个值，但是还没有定义。当尝试读取时会返回 undefined；
            例如变量被声明了，但没有赋值时，就等于undefined

            typeof null
            //"object"
            null : 是一个对象(空对象, 没有任何属性和方法)；
            例如作为函数的参数，表示该函数的参数不是对象；

        <5>.注意：
            在验证null时，一定要使用　=== ，因为 == 无法分别 null 和　undefined
            null == undefined // true
            null === undefined // false
  ```
---

# 事件是？IE与火狐的事件机制有什么区别？ 如何阻止冒泡？Javascript的事件流
  1. 事件是我们在网页中的某个操作（有的操作对应多个事件）。例如：当我们点击一个按钮就会产生一个事件。是可以被 JavaScript 侦测到的行为。
  2. 事件处理机制：IE是事件冒泡、Firefox同时支持两种事件模型，也就是：捕获型事件和冒泡型事件；
  3.  ev.stopPropagation();（旧ie的方法 ev.cancelBubble = true;）
  4.  事件流:  三个阶段：事件捕捉，目标阶段，事件冒泡
---

# 什么是闭包（closure）, 特点是什么? 有什么优缺点? 有什么作用?
  * 概念：闭包是指有权访问另一个函数作用域中变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量,利用闭包可以突破作用链域，将函数内部的变量和方法传递到外部。
  
  * 特点：
    1. 函数内再嵌套函数
    2. 内部函数可以引用外层的参数和变量
    3. 参数和变量不会被垃圾回收机制回收

  * 优点: 全局变量,可重用, 局部变量: 仅函数内可用，不会被污染。
  * 缺点：
    1. 比普通函数占用更多的内存。 解决：闭包不在使用时，要及时释放。
    2. 将引用内层函数对象的变量赋值为null

  * 作用：
    1. 读取函数内部的变量
    2. 让这些变量的值始终保持在内存中，不会在函数调用后被自动清除
---

# javascript 代码中的"use strict";是什么意思 ? 使用它有什么优缺点？
  * 概念: use strict是一种ECMAscript 5 添加的（严格）运行模式,这种模式使得 Javascript 在更严格的条件下运行

  * 优点:
    1. 无法再意外创建全局变量。
    2. 会使引起静默失败（silently fail，即：不报错也没有任何效果）的赋值操抛出异常。
    3. 试图删除不可删除的属性时会抛出异常（之前这种操作不会产生任何效果）。
    4. 要求函数的参数名唯一。
    5. 全局作用域下，this的值为undefined。
    6. 捕获了一些常见的编码错误，并抛出异常。
    7. 禁用令人困惑或欠佳的功能。

  * 缺点：
    1. 缺失许多开发人员已经习惯的功能。
    2. 无法访问function.caller和function.arguments。
    3. 以不同严格模式编写的脚本合并后可能导致问题。
---

# 如何判断一个对象是否属于某个类？
  * 使用instanceof 
---

# new操作符具体干了什么呢?
  * 创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。
  * 属性和方法被加入到 this 引用的对象中。
  * 新创建的对象由 this 所引用，并且最后隐式的返回 this。
---

# js延迟加载的方式有哪些？
  * defer和async、动态创建DOM方式（用得最多）、按需异步载入js

# Ajax 是什么? 如何创建一个Ajax？
  * 概念：
    ```
      ajax的全称：Asynchronous Javascript And XML。
      异步传输+js+xml。
      所谓异步，在这里简单地解释就是：向服务器发送请求的时候，我们不必等待结果，而是可以同时做其他的事情，等到有了结果它自己会根据设定进行后续操作，与此同时，页面是不会发生整页刷新的，提高了用户体验。
    ```

  * 创建：
    1. 创建XMLHttpRequest对象,也就是创建一个异步调用对象
    2. 创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息
    3. 设置响应HTTP请求状态变化的函数
    4. 发送HTTP请求
    5. 获取异步调用返回的数据
    6. 使用JavaScript和DOM实现局部刷新
   
# Ajax 解决浏览器缓存问题？
  * 在ajax发送请求前加上 anyAjaxObj.setRequestHeader("If-Modified-Since","0")。
  * 在ajax发送请求前加上 anyAjaxObj.setRequestHeader("Cache-Control","no-cache")。
  * 在URL后面加上一个随机数： "fresh=" + Math.random();。
  * 在URL后面加上时间戳："nowtime=" + new Date().getTime();。
  * 如果是使用jQuery，直接这样就可以了 $.ajaxSetup({cache:false})。这样页面的所有ajax都会执行这条语句就是不需要保存缓存记录。


