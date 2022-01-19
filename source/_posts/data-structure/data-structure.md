---
title: 数据结构与算法
date: 2021-10-21 15:49:46
tags: 
  - 数据结构与算法
type: 数据结构与算法                                                                 # 标签、分类
description: 记录一些算法题的结题过程
top_img: /images/vue/vue.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - vue                                                                 # 文章标签
cover: /images/vue/vue.jpg                 # 文章的缩略图（用在首页）
---

# 数据结构与算法是什么？
  * 数据结构：计算机储存、组织数据的方式
  * 算法：一系列解决问题的清晰指令
  * 程序 = 数据结构 + 算法
  * 数据结构为算法提供服务，算法围绕数据结构操作

# 将要学习的数据结构
  * 栈、队列、链表（有序、串联）
  * 集合、字典（无序）
  * 树、堆、图

# 将要学习的算法
  * 链表：遍历链表、删除链表节点
  * 树、图：深度/广度优先遍历
  * 数组：冒泡/选择/插入/归并/快速排序、顺序/二分搜索

# 时间复杂度是什么？
  * 一个函数，用大O 表示，比如O(1)、O(n)、O(logN)...
  * 定性描述该算法的运行时间
  * ![复杂度的图](/images/dataStructure/时间复杂度.jpg)

## O(1)
  * 代码：
    ```
      let i = 0;
      i += 1;
    ```
  * 解析：每次执行这两行代码的时候，这两行代码永远只执行一次，没有任何循环什么的东西，所以它的时间复杂度是O(1)

## O(n)
  * 代码：
    ```
      for(let i = 0; i < n; i += 1) {
        console.log(i)
      }
    ```
  * 解析：因为for循环里面的代码，执行了n次

## O(1) + O(n) = O(n)
  * 代码：
    ```
      let i = 0;
      i += 1;
      for(let i = 0; i < n; i += 1) {
        console.log(i)
      }
    ```
  * 解析：如果2个时间复杂度先后排列，则相加，取增长趋势更大的那个，因为n足够大的时候，1就忽略不计了

## O(n) * O(n) = O(n^2)
  * 代码：
    ```
      for(let i=0; i<n; i+=1) {
        for(let j=0; j<n; j+=1) {
          console.log(i, j)
        }
      }
    ```
  
## O(logN)
  * 代码：
    ```
      let i = 1;
      while(i < n) {
        console.log(i)
        i *=2
      }
    ```
  * 解析：由于i每次在乘以2之后都会更加逼近n，也就是说，在有x次后，i将会大于n从而跳出循环，所以2x=n, 也就是x=log2n，所以这个循环的复杂度为O(logn)

# 空间复杂度是什么？
  * 一个函数，用大O表示，比如O(1)、O(n)、O(n^2)...
  * 算法在运行过程中临时占用存储空间大小的量度

## O(1)
  * 代码：
    ```
      let i = 0;
      i += 1;
    ```
  * 解析：每次执行这两行代码的时候，这两行代码永远只执行一次，没有任何循环什么的东西，所以它的空间复杂度是O(1)

## O(n)
  * 代码：
    ```
      let list = []
      for(let i = 0; i < n; i += 1) {
        list.push(i)
      }
    ```
  * 解析：因为for循环里面的代码，执行了n次

## O(n^2)
  * 代码：
    ```
      let matrix = []
      for(let i = 0; i < n; i += 1) {
        matrix.push([])
        for(let j=0;j<n;j +=1) {
          matrix[i].push(j)
        }
      }
    ```

# 栈结构
  > 定义：一个**后进先出**的数据结构。![栈](/images/dataStructure/栈.jpg)
  > 特点：先进后出

## 栈常见有哪些操作？
  * push：添加一个新元素到栈顶
  * pop: 移除栈顶的元素，同时返回被移除的元素
  * peek: 返回栈顶的元素，不对栈做任何修改(这个方法不会移除栈顶的元素，仅仅返回他)
  * isEmpty: 如果栈里没有任何元素返回true, 否则返回false
  * size: 返回栈里的元素个数。这个方法和数组的length属性很类似
  * toString: 将栈结构的内容以字符形式返回

