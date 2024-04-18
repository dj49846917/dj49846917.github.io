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

# DOCTYPE有什么用? 标准模式与兼容模式各有什么区别?
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
  1. token是 服务经过计算发给客户端的，服务不保存，每次客户端来请求，经过解密等计算来验证是否是自己下发的
  2. session是服务本地保存，发给客户端，客户端每次访问都带着，直接和服务的session比对
  3. cookie是保存在客户端上的一些基本信息，服务不保存，每次请求时客户端带上cookie，里面有一些账户密码，浏览记录什么的
---

# token 如何传递？token 的安全性？token 的签名怎么实现？token 的缺点？
  * 如何传递：前端放到header里面传给后台
  
  * 安全性：
    - 接口的安全性主要围绕token、timestamp和sign三个机制展开设计，保证接口的数据不会被篡改和重复调用，下面具体来看：
    - Token授权机制：
      - 用户使用用户名密码登录后服务器给客户端返回一个Token（通常是UUID），并将Token-UserId以键值对的形式存放在缓存服务器中。服务端接收到请求后进行Token验证，如果Token不存在，说明请求无效。Token是客户端访问服务端的凭证。
    - 时间戳超时机制：
      - 用户每次请求都带上当前时间的时间戳timestamp，服务端接收到timestamp后跟当前时间进行比对，如果时间差大于一定时间（比如5分钟），则认为该请求失效。时间戳超时机制是防御DOS攻击的有效手段。
    - 签名机制：
      - 将 Token 和 时间戳 加上其他请求参数再用MD5或SHA-1算法（可根据情况加点盐）加密，加密后的数据就是本次请求的签名sign，服务端接收到请求后以同样的算法得到签名，并跟当前的签名进行比对，如果不一样，说明参数被更改过，直接返回错误标识。签名机制保证了数据不会被篡改。
    - token的认证流程：
      1. 客户端通过用户名密码登录服务器并获取Token
      2. 客户端生成时间戳timestamp，并将timestamp作为其中一个参数。
      3. 客户端将所有的参数，包括Token和timestamp按照自己的算法进行排序加密得到签名sign
      4. 将token、timestamp和sign作为请求时必须携带的参数加在每个请求的URL后边（http://api.com/users?token=asdsadasd&timestamp=123&sign=123123123）
      5. 服务端写一个过滤器对token、timestamp和sign进行验证，只有在token有效、timestamp未超时、缓存服务器中不存在sign三种情况同时满足，本次请求才有效
    - 在以上三中机制的保护下，如果有人劫持了请求，并对请求中的参数进行了修改，签名就无法通过；如果有人使用已经劫持的URL进行DOS攻击，服务器则会因为缓存服务器中已经存在签名或时间戳超时而拒绝服务，所以DOS攻击也是不可能的；
  
  * 签名实现：
    - 将 Token 和 时间戳 加上其他请求参数再用MD5或SHA-1算法（可根据情况加点盐）加密，加密后的数据就是本次请求的签名sign，服务端接收到请求后以同样的算法得到签名，并跟当前的签名进行比对，如果不一样，说明参数被更改过，直接返回错误标识。签名机制保证了数据不会被篡改。

  * 缺点：token 最大的缺点就是它的大小，最小的它都比 cookie 要大，如果 token 中包含很多声明，那问题就会变得比较严重，毕竟向服务器发送的每个请求都要有这个 token。（意思应该是太大了会导致请求缓慢）
  
  * 基于Token的验证原理
    - 基于Token的身份验证的过程如下:
      1. 用户通过用户名和密码发送请求。
      2. 程序验证。
      3. 程序返回一个签名的token 给客户端。
      4. 客户端储存token,并且每次用于每次发送请求。
      5. 服务端验证token并返回数据。
    - 每一次请求都需要token。token应该在HTTP的头部发送从而保证了Http请求无状态。我们同样通过设置服务器属性Access-Control-Allow-Origin:* ，让服务器能接受到来自所有域的请求。需要主要的是，在ACAO头部标明(designating)*时，不得带有像HTTP认证，客户端SSL证书和cookies的证书。

  * Token的优势（好处）
    - 无状态、可扩展
      - 在客户端存储的Tokens是无状态的，并且能够被扩展。基于这种无状态和不存储Session信息，负载负载均衡器能够将用户信息从一个服务传到其他服务器上。如果我们将已验证的用户的信息保存在Session中，则每次请求都需要用户向已验证的服务器发送验证信息(称为Session亲和性)。用户量大时，可能会造成一些拥堵。但是不要着急。使用tokens之后这些问题都迎刃而解，因为tokens自己hold住了用户的验证信息。
    - 安全性
      - 请求中发送token而不再是发送cookie能够防止CSRF(跨站请求伪造)。即使在客户端使用cookie存储token，cookie也仅仅是一个存储机制而不是用于认证。不将信息存储在Session中，让我们少了对session操作。token是有时效的，一段时间之后用户需要重新验证。我们也不一定需要等到token自动失效，token有撤回的操作，通过token revocataion可以使一个特定的token或是一组有相同认证的token无效。
    - 可扩展性
      - Tokens能够创建与其它程序共享权限的程序。例如，能将一个随便的社交帐号和自己的大号(Fackbook或是Twitter)联系起来。当通过服务登录Twitter(我们将这个过程Buffer)时，我们可以将这些Buffer附到Twitter的数据流上(we are allowing Buffer to post to our Twitter stream)。使用tokens时，可以提供可选的权限给第三方应用程序。当用户想让另一个应用程序访问它们的数据，我们可以通过建立自己的API，得出特殊权限的tokens。
    - 多平台跨域
      - 我们提前先来谈论一下CORS(跨域资源共享)，对应用程序和服务进行扩展的时候，需要介入各种各种的设备和应用程序。只要用户有一个通过了验证的token，数据和资源就能够在任何域上被请求到。
    - 基于标准 
      - 创建token的时候，你可以设定一些选项。我们在后续的文章中会进行更加详尽的描述，但是标准的用法会在JSON Web Tokens体现。最近的程序和文档是供给JSON Web Tokens的。它支持众多的语言。这意味在未来的使用中你可以真正的转换你的认证机制。
