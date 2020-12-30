---
title: 面试——css篇
date: 2020-12-29 11:25:27
tags: 
  - 面试
  - css
type: 面试                                                                 # 标签、分类
description:  Vue是一套用于构建用户界面的渐进式框架。
top_img: https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=2908530132,3956932538&fm=26&gp=0.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 面试
  - css                                                                 # 文章标签
cover: https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=2908530132,3956932538&fm=26&gp=0.jpg                 # 文章的缩略图（用在首页）
---

# css 优先级
  ```
    答：important > 内联 > ID选择器 > 类选择器 > 标签选择器
  ```
---

# 避免 css 全局污染。
---

# 页面导入样式时，使用link和@import有什么区别？
  1. link属于XHTML标签，除了加载CSS外，还能用于定义RSS, 定义rel连接属性等作用；而@import是CSS提供的，只能用于加载CSS;
  2. 页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载;
  3. import是CSS2.1 提出的，只在IE5以上才能被识别，而link是XHTML标签，无兼容问题;
  4. link支持使用js控制DOM去改变样式，而@import不支持;
---

# 介绍一下标准的CSS的盒子模型？低版本IE的盒子模型有什么不同的？
  1. 有两种， IE 盒子模型、W3C 盒子模型；
  2. 盒模型： 内容(content)、填充(padding)、边界(margin)、 边框(border)；
  3. 区  别： IE的content部分把 border 和 padding计算了进去;
---

# CSS选择符有哪些？哪些属性可以继承？
  * id选择器（ # myid）
  * 类选择器（.myclassname）
  * 标签选择器（div, h1, p）
  * 相邻选择器（h1 + p）
  * 子选择器（ul > li）
  * 后代选择器（li a）
  * 通配符选择器（ * ）
  * 属性选择器（a[rel = "external"]）
  * 伪类选择器（a:hover, li:nth-child）
  * **继承的样式： font-size font-family color, UL LI DL DD DT;**
  * **不可继承的样式：border padding margin width height ;**
---

# display有哪些值？说明他们的作用
| 值           | 含义                                                       |
| ------------ | ---------------------------------------------------------- |
| block        | 块类型。默认宽度为父元素宽度，可设置宽高，换行显示。       |
| none         | 元素不显示，并从文档流中移除。                             |
| inline       | 行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示。 |
| inline-block | 默认宽度为内容宽度，可以设置宽高，同行显示。               |
| list-item    | 象块类型元素一样显示，并添加样式列表标记。                 |
| table        | 此元素会作为块级表格来显示。                               |
| inherit      | 规定应该从父元素继承 display 属性的值。                    |

---

# CSS3有哪些新特性？
```
  新增各种CSS选择器	（: not(.input)：所有 class 不是“input”的节点）

  圆角		    （border-radius:8px）

  多列布局	    （multi-column layout）

  阴影和反射	（Shadow\Reflect）

  文字特效		（text-shadow、）

  文字渲染		（Text-decoration）

  线性渐变		（gradient）

  旋转		 	（transform）

  缩放,定位,倾斜,动画,多背景

  例如:transform:\scale(0.85,0.90)\ translate(0px,-30px)\ skew(-9deg,0deg)\Animation:
```

# 经常遇到的浏览器的兼容性有哪些？
---

# 为什么要初始化CSS样式?
  * 因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。
---

# 对BFC规范(块级格式化上下文：block formatting context)的理解？
  * 概念：
    1. （W3C CSS 2.1 规范中的一个概念,它是一个独立容器，决定了元素如何对其内容进行定位,以及与其他元素的关系和相互作用。）

    2. 一个页面是由很多个 Box 组成的,元素的类型和 display 属性,决定了这个 Box 的类型。

    3. 不同类型的 Box,会参与不同的 Formatting Context（决定如何渲染文档的容器）,因此Box内的元素会以不同的方式渲染,也就是说BFC内部的元素和外部的元素不会互相影响。

  * 补充: 
      1. 如何创建BFC ?
        - 根元素或包含根元素的元素
  	    - 浮动元素 （float不是none）
  	    - 绝对定位元素 （position取值为absolute或fixed）
  	    - display取值为inline-block,table-cell, table-caption,flex, inline-flex之一的元素
  	    - overflow ＝ hidden | auto 或 scroll (≠ visible)
  
      2. BFC的特性
        - BFC 是一个独立的容器，容器内子元素不会影响容器外的元素。反之亦如此。
  	    - 盒子从顶端开始垂直地一个接一个地排列，盒子之间垂直的间距是由 margin 决定的。
  	    - 在同一个 BFC 中，两个相邻的块级盒子的垂直外边距会发生重叠。
  	    - BFC 区域不会和 float box 发生重叠。
  	    - BFC 能够识别并包含浮动元素，当计算其区域的高度时，浮动元素也可以参与计算了。
      3. BFC的作用
        - 可以包含浮动元素
        - 不被浮动元素覆盖
        - 阻止父子元素的margin折叠
      
  * 详细请看: https://segmentfault.com/a/1190000013647777
---

# 浏览器是怎样解析CSS选择器的？
  * 按照从上到下,从右到左的顺序解析。
---

