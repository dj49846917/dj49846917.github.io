---
title: 面试——html篇
date: 2020-12-28 16:20:07
tags: 
  - 面试
  - html
type: 面试                                                                 # 标签、分类
description:  超文本标记语言（英语：HyperText Markup Language，简称：HTML）是一种用于创建网页的标准标记语言。
top_img: https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=2908530132,3956932538&fm=26&gp=0.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 面试
  - html                                                                 # 文章标签
cover: https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=2908530132,3956932538&fm=26&gp=0.jpg                 # 文章的缩略图（用在首页）
---

# DOCTYPEDOCTYPE有什么用? 标准模式与兼容模式各有什么区别?
  1. DOCTYPE是“document type”的缩写。它是 HTML 中用来区分标准模式和怪异模式的声明，用来告知浏览器使用标准模式渲染页面
  2. 标准模式的排版 和JS运作模式都是以该浏览器支持的最高标准运行。在兼容模式中，页面以宽松的向后兼容的方式显示,模拟老式浏览器的行为以防止站点无法工作。
---

# cookie和seession区别
  * cookie数据存放在客户的浏览器上，session数据放在服务器上。
  * cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗,考虑到安全应当使用session。
  * session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能,考虑到减轻服务器性能方面，应当使用COOKIE。
  * 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。
  * 所以个人建议：将登陆信息等重要信息存放为SESSION,其他信息如果需要保留，可以放在COOKIE中
---

# cookie 和 token 的区别？
---

# token 如何传递？token 的安全性？token 的签名怎么实现？token 的缺点？
---

# a.com 可以读取 b.com 的cookie吗？如何读取子域名的 cookie? 配置哪个参数？
---

# 请描述cookie、sessionStorage和localStorage的区别。
  1. cookie是网站为了标示用户身份而储存在用户本地终端（Client Side）上的数据（通常经过加密）。
  2. cookie数据始终在同源的http请求中携带（即使不需要），记会在浏览器和服务器间来回传递。
  3. sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。
  4. 存储大小：
     * cookie数据大小不能超过4k。
     * sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。

  5. 有期时间：
     * localStorage    存储持久数据，浏览器关闭后数据不丢失除非主动删除数据；
     * sessionStorage  数据在当前浏览器窗口关闭后自动删除。
     * cookie          设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭
---

# 如何设置cookie的过期时间
---

# 部署在多台服务器上，cookie会有问题，知道吗？cookie的安全问题了解吗？
---

# 浏览器可以访问所有cookie吗，js可以访问所有cookie吗，如何不让js访问cookie
---

# 什么是渐进式渲染？
  * 渐进式渲染是用于提高网页性能（尤其是提高用户感知的加载速度），以尽快呈现页面的技术。
	
  * 一些举例：
	  1. 图片懒加载——页面上的图片不会一次性全部加载。当用户滚动页面到图片部分时，JavaScript 将加载并显示图像。
	  2. 确定显示内容的优先级（分层次渲染）——为了尽快将页面呈现给用户，页面只包含基本的最少量的 CSS、脚本和内容，然后可以使用延迟加载脚本或监听DOMContentLoaded/load事件加载其他资源和内容。
	  3. 异步加载 HTML 片段——当页面通过后台渲染时，把 HTML 拆分，通过异步请求，分块发送给浏览器。更多相关细节可以在这里找到。
---

# 介绍一下你对浏览器内核的理解？
  * 主要分成两部分：渲染引擎(layout engineer或Rendering Engine)和JS引擎。

  * 渲染引擎：负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。所有网页浏览器、电子邮件客户端以及其它需要编辑、显示网络内容的应用程序都需要内核。

  * JS引擎则：解析和执行javascript来实现网页的动态效果。

  * 最开始渲染引擎和JS引擎并没有区分的很明确，后来JS引擎越来越独立，内核就倾向于只指渲染引擎。
---

# html5有哪些新特性、移除了那些元素?
  * HTML5 现在已经不是 SGML 的子集，主要是关于图像，位置，存储，多任务等功能的增加。
  	  绘画 canvas;
  	  用于媒介回放的 video 和 audio 元素;
  	  本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失;
        sessionStorage 的数据在浏览器关闭后自动删除;
  	  语意化更好的内容元素，比如 article、footer、header、nav、section;
  	  表单控件，calendar、date、time、email、url、search;
  	  新的技术webworker, websocket, Geolocation;

    移除的元素：
  	  纯表现的元素：basefont，big，center，font, s，strike，tt，u;
  	  对可用性产生负面影响的元素：frame，frameset，noframes；

  * 支持HTML5新标签：
  	 IE8/IE7/IE6支持通过document.createElement方法产生的标签，
    	 可以利用这一特性让这些浏览器支持HTML5新标签，
    	 浏览器支持新标签后，还需要添加标签默认的样式。

       当然也可以直接使用成熟的框架、比如html5shim;
---

# 介绍webworker
---

# 简述一下你对HTML语义化的理解？
  1. 用正确的标签做正确的事情。
  2. html语义化让页面的内容结构化，结构更清晰，便于对浏览器、搜索引擎解析;
  3. 即使在没有样式CSS情况下也以一种文档格式显示，并且是容易阅读的;
  4. 搜索引擎的爬虫也依赖于HTML标记来确定上下文和各个关键字的权重，利于SEO;
  5. 使阅读源代码的人对网站更容易将网站分块，便于阅读维护理解。
---

# iframe有那些优缺点？
  * 优点:
    1. 解决加载缓慢的第三方内容如图标和广告等的加载问题
    2. 并行加载脚本

  * 缺点
    1. iframe会阻塞主页面的Onload事件；
    2. 搜索引擎的检索程序无法解读这种页面，不利于SEO;
    3. iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。
    4. 即使内容为空，加载也需要时间
