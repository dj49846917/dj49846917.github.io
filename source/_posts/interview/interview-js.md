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

# import的原理
  1. 简单说就是闭包的运用
  2. 为了创建module的内部作用域，会调用一个包装函数
  3. 包装函数的返回值也就是module向外公开的api,也就是所有export出去的变量
  4. import也就是拿到module导出变量的引用
---

# commonjs 和 es module区别？
  * commonJs是被加载的时候运行，esModule是编译的时候运行
  * commonJs输出的是值的浅拷贝，esModule输出值的引用
  * commentJs具有缓存。在第一次被加载时，会完整运行整个文件并输出一个对象，拷贝（浅拷贝）在内存中。下次加载文件时，直接从内存中取值
---

# JS传值、传引用的区别？
  * 值传递，内存中的地址复制了一份，修改数据指的是修改复制出来的内存地址，对原先的值不会有影响。引用传递，将其指向同一个内存地址，修改数据会对原先的值有影响。
---

# 介绍下 symbol, symbol作用
  * 介绍
    1. Symbol 是一种基本数据类型，通过Symbol函数生成，唯一。
    2. 对象的属性名有两种类型，一种是字符串，另一种就是Symbol。
    3. Symbol函数不能用new，因为生成的 Symbol 是一个原始类型的值，不是对象。
    4. Symbol值不能与其他类型的值进行运算，会报错，但是可以显示的转换为字符串以及转换为布尔值
    5. Symbol 作为属性名，该属性不会出现在for…in、for…of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify()返回。但是，它也不是私有属性，有一个Object.getOwnPropertySymbols方法，可以获取指定对象的所有 Symbol属性名。
  
  * 作用：
    1. 防止变量名起冲突
    2. 可以使用symbol避免魔术字符串
      * 魔术字符串：在代码中多次出现、与代码形成强耦合的某一个具体的字符串或者数值。
    3. 定义不重复的常量
    4. symbol作为键名时，不被常规方法遍历出来，因此可以给对象定义非私有，但只用于内部使用的方法和属性
---

# Bigint简单介绍（理解）
  1. 是一种基本数据类型，用于当整数值大于Number数据类型支持的范围时。
  2. 使用场景包括时间戳、大整数id等
  3. 通过在数字末尾追加n或者使用BigInt函数生成
  4. 注意点：
    - 不能和number类型做运算，因为 JavaScript 在处理不同类型的运算时，会把他们先转换为同一类型，而 BigInt 类型变量在被隐式转换为 Number 类型时，可能会丢失精度，或者直接报错。
    - 不能使用Math对象的api，比如说Math.floor等
    - 相同的2n、2、"2"，两个等号是true，三个等是false
  5. 使用JSON.stringify会报错，因为默认情况下 BigInt 值不会在 JSON 中序列化。但是可以通过实现toString方法来满足
    ```
      BigInt.prototype.toJSON = function() { return this.toString(); }
      JSON.stringify(BigInt(1));
    ```
  6. 使用 BigInt 运算时，带小数的结果会被取整。如 5n / 2n = 2 ，而不是 2.5
---

# Symbol主要用于什么场景下
  1. 使用Symbol来作为对象属性名
  2. 使用Symbol来替代常量
  3. 使用Symbol定义类的私有属性/方法 
---

# 用过Symbol吗？ Symbol () == Symbol () 的结果是啥，你觉得Symbol可以用在哪些场景
---

# 介绍js的基本数据类型。
  * 答: Undefined、Null、Boolean、Number、String、Symbol(创建后独一无二且不可变的数据类型 )
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

# 说一下对于堆栈的理解
  * 堆和栈其实是两种数据结构。堆栈都是一种数据项按序排列的数据结构，只能在一端（成为栈顶）对数据项进行插入和删除。堆栈是个特殊的储存区，主要功能是暂时存放数据和地址
  * 堆：
    - 堆内存的存储的值大小不定，是由程序员自己申请并指明大小。因为堆内存是new分配的内存，所以运行效率会比较低
    - 堆内存是向高地址拓展的数据结构，是不连续的内存区域，系统也是用链表来存储空闲的内存地址，所以是不连续的。因为是记录的内存地址，所以获取是通过引用，存储的是对象居多。
    - 堆内存的回收是人为控制的，当程序结束后，系统会自动回收
  * 栈：
    - 栈内存的存储大小是固定的，申请时由系统自动分配内存空间，运行的效率比较快，但是因为存储的大小固定，所以容易存储的大小超过存储的大小，导致溢栈。
    - 栈内存存储的是基础数据类型，并且是按值访问，因为栈是一块连续的内存区域，以<span style="background:red">先进后出</span>的原则存储调用的，所以是连续存储的
---

# js栈怎么实现的
  > 栈是和链表相似的数据结构，相对来说更加的高效，因为它只能在栈顶进行插入和删除操作，实现起来更加的简单，而且很快。从表达式求值到函数的调用，都可以看到栈的使用。栈这种数据结构，我们可以具体的类似于餐厅里面的一叠盘子，每次我们取盘子都只能从最上面拿，放盘子只能放在最上面，所以栈是一种先入后出的数据结构。具体的操作有pop()入栈，和push()出栈，peek()方法只访问栈顶的元素，但是不删除它。下面看具体的实现方法。
---

# js队列怎么实现的
  > 队列是一种特殊的线性表，特殊之处在于它只允许在表的前端（front）进行删除操作，而在表的后端（rear）进行插入操作，和栈一样，队列是一种操作受限制的线性表。进行插入操作的端称为队尾，进行删除操作的端称为队头
---

# 介绍js有哪些内置对象？
  * Object 是 JavaScript 中所有对象的父对象
  * 数据封装类对象：Object、Array、Boolean、Number 和 String
  * 其他对象：Function、Arguments、Math、Date、RegExp、Error
---

# 面向对象有了解过？说说面向对象三大特性
---

# 说说多态的理解
---

# 如何判断Array类型（怎么区别array和object）
1. 使用instanceof方法。
   var arr=[];
   console.log(arr instanceof Array) //true
2. 使用constructor方法。
   console.log([].constructor == Array);  //true
3. 使用Object.prototype.toString.call(arr) === '[object Array]'方法。
   function isArray(o) {
　　return Object.prototype.toString.call(o);
   }
   var arr=[2,5,6,8];
   console.log(isArray(arr)); //[object Array]
4. ES5定义了Array.isArray
    Array.isArray([]) //true
---

# 何如判断对象类型
 * 同上
---

# Array都有什么方法
1. 给数组末尾添加新内容的push方法；
2. 删除数组最后一项的pop方法；
3. 删除数组第一项的shift方法；
4. 向数组首位添加新内容unshift方法；
5. concat();合并两个或更多的数组，并返回新数组。
6. join();就是将数组的每个元素以指定的字符连接，形成新字符串返回。
7. splice() 插入，删除，替换元素。
8. some();对数组的每个元素判断是否满足条件，如果都不满足就返回false，如果有一个满足的就返回true，并且不再判断后面的内容。
9. every();对数组的每个元素判断是否满足条件，如果有一个不满足条件就返回为false，全部满足时返回true。
10. filter();方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。
---

# JavaScript原型，原型链 ? 有什么特点？
  * 原型: 每个对象都会在其内部初始化一个属性，就是prototype(原型)
  * 当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么他就会去prototype里找这个属性，这个prototype又会有自己的prototype，于是就这样一直找下去，也就是我们平时所说的原型链的概念。
  * 关系：instance.constructor.prototype = instance.__proto__
  * 特点：JavaScript对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。
---

# 原型链顶端是什么
* 函数的prototype是一个在函数声明阶段就会产生的对象(prototype只有函数才会有)，这个对象只有两个属性constructor和__proto__，其中__proto__指向了我们原型链的顶点Object.prototype。constructor指向函数本身，里面包括了函数一些基本描述信息比如函数名称，参数个数等。

---

# 怎么判断某个属性是自身的还是原型链上的
1. hasOwnProperty
  var obj = {name: 'h5course-com'};
  obj.hasOwnProperty('name'); // true
  obj.hasOwnProperty('toString'); // false
  原型链上继承过来的属性无法通过hasOwnProperty检测到，返回false。
2. in 操作符
---

# 隐式原型，显式原型有什么用
  * 显式原型：每个函数function都有一个prototype，这个就是显式原型
  * 隐式原型：每个实例对象都有一个__proto__，这个就是隐式原型
  * 作用：
    1. 肯定是为了继承！
    2. prototype用来实现基于原型的继承与属性的共享。
    3. __proto__就构成了我们常说的原型链 访问构造方法中的显示原型，同样用于实现基于原型的继承。
---

# 什么拥有隐式对象？
  * 实例对象
---

# 隐式类型转换有哪几种
  1. 比较运算符（>, >=, <, <= ）
    * 运算符两边操作数都是数字，则进行数字比较；
    * 运算符两边操作数都是字符串，则将字符的ASCII码进行一一比较（数字<小写字母<大写字母<中文）；
    * 运算符一边操作数是数字，一边是字符串，则将字符串强制转换成number，再进行比较（因为字符串和数字都是值类型，应该是强制调用Number方法将字符串转换成数字，一般字符串转换成number类型得到的都是NaN，而NaN与其他所有操作数相比较得到的结果都是false）；
    * 运算符有一边操作数是布尔类型，则先将bool值转换成数字，然后再进行比较；
    * 运算符有一边操作数是对象，则进行强制类型转换成数字，得到的结果是NaN，与另一个操作数比较得到的结果肯定是false。
  2. 加法运算符(+)
    * 运算符两边操作数都是数字，则进行数字相加；
    * 运算符两边操作数都是字符串，则进行字符串连接；
    * 运算符一边操作数是数字/null/undefined，一边是字符串，则先将数字/null/undefined转换成字符串，再与另一边的字符串进行连接（注意这里的null和undefined没有对应的toString方法，因此这里将这两种类型的数据强制转化成string类型调用的可能是String这类的方法）;
    * 运算符一边是对象，则会调用其对应的toString 方法，将其转化成“[object Object]”，然后再和另一个操作数进行连接(另一个操作数也会进行相应的转换成string类型)；
    * 运算符有一边操作数是symbol类型，进行加法运算的时候会报错。