# 什么是 FOUC（无样式内容闪烁）？你如何来避免 FOUC？
  1. 概念: 文档样式闪烁
    
  2. 造成原因:`<style type="text/css" media="all">@import "../fouc.css";</style>` 而引用CSS文件的@import就是造成这个问题的罪魁祸首。IE会先加载整个HTML文档的DOM，然后再去导入外部的CSS文件，因此，在页面DOM加载完成到CSS导入完成中间会有一段时间页面上的内容是没有样式的，这段时间的长短跟网速，电脑速度都有关系。

  3. 解决方法: 只要在`<head>`之间加入一个`<link>`或者`<script>`元素就可以了。

# 知道css有个content属性吗？有什么作用？有什么应用？
  * css的content属性专门应用在 before/after 伪元素上，用于来插入生成内容。最常见的应用是利用伪类清除浮动。
---

# px和em的区别。
```
  <1>.px和em都是长度单位，区别是，px的值是固定的，指定是多少就是多少，计算比较容易。em得值不是固定的，并且em会继承父级元素的字体大小。

  <2>.浏览器的默认字体高都是16px。所以未经调整的浏览器都符合: 1em=16px。那么12px=0.75em, 10px=0.625em。

  补充: em与rem，vw,vh的区别
	
	em参考物是父元素的font-size，rem参考物是根元素的font-size, vw,vh参考物是窗口
```
---

# rem布局的原理和优缺点
  * 原理:  rem是根据html的font-size大小来变化, 在每一个设备下根据设备的宽度设置对应的html字号，从而实现了自适应布局

  * 优点: 他能让我们在手机各个机型的适配方面；大大减少我们代码的重复性，是我们的代码更兼容。

  * 缺点: 
    1. 目前ie不支持 对pc页面来讲使用次数不多；
    2. 数据量大：所有的图片，盒子都需要我们去给一个准确的值；才能保证不同机型的适配；

# rgba()和opacity的透明效果有什么不同？
  * opacity作用于元素，以及元素内的所有内容的透明度，而rgba()只作用于元素的颜色或其背景色。（设置rgba透明的元素的子元素不会继承透明效果！）
---

# 使用 CSS 预处理的优缺点分别是什么？
  * 优点：
    1. 提高 CSS 可维护性。
    2. 易于编写嵌套选择器。
    3. 引入变量，增添主题功能。可以在不同的项目中共享主题文件。
    4. 通过混合（Mixins）生成重复的 CSS。
    5. Splitting your code into multiple files. CSS files can be split up too but doing so will require a HTTP request to download each CSS file.将代码分割成多个文件。不进行预处理的 CSS，虽然也可以分割成多个文件，但需要建立多个 HTTP 请求加载这些文件。 

  * 缺点：
    1. 需要预处理工具。
    2. 重新编译的时间可能会很慢 
---

# 如果需要手动写动画，你认为最小时间间隔是多久，为什么？（阿里）
  * 多数显示器默认频率是60Hz，即1秒刷新60次，所以理论上最小间隔为1/60＊1000ms ＝ 16.7ms
---

# 怎么让Chrome支持小于12px 的文字？
  1. 用图片：如果是内容固定不变情况下，使用将小于12px文字内容切出做图片，这样不影响兼容也不影响美观。

  2. 使用12px及12px以上字体大小：为了兼容各大主流浏览器，建议设计美工图时候设置大于或等于12px的字体大小，如果是接单的这个时候就需要给客户讲解小于12px浏览器不兼容等事宜。

  3. 继续使用小于12px字体大小样式设置：如果不考虑chrome可以不用考虑兼容，同时在设置小于12px对象设置-webkit-text-size-adjust:none，做到最大兼容考虑。

  4. 使用12px以上字体：为了兼容、为了代码更简单 从新考虑权重下兼容性。
---

# CSS优化、提高性能的方法有哪些？
  1. 关键选择器（key selector）。选择器的最后面的部分为关键选择器（即用来匹配目标元素的部分）；

  2. 如果规则拥有 ID 选择器作为其关键选择器，则不要为规则增加标签。过滤掉无关的规则（这样样式系统就不会浪费时间去匹配它们了）；

  3. 提取项目的通用公有样式，增强可复用性，按模块编写组件；增强项目的协同开发性、可维护性和可扩展性;

  4. 使用预处理工具或构建工具（gulp对css进行语法检查、自动补前缀、打包压缩、自动优雅降级）；
---

# 什么是外边距合并？
  * 外边距合并指的是，当两个垂直外边距相遇时，它们将形成一个外边距。

  * 合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。
---

# 请解释一下为什么需要清除浮动？清除浮动的方式。 解释下浮动和它的工作原理
  * 清除浮动是为了清除使用浮动元素产生的影响。浮动的元素，高度会塌陷，而高度的塌陷使我们页面后面的布局不能正常显示

  * 方式：
    1. 空div方法：<div style="clear:both;"></div>。
    2. overflow: auto或overflow: hidden方法
    3. Clearfix 方法：
      ```
        .clearfix::before, .clearfix::after {
            content: " ";
            display: table;
        }
        .clearfix::after {
            clear: both;
        }
        .clearfix {
            *zoom: 1;
        }
      ```
    
    4. SASS编译的时候，浮动元素的父级div定义伪类:after
      ``` 
        &::after,&::before{
          content: " ";
          visibility: hidden;
          display: block;
          height: 0;
          clear: both;
        }
      ```

    5. 原理: 浮动元素脱离文档流，不占据空间。浮动元素碰到包含它的边框或者浮动元素的边框停留。
---