---

# a.com 可以读取 b.com 的cookie吗？如何读取子域名的 cookie? 配置哪个参数？
  * 可以
  * 读取：
    - 在添加和删除时一定要加上Resonse.Cookies(cookieName).Domain属性。 
  * 配置：
    - 通过path参数，您可以告诉浏览器cookie属于什么路径。
---

# 请描述cookie、sessionStorage和localStorage的区别。
  * cookie数据大小不能超过4k；sessionStorage和localStorage的存储比cookie大得多，可以达到5M+
  * cookie设置的过期时间之前一直有效；localStorage永久存储，浏览器关闭后数据不丢失除非主动删除数据；sessionStorage数据在当前浏览器窗口关闭后自动删除
  * cookie的数据会自动的传递到服务器；sessionStorage和localStorage数据保存在本地
---

# 如何设置cookie的过期时间
  * 通过设置expires属性。如果Cookie没有设置expires属性，那么 cookie 的生命周期只是在当前的会话中，关闭浏览器意味着这次会话的结束，此时 cookie 随之失效。
---

# cookie的作用域
  * domain属性设置。domain本身以及domain的子域名可以访问到相关cookie。不设置指当前域名
---

# cookie有哪些属性
  * name: 字段为一个cookie的名称。
  * value: 字段为一个cookie的值。
  * domain: 字段为可以访问此cookie的域名。
  * path: 字段为可以访问此cookie的页面路径。
  * expires/Max-Age: Expires和Max-age均为Cookie的有效期。
    - Expires是该Cookie被删除时的时间戳，格式为GMT,若设置为以前的时间，则该Cookie立刻被删除，并且该时间戳是服务器时间，不是本地时间！若不设置则默认页面关闭时删除该Cookie。
    - Max-age也是Cookie的有效期，但它的单位为秒，即多少秒之后失效，若Max-age设置为0，则立刻失效，设置为负数，则在页面关闭时失效。Max-age默认为 -1。
  * Size：cookie大小。
  * HttpOnly：HttpOnly值为 true 或 false,若设置为true，则不允许通过脚本document.cookie去更改这个值
  * secure：设置是否只能通过https来传递此条cookie
---

# 如何跨域访问cookie
  * 在前端请求的时候设置request对象的属性withCredentials为true
  * 在服务端设置Access-Control-Allow-Origin
  * 在服务端设置Access-Control-Allow-Credentials
---