---

# 如何实现浏览器内多个标签页之间的通信?
  1. 通过父页面window.open()和子页面postMessage, 异步下，通过 window.open('about: blank') 和 tab.location.href = '*'	
  2. 设置同域下共享的localStorage与监听window.onstorage
  3. 设置共享cookie与不断轮询脏检查(setInterval)
	4. 借助服务端或者中间层实现
---

# 浏览器渲染的整个过程(浏览器工作机制)
---

# css js渲染树如何一起工作
---

# 网页验证码是干嘛的，是为了解决什么安全问题。
  * 区分用户是计算机还是人的公共全自动程序。可以防止恶意破解密码、刷票、论坛灌水；
  * 有效防止黑客对某一个特定注册用户用特定程序暴力破解方式进行不断的登陆尝试。
---

# 你做的页面在哪些流览器测试过？这些浏览器的内核分别是什么?
  * IE: trident内核 
  * Firefox：gecko内核
  * Safari:webkit内核
  * Opera:以前是presto内核，Opera现已改用Google Chrome的Blink内核
  * Chrome:Blink(基于webkit，Google与Opera Software共同开发) 
---

# div+css的布局较table布局有什么优点？
  * 改版的时候更方便 只要改css文件。
  * 页面加载速度更快、结构化清晰、页面显示简洁。
  * 表现与结构相分离。
  * 易于优化（seo）搜索引擎更友好，排名更容易靠前。
---

# 什么是渐进增强和优雅降级？ 有什么区别？
  * 渐进增强 progressive enhancement：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。

  * 优雅降级 graceful degradation：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。

  * 区别:
	优雅降级是从复杂的现状开始，并试图减少用户体验的供给，而渐进增强则是从一个非常基础的，能够起作用的版本开始，并不断扩充，以适应未来环境的需要。降级（功能衰减）意味着往回看；而渐进增强则意味着朝前看，同时保证其根基处于安全地带。
---

# 为什么利用多个域名来存储网站资源会更有效？
  * CDN缓存更方便 
  * 突破浏览器并发限制 
  * 节约cookie带宽 
  * 节约主域名的连接数，优化页面响应速度 
  * 防止不必要的安全问题
---

# cdn知道吗？
---

# cdn用的是哪一种缓存？
---

# 简述一下src与href的区别。
  1. src用于替换当前元素，href用于在当前文档和引用资源之间确立联系。

  2. src是source的缩写，指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在位置；在请求src资源时会将其指向的资源下载并应用到文档内，例如js脚本，img图片和frame等元素。`<script src ="js.js"></script>`,当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕，图片和框架等元素也如此，类似于将所指向资源嵌入当前标签内。这也是为什么将js脚本放在底部而不是头部。

  3. href是Hypertext Reference的缩写，指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链接，如果我们在文档中添加`<link href="common.css" rel="stylesheet"/>`,那么浏览器会识别该文档为css文件，就会并行下载资源并且不会停止对当前文档的处理。这也是为什么建议使用link方式来加载css，而不是使用@import方式。
---

# 在css/js代码上线之后开发人员经常会优化性能，从用户刷新网页开始，一次js请求一般情况下有哪些地方会有缓存处理？
  * dns缓存，cdn缓存，浏览器缓存，服务器缓存。
---

# HTML与XHTML的区别？
  1. 所有的标记都必须要有一个相应的结束标记
  2. 所有标签的元素和属性的名字都必须使用小写
  3. 所有的XML标记都必须合理嵌套
  4. 所有的属性必须用引号""括起来
  5. 把所有<和&特殊符号用编码表示
  6. 给所有属性赋一个值
  7. 图片必须有说明文字
---

# XML和JSON的区别？
  1. 数据体积方面：JSON相对于XML来讲，数据的体积小，传递的速度更快些。
  2. 数据交互方面: JSON与JavaScript的交互更加方便，更容易解析处理，更好的数据交互。
  3. 数据描述方面: JSON对数据的描述性比XML较差。
  4. 传输速度方面: JSON的速度要远远快于XML。
---

# 请谈谈你对重绘与回流的理解
  * 重绘(repaint): 当元素样式的改变不影响布局时，浏览器将使用重绘对元素进行更新，此时由于只需要UI层面的重新像素绘制，因此 损耗较少

  * 回流(reflow): 当元素的尺寸、结构或触发某些属性时，浏览器会重新渲染页面，称为回流。此时，浏览器需要重新经过计算，计算后还需要重新页面布局，因此是较重的操作

  * 会触发回流的操作:
   	1. 页面初次渲染
   	2. 浏览器窗口大小改变
   	3. 元素尺寸、位置、内容发生改变
   	4. 元素字体大小变化
   	5. 添加或者删除可见的 dom 元素
   	6. 激活 CSS 伪类（例如：:hover）
   	7. 查询某些属性或调用某些方法：
   	    {1}.clientWidth、clientHeight、clientTop、clientLeft
   	    {2}.offsetWidth、offsetHeight、offsetTop、offsetLeft
   	    {3}.scrollWidth、scrollHeight、scrollTop、scrollLeft
   	    {4}.getComputedStyle()
   	    {5}.getBoundingClientRect()
   	    {6}.scrollTo()

  * 回流必定触发重绘，重绘不一定触发回流。重绘的开销较小，回流的代价较高。
---

# 回流和重绘的区别，怎么避免回流
---

# 骨架屏是什么，怎么实现的
---

# 为何建议把js放在body最后？
---

# 如果有很多if else，怎么优化
---





