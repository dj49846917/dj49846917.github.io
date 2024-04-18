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
  1. 通过在css文件的选择器上使用:local
    * 例如：:local(.className) { background: red; }
  2. 在vue项目中使用`deep`
    * 例如：:deep(.el-carousel__arrow--right) { right: 120px; }
  3. 在react项目中使用`:global`
    * 例如：
      ```
        <RangePicker className={styles.rangePick} showTime format="YYYY-MM-DD HH:mm:ss" /><br>

        .rangePick{
          :global {
            .ant-picker-input {
              color:red
            }
          }
        }
      ```
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
---

# CSS的哪些属性哪些可以继承？哪些不可以继承？
  1. 可以继承属性：
     - 字体的一些属性：font, font-family、 font-weight, font-size、font-style;
     - 文本的一些属性：
       * 内联元素: color, line-height, word -spacing、letter-spacing、text-transform;
       * 块级元素: text indent, text align;
     - 元素的可见性：visibility:hidden
     - 表格布局的属性： caption-side、border-collapse、 border-spacing、empty-cells、table-layout,
     - 列表的属性：list-style
     - 页面样式属性：page
     - 声音的样式属性

  2. 不可继承属性：
     - display：规定元素应该生成的框的类型;
     - 文本属性：vertical-align、 text decoration;
     - 盒子模型的属性：width、height, margin 、border、 padding;
     - 背景属性：background. background color background-image;
     - 定位属性：float, clear、 position. top、right、bottom, left min width、min-height. max width. max-height, overflow、 clip;
---

# display有哪些值？说明他们的作用
|      值      |                            含义                            |
| :----------: | :--------------------------------------------------------: |
|    block     |    块类型。默认宽度为父元素宽度，可设置宽高，换行显示。    |
|     none     |               元素不显示，并从文档流中移除。               |
|    inline    | 行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示。 |
| inline-block |        默认宽度为内容宽度，可以设置宽高，同行显示。        |
|  list-item   |         象块类型元素一样显示，并添加样式列表标记。         |
|    table     |                此元素会作为块级表格来显示。                |
|   inherit    |          规定应该从父元素继承 display 属性的值。           |
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

# css3的animation属性有哪些
> 语法：animation: name duration timing-function delay iteration-count direction;

|            值             |                   描述                   |
| :-----------------------: | :--------------------------------------: |
|      animation-name       | 规定需要绑定到选择器的 keyframe 名称。。 |
|    animation-duration     | 规定完成动画所花费的时间，以秒或毫秒计。 |
| animation-timing-function |           规定动画的速度曲线。           |
|      animation-delay      |        规定在动画开始之前的延迟。        |
| animation-iteration-count |         规定动画应该播放的次数。         |
|    animation-direction    |      规定是否应该轮流反向播放动画。      |
---

# transform的属性
translate(x,y)、scale(x,y)、rotate(angle)
---

# animation和transition有什么区别
transition需要触发一个事件才能改变属性，而animation不需要触发任何事件的情况下随时间改变属性值，并且transition为2帧，从from .... to，而animation可以一帧一帧的。
---

# 经常遇到的浏览器的兼容性有哪些？
  1. 不同浏览器的标签的默认外边距（margin）和内边距（padding）不同
    * 解决方案：css添加通配符设置margin和padding为0px
      ```
        * { margin: 0; padding: 0; }
      ```
  2. Chrome下12px以下的文本强制显示为12px大小
    * 解决方案：-webkit-text-size-adjust:none;
  3. IE中盒子模型的宽高计算方式与W3C标准不同
    * IE中盒子模型：元素的width或height =(content + border + padding)，
    * W3C标准盒子：元素的width或height = content
    * 解决办法：设置box-sizing
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

# 双边距重叠问题（外边距折叠）
  * 多个相邻（兄弟或者父子关系）普通流的块元素垂直方向marigin会重叠
  * 折叠的结果为：两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。 两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。 两个外边距一正一负时，折叠结果是两者的相加的和。