# 部署在多台服务器上，cookie会有问题，知道吗？cookie的安全问题了解吗？
  * 安全问题：
    1. cookie欺骗
    2. cookie截获
    3. Flash的内部代码隐患
  * 解决办法：
    1. 设置cookie有效期不要过长，合适即可
    2. 设置HttpOnly属性为true
      * 可以防止js脚本读取cookie信息，有效的防止XSS攻击。
    3. 设置复杂的cookie，加密cookie
      * cookie的key使用uuid，随机生成；
      * cookie的value可以使用复杂组合，比如：用户名+当前时间+cookie有效时间+随机数。这样可以尽可能使得加密后的cookie更难解密，也是保护了cookie中的信息。
    4. 用户第一次登录时，保存ip+cookie加密后的token
      * 每次请求，都去将当前cookie和ip组合起来加密后的token与保存的token作对比，只有完全对应才能验证成功。
    5. session和cookie同时使用
      * sessionId虽然放在cookie中，但是相对的session更安全，可以将相对重要的信息存入session。
    6. 如果网站支持https，尽可能使用https
      * 如果网站支持https，那么可以为cookie设置Secure属性为true，它的意思是，cookie只能使用https协议发送给服务器，而https比http更加安全。
---

# 浏览器可以访问所有cookie吗，js可以访问所有cookie吗，如何不让js访问cookie
  * 可以
  * 可以
  * 通过设置HttpOnly属性
---

# 如何防止cookie被盗用？
  1. 禁止第三方网站带cookie(same-site属性)
  2. 每次请求需要输入图形验证码
  3. 使用Token验证
  4. 为cookie设置HttpOnly
  5. 设置CSP
  6. 使用Referer验证
  7. 禁止网页内嵌
  8. 使用https
  9. cookie带上用户ip加密
---

# Cookie如何防范XSS攻击？
  * httponly-这个属性可以防止XSS,它会禁止javascript脚本来访问cookie。
  * secure - 这个属性告诉浏览器仅在请求为https的时候发送cookie
---

# 什么是渐进式渲染？
  * 渐进式渲染是用于提高网页性能（尤其是提高用户感知的加载速度），以尽快呈现页面的技术。
	
  * 一些举例：
	  1. 图片懒加载——页面上的图片不会一次性全部加载。当用户滚动页面到图片部分时，JavaScript 将加载并显示图像。
	  2. 确定显示内容的优先级（分层次渲染）——为了尽快将页面呈现给用户，页面只包含基本的最少量的 CSS、脚本和内容，然后可以使用延迟加载脚本或监听DOMContentLoaded/load事件加载其他资源和内容。
	  3. 异步加载 HTML 片段——当页面通过后台渲染时，把 HTML 拆分，通过异步请求，分块发送给浏览器。更多相关细节可以在这里找到。
---

# 说一下图片的懒加载和预加载？
  * 预加载：提前加载图片，当用户需要查看时可直接从本地缓存中渲染。
  * 懒加载：懒加载的主要目的是作为服务器前端的优化，减少请求数或延迟请求数。
    - 原理：
      * 就是我们先设置图片的 data-set 属性（当然也可以是其他任意的， 只要不会发送 http 请求就行了，作用就是为了存取值）值为其图片路径，由于不是 src，所 以不会发送 http 请求。 然后我们计算出页面 scrollTop 的高度和浏览器的高度之和， 如果 图片举例页面顶端的坐标 Y（相对于整个页面，而不是浏览器窗口）小于前两者之和，就说明图片就要显示出来了（合适的时机，当然也可以是 其他情况），这时候我们再将data-set 属性替换为 src 属性即可。
  * 区别：两者的行为是相反的，一个是提前加载，一个是迟缓甚至不加载。 懒加载对服务器前端有一定的缓解压力作用，预加载则会增加服务器前端压力。
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
> 在Javascript单线程执行的基础上，开启一个子线程，进行程序处理，当子线程执行完毕之后再回到主线程上，在这个过程中并不影响主线程的执行过程。使用场景包括：使用专用线程进行数学运算；图像处理；大量数据的检索
---

# Web Worker线程的限制是什么？
  1. 同源限制
    * 分配给 Worker 线程运行的脚本文件，必须与主线程的脚本文件同源。
  2. DOM 限制
    * Worker 线程所在的全局对象，与主线程不一样，无法读取主线程所在网页的 DOM 对象，也无法使用document、window、parent这些对象。但是，Worker 线程可以使用navigator对象和location对象。
  3. 通信联系
    * Worker 线程和主线程不在同一个上下文环境，它们不能直接通信，必须通过消息完成。
  4. 脚本限制
    * Worker 线程不能执行alert()方法和confirm()方法，但可以使用 XMLHttpRequest 对象发出 AJAX 请求。
  5. 文件限制
    * Worker 线程无法读取本地文件，即不能打开本机的文件系统（file://），它所加载的脚本，必须来自网络。
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