## js模拟栈
  * 代码实现：
    ```
      // 封装栈类
      function Stack() {
        // 栈中的属性
        this.items = []

        // 栈的相关操作
        // 1. 将元素压入栈
        Stack.prototype.push = function(element) {
          this.items.push(element)
        }
        // 2. 从栈中取出元素
        Stack.prototype.pop = function() {
          return this.items.pop()
        }

        // 3. 查看一下栈顶元素
        Stack.prototype.peek = function() {
          return this.items[this.items.length - 1]
        }

        // 4. 判断栈是否为空
        Stack.prototype.isEmpty = function() {
          return this.items.length === 0
        }

        // 5. 获取栈中元素的个数
        Stack.prototype.size = function() {
          return this.items.length
        }

        // 6. toString方法
        Stack.prototype.toString = function() {
          var resultString = ""
          for(var i = 0; i < this.items.length; i++) {
            resultString += this.items[i] + ''
          }
          return resultString
        }
      }

      // 栈的使用
      var s = new Stack()
      s.push(1)
      s.push(20)
      s.push(17)
      alert(s)

      s.pop()
      alert(s)

      alert(s.peek())
      alert(s.isEmpty())
      alert(s.size())
    ```

## 栈的应用场景
  * 需要后进先出的场景。
  * 比如：十进制转二进制、判断字符串括号是否有效、函数调用堆栈...

### 场景一：十进制转二进制
  * 如图所示：![十进制转二进制](/images/dataStructure/十进制转二进制.jpg)

### 场景二：有效的括号
  * 如图所示：![有效的括号](/images/dataStructure/有效的括号.jpg)

### 函数调用堆栈
  * 如图所示：![函数调用堆栈](/images/dataStructure/函数调用堆栈.jpg)

## 题目1：
  ```
    # 问：有6个元素6,5,4,3,2,1的顺序进栈，问下列哪一个不是合法的出栈顺序？
      A. 5 4 3 6 1 2   B. 4 5 3 2 1 6   C. 3 4 6 5 2 1   D. 2 3 4 1 5 6

    # 答：选C。
    # 解析：
      A选项第一个是5，65进栈, 5出栈，4进栈出栈，3进栈出栈，6出栈，1进栈出栈，2进栈出栈。
      B选项第一个是4，654进栈, 4出栈，5出栈，3进栈出栈，2进栈出栈，1进栈出栈，6出栈
      C选项第一个是3，6543进栈, 3出栈，4出栈，5出栈，所以C错了
      D选项第一个是2, 65432进栈，2出栈，3出栈，4出栈，1进栈出栈，5出栈，6出栈
  ```

## 题目2：leetcode20题：有效的括号
  * 题目路径：https://leetcode-cn.com/problems/valid-parentheses/
  * 描述：
    ```
      给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

      有效字符串需满足：

      左括号必须用相同类型的右括号闭合。
      左括号必须以正确的顺序闭合。
    ```
  * 结题思路：
    - 对于没有闭合的左括号而言，越靠后的左括号，对应的右括号越靠前
    - 满足后进先出，考虑用栈
      ```
        输入：s = "()"
        输出：true
      ```
  * 结题步骤：
    - 新建一个栈。
    - 扫描字符串，遇到左括号入栈，遇到和栈顶括号类型匹配的右括号就出栈，类型不匹配直接定为不合法
    - 最后栈空了就合法，否则不合法
  * 编写代码：
    ```
      var isValid = function(s) {
        if(s.length % 2 === 1) {
          return false
        }
        const stack = []
        for(let i=0; i<s.length; i++) {
          const c = s[i]
          if(c === "(" || c === "{" || c === "[") {
            stack.push(c)
          } else {
            // 拿到栈顶元素
            const t = stack[stack.length - 1]
            if(
              (t === "(" && c === ")") || (t === "{" && c === "}") || (t === "[" && c === "]")
            ) {
              stack.pop()
            } else {
              return false
            }
          }
        }
        return stack.length === 0;
      }
    ```
  * 时间复杂度是O(n)、空间复杂度也是O(n)

## 题目3：leetcode94: 

## 思考题：
  1. 请用ES6的class, 封装一个stack类，包括push、pop、peek方法
  2. 请用栈这个数据结构，将100这个十进制数字转为二进制

# 队列
  * 定义：一个**受限的、先进先出**的数据结构
    - 受限之处在于它只允许在表的前端进行删除操作
    - 而在表的后端进行插入操作
  * javascript中没有队列，但可以用array实现队列的所有功能