---

# 浏览器是怎样解析CSS选择器的？
浏览器解析CSS选择器的顺序是从右到左的，而不是直观上的从左到右。之所以是从右到左，是因为选择器一般也是有规律的，一般选择器的最右边是最宽泛的，比如div标签等，而选择器的最左边一般是最具体的，比如属性等。所以从最左边开始解析有助于能一开始就快速的判断出大部分标签是否是潜在的符合要求的目标
---

# 什么是 FOUC（无样式内容闪烁）？你如何来避免 FOUC？
  1. 概念: 文档样式闪烁
    
  2. 造成原因:`<style type="text/css" media="all">@import "../fouc.css";</style>` 而引用CSS文件的@import就是造成这个问题的罪魁祸首。IE会先加载整个HTML文档的DOM，然后再去导入外部的CSS文件，因此，在页面DOM加载完成到CSS导入完成中间会有一段时间页面上的内容是没有样式的，这段时间的长短跟网速，电脑速度都有关系。

  3. 解决方法: 只要在`<head>`之间加入一个`<link>`或者`<script>`元素就可以了。

# 知道css有个content属性吗？有什么作用？有什么应用？
  * css的content属性专门应用在 before/after 伪元素上，用于来插入生成内容。最常见的应用是利用伪类清除浮动。
---

# 1rem、1em、1vh、1px各自代表的含义？
  * rem: rem是全部的长度都相对于根元素`<html>`元素。通常做法是给html元素设置一个字体大小，然后其他元素的长度单位就为rem。
  * em: 子元素字体大小的em是相对于父元素字体大小元素的width/height/padding/margin用em的话是相对于该元素的font-size
  * vw/vh: 全称是 Viewport Width 和 Viewport Height，视窗的宽度和高度，相当于 屏幕宽度和高度的 1%，不过，处理宽度的时候%单位更合适，处理高度的 话 vh 单位更好。
  * px像素（Pixel）。相对长度单位。像素px是相对于显示器屏幕分辨率而言的
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
---

# vh/vw原理及缺点
  * 原理：vw是相对单位，1vw表示屏幕宽度的1%。基于此，我们可以把所有需要适配屏幕大小等比缩放的元素都使用vw做为单位。不需要缩放的元素使用px做单位。
  * 缺点：因为相对于视口，所以失去了最大宽度/高度的限制，可能你在宽屏上看到完美，竖屏上就不忍直视了。这时需要你额外为元素添加最大宽度/高度来限制
---

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

# css 动画与 js 动画，两者优缺点与性能
  * 性能：性能方面，css动画相对于优一些，css动画通过GUI解析，js动画需要经过js引擎代码解析，然后再进行 GUI 解析渲染。
  * 优缺点：
    1. 兼容方面，css 有浏览器兼容问题，js 大多情况下是没有的。
    2. 复杂度上：简单动画上，css代码实现简单，js稍微复杂点，复杂动画上，css代码冗长，js实现更优
    3. 动画运行时，对动画的控制程度上，js 比较灵活，能控制动画暂停，取消，终止等，css动画不能添加事件，只能设置固定节点进行什么样的过渡动画。
---