# 前端需要注意哪些SEO
  * 合理的title 、description 、keywords ：搜索对着三项的权重逐个减小， title值强调重点即可， 重要关键词出现不要超过2次， 而且要靠前，不同⻚⾯title 要有所不同； description 把⻚⾯内容高度概括， ⻓度合适，不可过分堆砌关键词，不同⻚⾯description 有所不同； keywords 列举出重要关键词即可
  * 语义化的HTML 代码，符合W3C规范：语义化代码让搜索引擎容易理解网⻚
  * 重要内容HTML 代码放在最前：搜索引擎抓取HTML 顺序是从上到下， 有的搜索引擎对抓取⻓度有限制，保证重要内容⼀定会被抓取
  * 重要内容不要用js 输出：爬虫不会执⾏js获取内容
  * 少用iframe ：搜索引擎不会抓取iframe 中的内容
  * ⾮装饰性图片必须加alt
  * 提高网站速度： 网站速度是搜索引擎排序的⼀个重要指标
---

# 如何实现浏览器内多个标签页之间的通信?
  1. WebSocket （可跨域）
  2. postMessage（可跨域）
  3. SharedWorker
  4. localStorage: localstorge在一个标签页里被添加、修改或删除时，都会触发一个storage事件，通过在另一个标签页里监听storage事件，即可得到localstorge存储的值，实现不同标签页之间的通信。
  5. Cookies: 使用cookie+setInterval，将要传递的信息存储在cookie中，每隔一定时间读取cookie信息，即可随时获取要传递的信息。
---

# 浏览器渲染的整个过程(浏览器工作机制)
  1. 解析DOM树 - 渲染引起首先解析HTML树生成DOM树
  2. 构建render树 - DOM树和CSS树一起生成render树
  3. 布局render树 - 对render树的每个节点进行布局处理，确定在屏幕的位置
  4. 绘制render树 - 最后遍历render树并用UI后端将每一个节点绘制出来
---

# css js渲染树如何一起工作
  1. 解析html为dom树，解析css为cssom。渲染引擎开始解析html，并将标签转化为内容树中的dom节点。
  2. 把dom和cssom结合起来生成渲染树(render)。接着，它解析外部CSS文件及style标签中的样式信息。这些样式信息以及html中的可见性指令将被用来构建另一棵树——render树。Render树由一些包含有颜色和大小等属性的矩形组成，它们将被按照正确的顺序显示到屏幕上。
  3. 布局渲染树，计算几何形状。Render树构建好了之后，将会执行布局过程，它将确定每个节点在屏幕上的确切坐标。
  4. 把渲染树展示到屏幕上。再下一步就是绘制，即遍历render树，并使用UI后端层绘制每个节点。
---

# DOM树和渲染树的区别
  * DOM 树与 HTML 标签一一对应，包括 head 和隐藏元素
  * 渲染树不包括 head 和隐藏元素，大段文本的每一个行都是独立节点，每一个节点都有对应的 css 属性
---

# CSS会阻塞DOM解析吗？
  * 对于一个HTML文档来说，不管是内联还是外链的css，都会阻碍后续的dom渲染，但是不会阻碍后续dom的解析。
  * 当css文件放在中时，虽然css解析也会阻塞后续dom的渲染，但是在解析css的同时也在解析dom，所以等到css解析完毕就会逐步的渲染页面了。
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
  * 区别：
    - 重绘：当渲染树中的元素外观（如：颜色）发生改变，不影响布局时，产生重绘
    - 回流：当渲染树中的元素的布局（如：尺寸、位置、隐藏/状态状态）发生改变时，产生重绘回流
    - 注意：JS 获取 Layout 属性值（如：offsetLeft、scrollTop、getComputedStyle 等）也会引起回流。因为浏览器需要通过回流计算最新值
    - 回流必将引起重绘，而重绘不一定会引起回流
  * 如何避免：
    - 集中改变样式，不要一条一条地修改 DOM 的样式。
    - 不要把 DOM 结点的属性值放在循环里当成循环里的变量。
    - 为动画的 HTML 元件使用 fixed 或 absoult 的 position，那么修改他们的 CSS 是不会 reflow 的。
    - 不使用 table 布局。因为可能很小的一个小改动会造成整个 table 的重新布局。
    - 尽量只修改position：absolute或fixed元素，对其他元素影响不大
    - 动画开始GPU加速，translate使用3D变化