---

# 介绍一下JavaScript的执行上下文
  1. 执行上下文就是当前js代码被解析和执行时存在的环境
  2. 执行上下文的类型：
    * 全局执行上下文：这是默认的，最基础的执行上下文
      - 不在函数内部的代码都位于全局执行上下文中
      - 创建一个全局对象，其实就是我们的window对象
      - 将this指针指向这个全局对象
    * 函数执行上下文：每次函数被调用的时候，就会创建一个新的执行上下文
      - 每个函数都有自己的执行上下文
      - 一个函数中可以存在任意数量的函数执行上下文
      - 每一个函数执行上下文被创建，它都会按照特定的顺序执行一系列的操作
    * eval函数执行上下文：eval内部的文本被执行时。
  3. 执行上下文的生命周期：
    * 总的来说：创建阶段 => 执行阶段 =>回收阶段
      - 创建阶段：
        - 当函数被调用，但是未执行内部的任何代码之前，会做以下事情：
          - 创建变量对象：首先会初始化函数的arguments,提升函数声明和变量声明
          - 创建作用域链：在执行上下文创建阶段，作用域链是在变量之后创建的，作用域链本身包括变量对象，作用域链用来解析变量
        - 确定this指向：很多种情况
      - 执行阶段:
        - 执行变脸赋值，代码执行
      - 回收阶段：
        - 执行上下文出栈，等待虚拟机回收
  4. 变量声明提升
  5. 函数变量提升
---

# prototype和__proto__属性？原型对象如何指向自己的实例化对象？
  1. 对象有属性__proto__,指向该对象的构造函数的原型对象。
  2. 方法除了有属性__proto__,还有属性prototype，prototype指向该方法的原型对象。
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
    1. 作用域是代码在运行时，某些特定部分中的变量、函数的可访问性
    2. 作用域决定了变量、函数的可访问范围，及作用域控制着变量与函数的可见性和生命周期
  * 作用域同上
---

# 怎么理解JS 静态作用域
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

# typeof(null) 为什么返回的是 'object'
---

# typeof(NaN) 返回什么
  number
---

# 事件是？IE与火狐的事件机制有什么区别？ 如何阻止冒泡？Javascript的事件流（浏览器事件机制）
  1. 事件是我们在网页中的某个操作（有的操作对应多个事件）。例如：当我们点击一个按钮就会产生一个事件。是可以被 JavaScript 侦测到的行为。
  2. 事件处理机制：IE是事件冒泡、Firefox同时支持两种事件模型，也就是：捕获型事件和冒泡型事件；
  3.  ev.stopPropagation();（旧ie的方法 ev.cancelBubble = true;）
  4.  事件流:  三个阶段：事件捕捉，目标阶段，事件冒泡
---

# 全局变量和局部变量有什么区别？
  1. 作用域不同：全局变量的作用域为整个程序，而局部变量的作用域为当前函数或循环等
  2. 内存存储方式不同：全局变量存储在全局数据区中，局部变量存储在栈区
  3. 生命期不同：全局变量的生命期和主程序一样，随程序的销毁而销毁，局部变量在函数内部或循环内部，随函数的退出或循环退出就不存在了
  4. 使用方式不同：全局变量在声明后程序的各个部分都可以用到，但是局部变量只能在局部使用。函数内部会优先使用局部变量再使用全局变量。
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

# 为什么闭包不会被垃圾机制回收?
  > 首先JS垃圾回收机制中，如果一个对象不再被引用，那么这个对象就会被垃圾机制回收，如果两个对象相互引用，而不在被第三方引用，那么这两个对象就会被垃圾回收机制回收。
  > 因为闭包中，父函数被子函数引用，子函数又被外部所引用，这就是父函数不被回收的原因。
---

# 哪些操作会造成内存泄漏？
  * 概念: 内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。

  * 操作：
    1. 意外的全局变量引起的内存泄露
    2. 闭包引起的内存泄露
    3. 没有清理的DOM元素引用
    4. 被遗忘的定时器或者回调子元素存在引起的内存泄露
---

# 如何避免闭包
  * 在不使用闭包时，把被引用的变量设置为null，即手动清除变量，这样下次js垃圾回收机制回收时，就会把设为null的量给回收了。
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

# new操作符具体干了什么呢?(new 和 instanceof 的内部机制、原理)
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
1. JS执行环境：单线程.
2. Promises
3. 发布、订阅模式
4. 回调函数（callback）可能会造成回调地狱
5. async及await   
---

# Js执行会阻塞dom渲染吗，如何避免呢
  * 会
  * defer和async、动态创建DOM方式（用得最多）、按需异步载入js
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

# 使用 jQuery 中止 Ajax 请求
abort（）
---
   
# Ajax 解决浏览器缓存问题？
  * 在ajax发送请求前加上 anyAjaxObj.setRequestHeader("If-Modified-Since","0")。
  * 在ajax发送请求前加上 anyAjaxObj.setRequestHeader("Cache-Control","no-cache")。
  * 在URL后面加上一个随机数： "fresh=" + Math.random();。
  * 在URL后面加上时间戳："nowtime=" + new Date().getTime();。
  * 如果是使用jQuery，直接这样就可以了 $.ajaxSetup({cache:false})。这样页面的所有ajax都会执行这条语句就是不需要保存缓存记录。
---

# ajax返回的状态有哪些？
  * 0 － （未初始化）还没有调用send()方法
  * 1 － （载入）已调用send()方法，正在发送请求
  * 2 － （载入完成）send()方法执行完成，已经接收到全部响应内容
  * 3 － （交互）正在解析响应内容
  * 4 － （完成）响应内容解析完成，可以在客户端调用了
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

# 跨域通信有哪些？
  * postMessage
  * document.domain
  * WebSocket
  * JSONP
  * window.name
  * location.hash
  * http-proxy
---

# proxy解决跨域的原理？
* 客户端发送请求时不直接到服务器而是先到代理的中间层在这里将localhost：8088的这个域名装换为192.168.0.67:8061，再将请求发送到服务器这样在服务器端收到的请求就是使用的192.168.0.67:8061域名
  同理，当服务器返回数据的时候，也是先到代理的中间层将192.168.0.67:8061转换成localhos：8088；这样在客户端也是在相同域名下访问的了。
---

# 跨域时用cors 有的时候会多发一个option请求是为什么？
  * Preflighted Requests是CORS中一种透明服务器验证机制。预检请求首先需要向另外一个域名的资源发送一个 HTTP OPTIONS 请求头，其目的就是为了判断实际发送的请求是否是安全的
---

# 使用Access-Control-Allow-Origin为什么可以解决跨域问题(流程)
  * Access-Control-Allow-Origin是一个CORS（跨源资源共享）标头。
  * 当站点A尝试从站点B获取内容时，站点B可以发送Access-Control-Allow-Origin响应头以告知浏览器该页面的内容可供某些来源访问。（原点是域，加上方案和端口号。）默认情况下，站点B的页面不能被任何其他来源访问 ; 使用Access-Control-Allow-Origin标题为特定请求来源的跨域访问打开了一扇门。
---

# 对 CORS 有了解吗?具体是怎么实现跨域的? 需要后端去设置哪些值了解吗
  * 概念：跨域资源共享(CORS) 是一种机制，它使用额外的 HTTP 头来告诉浏览器 让运行在一个 origin (domain) 上的Web应用被准许访问来自不同源服务器上的指定的资源。当一个资源从与该资源本身所在的服务器不同的域、协议或端口请求一个资源时，资源会发起一个跨域 HTTP 请求。
  * 后端设置：Access-Control-Allow-Origin
---

# CORS 是如何做的？
  > 服务端设置 Access-Control-Allow-Origin 就可以开启 CORS。
---

# cors原理
  > 在服务端程序中，在发送消息之前手动修改响应头，将带有客户端浏览器的 IP 地址的数据包发送给客户端浏览器，然后客户端浏览器在查验服务端返回的数据时，发现响应头激活中的 IP 和当前网页的 IP 一样，那浏览器就认为这个数据包合法，可以使用该数据。
---

# 说一下CORS的原理和相关的header
---

# CORS通常完成一个请求需要请求多少次服务器？
  * 2次
  * 第一次是会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求。浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则就报错。请求方法是options
  * 服务器收到"预检"请求以后，检查了Origin、Access-Control-Request-Method和Access-Control-Request-Headers字段以后，确认允许跨源请求，就可以做出回应。
  * 第二次是正常请求
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
  * CommonJS是一种被广泛使用的js模块化规范，核心思想是通过require方法来同步加载依赖的其他模块，通过module.exports导出需要暴露的接口。
  * 优点：代码可复用于 Node.js 环境下井运行，例如做同构应用：通过 Npm 发布的很多第三方模块都采用了 CommonJS 规范。
  * 缺点：这样的代码无法直接运行在浏览器环境下，必须通过工具转换 成标准的 ES5。
---