# 怎么让Chrome支持小于12px 的文字？
  1. 用图片：如果是内容固定不变情况下，使用将小于12px文字内容切出做图片，这样不影响兼容也不影响美观。
  2. transform: scale
  3. -webkit-text-size-adjust:none; 这个属性在高版本的 Chrome 中已经被废除
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
    1. 父级div定义 height
      - 原理：父级div手动定义height，就解决了父级div无法自动获取到高度的问题。
      - 优点：简单、代码少、容易掌握
      - 缺点：只适合高度固定的布局，要给出精确的高度，如果高度和父级div不一样时，会产生问题
    2. 结尾处加空div标签 clear:both
      - 原理：添加一个空div，利用css提高的clear:both清除浮动，让父级div能自动获取到高度
      - 优点：简单、代码少、浏览器支持好、不容易出现怪问题
      - 缺点：不少初学者不理解原理；如果页面浮动布局多，就要增加很多空div，让人感觉很不好
    3. 父级div定义 伪类:after 和 zoom
      - 原理：IE8以上和非IE浏览器才支持:after，原理和方法2有点类似，zoom(IE转有属性)可解决ie6,ie7浮动问题
      - 优点：浏览器支持好、不容易出现怪问题（目前：大型网站都有使用，如：腾迅，网易，新浪等等）
      - 缺点：代码多、不少初学者不理解原理，要两句代码结合使用才能让主流浏览器都支持
    4. 父级div定义 overflow:hidden
      - 原理：必须定义width或zoom:1，同时不能定义height，使用overflow:hidden时，浏览器会自动检查浮动区域的高度
      - 优点：简单、代码少、浏览器支持好
      - 缺点：不能和position配合使用，因为超出的尺寸的会被隐藏。

# 使用过 flex 布局吗？flex-grow 和 flex-shrink 属性有什么用？
  1. 介绍
    * 他是弹性盒子模型，设为flex布局后，子元素的float、clear、vertical-align将失效
    * 称采用Flex布局的元素为Flex容器（flex container），它的所有子元素自动成为容器成员，称为Flex项目(flex item)。他有主轴（默认水平）和副轴(交叉轴)
    * flex容器属性：
      - flex-direction 决定主轴方向=项目排列方向
        - row(默认值) 主轴为水平方向，起点在左端
        - row-reverse 主轴为水平方向，起点在右端
        - column 主轴为垂直方向，起点在左端
        - column-reverse 主轴为垂直方向，起点在右端

      - flex-wrap 定义如何换行
        - nowrap(默认) 不换行
        - wrap 换行，第一行在上方
        - wrap-reverse 换行，第一行在下方

      - flex-flow 它是flex-direction和flex-wrap的简写形式，语法糖

      - justify-content 定义项目在主轴上的对齐方式
        - flex-start(默认值) 左对齐
        - flex-end 右对齐
        - center 居中
        - space-between 两端对其，项目间间隔相等
        - space-around 单个项目两侧的间隔相等，故项目间间隔相比项目与边框间间隔多一倍

      - align-items 定义项目在交叉轴上的对齐方式
        - strech(默认值) 如果flex项目未设置高度或设置高度为auto，将占满整个容器的高度
        - flex-start 交叉轴起点处对齐
        - flex-end 交叉轴终点处对齐
        - center 交叉轴中点处对齐
        - baseline 项目第一行文字的基线对齐
      - align-content 定义多根轴线的对齐方式。若项目只有一根轴线则不生效。

    * flex项目属性
      - order 定义项目排列顺序。数值越小，排列越靠前，默认为0
      - flex-grow 定义项目放大比例，默认为0
      - flex-shrink 定义项目缩小比例，默认为1
      - flex-basis 定义在分配多余空间前，项目占据的主轴空间(main size)

  2. 作用
    * flex-grow: 定义剩余空间的分成。默认为0，即如果存在剩余空间，也不放大。
      - flex-grow会覆盖你设置的width属性,使之失效
      - 当三个div同时设置相同的非 0 正整数时,即为三个div平分剩余空间.
        - 剩余空间=浏览器宽度-item内容宽度之和
        - flex-grow设置为负值时等于0  
      - 当三个div flex-grow数值不同,则按比例分配剩余空间.当有没有设置felx-grow,且有width属性的话,先减去其占有的宽度;
        - 这里有一个误区就是认为flex-grow:1,flex-grow:2 的宽度时1:2,错! 是分配的剩余空间1:2
    * flex-shrink: 定义了项目的缩小比例。flex-shrink的默认值为1，flex-shrink的值为0时，不缩放。
---