---

# translate()是否会触发重绘
  * 不会。使用 translate会为对应DOM节点生成新的图层(也叫开启硬件加速)。translate样式变化会移交 GPU处理。
---

# 怎么避免触发重绘
  1. 集中改变样式
  2. 使⽤DocumentFragment
    * 我们可以通过createDocumentFragment创建⼀个游离于DOM树之外的节点，然后在此节点上批量操作，最后插⼊ DOM树中，因此只触发⼀次重排
  3. 提升为合成层
    * 将元素提升为合成层有以下优点：
      - 合成层的位图，会交由 GPU 合成，⽐ CPU 处理要快
      - 当需要 repaint 时，只需要 repaint 本身，不会影响到其他的层
      - 对于 transform 和 opacity 效果，不会触发 layout 和 paint
    * 提升合成层的最好⽅式是使⽤ CSS 的 will-change 属性：
      ```
        #target {
          will-change: transform;
        }
      ```
---

# 骨架屏是什么，怎么实现的
  * 骨架屏就是在页面数据尚未加载前先给用户展示出页面的大致结构，直到请求数据返回后再渲染页面，补充进需要显示的数据内容。常用于文章列表、动态列表页等相对比较规则的列表页面。

  * 原理：
    - 通过 puppeteer 在服务端操控 headless Chrome 打开开发中的需要生成骨架屏的页面，在等待页面加载
    - 渲染完成之后，在保留页面布局样式的前提下，通过对页面中元素进行删减或增添，对已有元素通过层叠样
    - 式进行覆盖，这样达到在不改变页面布局下，隐藏图片和文字，通过样式覆盖，使得其展示为灰色块。然后
    - 将修改后的 HTML 和 CSS 样式提取出来，这样就是骨架屏了

  * 如何实现（生成骨架屏的技术方案大概有三种）
    1. 使用图片、svg 或者手动编写骨架屏代码：使用 HTML + CSS 的方式，我们可以很快的完成骨架屏效果，但是面对视觉设计的改版以及需求的更迭，我们对骨架屏的跟进修改会非常被动，这种机械化重复劳作的方式此时未免显得有些机动性不足；
    2. 通过预渲染手动书写的代码生成相应的骨架屏：该方案做的比较成熟的是 vue-skeleton-webpack-plugin，通过 vueSSR 结合 webpack 在构建时渲染写好的 vue 骨架屏组件，将预渲染生成的 DOM 节点和相关样式插入到最终输出的 html 中。
    3. 饿了么内部的生成骨架页面的工具：该方案通过一个 webpack 插件 page-skeleton-webpack-plugin 的方式与项目开发无缝集成，属于在自动生成骨架屏方面做的非常强大的了，并且可以启动 UI 界面专门调整骨架屏，但是在面对复杂的页面也会有不尽如人意的地方，而且生成的骨架屏节点是基于页面本身的结构和 CSS，存在嵌套比较深的情况，体积不会太小，并且只支持 history 模式。
---

# 为何建议把js放在body最后？
  * BODY中编写的都是HTML标签，JS很多时候需要操作这些元素，首先我们要保证元素加载成功，才可以在JS中获取到，所以我们通常会把JS放在BODY的末尾。
---

# 如果有很多if else，怎么优化
1. 提前return，去除不必要的else
2. 使用条件三目运算符
3. 学会使用 Optional
4. 使用枚举
5. 合并条件表达式
---