# commonJS的底层原理是什么？
  * CommonJS 使用 Node.js 的四个环境变量
    - module
    - exports
    - require
    - global
  * 只要能够提供这四个变量，浏览器就能加载 CommonJS 模块。
---

# commonJS 与 ES6 模块化区别；
  1. CommonJs模块的require()是同步加载模块，ES6模块的import命令是异步加载模块
  2. CommonJs模块是运行时加载，ES6模块是编译时加载或静态加载
  3. CommonJs模块加载有缓存，ES6模块加载没有缓存
  4. CommonJs模块输出的是一个值的复制，ES6模块输出的是值的只读引用
  5. CommonJs加载的是整个模块，即将所有的方法都加载进来，ES6可以单独加载其中的某个方法
  6. CommonJs模块中this指向当前模块，ES6中指向undefined
---

# 当我们用 CommonJS 规范去 require 加载一个包，require 的加载机制大概是怎么样的
  * require优先加载缓存中的模块。同一个模块第一次require之后，就会缓存一份，下一次require时就直接从缓存中去取。
  * 如果是加载核心模块，直接从内存中加载，并缓存
    - 加载核心模块的格式是const xxx = require("模块名") 。不能写相对路径！
  * 如果是相对路径，则根据路径加载自定义模块，并缓存
    - 以require('./main')为例（ 省略扩展名的情况）
    - 先加载main.js，如果没有再加载main.json，如果没有再加载 main.node(c/c++编写的模块)，找不到就报错。
  * 如果不是自定义模块，也不是核心模块，则加载第三方模块。以require('XXX')为例：
    - node 会去本级 node_modules 目录中找xxx.js---->xxx.json ----> xxx.node(找到一个即返回)，找到并缓存。
    - 如果找不到，node 则取上一级目录下找node_modules/xxx 目录，规则同上
    - 如果一直找到代码文件的文件系统的根目录还找不到，则报错：模块没有找到。
---

# requireJS的核心原理是什么？
  > 核心是js的加载模块，通过正则匹配模块以及模块的依赖关系，保证文件加载的先后顺序，根据文件的路径对加载过的文件做了缓存。
---

# 说下AMD，CMD的区别
* . AMD推崇依赖前置，在定义模块的时候就要声明其依赖的模块,CMD推崇就近依赖，只有在用到某个模块的时候再去require。AMD推崇依赖前置，因此，JS可以及其轻巧地知道某个模块依赖的模块是哪一个，因此可以立即加载那个模块；而CMD是就近依赖，它要等到所有的模块变为字符串，解析一遍之后才知道他们之间的依赖关系。
---

# 说说你对AMD和Commonjs的理解
  > CommonJS是服务器端模块的规范，Node.js采用了这个规范。CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。AMD规范则是非同步加载模块，允许指定回调函数。 AMD推荐的风格通过返回一个对象做为模块对象，CommonJS的风格通过对module.exports或exports的属性赋值来达到暴露模块对象的目的。
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
* map()是把array中的所有元素，都依次作为increase函数的参数传进去，即依次执行,最后返回的是一个新数组。而reduce()是先把数组的前两个数作为参数传进multiply函数，然后把返回值加上数组的第三个数，再次作为参数传进multiply函数，如此类推，最终得到的就不是数组了，而是一个数。
---

# reduce()讲讲 reduce怎么统计数据出现次数
  * 定义：reduce() 方法接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值。对空数组是不会执行回调函数的。
  * 语法：reduce(function, iterable[, initializer])
  * 参数：
    1. function：函数，有两个参数
    2. iterable：可迭代对象
    3. initializer：可选，初始参数
  * 返回值：返回函数计算结果。
---

# 宿主对象（host objects）和原生对象（native objects）的区别是什么？
  * 原生对象是由 ECMAScript 规范定义的 JavaScript 内置对象，比如String、Math、RegExp、Object、Function等等。
  * 宿主对象是由运行时环境（浏览器或 Node）提供，比如window、XMLHTTPRequest等等。
---

# 请说说js中的call和apply方法？应用场景？以及两者的区别？
  * 概念：
    - call()和apply()都是借用其他对象中已实现的方法; 通过改变方法执行时的上下文，达到改变方法内部的this指向; 主要目的在于：减少重复代码，节省内存。
    - 注意点：调用call()和apply()的必须是个函数，因为他们是挂在Function对象上的方法，只有函数才有这些方法
  * 应用场景：
    1. 判断数据类型
      - 调用Object.prototype.toString.call方法来判断数据类型
    2. 类数组借用数组的方法
      - 类数组因为不是真正的数组所以没有数组类型上自带的种种方法，所以要借用数组的方法，比如push
        ```
          let likeArr = {
            0: '快快快',
            1: 'itlike.com',
            length: 2
          }
          Array.prototype.push.call(likeArr, '222', '333')
          console.log(likeArr)
        ```
  * 区别：
    - 参数数量/顺序确定就用call，不确定就用apply
    - 参数数量不多就用call，参数数量较多的话，把参数整合成数组，使用apply
    - 尝试集合已经是一个数组的情况下，用apply
---

# 说说js中的bind方法，和call/apply的区别？
  * 创建了一个新的函数，在bind被调用时，这个新函数的this被指定为bind的第一个参数，而其余参数会被指定为新函数的参数，共调用时使用
  * 区别：
    1. 执行上的区别：
      - call/apply改变了函数的this上下文后马上执行该函数
      - bind则是返回改变了上下文后的函数，不执行该函数
    2. 返回值的区别：
      - call/apply返回fun的执行结果
      - bind返回fun的拷贝，并指定了fun的this指向，保存了fun的参数
---

# bind函数绑定和执行过程，如何实现bind函数？
  * 调用bind方法会返回一个新的函数(这个新的函数的函数体应该和fun是一样的)。同时bind中传递两个参数，第一个是this指向。第二个参数为一个序列，你可以传递任意数量的参数到其中。并且会预置到新函数参数之前。
  * 实现：
    ```
      Function.prototype.bind = function(ctx) {
      var fn = this;
      return function() {
          fn.apply(ctx, arguments);
      };
    ```
---

# 请说明 JSONP 的工作原理
  * JSONP利用html中script的src属性不受同源策略的限制，加入了回调函数
---

# jsonp的原理，怎么把js文件返回调用？
1. script的src属性不受同源策略的限制
2. 将不同源的服务器端请求地址写在 script 标签的 src 属性中
---

# jsonp的缺点
  1. 它只支持GET请求而不支持POST等其它类型的HTTP请求
  2. 它只支持跨域HTTP请求这种情况，不能解决不同域的两个页面之间如何进行JavaScript调用的问题。
  3. jsonp在调用失败的时候不会返回各种HTTP状态码。
  4. 缺点是安全性。万一假如提供jsonp的服务存在页面注入漏洞，即它返回的javascript的内容被人控制的。那么结果是什么？所有调用这个 jsonp的网站都会存在漏洞。于是无法把危险控制在一个域名下…所以在使用jsonp的时候必须要保证使用的jsonp服务必须是安全可信的。
---

# 请解释变量提升，原理是什么, 如何避免
  * 简介：
    - 变量提升（hoisting）是用于解释代码中变量声明行为的术语。使用var关键字声明或初始化的变量，会将声明语句“提升”到当前作用域的顶部。 但是，只有声明才会触发提升，赋值语句（如果有的话）将保持原样。
    - 变量声明提升：由 var 声明的变量，在声明语句之前可以访问到，返回值为 undefined
    - 函数声明提升：通过 function 声明的函数，可以在声明函数之前使用该函数。（正确的方式：先声明函数，再调用函数 (最佳实践)）
    - 函数表达式声明的变量会被当做变量提升，声明语句之后可以调用该函数
  * 原理：
    - js 也是有编译阶段的，其不会早早地完成编译阶段，而是一边编译一边执行。
    - 在短暂地编译阶段，js引擎会收集所有的变量声明，并且提前让声明生效。
  * 如何避免：
    - 使用let和const声明的变量不会出现声明提前, 并且具有块级作用域
---

# let 声明的变量会变量提升吗？
  > 当程序的控制流程在新的作用域（module function 或 block 作用域）进行实例化时，在此作用域中用 let / const 声明的变量会先在作用域中被创建出来，但因此时还未进行词法绑定，所以是不能被访问的，如果访问就会抛出错误。因此，在这运行流程进入作用域创建变量，到变量可以被访问之间的这一段时间，就称之为暂时死区。
---

# 变量提升和暂时性死区的关系
  * 变量提升：“变量提升”意味着变量和函数的声明会在物理层面移动到代码的最前面，但这么说并不准确。实际上变量和函数声明在代码里的位置是不会动的，而是在编译阶段被放入内存中。
  * 特点：
    - 变量声明提升：由 var 声明的变量，在声明语句之前可以访问到，返回值为 undefined
    - 函数声明提升：通过 function 声明的函数，可以在声明函数之前使用该函数。（正确的方式：先声明函数，再调用函数 (最佳实践)）
    - 函数表达式声明的变量会被当做变量提升，声明语句之后可以调用该函数
  * 暂时性死区：就是不能在变量初始化之前，使用变量。在代码块内，使用 let 命令声明变量之前，该变量都是不可用的。
---

# 哪些会引起暂时性死区？
  * typeof
  * import关键字引入公共模块
  * 使用new class创建类的方式
  * 原因还是变量的声明先于使用
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
  * w3c的方法是e.preventDefault()，IE则是使用e.returnValue = false;
---