# flex: 1 的原理（为什么flex: 1就可以实现自适应，1是哪个属性的值）
  * flex 是 flex-grow、flex-shrink、flex-basis的缩写。默认值是 0 1 auto。
  * flex-grow：定义弹性盒子项（flex item）的拉伸因子
  * flex-shrink：指定了 flex 元素的收缩规则。flex 元素仅在默认宽度之和大于容器的时候才会发生收缩，其收缩的大小是依据 flex-shrink 的值。
  * flex-basis：指定了flex元素在主轴方向上的初始大小。如果不使用 box-sizing 改变盒模型的话，那么这个属性就决定了 flex 元素的内容盒（content-box）的尺寸。
---

# 说一下flex怎么做出0.5px的线
  * css使用transform的scale(0.5)
  * flex使用flex-shrink(0.5)
---

# 伪类和伪元素的区别，讲讲伪类有哪些
  * 区别：
    1. 伪元素用于创建不存在于文档结构中的元素，并为其添加样式化内容。
    2. 伪类用于选择元素的特定状态或条件，并为其应用样式。
    3. 伪元素使用双冒号 :: 表示，伪类使用单冒号 : 表示。
    4. 伪元素在文档中并不存在，而伪类选择的是实际存在的元素。
  * 伪类的api
    | 选择器               | 例子                  | 例子描述                                                         |
    | :------------------- | :-------------------- | :--------------------------------------------------------------- |
    | :active              | a:active              | 选择活动的链接                                                   |
    | :checked             | input:checked         | 选择每个被选中的`<input>`元素。                                  |
    | :disabled            | input:disabled        | 选择每个被禁用的`<input>`元素。                                  |
    | :empty               | p:empty               | 选择没有子元素的每个`<p>`元素。                                  |
    | :enabled             | input:enabled         | 选择每个已启用的`<input>`元素。                                  |
    | :first-child         | p:first-child         | 选择作为其父的首个子元素的每个`<p>`元素。                        |
    | :first-of-type       | p:first-of-type       | 选择作为其父的首个`<p>`元素的每个`<p>`元素。                     |
    | :focus               | input:focus           | 选择获得焦点的`<input>`元素。                                    |
    | :hover               | a:hover               | 选择鼠标悬停其上的链接。                                         |
    | :in-range            | input:in-range        | 选择具有指定范围内的值的`<input>`元素。                          |
    | :invalid             | input:invalid         | 选择所有具有无效值的`<input>`元素。                              |
    | :lang(language)      | p:lang(it)            | 选择每个 lang 属性值以 "it" 开头的`<p>`元素。                    |
    | :last-child          | p:last-child          | 选择作为其父的最后一个子元素的每个`<p>`元素。                    |
    | :last-of-type        | p:last-of-type        | 选择作为其父的最后一个`<p>`元素的每个`<p>`元素。                 |
    | :link                | a:link                | 选择所有未被访问的链接。                                         |
    | :not(selector)       | :not(p)               | 选择每个非`<p>`元素的元素。                                      |
    | :nth-child(n)        | p:nth-child(2)        | 选择作为其父的第二个子元素的每个`<p>`元素。                      |
    | :nth-last-child(n)   | p:nth-last-child(2)   | 选择作为父的第二个子元素的每个`<p>`元素，从最后一个子元素计数。  |
    | :nth-last-of-type(n) | p:nth-last-of-type(2) | 选择作为父的第二个`<p>`元素的每个`<p>`元素，从最后一个子元素计数 |
    | :nth-of-type(n)      | p:nth-of-type(2)      | 选择作为其父的第二个`<p>`元素的每个`<p>`元素。                   |
    | :only-of-type        | p:only-of-type        | 选择作为其父的唯一`<p>`元素的每个`<p>`元素。                     |
    | :only-child          | p:only-child          | 选择作为其父的唯一子元素的`<p>`元素。                            |
    | :optional            | input:optional        | 选择不带 "required" 属性的`<input>`元素。                        |
    | :out-of-range        | input:out-of-range    | 选择值在指定范围之外的`<input>`元素。                            |
    | :read-only           | input:read-only       | 选择指定了 "readonly" 属性的 `<input>` 元素。                    |
    | :read-write          | input:read-write      | 选择不带 "readonly" 属性的 `<input>` 元素。                      |
    | :required            | input:required        | 选择指定了 "required" 属性的 `<input>` 元素。                    |
    | :root                | root                  | 选择元素的根元素。                                               |
    | :target              | #news:target          | 选择当前活动的 #news 元素（单击包含该锚名称的 URL）。            |
    | :valid               | input:valid           | 选择所有具有有效值的 `<input>`元素。                             |
    | :visited             | a:visited             | 选择所有已访问的链接                                             |

  * 伪元素：
    | 选择器         | 例子            | 例子描述                        |
    | :------------- | :-------------- | :------------------------------ |
    | ::after        | p::after        | 在每个 `<p>` 元素之后插入内容。 |
    | ::before       | p::before       | 在每个 `<p>` 元素之前插入内容。 |
    | ::first-letter | p::first-letter | 选择每个 `<p>` 元素的首字母。   |
    | ::first-line   | p::first-line   | 选择每个 `<p>` 元素的首行。     |
    | ::selection    | p::selection    | 选择用户选择的元素部分。        |