## 队列的常见操作
  * enqueue: 向队列尾部添加一个（或多个）新的项
  * dequeue: 移除队列的第一（即排在队列最前面的）项，并返回被移除的元素
  * front: 返回队列中的第一个元素——最先被添加，也将是最先被移除的元素。队列不做任何变动
  * isEmpty: 如果队列不包含任何元素，返回true，否则返回false
  * size: 返回队列包含的元素个数，与数组的length属性类似
  * toString: 将队列中的内容，转成字符串形式

## js封装队列
  * 代码编写
    ```
      // 封装队列类
      function Queue() {
        // 属性
        this.items = []

        // 队列的相关操作
        // 1. 将元素加入到队列
        Queue.prototype.enqueue = function(element) {
          this.items.push(element)
        }
        // 2. 从队列中删除前端元素
        Queue.prototype.dequeue = function() {
          return this.items.shift()
        }

        // 3. 查看前端的元素
        Queue.prototype.front = function() {
          return this.items[0]
        }

        // 4. 判断队列是否为空
        Queue.prototype.isEmpty = function() {
          return this.items.length === 0
        }

        // 5. 获取队列中元素的个数
        Queue.prototype.size = function() {
          return this.items.length
        }

        // 6. toString方法
        Queue.prototype.toString = function() {
          var resultString = ""
          for(var i = 0; i < this.items.length; i++) {
            resultString += this.items[i] + ''
          }
          return resultString
        }
      }

      // 队列的使用
      var s = new Queue()
      // 添加队列
      s.enqueue(1)
      s.enqueue(20)
      s.enqueue(17)
      alert(s)
      // 删除元素
      s.dequeue()
      alert(s)
      
      alert(s.front())
      alert(s.size())
      alert(s.isEmpty())
    ```

## 队列的使用场景
  * 需要先进先出的场景
  * 比如：食堂排队打饭、js异步中的任务队列、计算最近请求次数

## 题目1：leetcode933题 最近的请求次数
  * 题目路径：https://leetcode-cn.com/problems/number-of-recent-calls/
  * 描述：
    ```
      写一个 RecentCounter 类来计算特定时间范围内最近的请求。

      请你实现 RecentCounter 类：

        RecentCounter() 初始化计数器，请求数为 0 。
        int ping(int t) 在时间 t 添加一个新请求，其中 t 表示以毫秒为单位的某个时间，并返回过去 3000 毫秒内发生的所有请求数（包括新请求）。确切地说，返回在 [t-3000, t] 内发生的请求数。
        保证 每次对 ping 的调用都使用比之前更大的 t 值。
    ```
  * 解题思路：
    - 越早发出的请求越早不在最近，3000ms内的请求里
    - 满足先进先出，考虑用队列
  * 代码编写：
    ```
      var RecentCounter = function () {
        this.q = [];
      }

      RecentCounter.prototype.ping = function(t) {
        this.q.push(t);
        while(this.q[0] < t - 3000) {
          this.q.shift();
        }
      }
      return this.q.length;
    ```
## 思考题：
  1. 请用ES6的class,封装一个Quene类，包括push、shift、peek方法
  2. 请用队列这个数据结构结合React或者vue写一个任务app, 包括添加任务、完成任务功能，要求任务只能先进先出

# 链表是什么？
  * 定义：多个元素组成的列表
  * 元素存储不连续，用next指针连在一起

## 数组与链表的区别
  * 数组：增删非首尾元素时往往需要移动元素
  * 链表：增删非首尾原色，不需要移动元素，只需要更改this指向即可

## 链表常见操作
  * append(element): 向队列尾部添加一个新的项
  * insert(position, element): 向列表的特定位置插入一个新的项
  * get(position): 获取对应位置的元素
  * indexOf(element): 返回元素在列表中的索引。如果列表中没有该元素则返回-1
  * updae(positionn): 修改某个位置的元素
  * removeAt(position): 从列表的特定位置移除一项
  * remove(element): 从列表中移除一项
  * isEmpty(): 如果列表中不包含任何元素，返回true, 如果链表长度大于0则返回false
  * size(): 返回链表包含的元素个数，与数组的length属性类似
  * toString(): 由于列表项使用了Node类，就需要重写集成至js对象默认的toString方法，让其只输出元素的值