# 什么是事件冒泡和事件捕获，区别是什么？
  * 冒泡：从触发事件的那个节点一直到document，是自下而上的去触发事件。
  * 捕获：从document到触发事件的那个节点，即自上而下的去触发事件。
---

# 事件捕获是怎么样的，怎么设置事件捕获
  * addEventListener的第三个参数设置为false
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

# 什么是回调函数？
  * 回调函数，就是放在另外一个函数（如 parent）的参数列表中，作为参数传递给这个 parent，然后在 parent 函数体的某个位置执行，Node.js 异步编程的直接体现就是回调
---

# 回调函数实现的机制是？
  1. 定义一个回调函数；
  2. 提供函数实现的一方在初始化的时候，将回调函数的函数指针注册给调用者；
  3. 当特定的事件或条件发生的时候，调用者使用函数指针调用回调函数对事件进行处理。
---

# 说说你对promise的理解，promise代替回调函数有什么优缺点?
  * Promise是异步编程的一种解决方案，它是一个对象，可以获取异步操作的消息，他的出现大大改善了异步编程的困境，避免了地狱回调，它比传统的解决方案回调函数和事件更合理和更强大。
  * 所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。
  * Promise的实例有三个状态:
    - Pending（进行中）
    - Resolved（已完成）
    - Rejected（已拒绝）
  * 当把一件事情交给promise时，它的状态就是Pending，任务完成了状态就变成了Resolved、没有完成失败了就变成了Rejected。
  * Promise的特点：
    - 对象的状态不受外界影响。promise对象代表一个异步操作，有三种状态，pending（进行中）、fulfilled（已成功）、rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态，这也是promise这个名字的由来——“承诺”；
    - 一旦状态改变就不会再变，任何时候都可以得到这个结果。promise对象的状态改变，只有两种可能：从pending变为fulfilled，从pending变为rejected。这时就称为resolved（已定型）。如果改变已经发生了，你再对promise对象添加回调函数，也会立即得到这个结果。这与事件（event）完全不同，事件的特点是：如果你错过了它，再去监听是得不到结果的。

  * 优点:
	  1. 避免可读性极差的回调地狱。
    2. 使用.then()编写的顺序异步代码，既简单又易读。
    3. 使用Promise.all()编写并行异步代码变得很容易。

  * 缺点:
    1. 轻微地增加了代码的复杂度（这点存在争议）。
    2. 在不支持 ES2015 的旧版浏览器中，需要引入 polyfill 才能使用。
---

# Promise和Callback有什么区别?
  * 相比于callback，Promise 具有更易读的代码组织形式（链式结构调用），更好的异常处理方式（在调用 Promise 的末尾添加上一个catch方法捕获异常即可），以及异步操作并行处理的能力（Promise.all() Promise.race()等）。
  * callback最大的问题就是我们通常说的回调地狱,一旦业务逻辑复杂了,我们不得不使用大量的嵌套回调代码,可维护性很低.
---

# promise的基本规则
  1. 首先Promise构造函数会立即执行，而Promise.then()内部的代码在当次事件循环的结尾立即执行(微任务)。
  2. promise的状态一旦由等待pending变为成功fulfilled或者失败rejected。那么当前promise被标记为完成，后面则不会再次改变该状态。
  3. resolve函数和reject函数都将当前Promise状态改为完成，并将异步结果，或者错误结果当做参数返回。
  4. Promise.resolve(value)
  返回一个状态由给定 value 决定的 Promise 对象。如果该值是 thenable(即，带有 then 方法的对象)，返回的 Promise 对象的最终状态由 then 方法执行决定；否则的话(该 value 为空，基本类型或者不带 then 方法的对象),返回的 Promise 对象状态为 fulfilled，并且将该 value 传递给对应的 then 方法。通常而言，如果你不知道一个值是否是 Promise 对象，使用 Promise.resolve(value) 来返回一个 Promise 对象,这样就能将该 value 以 Promise 对象形式使用。
  5. Promise.all(iterable)/Promise.race(iterable)
  简单理解，这2个函数，是将接收到的promise列表的结果返回，区别是，all是等待所有的promise都触发成功了，才会返回，而arce有一个成功了就会返回结果。其中任何一个promise执行失败了，都会直接返回失败的结果。
  6. promise对象的构造函数只会调用一次，then方法和catch方法都能多次调用，但一旦有了确定的结果，再次调用就会直接返回结果。
---

# Promise内部实现原理
  1. 创建 Promise 对象 => 进入等待处理阶段 Pending
  2. 处理完成后，切换到 Fulfilled 状态／ Rejected 状态
  3. 根据状态，执行 then 方法／执行 catch 方法 内的回调函数，then方法返回新的 Promise，此时就支持链式调用
---

# 如何中断Promise的链式调用
  * 通过抛出一个异常来终止
  * 通过reject来中断
---

# Promise的执行过程；以及resolve和reject分别调用了哪个钩子？
  1. 实例化一个最初的 Promise 对象，设置最初的状态为 pending。
  2. 通过 then 方法，创建一个新的 Promise 对象，由于上一级 Promise 暂时处于 pending 状态，当前 then 方法的 onFulfilled 函数和新 Promise 的 resolve 方法放入到上一级 Promise 的 deferreds 数组中。
  3. 这样就形成这样一个画面：第一个 Promise 被实例化，调用 then 方法。then 会返回一个新的 Promise 对象，在上一个 then 方法的基础上继续通过新 Promise 的 then，形成一条调用链。 每一个被创建出来的新 Promise 的 resolve 都将传给上一级的 Promise 的 deferreds 数组来维护。
  4. 在第一个 Promise 对象的回调函数中执行异步操作，完成后调用 Promise 的 resolve 方法。
  5. resolve 允许传入一个参数，该参数的值通过 Promise 内部的 value 变量维护。resolve 会把 Promise 的状态修改为 fulfilled，然后异步调用 handle 依次处理 deferreds 数组中的每一个 deferred。
  6. 此时第一个 Promise 的状态在上一步骤中被改为 fulfilled，于是 handle 主要完成的工作是，执行 deferred 的 onFulfilled 函数，并调用下一个 Promise 的 resolve 方法。
  7. 下一个 Promise 的 resovle 在上一级被执行成功后，同样会将状态切换到 fulfilled ，重复步骤 6 直到结束。
---

# promise的几种状态？
  * 三个状态
  * Pending（进行中）
  * Resolved（已完成）
  * Rejected（已拒绝）
---

# 我在promise里面用到了resolve和rejected都会发生回调吗？
  * 会
---

# promise.all遇到reject，后面的reject还会执行嘛
  * 不会
---

# 说一下Promise和Async的区别。
  * 函数前面多了一个async关键字。await关键字只能用在async定义的函数内。async函数会引式返回一个promise,改promise的resolve值就是函数return的值。
  * 简洁：使用async和await明显节约了不少代码，不需要.then,不需要写匿名函数处理promise的resolve的值，不需要定义多余的data变量，还避免了嵌套代码。
  * async/await让try/catch 可以同时处理同步和异步错误。try/catch不能处理JSON.parse的错误，因为他在promise中。此时需要.catch，这样的错误处理代码非常冗余。并且，在我们的实际生产代码会更加复杂
---

# Promise和AJAX的区别？
  两个不同的东西，ajax用来获取后台的数据，promise是ES6引入的，用来充当异步操作与回调函数之间的中介。
---

# promise的理解，有哪些方法，all的用法及原理
  * 概念：Promise 是异步编程的一种解决方案，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。有三个状态：Pending（进行中）、Resolved（已完成）、Rejected（已拒绝），当把一件事情交给promise时，它的状态就是Pending，任务完成了状态就变成了Resolved、没有完成失败了就变成了Rejected。
  * 特点：
    - 将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。流程更加清晰，代码更加优雅。
    - Promise对象提供统一的接口，使得控制异步操作更加容易。
  * 缺点：
    - 无法取消Promise，一旦新建它就会立即执行，无法中途取消。
    - 如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。
    - 当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）
  
  * 方法：
    - Promise.all()
      - 它接受一个数组作为参数。
      - 数组可以是Promise对象，也可以是其它值，只有Promise会等待状态改变。
      - 当所有的子Promise都完成，该Promise完成，返回值是全部值的数组。
      - 如果有任何一个失败，该Promise失败，返回值是第一个失败的子Promise的结果。
    - Promise.race()
      - 类似于Promise.all() ,区别在于它有任意一个返回成功后，就算完成，但是 进程不会立即停止
    - Promise.resolve()
    - Promise.reject()
  * promise.all原理
    - Promise.all是挂载到Promise类实例上
    - 返回的是一个Promise
    - 需要遍历入参数组中的每一项，判断传入的是不是promise，如果是promise则执行then方法，然后将then方法中的成功回调的data返回，失败则reject
    - 如果入参数组中有基本数值，则直接返回
    - 通过计数器，来判断函数的执行结果
---

# promise 放在try catch里面有什么结果
  > Promise 对象的错误具有冒泡性质，会一直向后传递，直到被捕获为止，也即是说，错误总会被下一个catch语句捕获。当Promise链中抛出一个错误时，错误信息沿着链路向后传递，直至被捕获
---

# Promise 是怎么解决串行和并行的？
---

# Promise.all 是怎么解决并行的？
---

# 既然 JS 是单线程，那么面对网络请求的时候为什么能异步处理
  * JS是单线程语言
  * 由js的执行机制决定的，事件循环。
---

# 什么是事件循环？调用堆栈和任务队列之间有什么区别？
  1. 事件循环: 执行一个宏任务，然后执行清空微任务列表，循环再执行宏任务，再清微任务列表
    * 微任务: promise / ajax 
    * 宏任务: setTimout / script / IO

  2. 堆栈: 先进后出，任务队列: 先进先出