---

# css 中的颜色单位
  1. 十六进制表示。十六进制颜色的组成部分是：＃RRGGBB，其中RR（红色），GG（绿色）和BB（蓝色），所有值都必须介于0和FF之间。
  2. RGB表示。R表示red红色，G表示green绿色，B表示blue蓝色。RGB写法：rgb（0,0,0）。它的取值范围都在0-255之间，值越大越颜色越深。
  3. HSL表示。HSL写法：hsl(0,0%,0%)。h：色相，表示范围是从0到360。s：饱和度，饱和度是一个百分比值，范围是0%到100%。l：亮度，亮度也是一个百分比，范围是0%到100%。
  4. RGBA表示。RGBA 颜色值是 RGB 颜色值的扩展，带有一个 alpha 通道 - 它规定了对象的不透明度。RGBA 颜色值：rgba(255, 0, 0, alpha)。alpha 参数是介于 0.0（完全透明）与 1.0（完全不透明）的数字。
  5. HSLA表示。HSLA 颜色值是 HSL 颜色值的扩展，带有一个 alpha 通道 - 它规定了对象的不透明度。HSLA 颜色值：hsla(120,65%,75%, alpha)。alpha 参数是介于 0.0（完全透明）与 1.0（完全不透明）的数字。
---

# 16进制颜色对应多少字节，如果 32 位 的呢
---

# css3里面有些些属性会影响z-index
  * transfrom
  * 其他影响：
    - position
    - float
    - opacity
---

# z-index 属性原理
  1. z-index这个属性控制着元素在z轴上的表现形式。
  2. 适用范围:仅适用于定位元素，即拥有relative,absolute,fixed属性的position元素。
  3. 堆叠顺序是当前元素位于z轴上的值，数值越大说明元素的堆叠1顺序越高，越靠近屏幕。
  4. 未定义时，后来居上，未定义z-index的属性，元素的堆叠顺序基于它所在的文档树。默认情况下，后来的元素的z-index的值越大。
---

# overflow的原理
  > 当元素设置了overflow样式且值部位visible时，该元素就构建了一个BFC，BFC在计算高度时，内部浮动元素的高度也要计算在内，也就是说技术BFC区域内只有一个浮动元素，BFC的高度也不会发生塌缩，所以达到了清除浮动的目的
---

# 用css创建一个三角形，并简述原理
  ```
    <div class='rect'></div>
    <style>
        .rect {
          width: 0;
          height: 0;
          background-color: #fff;
          border-right: 100px solid rgb(34, 230, 220);
          border-left: 100px solid rgb(202, 146, 25);
          border-top: 100px solid rgb(29, 156, 194);
          border-bottom: 100px solid rgb(16, 204, 101);
        }
      </style>
  ```
  * 原理：宽高设为0，四个边框设置border-width，border-style，border-color即可，如果某个三角要变为透明，设置border-color为transparent
