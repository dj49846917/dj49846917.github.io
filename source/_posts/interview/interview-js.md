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

# ES6 module与require的区别
---

# 介绍js的基本数据类型。
  * 答: Undefined、Null、Boolean、Number、String、Symbol(创建后独一无二且不可变的数据类型 )
---

# JS传值、传引用的区别？
---

# 介绍下 symbol, symbol作用
---

# Symbol主要用于什么场景下
---

# JavaScript有几种类型的值？有什么区别？(堆和栈的概念)
  * 类型
    1. 栈：原始数据类型（Undefined，Null，Boolean，Number、String）
    2. 堆：引用数据类型（对象、数组和函数）

  * 区别
    1. 两种类型的区别是：存储位置不同；
    2. 原始数据类型直接存储在栈(stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；
    3. 引用数据类型存储在堆(heap)中的对象,占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体
---

# js栈怎么实现的
---

# js队列怎么实现的
---

# 介绍js有哪些内置对象？
  * Object 是 JavaScript 中所有对象的父对象
  * 数据封装类对象：Object、Array、Boolean、Number 和 String
  * 其他对象：Function、Arguments、Math、Date、RegExp、Error
---

# 如何判断Array类型
---

# 何如判断对象类型
---

# Array都有什么方法
---

# JavaScript原型，原型链 ? 有什么特点？
  * 原型: 每个对象都会在其内部初始化一个属性，就是prototype(原型)
  * 当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么他就会去prototype里找这个属性，这个prototype又会有自己的prototype，于是就这样一直找下去，也就是我们平时所说的原型链的概念。
  * 关系：instance.constructor.prototype = instance.__proto__
  * 特点：JavaScript对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。
---

# 原型链顶端是什么
---

# 怎么判断某个属性是自身的还是原型链上的
---

# 隐式原型，显式原型有什么用
---

# 隐式类型转换有哪几种
---

# 介绍一下JavaScript的执行上下文
---

# prototype和__proto__属性？原型对象如何指向自己的实例化对象？
---

# 说说你对原型，构造函数，实例的理解, 他们之间的关系是什么？
  * 原型:
    - 一个简单的对象，用于实现对象的 属性继承。每个JavaScript对象中都包含一个__proto__的属性指向原型
  * 构造函数: 
    - 可以通过new来 新建一个对象 的函数。
  * 实例:
    - 通过构造函数和new创建出来的对象，便是实例。 实例通过__proto__指向原型，通过constructor指向构造函数。
  * 举例：
    ```
      	举例:
        // 实例
        const instance = new Object() 

        则此时， 实例为instance, 构造函数为Object，我们知道，构造函数拥有一个prototype的属性指向原型，因此原型为:

        // 原型
        const prototype = Object.prototype
    ```

  * 三者的关系:
    1. 实例.__proto__ === 原型
    2. 原型.constructor === 构造函数
    3. 构造函数.prototype === 原型
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

# ES5 的继承和 ES6 的继承有什么区别 ？
  * ES5 的继承时通过 prototype 或构造函数机制来实现。
    - ES5 的继承实质上是先创建子类的实例对象，然后再将父类的方法添加到 this 上（Parent.apply(this)）。

  * ES6 的继承机制完全不同，实质上是先创建父类的实例对象 this（所以必须先调用父类的 super()方法），然后再用子类的构造函数修改this。
    - 具体的：ES6 通过 class 关键字定义类，里面有构造方法，类之间通过 extends 关键字实现继承。子类必须在 constructor 方法中调用 super 方法，否则新建实例报错。因为子类没有自己的 this 对象，而是继承了父类的 this 对象，然后对其进行加工。如果不调用 super 方法，子类得不到 this 对象。
---

# 请解释原型继承（prototypal inheritance）的工作原理， 如何避免原型链上面的对象共享
  * 所有 JS 对象都有一个prototype属性，指向它的原型对象。当试图访问一个对象的属性时，如果没有在该对象上找到，它还会搜寻该对象的原型，以及该对象的原型的原型，依次层层向上搜索，直到找到一个名字匹配的属性或到达原型链的末尾

  * 如何避免: 避免对象共享可以参考经典的extend()函数，很多前端框架都有封装的，就是用一个空函数当做中间变量
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

# 浏览器事件机制
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


# 哪些操作会造成内存泄漏？
  * 概念: 内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。

  * setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）
---

# 如何避免
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

# new操作符具体干了什么呢?(new 和 instanceof 的内部机制)
  * 创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。
  * 属性和方法被加入到 this 引用的对象中。
  * 新创建的对象由 this 所引用，并且最后隐式的返回 this。
---

# 什么是异步加载和延迟加载？有什么区别?(简述异步加载的几种方式)
  * 异步加载的方案： 
    1. 动态插入script标签
    2. 通过ajax去获取js代码，然后通过eval执行
    3. script标签上添加defer或者async属性
    4. 创建并插入iframe，让它异步执行js

  * 延迟加载：有些 js 代码并不是页面初始化的时候就立刻需要的，而稍后的某些情况才需要的。
---

# js里实现异步有哪几种方式
---

# js延迟加载的方式有哪些？
  * defer和async、动态创建DOM方式（用得最多）、按需异步载入js
---

# defer 和 async 的区别
  1. script 没有 defer 和 async
    - 会停止（阻塞）dom 树构建，立即加载，并执行脚本
  2. script 带 async
    - 不会停止（阻塞）dom 树构建，立即异步加载，加载好后立即执行
  3. script 带 defer
    - 不会停止（阻塞）dom 树构建，立即异步加载。加载好后，如果 dom 树还没构建好，则先等 dom 树解析好再执行；如果 dom 树已经准备好，则立即执行。
---

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
---

# Ajax的工作原理
  * Ajax的工作原理相当于在用户和服务器之间加了—个中间层(AJAX引擎)，使用户操作与服务器响应异步化。并不是所有的用户请求都提交给服务器。像—些数据验证和数据处理等都交给Ajax引擎自己来做,，只有确定需要从服务器读取新数据时再由Ajax引擎代为向服务器提交请求。
---

# ajax请求和浏览器常规请求的区别
---
   
# Ajax 解决浏览器缓存问题？
  * 在ajax发送请求前加上 anyAjaxObj.setRequestHeader("If-Modified-Since","0")。
  * 在ajax发送请求前加上 anyAjaxObj.setRequestHeader("Cache-Control","no-cache")。
  * 在URL后面加上一个随机数： "fresh=" + Math.random();。
  * 在URL后面加上时间戳："nowtime=" + new Date().getTime();。
  * 如果是使用jQuery，直接这样就可以了 $.ajaxSetup({cache:false})。这样页面的所有ajax都会执行这条语句就是不需要保存缓存记录。
---

# 同步和异步的区别?
  * 同步阻塞，异步不阻塞
---

# 如何解决跨域问题?
  * 通过jsonp跨域
  * document.domain + iframe跨域
  * location.hash + iframe
  * window.name + iframe跨域
  * postMessage跨域
  * 跨域资源共享（CORS）
  * nginx代理跨域
  * nodejs中间件代理跨域
  * WebSocket协议跨域
  * 详细代码请看:  https://segmentfault.com/a/1190000011145364
---

# nginx跨域原理
---

# proxy解决跨域的原理？
---

# 跨域时用cors 有的时候会多发一个option请求是为什么？
---

# 使用Access-Control-Allow-Origin为什么可以解决跨域问题(流程)
---

# ajax 跨域有哪些方法，jsonp 的原理是什么，如果页面编码和被请求的资源编码不一致如何处理？
---

# 对 CORS 有了解吗?具体是怎么实现跨域的? 需要后端去设置哪些值了解吗
---

# CORS 是如何做的？
    服务端设置 Access-Control-Allow-Origin 就可以开启 CORS。
---

# cors原理
---

# CORS通常完成一个请求需要请求多少次服务器？
---

# 什么是模块化？怎么实现? 对前端模块化的认识
  * 概念: 模块化就是讲js文件按照功能分离，根据需求引入不同的文件中。
  * 实现: 就是将一段js、html、css组合在一起，形成有一定功能的代码片段
  * 认识：
    	1. AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。
    	2. CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。
    	3. AMD 是提前执行，CMD 是延迟执行。
    	4. AMD推荐的风格通过返回一个对象做为模块对象，CommonJS的风格通过对module.exports或exports的属性赋值来达到暴露模块对象的目的。
---

# 详细说说common.js?
---

# commonJS的底层原理是什么？
---

# commonJS 与 ES6 模块化区别；
---

# 当我们用 CommonJS 规范去 require 加载一个包，require 的加载机制大概是怎么样的
---

# 说下AMD，CMD的区别
---

# 请解释事件委托
  * 利用事件冒泡的原理，让自己的所触发的事件，让他的父元素代替执行！
  * 好处：
    1. 内存占用减少，因为只需要一个父元素的事件处理程序，而不必为每个后代都添加事件处理程序。
    2. 无需从已删除的元素中解绑处理程序，也无需将处理程序绑定到新元素上。
    3. 详细代码请看: https://www.cnblogs.com/liugang-vip/p/5616484.html
---

# 请说明.forEach循环和.map()循环的主要区别，它们分别在什么情况下使用？(map和foreach对数组元素进行操作是否会影响原数组的元素)
  * 都是数组循环，.forEach和.map()的主要区别在于.map()返回一个新的数组。
---

# reduce和map的区别
---

# reduce()讲讲 reduce怎么统计数据出现次数
---

# 宿主对象（host objects）和原生对象（native objects）的区别是什么？
  * 原生对象是由 ECMAScript 规范定义的 JavaScript 内置对象，比如String、Math、RegExp、Object、Function等等。
  * 宿主对象是由运行时环境（浏览器或 Node）提供，比如window、XMLHTTPRequest等等。
---

# call和apply有什么区别？
  * call和apply都用于调用函数，第一个参数将用作函数内 this 的值。然而，call接受逗号分隔的参数作为后面的参数，而apply接受一个参数数组作为后面的参数。一个简单的记忆方法是，从call中的 C 联想到逗号分隔（comma-separated），从apply中的 A 联想到数组（array）。
---

# apply、call、bind的区别？
---

# ajax 跨域有哪些方法，jsonp 的原理是什么，如果页面编码和被请求的资源编码不一致如何处理？
---

# bind函数绑定和执行过程，如何实现bind函数？
---

# 请说明 JSONP 的工作原理
  * JSONP（带填充的 JSON）是一种通常用于绕过 Web 浏览器中的跨域限制的方法，因为 Ajax 不允许跨域请求。
---

# jsonp的原理，怎么把js文件返回调用？
---

# jsonp的缺点
---

# 请解释变量提升
  * 变量提升（hoisting）是用于解释代码中变量声明行为的术语。使用var关键字声明或初始化的变量，会将声明语句“提升”到当前作用域的顶部。 但是，只有声明才会触发提升，赋值语句（如果有的话）将保持原样。
---

# 变量提升和暂时性死区的关系
---

# 请描述事件冒泡,如何阻止事件冒泡？
  * 当一个事件在 DOM 元素上触发时，如果有事件监听器，它将尝试处理该事件，然后事件冒泡到其父级元素，并发生同样的事情。最后直到事件到达祖先元素。

  * 阻止：
    ```
      1.event.stopPropagation();
        $("#hr_three").click(function(e) {
            e.stopPropagation();
        });

      2. return false
        $("#hr_three").click(function(event) { return false; });
    ```
---

# 事件的默认行为怎么阻止？
---

# 什么是事件冒泡和事件捕获，区别是什么？
---

# 事件捕获是怎么样的，怎么设置事件捕获
---

# 请解释关于 JavaScript 的同源策略。
  * 同源策略可防止 JavaScript 发起跨域请求。源被定义为 URI、主机名和端口号的组合。此策略可防止页面上的恶意脚本通过该页面的文档对象模型，访问另一个网页上的敏感数据。
---

# 为什么不要使用全局作用域？
  * 每个脚本都可以访问全局作用域，如果人人都使用全局命名空间来定义自己的变量，肯定会发生冲突。
---

# 为什么扩展 JavaScript 内置对象是不好的做法？
  * 扩展 JavaScript 内置（原生）对象意味着将属性或方法添加到其prototype中。如果你的代码使用了一些库，它们通过添加相同的 contains 方法扩展Array.prototype，如果这两个方法的行为不相同，那么这些实现将会相互覆盖，你的代码将不能正常运行。
---

# 说说你对promise的理解，promise代替回调函数有什么优缺点?
  * 概念: Promise是一个可能在未来某个时间产生结果的对象：操作成功的结果或失败的原因,用户可以对Promise添加回调函数来处理操作成功的结果或失败的原因。

  * 优点:
	  1. 避免可读性极差的回调地狱。
    2. 使用.then()编写的顺序异步代码，既简单又易读。
    3. 使用Promise.all()编写并行异步代码变得很容易。

  * 缺点:
    1. 轻微地增加了代码的复杂度（这点存在争议）。
    2. 在不支持 ES2015 的旧版浏览器中，需要引入 polyfill 才能使用。
---

# Promise内部实现原理
---

# 如何中断Promise的链式调用
---

# Promise的执行过程；以及resolve和reject分别调用了哪个钩子？
---

# promise的几种状态？
---

# 我在promise里面用到了resolve和rejected都会发生回调吗？
---

# promise.all遇到reject，后面的reject还会执行嘛
---

# 说一下Promise和Async的区别。
---

# Promise和AJAX的区别？
---

# promise的理解，有哪些方法，all的用法及原理
---

# 什么是事件循环？调用堆栈和任务队列之间有什么区别？
  1. 事件循环: 执行一个宏任务，然后执行清空微任务列表，循环再执行宏任务，再清微任务列表
    * 微任务: promise / ajax 
    * 宏任务: setTimout / script / IO

  2. 堆栈: 先进后出，任务队列: 先进先出
---

# ES6 的类和 ES5 的构造函数有什么区别？
  * 详细请看:  https://www.jdon.com/51322

# 什么是Ajax和JSON，它们的优缺点
  * Ajax是异步JavaScript和XML，用于在Web页面中实现异步数据交互。
    1. 优点：
       * 可以使得页面不重载全部内容的情况下加载局部内容，降低数据传输量
       * 避免用户不断刷新或者跳转页面，提高用户体验
    
    2. 缺点：
       * 对搜索引擎不友好
       * 要实现ajax下的前后退功能成本较大
       * 可能造成请求数的增加
       * 跨域问题限制

  * JSON是一种轻量级的数据交换格式，ECMA的一个子集
    1. 优点：轻量级、易于人的阅读和编写，便于机器（JavaScript）解析，支持复合数据类型（数组、对象、字符串、数字）
---

# 怎样添加、移除、移动、复制、创建和查找节点
  ```
  <1>.创建新节点
    createDocumentFragment() //创建一个DOM片段
    createElement() //创建一个具体的元素
    createTextNode() //创建一个文本节点

  <2>.添加、移除、替换、插入
    appendChild() //添加
    removeChild() //移除
    replaceChild() //替换
    insertBefore() //插入

  <3>.查找
    getElementsByTagName() //通过标签名称
    getElementsByName() //通过元素的Name属性的值
    getElementById() //通过元素Id，唯一性
  ```
---

# 什么是伪数组？如何将伪数组转化为标准数组？
  * 概念: 无法直接调用数组方法或期望length属性有什么特殊的行为，但仍可以对真正数组遍历方法来遍历它们。典型的是函数的argument参数，还有像调用getElementsByTagName,document.childNodes之类的,它们都返回NodeList对象都属于伪数组。

  * 可以使用Array.prototype.slice.call(fakeArray)将数组转化为真正的Array对象。
---

# 原生JS的window.onload与Jquery的$(document).ready(function(){})有什么不同？
  * window.onload()方法是必须等到页面内包括图片的所有元素加载完毕后才能执行。
  * $(document).ready()是DOM结构绘制完毕后就执行，不必等到加载完毕。
---

# 哪些地方会出现css阻塞，哪些地方会出现js阻塞？
```
  <1>.js阻塞:
    	1).所有浏览器在下载JS的时候，会阻止一切其他活动，比如其他资源的下载，内容的呈现等等。直到JS下载、解析、执行完毕后才开始继续并行下载其他资源并呈现内容。为了提高用户体验，新一代浏览器都支持并行下载JS，但是JS下载仍然会阻塞其它资源的下载

    	2).嵌入JS会阻塞所有内容的呈现，而外部JS只会阻塞其后内容的显示，2种方式都会阻塞其后资源的下载。也就是说外部样式不会阻塞外部脚本的加载，但会阻塞外部脚本的执行。

  <2>.css阻塞:
    	当CSS后面跟着嵌入的JS的时候，该CSS就会出现阻塞后面资源下载的情况。而当把嵌入JS放到CSS前面，就不会出现阻塞的情况了

  <3>.嵌入JS应该放在什么位置？
答:
  	 1)、放在底部，虽然放在底部照样会阻塞所有呈现，但不会阻塞资源下载。
   
  	 2)、如果嵌入JS放在head中，请把嵌入JS放在CSS头部。
   
  	 3)、使用defer（只支持IE）
   
  	 4)、不要在嵌入的JS中调用运行时间较长的函数，如果一定要用，可以用`setTimeout`来调用

  <4>.Javascript无阻塞加载具体方式
	
	<1>.将脚本放在底部。<link>还是放在head中，用以保证在js加载前，能加载出正常显示的页面。<script>标签放在</body>前。

	<2>.成组脚本：由于每个<script>标签下载时阻塞页面解析过程，所以限制页面的<script>总数也可以改善性能。适用于内联脚本和外部脚本。

	<3>.非阻塞脚本：等页面完成加载后，再加载js代码。也就是，在window.onload事件发出后开始下载代码。 （1）defer属性：支持IE4和fierfox3.5更高版本浏览器 （2）动态脚本元素：文档对象模型（DOM）允许你使用js动态创建HTML的几乎全部文档内容。
```
---

# Js执行会阻塞dom渲染吗，如何避免呢
---

# Javascript垃圾回收方法
  * 标记清除
    - 当变量进入执行环境的时候，比如函数中声明一个变量，垃圾回收器将其标记为“进入环境”，当变量离开环境的
时候（函数执行结束）将其标记为“离开环境”。垃圾回收器会在运行的时候给存储在内存中的所有变量加上标记，然后去掉环境中的变量以及被环境中变量所引用的变量（闭包），在这些完成之后仍存在标记的就是要删除的变量了

  * 引用计数
    - 在低版本IE中经常会出现内存泄露，很多时候就是因为其采用引用计数方式进行垃圾回收。引用计数的策略是跟踪记录每个值被使用的次数，当声明了一个变量并将一个引用类型赋值给该变量的时候这个值的引用次数就加1，如果该变量的值变成了另外一个，则这个值得引用次数减1，当这个值的引用次数变为0的时候，说明没有变量在使用，这个值没法被访问了，因此可以将其占用的空间回收，这样垃圾回收器会在运行的时候清理掉引用次数为0的值占用的空间。在IE中虽然JavaScript对象通过标记清除的方式进行垃圾回收，但BOM与DOM对象却是通过引用计数回收垃圾的，也就是说只要涉及BOM及DOM就会出现循环引用问题。
---

# documen.write和 innerHTML的区别
  * document.write只能重绘整个页面
  * innerHTML可以重绘页面的一部分
---

# 如何判断当前脚本运行在浏览器还是node环境中？
  * 通过判断Global对象是否为window，如果不为window，当前脚本没有运行在浏览器中
---

# 箭头函数和普通函数的区别
---

# 简单说明一下你对ES6中箭头函数的理解
---

# 箭头函数可以new吗 为什么？
---

# fastclick 解决点击穿透的原理；
  * 原理是在 目标元素 上绑定touchstart ,touchend事件，然后在touchend结束的时候立马执行click事件，这样就解决了“点透”的问题（实质是事件冒泡导致）以及300ms延迟问题，300ms延迟是因为浏览器为了实现用户双击屏幕放大页面的效果。
---

# fastclick什么作用
---

# typeof 和 instanceof 的区别
  1. 在 JavaScript 中，判断一个变量的类型尝尝会用 typeof 运算符，在使用 typeof 运算符时采用引用类型存储值会出现一个问题，无论引用的是什么类型的对象，它都返回 “object”。

  2. instanceof 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。 语法：object instanceof constructor 参数：object（要检测的对象.）constructor（某个构造函数） 描述：instanceof 运算符用来检测 constructor.prototype 是否存在于参数 object 的原型链上。
---

# for...in 和 for...of 有什么区别？
---

# 说一下你对 generator 的了解？
---

# JavaScript 异步编程原理
  * JavaScript 引擎负责解析，执行 JavaScript 代码，但它并不能单独运行，通常都得有一个宿主环境，一般如浏览器或 Node 服务器，前文说到的单线程是指在这些宿主环境创建单一线程，提供一种机制，调用 JavaScript 引擎完成多个 JavaScript 代码块的调度，这种机制就称为事件循环（ Event Loop ）。

  * 关于事件循环流程分解如下：
    1. 宿主环境为JavaScript 创建线程时，会创建堆 (heap) 和栈 (stack) ，堆内存储 JavaScript 对象，栈内存储执行上下文；

    2. 栈内执行上下文的同步任务按序执行，执行完即退栈，而当异步任务执行时，该异步任务进入等待状态（不入栈），同时通知线程：当触发该事件时（或该异步操作响应返回时），需向消息队列插入一个事件消息；

    3. 当事件触发或响应返回时，线程向消息队列插入该事件消息（包含事件及回调）；

    4. 当栈内同步任务执行完毕后，线程从消息队列取出一个事件消息，其对应异步任务（函数）入栈，执行回调函数，如果未绑定回调，这个消息会被丢弃，执行完任务后退栈；

    5. 当线程空闲（即执行栈清空）时继续拉取消息队列下一轮消息（next tick ，事件循环流转一次称为一次 tick ）。
---

# 既然 JS 是单线程，那么面对网络请求的时候为什么能异步处理
---

# 什么是回调函数？
  * 回调函数就是一个通过函数指针调用的函数。如果你把函数的指针(地址)作为参数传递给另一个函数，当这个指针被用为调用它所指向的函数时，我们就说这是回调函数。回调函数不是由该函数的实现方直接调用，而是在特定的事件或条件发生时由另外的一方调用的，用于对该事件或条件进行响应。
---

# 回调函数实现的机制是？
  1. 定义一个回调函数；
  2. 提供函数实现的一方在初始化的时候，将回调函数的函数指针注册给调用者；
  3. 当特定的事件或条件发生的时候，调用者使用函数指针调用回调函数对事件进行处理。
---

# DOM事件的绑定的几种方式
---

# JS中的async/await的用法和理解
---

# jquery选择器实现原理
  * jquery原型里面有一个init初始化的方法，将传入的值进行解析，比如传入的id还是class还是标签名。然后通过相应的方法返回数组型对象。既可以通过对象直接调用方法，也可以使用数组的length。
---

# setTimeout和setInterval区别，如何互相实现？
---

# setTimeout能准时执行吗，为什么，怎么能让它准时执行
---

# 解释一下settimeout的原理
---

# 问jquery的链式调用原理。
---

# JQuery和Vue有什么区别
---

# splice和slice的区别
---

# es6 map和普通对象自变量有什么区别
---

# 了解过函数式编程嘛
---

# hash路由和history路由
---

# hash了解吗，用什么来监听hash的变化，原生api有用过吗
---

# es6的迭代器了解吗
---

# object.create和new的区别
---

# JS设计模式
---

# 了解设计模式么，说说单例模式的优缺点;
---

# 订阅者模式大致介绍一下
---

# addeventlistener是什么设计模式
---

# onclick和addEventListener('click',handler)有什么区别
---

# 防抖节流，原理是什么，区别是什么
---

# click和onmousedown和onmouseup的执行顺序
---

# 说一下你知道的鼠标键盘响应事件方法
---

# 浏览器是多线程的吗？为什么？那js是多线程还是单线程？为什么？
---

# let var和const的区别是什么？什么是暂时性死区？
---

# 块级作用域和函数作用域的区别
---

# mockjs和echarts原理了解吗
---

# Echarts与d3的区别
---

# 我们使用函数传参的时候传递的是值还是引用？
---

# better-scroll作用？都用了什么，怎么用的？
---

# window.onload和DOMContentLoaded的区别
---

# 当有很多个a标签时，怎么监听它们的点击事件（事件委托），除了事件冒泡还有吗
---

# 使用装饰器的好处
---

# Mixins 和 装饰器的不同，Mixins 的缺点
---

# 拿到对象的 key 有哪些方法，使用 for...in 和 Object.keys 有什么不同
---

# ==与===的区别，==比较的规则，隐式转换的规则
---

# 说一下深拷贝的实现原理。
---

# 如何实现深拷贝
---

# 实现深拷贝的方式有哪些
---

# 用sort好吗，为什么不好？   那你有什么更好的方案？ （提了快排和归并，然后口述快排原理）
---

# 数组sort（）方法底层原理？用了什么算法？
---

# JSbridge原理, js和native是如何通信的?
---

# 介绍下argument
---

# weakSet和weakMap的区别
---

# 为什么Set能去重
---

# object和map有什么区别
---

# map数据类型和set数据类型
---

# map和set的区别
---

# 介绍Object.assign的使用
---

# es6新增哪些对象？多了哪些类型？
---

# new String('a')和'a'是一样的么?
---