---

# 描述一下EventLoop的执行过程
  * 一开始整个脚本作为一个宏任务执行
  * 执行过程中同步代码直接执行，宏任务进入宏任务队列，微任务进入微任务队列
  * 当前宏任务执行完出队，检查微任务列表，有则依次执行，直到全部执行完
  * 执行浏览器UI线程的渲染工作
  * 检查是否有Web Worker任务，有则执行
  * 执行完本轮的宏任务，回到2，依此循环，直到宏任务和微任务队列都为空
---

# ES6 的类和 ES5 的构造函数有什么区别？
  * 类的内部所有定义的方法，都是不可枚举的
  * 类和模块的内部，默认就是严格模式（this 实际指向的是undefined），非严格模式指向window，所以不需要使用use strict指定运行模式
  * 类不存在变量提升（hoist）
  * 类的方法内部如果含有this，它默认指向类的实例
  * 类的静态方法不会被实例继承，静态方法里的this指的是类，而不是他的实例
  * 类的继承extends，在constructor中使用super，让this指向当前类，等同于Parent.prototype.constructor.call(this)
  * 类的数据类型就是函数，类本身就指向构造函数；使用的时候，也是直接对类使用new命令，跟构造函数的用法完全一致；Object.assign方法可以很方便地一次向类添加多个方法；prototype对象的constructor属性，直接指向“类”的本身，这与 ES5 的行为是一致的；

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

# 数组和伪数组的区别？为什么要设置成伪数组？
---

# 伪数组怎么调用数组的方法
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

# Javascript垃圾回收方法
  * 标记清除
    - 当变量进入执行环境的时候，比如函数中声明一个变量，垃圾回收器将其标记为“进入环境”，当变量离开环境的
时候（函数执行结束）将其标记为“离开环境”。垃圾回收器会在运行的时候给存储在内存中的所有变量加上标记，然后去掉环境中的变量以及被环境中变量所引用的变量（闭包），在这些完成之后仍存在标记的就是要删除的变量了

  * 引用计数
    - 在低版本IE中经常会出现内存泄露，很多时候就是因为其采用引用计数方式进行垃圾回收。引用计数的策略是跟踪记录每个值被使用的次数，当声明了一个变量并将一个引用类型赋值给该变量的时候这个值的引用次数就加1，如果该变量的值变成了另外一个，则这个值得引用次数减1，当这个值的引用次数变为0的时候，说明没有变量在使用，这个值没法被访问了，因此可以将其占用的空间回收，这样垃圾回收器会在运行的时候清理掉引用次数为0的值占用的空间。在IE中虽然JavaScript对象通过标记清除的方式进行垃圾回收，但BOM与DOM对象却是通过引用计数回收垃圾的，也就是说只要涉及BOM及DOM就会出现循环引用问题。
---

# js的垃圾回收原理
  > 在javascript中，如果一个对象不再被引用，那么这个对象就会被GC回收；
  > 如果两个对象互相引用，而不再被第3者所引用，那么这两个互相引用的对象也会被回收。
---

# documen.write和 innerHTML的区别
  * document.write只能重绘整个页面
  * innerHTML可以重绘页面的一部分
---

# 如何判断当前脚本运行在浏览器还是node环境中？
  * 通过判断Global对象是否为window，如果不为window，当前脚本没有运行在浏览器中
---

# 箭头函数和普通函数的区别
  > 箭头函数根本就没有绑定自己的this，在箭头函数中调用 this 时，仅仅是简单的沿着作用域链向上寻找，找到最近的一个 this 拿来使用
---

# 什么是类数组？
---

# arguments 类数组，如何遍历类数组
---

# 箭头函数有super吗？
---

# 简单说明一下你对ES6中箭头函数的理解
  * 概念：是普通函数的简化版
  * 特点：
    - 不能作为构造函数，不能使用new
    - 不能使用argumetns,取而代之用rest参数
    - 不绑定this，会捕获其定义时所在的this指向作为自己的this。由于在vue中自动绑定 this 上下文到实例中，因此不能使用箭头函数来定义一个周期方法。箭头函数的this永远指向上下文的this，call、apply、bind也无法改变
    - 箭头函数没有原型对象
---

# 箭头函数可以new吗 为什么？
  * 不能
  * 原因：
    - new 的本质是生成一个新对象，将对象的_proto_指向函数的prototype,再执行call 方法
    - 普通构造函数通过 new 实例化对象时 this 指向实例对象，而箭头函数没有自己的this值，用call()或者apply()调用箭头函数时，无法对this进行绑定
    - 箭头函数没有 prototype 属性 ，不能作为构造函数，否则 new 命令在执行时无法将构造函数的 prototype 赋值给新的对象的 proto
---

# fastclick 解决点击穿透的原理；
  * 原理是在 目标元素 上绑定touchstart ,touchend事件，然后在touchend结束的时候立马执行click事件，这样就解决了“点透”的问题（实质是事件冒泡导致）以及300ms延迟问题，300ms延迟是因为浏览器为了实现用户双击屏幕放大页面的效果。
---

# fastclick什么作用
  > 移动端浏览器在派发点击事件的时候，通常会出现300ms左右的延迟,就是为了解决这个问题
---

# typeof 和 instanceof、isPrototypeOf 的区别
  > typeof是一个运算符，用于检测数据的类型，比如基本数据类型null、undefined、string、number、boolean，以及引用数据类型object、function，但是对于正则表达式、日期、数组这些引用数据类型，它会全部识别为object； instanceof同样也是一个运算符，它就能很好识别数据具体是哪一种引用类型。它与isPrototypeOf的区别就是它是用来检测构造函数的原型是否存在于指定对象的原型链当中；而isPrototypeOf是用来检测调用此方法的对象是否存在于指定对象的原型链中，所以本质上就是检测目标不同。 
---

# instaneof的原理和实现
  * 只要右边变量的prototype在左边变量的原型链上即可。因此，instanceof在查找的过程中会遍历左边变量的原型链，直到找到右边变量的prototype,如果查找失败，会返回false
---

# for...in 和 for...of 有什么区别？
 1. for … of遍历获取的是对象的键值,for … in 获取的是对象的键名
 2. for … in会遍历对象的整个原型链,性能非常差不推荐使用,而for … of只遍历当前对象不会遍历原型链
 3. 对于数组的遍历,for … in会返回数组中所有可枚举的属性(包括原型链上可枚举的属性),for … of只返回数组的下标对应的属性值
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

# 异步编程的实现方式
  * 回调函数
    - 优点：简单、容易理解
    - 缺点：不利于维护，代码耦合度高
  * 事件监听（采用时间驱动模式，取决于某个事件是否发生）
    - 优点：容易理解，可以绑定多个事件，每个事件可以指定多个回调函数
   - 缺点：事件驱动型，流程不够清晰
  * promise对象
    - 优点：可以利用then方法，进行链式写法，可以书写错误时的回调函数
    - 缺点：编写和理解，相对比较难
  * generator函数
    - 优点：函数体内外的数据交换，错误处理机制
    - 缺点：流程管理不方便
  * async函数
    - 优点：内置执行器、更好的语义、更广的适用性、返回的是promise、结构清晰
    - 缺点：错误的处理机制
---

# DOM事件的绑定的几种方式
  1. 在DOM元素中直接绑定；
    * JavaScript支持在标签中直接绑定事件，语法为：onXXX="JavaScript Code"
  2. 在JavaScript代码中绑定；
    * script标签内绑定事件, 语法为：dom.onXXX=function
  3. 绑定事件监听函数。
    * addEventListener()
---

# JS中的async/await的用法和理解
---

# jquery选择器实现原理
  * jquery原型里面有一个init初始化的方法，将传入的值进行解析，比如传入的id还是class还是标签名。然后通过相应的方法返回数组型对象。既可以通过对象直接调用方法，也可以使用数组的length。
---

# setTimeout和setInterval区别，如何互相实现？
  * setTimeout() 方法用于在指定的毫秒数后调用函数或计算表达式，
  * setInterval()则是在每隔指定的毫秒数循环调用函数或表达式，直到clearInterval把它清除。
  * 也就是说setTimeout()只执行一次，setInterval()可以执行多次。
---

# setTimeout能准时执行吗，为什么，怎么能让它准时执行
  * 不能
  * 因为是单线程, 先执行同步主线程, 再执行异步任务队列
  * 解决：
    1. 动态计算时差
      - 在定时器开始前和运行时动态获取当前时间，在设置下一次定时时长时，在期望值基础上减去当前时延，以获得相对精准的定时运行效果。
      - 此方法仅能消除setInterval()长时间运行造成的误差累计，但无法消除单个定时器执行延迟问题。
    2. 使用 Web Worker
      - Web Worker 的作用，就是为 JavaScript 创造多线程环境，允许主线程创建 Worker 线程，将一些任务分配给后者运行。在主线程运行的同时，Worker 线程在后台运行，两者互不干扰。等到 Worker 线程完成计算任务，再把结果返回给主线程。这样的好处是，一些计算密集型或高延迟的任务，被 Worker 线程负担了，主线程（通常负责 UI 交互）就会很流畅，不会被阻塞或拖慢。
---