## 题目1：leetcode237题 删除链表中的节点
  * 题目路径：https://leetcode-cn.com/problems/delete-node-in-a-linked-list/
  * 描述：
    ```
      请编写一个函数，使其可以删除某个链表中给定的（非末尾）节点。传入函数的唯一参数为 要被删除的节点 。

      现有一个链表 -- head = [4,5,1,9]，它可以表示为:
    ```
  * 解题思路：
    - 无法直接获取被删除节点的上个节点
    - 将被删除节点转移到下个节点
  * 代码编写：
    ```
      var deleteNode = function(node) {
        node.val = node.next.val;
        node.next = node.next.next;
      }
    ```

## 题目2：leetcode206 反转列表
  * 题目路径：https://leetcode-cn.com/problems/reverse-linked-list/
  * 题目描述：
    ```
      给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。

      示例1：
        输入：head = [1,2,3,4,5]
        输出：[5,4,3,2,1]
    ```
  * 解题思路：
    - 反转两个节点：将n+1的next指向n
    - 反转多个节点：双指针遍历链表，重复商都操作
  * 解题步骤：
    - 双指针一前一后遍历链表
    - 反转双指针
  * 编写代码：
    ```
      var reverseList = function(head) {
        let p1 = head;
        let p2 = null;
        while(p1) {
          const tmp = p1.next;
          p1.next = p2;
          p2 = p1;
          p1 = tmp
        }
        return p2;
      }
    ```

## 题目3：leetCode2 两数相加
  * 题目路径：https://leetcode-cn.com/problems/add-two-numbers/
  * 题目描述：
    ```
      给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

      请你将两个数相加，并以相同形式返回一个表示和的链表。

      你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

      实例：输入：l1 = [2,4,3], l2 = [5,6,4]
      输出：[7,0,8]
      解释：342 + 465 = 807.
    ```
  * 解题思路：
    - 小学数学题，模拟相加操作
    - 需要遍历链表
  * 解题步骤：
    - 新建一个空链表
    - 遍历被相加的两个链表，模拟相加操作，将个位数追加到新链表上，将十位数留到下一位去相加
  * 代码编写：
    ```
      var addTwoNumbers = function(l1,l2) {
        const l3 = new ListNode(0);
        let p1 = l1;
        let p2 = l2;
        let p3 = l3;
        let carry = 0;
        while(p1 || p2) {
          const v1 = p1 ? p1.val : 0
          const v2 = p2 ? p2.val : 0
          const val = v1 + v2 + carry
          carry = Math.floor(val / 10)
          p3.next = new ListNode(val % 10)
          if (p1) p1 = p1.next;
          if(p2) p2 = p2.next;
          p3 = p3.next;
        }
        if(carry) {
          p3.next = new ListNode(carry)
        }
        return l3.next;
      }
    ```

# 集合是什么？
  * 定义：集合是一种无序且唯一的数据结构
  * ES6中有集合，名为Set
  * 集合的常用操作：去重、判断某元素是否在集合中、求交集

# 字典是什么？
  * 与集合类似，字典也是一种存储唯一值的数据结构，但他对键值对的形式来存储
  * ES6中有字典，名为map
  * 字典的常用操作：键值对的增删改查

# 树是什么？
  * 一种分层数据的抽象模型
  * 前端工作中常见的树包括：DOM树，级联选择、树形控件
  * js中没有树，但是可以用Object和Array构建树
  * 树的常用操作：深度/广度优先遍历、先中后序遍历

## 二叉树是什么？
  * 树中每个节点最多只能有2个子节点
  * 在js中通常用Object来模拟树

### 二叉树先序遍历
  * 思路：![二叉树先序遍历](/images/dataStructure/二叉树先序遍历.jpg)

### 二叉树中序遍历
  * 思路：![二叉树中序遍历](/images/dataStructure/二叉树中序遍历.jpg)


### 二叉树后序遍历算法
  * 思路：![二叉树后序遍历](/images/dataStructure/二叉树后序遍历.jpg)

# 图是什么？
  * 图是网络结构的抽象模型，是一组由边连接的节点
  * 图可以表示任何二元关系，比如道路、航班......
  * js没有图，但是可以用Object和array构件图
  * 图的表示法：邻接矩阵、邻接表、关联矩阵......

# 堆是什么？
  * 堆是一种特殊的完全二叉树
  * 所有的节点都大于等于（最大堆）或小于等于（最小堆）他的子节点

## js中的堆
  * ![js中的堆](/images/dataStructure/堆.jpg)