---

# 精灵图和base64如何选择呢？
  * 精灵图
    - 优点：
      - 将多个图像加载请求合并为一个请求
    - 缺点：
      - 难以维护和更新
      - 增加内存消耗
  * base64:
    - 优点：
      - 将多个图像加载请求合并为一个CSS文件请求
      - 轻松更新生成文件
    - 缺点：
      - base64编码比原始二进制表示大约大25%
---

# 解释下 CSS sprites的原理和优缺点分别是什么？
  * 原理：把项目需要用到的图标合并成一张大图，在使用的时候通过position定位来展示指定的图标
  * 优点：大大减少了请求次数,提高了性能。解决了命名的困扰。减少了图片的字节
  * 缺点：
    - 定位不太好控制，多用于小图标的展示。
    - 可维护性差：页面背景需要少许改动，可能要修改部分或整张已合并的图片，进而要改动css。
    - 适应性差：在高分辨的屏幕下自适应页面，若图片不够宽会出现背景断裂。
---

# 请描述margin边界叠加是什么及解决方案
  * margin 的边界叠加发生在竖直方向上（左右方向上不会叠加）。兄弟 DOM 节点、父元素中的第一个子节点、以及最后一个尾节点都会产生 margin 边界叠加的现象。
  * 解决方案：
    1. 使用padding代替，但是父盒子要减去相应的高度
    2. 使用boder（透明）代替（不推荐，不符合书写规范，如果父盒子子盒子时有颜色的不好处理）
    3. 给父盒子设置overflow：hidden(如果有移除元素无法使用)
    4. 给父盒子设置1px的padding
    5. 给父盒子设置1px的透明border，高度减1px
    6. 子盒子使用定位position
    7. 子盒子浮动, 但是居中比较难以控制
    8. 给子盒子设置display: inline-block;
    9. 子盒子上面放一个table标签
---

# bootstrap响应式实现的原理
  * 百分比布局+媒体查询
---

# 什么是1px像素问题，如何解决?
  * 简单说就是多倍的设计图设计了1px的边框，在手机上缩小呈现时，由于css最低只支持显示1px大小，导致边框太粗的效果。
  * 解决办法：transform: scale(0.5)
    - 设置height: 1px，根据媒体查询结合transform缩放为相应尺寸。
---

# margin:auto 为什么可以实现垂直居中？
  * 想要实现垂直方向的居中可以用绝对定位
    ```
      div  {
        width: 20px;
        height: 20px;
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        margin: auto;
      }
    ```
  * 原因：块状水平元素，如div元素（下同），在默认情况下（非浮动、绝对定位等），水平方向会自动填满外部的容器；如果有margin-left/margin-right,padding-left/padding-right,border-left-width/border-right-width等，实际内容区域会响应变窄。但是，当一个绝对定位元素，其对立定位方向属性同时有具体定位数值的时候，流体特性就发生了。具有流体特性绝对定位元素的margin:auto的填充规则和普通流体元素一模一样，含有以下特性：
    - 如果一侧定值，一侧auto，auto为剩余空间大小；
    - 如果两侧均是auto, 则平分剩余空间
---

# calc, support, media各自的含义及用法？
  * @support主要是用于检测浏览器是否支持CSS的某个属性，其实就是条件判断，如果支持某个属性，你可以写一套样式，如果不支持某个属性，你也可以提供另外一套样式作为替补。
  * calc() 函数用于动态计算长度值。 calc()函数支持 "+", "-", "*", "/" 运算；
  * @media 查询，你可以针对不同的媒体类型定义不同的样式。
---

# 脱离文档流指的是什么？
  * 当为某个元素设置浮动，或者绝对定位时，后面的兄弟元素占据了它以前的位置，这样后面的兄弟元素的位置就变了。这个现象就是元素脱离文档流。
---