# 解释一下settimeout的原理
  * setTimeout的运行机制是，将指定的代码移出本次执行，等到下一轮Event Loop时，再检查是否到了指定时间。如果到了，就执行对应的代码；如果不到，就等到再下一轮Event Loop时重新判断。这意味着，setTimeout指定的代码，必须等到本次执行的所有代码都执行完，才会执行。
  * 每一轮Event Loop时，都会将“任务队列”中需要执行的任务，一次执行完。setTimeout是把任务添加到“任务队列”的尾部。因此，它们实际上要等到当前脚本的所有同步任务执行完，然后再等到本次Event Loop的“任务队列”的所有任务执行完，才会开始执行。由于前面的任务到底需要多少时间执行完，是不确定的，所以没有办法保证，setTimeout指定的任务，一定会按照预定时间执行。
---

# 问jquery的链式调用原理。
  > jq的方法都是挂在原型的，那么如果我们每次在内部方法返回this，也就是返回实例对象，那么我们就可以继续调用原型上的方法了，这样的就节省代码量，提高代码的效率，代码看起来更优雅。
---

# JQuery和Vue有什么区别
  * jq的数据与视图混在一块，Vue的数据与视图分离
  * jq直接用js修改视图，Vue以数据驱动视图
---

# splice和slice的区别
  > slice不会改变原数组，但是splice会直接改变原数组。
---

# 了解过函数式编程嘛
  * 函数式编程是一种编程范式，是一种构建计算机程序结构和元素的风格，它把计算看作是对数学函数的评估，避免了状态的变化和数据的可变。
---

# hash了解吗，用什么来监听hash的变化，原生api有用过吗
  * Hash也称散列、哈希。基本原理就是把任意长度的输入，通过Hash算法变成固定长度的输出。这个映射的规则就是对应的Hash算法，而原始数据映射后的二进制串就是哈希值。
  * 特点：
    - 从hash值不可以反向推导出原始的数据
    - 输入数据的微小变化会得到完全不同的hash值，相同的数据会得到相同的值
    - 哈希算法的执行效率要高效，长的文本也能快速地计算出哈希值
    - hash算法的冲突概率要小
  * 监听方法：
    - 监视hash的变化 onhashchange事件
    - 定时器
  * api: 
    - hash.copy：创建新的 Hash 对象
    - hash.update：使用给定的 data 更新哈希内容，其编码在 inputEncoding 中给出。 如果未提供 encoding，且 data 是字符串，则强制为 'utf8' 编码。 如果 data 是 Buffer、TypedArray 或 DataView，则忽略 inputEncoding。
    - hash.digest：计算传给被哈希的所有数据的摘要。如果提供了 encoding，则将返回字符串；否则返回 Buffer。
---

# es6的迭代器了解吗
  * 迭代器是一个拥有next()方法的特殊对象，每次调用next()都返回一个结果对象。
  * 在ES6中，所有的集合对象（数组、Set集合及Map集合）和字符串都是可迭代对象。
  * 可迭代对象的表现形式为，可以使用 for...of 循环，解构赋值，拓展运算符(spread)，yield* 这些语法来调用 Symbol.iterator 函数。也就是说这些语法糖在被调用时本质上都是在调用 Symbol.iterator 函数。
  * 生成器是一种返回迭代器的函数，通过function关键字后的星号（*）来表示，函数中会用到新的关键字yield。
---

# object.create和new的区别
  * Object.create 是创建一个新对象，使用现有的对象来提供新创建对象的 proto。意思就是生成一个新对象，该新对象的 proto（原型） 指向现有对象。
  * new 生成的是构造函数的一个实例，实例继承了构造函数及其 prototype（原型属性）上的属性和方法。
  * new关键字创建的对象会保留原构造函数的属性，而用Object.create()创建的对象不会。
---

# JS设计模式
  1. 单例模式
  2. 策略模式
  3. 代理模式
  4. 迭代器模式
  5. 发布—订阅模式
  6. 命令模式
  7. 组合模式
  8. 模板方法模式
  9. 享元模式
  10. 职责链模式
  11. 中介者模式
  12. 装饰者模式
  13. 状态模式
  14. 适配器模式
  15. 外观模式
---

# 工厂模式能想到什么应用场景吗？
---

# 了解设计模式么，说说单例模式的优缺点;
  * 单例模式（Singleton），也叫单子模式，是一种常用的软件设计模式。在应用这个模式时，单例对象的类必须保证只有一个实例存在。许多时候整个系统只需要拥有一个的全局对象，这样有利于我们协调系统整体的行为。比如在某个服务器程序中，该服务器的配置信息存放在一个文件中，这些配置数据由一个单例对象统一读取，然后服务进程中的其他对象再通过这个单例对象获取这些配置信息。这种方式简化了在复杂环境下的配置管理。
  * 实现单例模式的思路是：一个类能返回对象一个引用(永远是同一个)和一个获得该实例的方法（必须是静态方法，通常使用getInstance这个名 称）；当我们调用这个方法时，如果类持有的引用不为空就返回这个引用，如果类保持的引用为空就创建该类的实例并将实例的引用赋予该类保持的引用；同时我们 还将该类的构造函数定义为私有方法，这样其他处的代码就无法通过调用该类的构造函数来实例化该类的对象，只有通过该类提供的静态方法来得到该类的唯一实例。 
  * 优点： 
    1. 在单例模式中，活动的单例只有一个实例，对单例类的所有实例化得到的都是相同的一个实例。这样就 防止其它对象对自己的实例化，确保所有的对象都访问一个实例 
    2. 单例模式具有一定的伸缩性，类自己来控制实例化进程，类就在改变实例化进程上有相应的伸缩性。 
    3. 提供了对唯一实例的受控访问。 
    4. 由于在系统内存中只存在一个对象，因此可以 节约系统资源，当 需要频繁创建和销毁的对象时单例模式无疑可以提高系统的性能。 
    5. 允许可变数目的实例。 
    6. 避免对共享资源的多重占用。 
  * 缺点： 
    1. 不适用于变化的对象，如果同一类型的对象总是要在不同的用例场景发生变化，单例就会引起数据的错误，不能保存彼此的状态。 
    2. 由于单利模式中没有抽象层，因此单例类的扩展有很大的困难。 
    3. 单例类的职责过重，在一定程度上违背了“单一职责原则”。 
    4. 滥用单例将带来一些负面问题，如为了节省资源将数据库连接池对象设计为的单例类，可能会导致共享连接池对象的程序过多而出现连接池溢出；如果实例化的对象长时间不被利用，系统会认为是垃圾而被回收，这将导致对象状态的丢失。 
---

# 订阅者模式大致介绍一下
  * 发布/订阅模式(pub/sub模式)指的是消息的发送方可以将消息异步地发送给一个系统中不同组件，而无需知道接收方是谁。在发布/订阅模式中，发送方被称为发布者(Publisher)，接收方则被称作订阅者(Subscriber)。发布者将消息发送到消息队列中，订阅者可以从消息队列里取出自己感兴趣的消息。在发布/订阅模式里，可以有任意多个发布者发送消息，也可以有任意多个订阅者接收消息。
  * 优点：
    - 解耦: 消息的发布者和消息的订阅者在开发的时候完全不需要事先知道对方的存在，可以独立地进行开发。
    - 高伸缩性: 发布/订阅模式中的消息队列可以独立的作为一个数据存储中心存在。在分布式环境中，更是消息队列更是可以扩展至上干个服务器中。
    - 系统组件间通信更加简洁:因为不需要为每一个消息的订阅者准备专门的消息格式，只要知道了消息队列中保存消息的格式，发布者就可以按照这个格式发送消息，订阅者也只需要按照这个格式接收消息。
  * 缺点：
    - 在整个数据模式中，我们不能保证发布者发送的数据一定会送达订阅者。如果要保证数据一定送达的话，需要开发者自己实现响应机制。
  * 适用场景：
    - 系统的发送方需要向大量的接收方广播消息。
    - 系統中某一个组件需要与多个独立开发的组件或服务进行通信，而这些独立开发的组件或服务可以使用不同的编程语言和通信协议。
    - 系统的发送方在向接收方发送消息之后无需接收方进行实时响应。
    - 系统中对数据一致性的要求只需要支持数据的最终一致性(Eventual Consistency)模型。
---

# addeventlistener是什么设计模式
  > 发布订阅
---

# onclick和addEventListener('click',handler)有什么区别
  1. onclick事件在同一时间只能指向唯一对象
  2. addEventListener给一个事件注册多个listener
  3. addEventListener对任何DOM都是有效的，而onclick仅限于HTML
  4. addEventListener可以控制listener的触发阶段，（捕获/冒泡）。对于多个相同的事件处理器，不会重复触发，不需要手动使用removeEventListener清除
---

# 防抖节流，原理是什么，区别是什么
  * 概念：
    - 防抖(debounce)： 触发高频事件后n秒内函数只会执行一次，如果n秒内高频事件再次被触发，则重新计算时间。
    - 节流(throttle)： 高频事件触发，但在n秒内只会执行一次，所以节流会稀释函数的执行频率。
  * 使用场景：
    - window对象的resize、scroll事件
    - 拖拽时的mousemove事件
    - 文字输入、自动完成的keyup事件
    - 射击游戏中的mousedown、keydown事件
    - 实际上对于window的resize事件，实际需求大多为停止改变大小n毫秒后执行后续处理；而其他事件大多的需求是以一定的频率执行后续处理。
  * 区别：函数节流不管事件触发有多频繁，都会保证在规定时间内一定会执行一次真正的事件处理函数，而函数防抖只是在最后一次事件后才触发一次函数。

  * 原理：
    - 节流：每次触发事件时，如果当前有等待执行的延时函数，则直接return
    - 防抖：每次触发事件时设置一个延迟调用方法，并且取消之前的延时调用方法
---

# click和onmousedown和onmouseup的执行顺序
  > onmousedown > onmouseup > click