# encodeURL原理
  * .encodeURL函数主要是来对URI来做转码，它默认是采用的UTF-8的编码.
  * . UTF-8编码的格式:一个汉字来三个字节构成，每一个字节会转换成16进制的编码，同时添加上%号.
  * 假设页面端输入的中文是一个“中”，按照下面步骤进行解码
    1. 第一次encodeURI，按照utf-8方式获取字节数组变成[-28,-72-83]，对字节码数组进行遍历，把每个字节转化成对应的16进制数，这样就变成了[E4,B8,AD],最后变成[%E4,%B8,%AD]  此时已经没有了多字节字符，全部是单字节字符。
    2. 第二次encodeURI，进行编码，会把%看成一个转义字符，并不编码%以后字符，会把%编码成%25.把数组最后变成[%25E4,%25B8,%25AD]然后就把处理后的数据[%25E4,%25B8,%25AD]发往服务器端，当应用服务器调用getParameter方法，getParameter方法会去向应用服务器请求参数应用服务器最初获得的就是发送来的[%25E4,%25B8,%25AD]，应用服务器会对这个数据进行URLdecode操作，应用服务器进行解码的这一次，不管是按照UTF-8，还是GBK，还是ISO-8859，,都能得到[%E4,%B8,%AD]，因为都会把%25解析成%.并把这个值返回给getParameter方法
    3. 再用UTF-8解码一次，就得到"中"了。
---

# 矢量图比起其他图片到底好在哪
  * 可以无限放大或缩小，不会影响图像素质，文件体积较小，编辑灵活。
---

# 雪碧图怎么获取需要图片的位置
  * 使用css的background-position属性
---

# label都有哪些作用？并举相应的例子说明
lable可以关联控件，可以和表单元素结合。有两个属性，for和accesskey。
---

# 元素的alt和title有什么区别？
  1. alt属性定义了图像的备用文本描述，只能用在<img>、<area>和<input>元素中，用于网页中图片无法正常显示时给用户提供文字说明使其了解图像信息。alt是替代图像作用而不是提供额外说明文字的。使用alt属性还具有搜索引擎优化效果，因为搜素引擎是无法直接读取图像的信息的，alt可以为其提供文字信息所以对搜索引擎比较友好。
  2. title是全局属性，可以用在所有HTML元素上，包含了表示咨询信息文本，和它属于的元素相关。把鼠标移动到元素上面，就会显示title的内容，以达到补充说明或者提示的效果。
---

# 谈谈你对input元素中readonly和disabled属性的理解
  * 相同点：都会使文本框变成只读，不可编辑。
  * 不同点：
    1. disabled属性在将input文本框变成只读不可编辑的同时，还会使文本框变灰，但是readonly不会。
    2. disabled属性修饰后的文本框内容，在不可编辑的同时，通过js也是获取不到的。但是用readonly修饰后的文本框内容，是可以通过js获取到的，也就只是简单的不可编辑而已！
    3. disabled属性对input文本框，单选radio,多选checkbox都适用，但是readonly就不适用，用它修饰后的单选以及多选按钮仍然是可以编辑状态的。
---

# 说说你对属性data-的理解
> data- 属性是H5新增的自定义属性，也可以用来存储值。
---

# 标准模式和怪异模式有什么区别？
standards盒模型：width = content
quirks盒模型：width = content + border + padding
---

# Form表单是怎么上传文件的？你了解它的原理吗？
  * 简单来说就是把文件转化成字节流，然后使用http进行传输，后端接受后在把二进制转化成原先的文件格式。
  * WEB服务器端程序接收到“multipart/form-data”类型的HTTP请求消息后，其核心和基本的编程工作就是读取请求消息中的实体内容，然后解析出每个分区的数据，接着再从每个分区中解析出描述头和主体内容部分。
---

# a标签的href和onclick属性同时存在时哪个先触发？
> onclick事件先触发， 如果函数执行返回false(全等), 则href不会被触发。
---

# 说说你对accesskey的理解，举例说明它有什么运用场景？
accessKey 可以注入到任意的元素中，通过快捷键触发对应元素的绑定事件。
```
  <a href="http://www.baidu.com" accesskey="1">快捷键1直接跳转百度</a>
``` 
---

# 说说form-data、x-www-form-urlencoded、raw、binary的区别是什么？
  1. multipart/form-data 其请求内容格式为Content-Type: multipart/form-data,用来指定请求内容的数据编码格式，一般用来文件上传。
  2. application/x-www-form-urlencoded 是post的默认格式，使用js中URLencode转码方法。
  3. raw 可上传任意格式的文本，可以上传text、json、xml、html等各种文本类型。
  4. binary 等同于Content-Type:application/octet-stream，只可上传二进制数据。
---

# 请解释下href="javascript:void(0)"和href="#"的区别是什么？
> "#" 包含了一个位置信息，默认的锚是#top 也就是网页的上端。而javascript:void(0), 仅仅表示一个死链接。在页面很长的时候会使用 # 来定位页面的具体位置，格式为：# + id。如果你要定义一个死链接请使用 javascript:void(0) 。
---