---

# 说一下你知道的鼠标键盘响应事件方法
  * 键盘事件
    - keydown → keypress → keyup。
  * 鼠标事件
    - mousedown:鼠标按钮被按下（左键或者右键）时触发。不能通过键盘触发。
    - mouseup:鼠标按钮被释放弹起时触发。不能通过键盘触发。
    - click:单击鼠标左键或者按下回车键时触发。这点对确保易访问性很重要，意味着onclick事件处理程序既可以通过键盘也可以通过鼠标执行。
    - dblclick:双击鼠标左键时触发。
    - mouseover:鼠标移入目标元素上方。鼠标移到其后代元素上时会触发。
    - mouseout:鼠标移出目标元素上方。
    - mouseenter:鼠标移入元素范围内触发，该事件不冒泡，即鼠标移到其后代元素上时不会触发。
    - mouseleave:鼠标移出元素范围时触发，该事件不冒泡，即鼠标移到其后代元素时不会触发。
    - mousemove:鼠标在元素内部移到时不断触发。不能通过键盘触发。
---

# 浏览器是多线程的吗？为什么？那js是多线程还是单线程？为什么？
---

# let var和const的区别是什么？什么是暂时性死区？
  * var声明的变量是全局或者整个函数块的，而let,const声明的变量是块级的变量，var声明的变量存在变量提升，let,const不存在，let声明的变量允许重新赋值，const不允许。
  * 暂时性死区：在代码块内，使用let、const命令声明变量之前，该变量都是不可用的。
---

# 块级作用域和函数作用域的区别
  > 函数作用域：变量在定义的函数内及嵌套的子函数内处处可见；块级函数域：变量在离开定义的块级代码后马上被回收。
---

# mockjs和echarts原理了解吗
  * 拦截请求，返回指定的数据，通过改写send方法，使原有的send方法失效
  * ECharts 是一个轻量级的 javascript 图形库， 纯 js 实现， mvc 框架， 数据驱动。ECharts 总体结构是基于 MVC 架构的，各部分的主要作用是：Storage(M)：模型层，实现图形数据的CURD（增删改查）管理；Painter(V)：  视图层，实现canvas 元素的生命周期管理，即：视图渲染、更新控制、绘图；Handler(C)：控制层，事件交互处理，实现完整的dom事件模拟封装。

# Echarts与d3的区别
  1. D3底层是通过svg绘图，学习成本大，echarts底层是用canvas
  2. D3不依赖分辨率，echarts依赖分辨率
  3. D3基于xml绘制图形，可以操作dom，	Echarts基于基于js绘制图形
  4. D3支持事件处理器，echarts不支持事件处理器
  5. D3复杂度高，会减慢页面的渲染速度, echarts能以png或者jpg的格式保存图片
---

# 我们使用函数传参的时候传递的是值还是引用？
  * 值
  * 原因：
    - JS中栈和堆里存储的数据的区别，栈内存中主要存储的是一些基本类型的变量，而堆内存中存储的则是对象类型。引用类型的对象在堆中存储，而地址在栈中存储。
    - 在函数传参的时候实际上是做了两个步骤，先从栈中获取实参对象的值，然后做一个赋值操作，将值赋给形参。在获取基本类型的时候，是从栈中取得基本类型的值，而获取对象类型的时候，从栈中得到的是这个对象类型所指向的地址，然后由这个地址去堆中寻找到对象内属性的值。
---

# better-scroll作用？都用了什么，怎么用的？
  * 作用：一款重点解决移动端（已支持 PC）各种滚动场景需求的插件。
  * scroll方法：参数：{Object} {x, y} 滚动的实时坐标，触发时机：滚动过程中，具体时机取决于选项中的 probeType。
  * scrollEnd方法：参数：{Object} {x, y} 滚动结束的位置坐标，触发时机：滚动结束。
  * refresh方法：重新计算 better-scroll，当 DOM 结构发生变化的时候务必要调用确保滚动的效果正常。
---

# window.onload和DOMContentLoaded的区别
  * 顺序：
    - 一般情况下，DOMContentLoaded事件要在window.onload之前执行，当DOM树构建完成的时候就会执行 DOMContentLoaded事件，而window.onload是在页面载入完成的时候，才执行
  * 区别：
    - 当 onload 事件触发时，页面上所有的DOM，样式表，脚本，图片，flash都已经加载完成了。
    - 当 DOMContentLoaded 事件触发时，仅当DOM加载完成，不包括样式表，图片，flash。
---

# 使用装饰器的好处
  * 装饰器是对类、函数、属性之类的一种装饰，可以针对其添加一些额外的行为。
  * 装饰器的作用就是为已经存在的函数或对象添加额外的功能。
  * 合理利用装饰器可以极大的提高开发效率，对一些非逻辑相关的代码进行封装提炼能够帮助我们快速完成重复性的工作，节省时间。
---

# 拿到对象的 key 有哪些方法，使用 for...in 和 Object.keys 有什么不同
* 使用 for...in 和 Object.keys, 两者之间最主要的区别就是Object.keys( )不会走原型链，而for in 会走原型链；
---

# ==与===的区别，==比较的规则，隐式转换的规则
  * 比较的规则：
    - Number,Boolean,String,Undefined这几种基本类型混合比较时，会将其转换成数字再进行比较 基本类型与复合对象进行比较时，会先将复合对象转换成基本类型（依次调用valueOf与toString方法）再进行比较 undefined被当成基本类型，undefined转换成数字是NaN，因此undefined与除null之外的其它类型值进行比较时始终返回false(注意NaN==NaN返回false) null被当成复合对象，由于null没有valueOf与toString方法，因此和除了undefined之外的其它类型值进行比较时始终返回false 
  * 隐式转换的规则就是==比较的规则
---

# Object.is()与原来的比较操作符"==="、"==” 的区别？
  1. 两等号判等，会在比较时进行类型转换；
  2. 三等号判等(判断严格)，比较时不进行隐式类型转换，(类 型不同则会返回false)；
  3. Object.is 在三等号判等的基础上特别处理了NaN、-0和+0，保证-0和+0不再相同，但Object.is(NaN, NaN)会返回true。
---

# 实现深拷贝的方式有哪些
1. slice 方法实现数组的深拷贝，该方法可以将原数组中抽离部分出来形成一个新数组，只要设置为抽离全部，即可完成数组的深拷贝。
2. concat 方法实现数组的深拷贝，该方法用于连接多个数组组成一个新的数组的方法。那么只要连接它自己，即可完成数组的深拷贝。
3. Object.assign() 拷贝，当对象中只有一级属性，没有二级属性的时候，此方法为深拷贝，但是对象中有对象的时候（有二级属性），此方法就是浅拷贝。
4. JSON.stringify 和 JSON.parse
---

# json解析的底层原理
---

# 请说说js的浅拷贝和深拷贝?
  * 浅拷贝是创建一个新的对象，如果对象的属性是基本属性，拷贝的就是基本类型的值，如果是引用类型，拷贝的是内存地址，如果对象改变了地址，就会影响另一个对象
  * 深拷贝是将一个对象从内存中完整的拷贝一份出来，从堆内存中开辟一个新的区域来存放新对象，所以，修改新对象不会影响旧对象
---

# 用sort好吗，为什么不好？   那你有什么更好的方案？ （提了快排和归并，然后口述快排原理）
  * 不好。
  * 原因：用于对数组的元素进行排序，并返回数组。默认排序顺序是根据字符串UniCode码。
    - 使用排序时你可能会遇到的第一个问题就是当数组中有 null 时。排序方法会强行把空值转换成字符串"null"，而 n 位于字母表大概中间的一个位置。
    - 数字排序需要额外的操作但也能实现
---

# 数组sort（）方法底层原理？用了什么算法？
  * 原理：当没有参数传入的时候，其排序顺序默认为，将待排序数据转换为字符串，并按照Unicode序列排序；当然，比较函数可以自定义，自定义排序函数需要返回值，其返回值为-1，0，1，分别表示`a<b`, `a=b`, `a>b`
  * 算法：sort使用的是插入排序和快速排序结合的排序算法。数组长度不超过10时，使用插入排序。长度超过10使用快速排序。
---

# JSbridge原理, js和native是如何通信的?
  * 原理：JavaScript 是运行在一个单独的 JS Context 中（例如，WebView 的 Webkit 引擎、JSCore）。由于这些 Context 与原生运行环境的天然隔离，我们可以将这种情况与 RPC（Remote Procedure Call，远程过程调用）通信进行类比，将 Native 与 JavaScript 的每次互相调用看做一次 RPC 调用。在 JSBridge 的设计中，可以把前端看做 RPC 的客户端，把 Native 端看做 RPC 的服务器端，从而 JSBridge 要实现的主要逻辑就出现了：通信调用（Native 与 JS 通信） 和句柄解析调用。
  * 如何通信：
    1. JavaScript 调用 Native的方式
      - 注入API 和 拦截URL SCHEME。
        - 注入 API 方式的主要原理是，通过 WebView 提供的接口，向 JavaScript 的 Context（window）中注入对象或者方法，让 JavaScript 调用时，直接执行相应的 Native 代码逻辑，达到 JavaScript 调用 Native 的目的。
        - 拦截 URL SCHEME 的主要流程是：Web 端通过某种方式（例如 iframe.src）发送 URL Scheme 请求，之后 Native 拦截到请求并根据 URL SCHEME（包括所带的参数）进行相关操作。
    2. Native 调用 JavaScript 的方式
      - Native调用JavaScript较为简单，直接执行拼接好的 JavaScript 代码即可。因此JavaScript的方法必须在全局的window上。
---

# 介绍下argument
  * arguments对象是比较特别的一个对象，实际上是当前函数的一个内置属性。可以用数组的[i]和.length。
  * 每个函数都有一个arguments属性，表示函数的实参集合。
  * arguments对象不能显式的创建，它只有在函数开始时才可用。
  * arguments对象的长度是由实参个数而不是形参个数决定的。
  * 如何将arguments转成了一个实实在在的数组：Array.prototype.slice.call(arguments)能将具有length属性的对象转成数组
---

# es6 map和普通对象自变量有什么区别
  * 对象的key是字符串或者是Symbol，map的key可以是任何类型；
  * object获取键值使用Object.keys（返回数组）；Map获取键值使用 map变量.keys() (返回迭代器)。
  * 我们可以借用arguments.callee来代替函数本身实现递归
  * 可以借用arguments.length可以来查看实参和形参的个数是否一致。
---

# weakSet和weakMap的区别
  * WeakSet成员都是对象;成员都是弱引用，可以被垃圾回收机制回收，可以用来保存DOM节点，不容易造成内存泄漏;不能遍历，方法有add、delete、has。
  * weakMap只接受对象作为键名（null除外），不接受其他类型多人值作为键名;键名是弱引用，键值可以是任意的，键名所指向的对象可以被垃圾机制回收，此时键名是无效的;不能遍历，方法有get、set、has、delete。
---

# weakMap与map的区别
  1. WeakMap只接受对象作为key，如果设置其他类型的数据作为key，会报错。
  2. WeakMap的key所引用的对象都是弱引用，只要对象的其他引用被删除，垃圾回收机制就会释放该对象占用的内存，从而避免内存泄漏。
  3. 由于WeakMap的成员随时可能被垃圾回收机制回收，成员的数量不稳定，所以没有size属性。
  4. WeakMap没有clear()方法
  5. WeakMap不能遍历
---

# Weakset与set的区别
  * WeakSet 只能储存对象引用，不能存放值，而 Set 对象都可以
  * WeakSet 对象中储存的对象值都是被弱引用的，即垃圾回收机制不考虑 WeakSet 对该对象的应用，如果没有其他的变量或属性引用这个对象值，则这个对象将会被垃圾回收掉（不考虑该对象还存在于 WeakSet 中），所以，WeakSet 对象里有多少个成员元素，取决于垃圾回收机制有没有运行，运行前后成员个数可能不一致，遍历结束之后，有的成员可能取不到了（被垃圾回收了），WeakSet 对象是无法被遍历的（ES6 规定 WeakSet 不可遍历），也没有办法拿到它包含的所有元素
---

# map和set的区别
  * 共同点: set和map可以存储不重复的值
  * 区别：set是以 [value, value]的形式储存元素，map是以 [key, value] 的形式储存
---

# 为什么Set能去重
  * set的去重是通过两个函数__hash__和__eq__结合实现的。
    - 当两个变量的哈希值不相同时，就认为这两个变量是不同的
    - 当两个变量哈希值一样时，调用__eq__方法，当返回值为True时认为这两个变量是同一个，应该去除一个。返回FALSE时，不去重
---

# map数据类型和set数据类型
  * map
    - 介绍：键/值对的集合。集合中的键和值可以是任何类型。和 JSON 对象类似，但是键不仅可以是字符串还可以是对象
    - 常见方法：
      - get: 获取某一个属性值。
      - set: 添加一个值。
      - has: 判断是否有某一个属性值。
      - dlete: 删除一个值。
      - clear: 清除所有值。
      - size: 获取属性个数。
  * set
    - 介绍：Set类似于数组，但是它里面每一项的值是唯一的，没有重复的值，Set本身是一个构造函数，用来生成set的数据结构
    - 常见方法：
      - add(value)：添加某个值，返回 Set 结构本身。
      - delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
      - has(value)：返回一个布尔值，表示该值是否为Set的成员。
      - clear()：清除所有成员，没有返回值。
---

# 介绍Object.assign的使用
  * Object.assign方法用来将源对象（source）的所有可枚举属性，复制到目标对象（target）。它至少需要两个对象作为参数，第一个参数是目标对象，后面的参数都是源对象。
  * 如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。
  * 如果只有一个参数，Object.assign会直接返回该参数。
  * 如果该参数不是对象，则会先转成对象，然后返回。
  * 由于undefined和null无法转成对象，所以如果它们作为参数，就会报错。
---

# es6新增哪些对象？多了哪些类型？
  1. 字符串拓展：
    * includes()：返回布尔值，表示是否找到了参数字符串。
    * startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
    * endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。 
    * 可以使用模板字符串
  2. 结构表达式
  3. 函数优化
    * 可以设置函数参数默认值
    * 新增箭头函数
    * 数组中新增了map和reduce方法。
      - reduce()：会从左到右依次把数组中的元素用reduce处理，并把处理的结果作为下次reduce的第一个参数。接收一个函数（必须）和一个初始值（可选），该函数接收两个参数：
        - 第一个参数是上一次reduce处理的结果
        - 第二个参数是数组中要处理的下一个元素
      - map()：接收一个函数，将原数组中的所有元素用这个函数处理后放入新数组返回。
  4. 新增promise对象
  5. 提供了Set和Map的数据结构。
  6. 新增模块化
  7. 对象扩展：
    * keys(obj)：获取对象的所有key形成的数组
    * values(obj)：获取对象的所有value形成的数组
    * entries(obj)：获取对象的所有key和value形成的二维数组。格式：[[k1,v1],[k2,v2],...]
    * assign(dest, ...src) ：将多个src对象的值 拷贝到 dest中（浅拷贝）。
  8. 数组扩展：
    * find(callback)：把数组中的元素逐个传递给函数callback执行，如果返回true，则返回该元素
    * findIndex(callback)：与find类似，不过返回的是品牌到的元素的索引
    * includes（callback）：与find类似，如果匹配到元素，则返回true，代表找到了。 
---

# 谈谈你对es6的理解
  * 新增模板字符串（为javascript提供了简单的字符串插值功能）
  * 箭头函数
  * for-of（用来遍历数据——例如数组中的值）
  * arguments对象可被不定参数和默认参数完美代替
  * es6将promise对象纳入规范，提供了原生的promise对象
  * 增加了let和const命令，用来声明变量
  * 引入了module模块的概念
---

# new String('a')和'a'是一样的么?
  * 不完全一样。
  * 通过String直接创建的字符串和字符串表面量为基本数据类型。
  * 通过 New String来实例化的是一个String对象， 所以我们可以调用String对象的方法。
  * 使用三个等号不全等
    ```
      var a = "foo";
      var b = new String("foo");
      var c = String("foo");

      console.log(typeof a); // >"string"
      console.log(typeof b); // >"object"
      console.log(typeof c); // >"string"

      console.log(a == b);  // >true
      console.log(a.repeat === b.repeat); //> true
      console.log(a === b); // >false
      console.log(a === c); // >true
    ```
---

# js的各种位置，比如clientHeight,scrollHeight,offsetHeight ,以及scrollTop, offsetTop,clientTop的区别？
  * clientHeight：表示的是可视区域的高度，不包含border和滚动条
  * offsetHeight：表示可视区域的高度，包含了border和滚动条
  * scrollHeight：表示了所有区域的高度，包含了因为滚动被隐藏的部分。
  * clientTop：表示边框border的厚度，在未指定的情况下一般为0
  * scrollTop：滚动后被隐藏的高度，获取对象相对于由offsetParent属性指定的父坐标(css定位的元素或body元素)距离顶端的高度。
---

# 插入几万个 DOM，如何实现页面不卡顿？
  1. 分页，懒加载，把数据分页，然后每次接受一定的数据，避免一次性接收太多
  2. 使用setInterval，setTimeout，requestAnimationFrame等，分批渲染，让数据在不同帧内去做渲染
  3. 使用 virtual-scroll，虚拟滚动。 这种方式是指根据容器元素的高度以及列表项元素的高度来显示长列表数据中的某一个部分，而不是去完整地渲染长列表，以提高无限滚动的性能
    * virtual-scroll原理：
      - 计算当前可见区域起始数据的 startIndex
      - 计算当前可见区域结束数据的 endIndex
      - 计算当前可见区域的数据，并渲染到页面中
      - 计算 startIndex 对应的数据在整个列表中的偏移位置 startOffset，并设置到列表上
      - 计算 endIndex 对应的数据相对于可滚动区域最底部的偏移位置 endOffset，并设置到列表上
    * startOffset 和 endOffset 会撑开容器元素的内容高度，让其可持续的滚动；此外，还能保持滚动条处于一个正确的位置。
---

# 通过什么做到并发请求?
  > 使用异步Prmosie All或者web worker
---

# 讲讲 window.history 的原理, 为什么 popState 能够实现前端路由
---

# 数组中的方法如何实现 break
---

# 比较常用的数组方法 map() reduce() find() findIndex() push() .... 哪些可以改变原数组，那些不可以改变
---

# 如果在Object.keys()中传入一个字符串输出什么？为什么？
---

# isNaN 和 Number.isNaN 函数的区别？
  * 函数 isNaN 接收参数后，会尝试将这个参数转换为数值，任何不能被转换为数值的的值都会返回 true，因此非数字值传入也会返回 true ，会影响 NaN 的判断。
  * 函数 Number.isNaN 会首先判断传入参数是否为数字，如果是数字再继续判断是否为 NaN ，不会进行数据类型的转换，这种方法对于 NaN 的判断更为准确。
---