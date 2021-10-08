---
title: 面试——webpack篇
date: 2020-12-29 10:50:01
tags: 
  - 面试
  - webpack
type: 面试                                                                 # 标签、分类
description:  Webpack 是一个前端资源加载/打包工具。它将根据模块的依赖关系进行静态分析,然后将这些模块按照指定的规则生成对应的静态资源。
top_img: https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=3813469802,1665117316&fm=26&gp=0.jpg             # 文章的顶部图片
aside: true                                                                         # 展示文章侧边栏(默认为true)
categories: 
  - 面试
  - webpack                                                                 # 文章标签
cover: https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=3813469802,1665117316&fm=26&gp=0.jpg                 # 文章的缩略图（用在首页）
---

# webpack与grunt、gulp的不同？
  * 三者都是前端构建工具，grunt和gulp在早期比较流行，现在webpack相对来说比较主流，不过一些轻量化的任务还是会用gulp来处理，比如单独打包CSS文件等。
  * grunt和gulp是基于任务和流（Task、Stream）的。类似jQuery，找到一个（或一类）文件，对其做一系列链式操作，更新流上的数据， 整条链式操作构成了一个任务，多个任务就构成了整个web的构建流程。
  * webpack是基于入口的。webpack会自动地递归解析入口所需要加载的所有资源文件，然后用不同的Loader来处理不同的文件，用Plugin来扩展webpack功能。
  
  * 所以总结一下：gulp和grunt需要开发者将整个前端构建过程拆分成多个`Task`，并合理控制所有`Task`的调用关系 webpack需要开发者找到入口，并需要清楚对于不同的资源应该使用什么Loader做何种解析和加工
---

# Grunt与Gulp的区别
  1. 书写方式：
    - grunt 运用配置的思想来写打包脚本，一切皆配置，所以会出现比较多的配置项，诸如option,src,dest等等。而且不同的插件可能会有自己扩展字段，导致认知成本的提高，运用的时候要搞懂各种插件的配置规则。
    - gulp 是用代码方式来写打包脚本，并且代码采用流式的写法，只抽象出了gulp.src, gulp.pipe, gulp.dest, gulp.watch 接口,运用相当简单。经尝试，使用gulp的代码量能比grunt少一半左右。

  2. 任务划分:
    - grunt 中每个任务对应一个最外层配置的key, 大任务可以包含小任务，以一种树形结构存在。
    - gulp 中没有子任务的概念，通过注册task来完成

  3. 运行效率：
    - Grunt: 每个任务处理完成后存放在本地磁盘.tmp目录中，有本地磁盘的I/O操作，会导致打包速度比较慢。
    - Gulp: gulp与grunt都是按任务执行，gulp有一个文件流的概念。每一步构建的结果并不会存在本地磁盘，而是保存在内存中，下一个步骤是可以使用上一个步骤的内存，大大增加了打包的速度。

# webpack 如何实现动态加载
  1. 使用import()
  2. require.ensure
  3. 使用bundle-loader
---

# React.lazy 的原理是什么？
  1. React.lazy的用法
    * React.lazy方法可以异步加载组件文件。
      ```
        const Foo = React.lazy(() => import('../componets/Foo));
      ```
    
    * React.lazy不能单独使用，需要配合React.suspense，suspence是用来包裹异步组件，添加loading效果等。 
      ```
        <React.Suspense fallback={<div>loading...</div>}>
          <Foo/>
        </React.Suspense>
      ```

  2. React.lazy原理
    * React.lazy使用import来懒加载组件，import在webpack中最终会调用requireEnsure方法，动态插入script来请求js文件，类似jsonp的形式。
---

# webpack如何做异步加载？
  1. import函数
  2. require.ensure
---

# webpack怎么处理内联css的？
  * 将页面打包过程产生的所有css提取层一个独立的文件，然后将这个css文件内联进html header里。
---

# webpack 能动态加载 require 引入的模块吗？
不能
---

# require 引入的模块 webpack 能做 Tree Shaking 吗？
不支持
---

# webpack的plugins和loaders的实现原理。
  * loaders原理： 
    - webpack 只能直接处理 javascript 格式的代码。任何非 js 文件都必须被预先处理转换为 js 代码，才可以参与打包。loader（加载器）就是这样一个代码转换器。它由 webpack 的 `loader runner` 执行调用，接收原始资源数据作为参数（当多个加载器联合使用时，上一个loader的结果会传入下一个loader），最终输出 javascript 代码（和可选的 source map）给 webpack 做进一步编译。
  * plugins原理：
    - 读取配置的过程中会先执行 new HelloPlugin(options) 初始化一个 HelloPlugin 获得其实例。
    - 初始化 compiler 对象后调用 HelloPlugin.apply(compiler) 给插件实例传入 compiler 对象。
    - 插件实例在获取到 compiler 对象后，就可以通过compiler.plugin(事件名称, 回调函数) 监听到 Webpack 广播出来的事件。 并且可以通过 compiler 对象去操作 Webpack。
---

# webpack如何优化编译速度?
1. 使用高版本的 Webpack 和 Node.js
2. 多进程/多实例构建：HappyPack(不维护了)、thread-loader
3. 压缩代码
  * 多进程并行压缩
    - webpack-paralle-uglify-plugin
    - uglifyjs-webpack-plugin 开启 parallel 参数 (不支持ES6)
    - terser-webpack-plugin 开启 parallel 参数
  * 通过 mini-css-extract-plugin 提取 Chunk 中的 CSS 代码到单独文件，通过 css-loader 的 minimize 选项开启 cssnano 压缩 CSS。
4. 图片压缩
  * 使用基于 Node 库的 imagemin (很多定制选项、可以处理多种图片格式)
  * 配置 image-webpack-loader
5. 缩小打包作用域：
  * exclude/include (确定 loader 规则范围)
  * resolve.modules 指明第三方模块的绝对路径 (减少不必要的查找)
  * resolve.mainFields 只采用 main 字段作为入口文件描述字段 (减少搜索步骤，需要考虑到所有运行时依赖的第三方模块的入口文件描述字段)
  * resolve.extensions 尽可能减少后缀尝试的可能性
  * noParse 对完全不需要解析的库进行忽略 (不去解析但仍会打包到 bundle 中，注意被忽略掉的文件里不应该包含 import、require、define 等模块化语句)
  * IgnorePlugin (完全排除模块)
  * 合理使用alias
6. 提取页面公共资源：
  * 基础包分离：
    - 使用 html-webpack-externals-plugin，将基础包通过 CDN 引入，不打入 bundle 中
    - 使用 SplitChunksPlugin 进行(公共脚本、基础包、页面公共文件)分离(Webpack4内置) ，替代了 CommonsChunkPlugin 插件
7. DLL：
  * 使用 DllPlugin 进行分包，使用 DllReferencePlugin(索引链接) 对 manifest.json 引用，让一些基本不会改动的代码先打包成静态资源，避免反复编译浪费时间。
  * HashedModuleIdsPlugin 可以解决模块数字id问题
8. 充分利用缓存提升二次构建速度：
  * babel-loader 开启缓存
  * terser-webpack-plugin 开启缓存
  * 使用 cache-loader 或者 hard-source-webpack-plugin
9. Tree shaking
  * 打包过程中检测工程中没有引用过的模块并进行标记，在资源压缩时将它们从最终的bundle中去掉(只能对ES6 Modlue生效) 开发中尽可能使用ES6 Module的模块，提高tree shaking效率
  * 禁用 babel-loader 的模块依赖解析，否则 Webpack 接收到的就都是转换过的 CommonJS 形式的模块，无法进行 tree-shaking
  * 使用 PurifyCSS(不在维护) 或者 uncss 去除无用 CSS 代码
    - purgecss-webpack-plugin 和 mini-css-extract-plugin配合使用(建议)
10. Scope hoisting

  * 构建后的代码会存在大量闭包，造成体积增大，运行代码时创建的函数作用域变多，内存开销变大。Scope hoisting 将所有模块的代码按照引用顺序放在一个函数作用域里，然后适当的重命名一些变量以防止变量名冲突
  * 必须是ES6的语法，因为有很多第三方库仍采用 CommonJS 语法，为了充分发挥 Scope hoisting 的作用，需要配置mainFields 对第三方模块优先采用 jsnext:main 中指向的ES6模块化语法
11. 动态Polyfill
  * 建议采用 polyfill-service 只给用户返回需要的polyfill，社区维护。 (部分国内奇葩浏览器UA可能无法识别，但可以降级返回所需全部polyfill)
---

# webpack优化产出代码
  * 小图片base64编码
  * bundle加hash
  * 懒加载
  * 提取公共代码
  * 使用cdn加速
  * IgnorePlugin
  * 使用production
  * Scope Hosting
---

# webpack 热更新原理
  * 大概是我们用 webpack-dev-server启动一个服务之后，浏览器和服务端是通过 websocket进行长连接， webpack内部实现的 watch就会监听文件修改，只要有修改就 webpack会重新打包编译到内存中，然后 webpack-dev-server依赖中间件 webpack-dev-middleware和 webpack之间进行交互，每次热更新都会请求一个携带 hash值的 json文件和一个 js， websocker传递的也是 hash值，内部机制通过 hash值检查进行热更新
---

# webpack原理
  1. 先逐级递归识别依赖，构建依赖图谱
  2. 将代码转化成AST抽象语法树
  3. 在AST阶段中去处理代码
  4. 把AST抽象语法树变成浏览器可以识别的代码， 然后输出
---

# webpack的构建流程(生命周期)
  1. 初始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数；
  2. 开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译；
  3. 确定入口：根据配置中的 entry 找出所有的入口文件；
  4. 编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理；
  5. 完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系；
  6. 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会；
  7. 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。
  8. 整个过程中webpack会通过发布订阅模式，向外抛出一些hooks，而webpack的插件即可通过监听这些关键的事件节点，执行插件任务进而达到干预输出结果的目的。
---

# webpack的打包过程
  1. 识别入口文件
  2. 通过逐层识别模块依赖。（Commonjs、amd或者es6的import，webpack都会对其进行分析。来获取代码的依赖）
  3. webpack做的就是分析代码。转换代码，编译代码，输出代码
  4. 最终形成打包后的代码
---

# babel原理(聊聊babel的抽象语法树)
  * babel的转译过程分为三个阶段：解析(parsing)、转换(transforming)、生成(generating)，以ES6代码转译为ES5代码为例，babel的转换过程就是构建和修改抽象语法树的过程。babel转译的具体过程如下：
    1. 解析：将代码转换成 AST
      - 词法分析：将代码(字符串)分割为token流，即语法单元成的数组
      - 语法分析：分析token流(上面生成的数组)并生成 AST
    2. 转换：访问 AST 的节点进行变换操作生产新的 AST
      - Taro就是利用 babel 完成的小程序语法转换
    3. 生成：以新的 AST 为基础生成代码
---

# 写过babel插件吗？是用来干什么？怎么写的？
---

# webpack怎么配置多入口
  1. 在webpack.config.js的entry定义入口文件，这些写多个
    ```
      entry:{ //定义入口文件
        index:"./src/index.js",
        about:"./src/about.js"
      }
    ```
  2. 修改output,把output里的filename写成"[name].js"
    ```
      output:{
        filename:’[name].js’,
        path:path.resolve(__dirname,“dist”) //默认
      }
    ```
---

# webpack配置多出口
  1. 在根路径下新建html，然后调整一下webpack.config.json文件下的plugins配置
    ```
      plugins:[//设置插件使用
        new HtmlWebpackPlugin({
          template:"./index.html",	//设置打包后的文件,插入到的模板html文件是哪个(以来的模板文件)
          filename:"index.html",		//输出文件的名称(打包后的html文件名,可以自己设置,最好不要变动)
          hot:true,					//自动刷新浏览器
          chunks:['index']			//代表指定的入口文件是哪个
        }),
        
        //那么同理我们有两个出口是不是要配置两次，像这样
        
        new HtmlWebpackPlugin({
          template:"./about.html",	//设置打包后的文件,插入到的模板html文件是哪个(以来的模板文件)
          filename:"about.html",		//输出文件的名称(打包后的html文件名,可以自己设置,最好不要变动)
          hot:true,					//自动刷新浏览器
          chunks:['about']			//代表指定的入口文件是哪个
        })
      ]
    ```

# inline/pre/post/normal loader执行先后顺序是?(从右到左或者从下到上)
所有的loader的执行顺序都有两个阶段：标记(pitching)和执行(normal)阶段，类似于js中的事件冒泡、捕获阶段。
  * 标记阶段： post，inline，normal，pre
  * 执行阶段：pre，normal，inline，post
---

# vue现在出了一个打包工具vite，介绍下为什么会比其他的打包工具快
  * 启动方面：
    1. Vite 通过在一开始将应用中的模块区分为 依赖 和 源码 两类，改进了开发服务器启动时间。
    2. 依赖 大多为纯 JavaScript 并在开发时不会变动。一些较大的依赖（例如有上百个模块的组件库）处理的代价也很高。依赖也通常会以某些方式（例如 ESM 或者 CommonJS）被拆分到大量小模块中。
    3. Vite 将会使用 esbuild 预构建依赖。Esbuild 使用 Go 编写，并且比以 JavaScript 编写的打包器预构建依赖快 10-100 倍。
    4. 源码 通常包含一些并非直接是 JavaScript 的文件，需要转换（例如 JSX，CSS 或者 Vue/Svelte 组件），时常会被编辑。同时，并不是所有的源码都需要同时被加载。（例如基于路由拆分的代码模块）。
    5. Vite 以 原生 ESM 方式服务源码。这实际上是让浏览器接管了打包程序的部分工作：Vite 只需要在浏览器请求源码时进行转换并按需提供源码。根据情景动态导入的代码，即只在当前屏幕上实际使用时才会被处理。

  * 热更新方面：
    1. 在 Vite 中，HMR 是在原生 ESM 上执行的。当编辑一个文件时，Vite 只需要精确地使已编辑的模块与其最近的 HMR 边界之间的链失效（大多数时候只需要模块本身），使 HMR 更新始终快速，无论应用的大小。
    2. Vite 同时利用 HTTP 头来加速整个页面的重新加载（再次让浏览器为我们做更多事情）：源码模块的请求会根据 304 Not Modified 进行协商缓存，而依赖模块请求则会通过 Cache-Control: max-age=31536000,immutable 进行强缓存，因此一旦被缓存它们将不需要再次请求。
---

# vite是什么？特点是什么？
  * Vite是一个基于浏览器原生 ES Modules 的开发服务器。利用浏览器去解析模块，在服务器端按需编译返回，完全跳过了打包这个概念，服务器随起随用。同时不仅有 Vue 文件支持，还搞定了热更新，而且热更新的速度不会随着模块增多而变慢。

  * 特点：
    1. 启动快
    2. 即时热更新
    3. 真正的按需编译 
---

# Webpack与Vite的区别
  * webpack会先打包，然后启动开发服务器，请求服务器时直接给予打包结果。
  * 而vite是直接启动开发服务器，请求哪个模块再对该模块进行实时编译。
  * 由于现代浏览器本身就支持ES Module，会自动向依赖的Module发出请求。vite充分利用这一点，将开发环境下的模块文件，就作为浏览器要执行的文件，而不是像webpack那样进行打包合并。
  * 由于vite在启动的时候不需要打包，也就意味着不需要分析模块的依赖、不需要编译，因此启动速度非常快。当浏览器请求某个模块时，再根据需要对模块内容进行编译。这种按需动态编译的方式，极大的缩减了编译时间，项目越复杂、模块越多，vite的优势越明显。
  * 在HMR（热更新）方面，当改动了一个模块后，仅需让浏览器重新请求该模块即可，不像webpack那样需要把该模块的相关依赖模块全部编译一次，效率更高。
  * 当需要打包到生产环境时，vite使用传统的rollup（也可以自己手动安装webpack来）进行打包，原因是esbuild对于css和代码分割不是很友好。因此，vite的主要优势在开发阶段。另外，由于vite利用的是ES Module，因此在代码中（除了vite.config.js里面，这里是node的执行环境）不可以使用CommonJS

# vite原理
  * 当声明一个script标签类型为module时，浏览器就会像服务器发起一个GET
  * 浏览器请求到了main.js文件，检测到内部含有import引入的包，又会对其内部的 import 引用发起 HTTP 请求获取模块的内容文件
  * Vite 的主要功能就是通过劫持浏览器的这些请求，并在后端进行相应的处理将项目中使用的文件通过简单的分解与整合，然后再返回给浏览器，vite整个过程中没有对文件进行打包编译，所以其运行速度比原始的webpack开发编译速度快出许多！

# webpack的缺点
  1. 缓慢的服务器启动
    * 当冷启动开发服务器时，基于打包器的方式是在提供服务前去急切地抓取和构建你的整个应用。

  2. webpack致命缺点3.热更新效率低下
    * 当基于打包器启动时，编辑文件后将重新构建文件本身。显然我们不应该重新构建整个包，因为这样更新速度会随着应用体积增长而直线下降
    * 一些打包器的开发服务器将构建内容存入内存，这样它们只需要在文件更改时使模块图的一部分失活[1]，但它也仍需要整个重新构建并重载页面。这样代价很高，并且重新加载页面会消除应用的当前状态，所以打包器支持了动态模块热重载（HMR）：允许一个模块 “热替换” 它自己，而对页面其余部分没有影响。这大大改进了开发体验 - 然而，在实践中我们发现，即使是 HMR 更新速度也会随着应用规模的增长而显著下降。
---

# 与webpack类似的工具还有哪些？谈谈你为什么最终选择（或放弃）使用webpack？
  1. 同样是基于入口的打包工具还有以下几个主流的：
    * webpack
    * rollup
    * parcel
  2. 从应用场景上来看：
    * webpack适用于大型复杂的前端站点构建
    * rollup适用于基础库的打包，如vue、react
    * parcel适用于简单的实验性项目，他可以满足低门槛的快速看到效果
  3. 由于parcel在打包过程中给出的调试信息十分有限，所以一旦打包出错难以调试，所以不建议复杂的项目使用parcel
---

# Rollup和webpack区别
---

# 有哪些常见的Loader？他们是解决什么问题的？
  * style-loader 将css添加到DOM的内联样式标签style里
  * css-loader 允许将css文件通过require的方式引入，并返回css代码
  * less-loader 处理less
  * sass-loader 处理sass
  * postcss-loader 用postcss来处理CSS
  * autoprefixer-loader 处理CSS3属性前缀，已被弃用，建议直接使用postcss
  * file-loader 分发文件到output目录并返回相对路径
  * url-loader 和file-loader类似，但是当文件小于设定的limit时可以返回一个Data Url
  * html-minify-loader 压缩HTML
  * babel-loader 用babel来转换ES6文件到ES5
---

# 有哪些常见的Plugin？他们是解决什么问题的？
  * define-plugin：定义环境变量
  * commons-chunk-plugin：提取公共代码
  * uglifyjs-webpack-plugin：通过UglifyES压缩ES6代码
---

# Loader和Plugin的不同？
  1. 不同的作用
    * Loader直译为"加载器"。Webpack将一切文件视为模块，但是webpack原生是只能解析js文件，如果想将其他文件也打包的话，就会用到loader。 所以Loader的作用是让webpack拥有了加载和解析非JavaScript文件的能力。
    * Plugin直译为"插件"。Plugin可以扩展webpack的功能，让webpack具有更多的灵活性。 在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

  2. 不同的用法
    * Loader在module.rules中配置，也就是说他作为模块的解析规则而存在。 类型为数组，每一项都是一个Object，里面描述了对于什么类型的文件（test），使用什么加载(loader)和使用的参数（options）

    * Plugin在plugins中单独配置。 类型为数组，每一项是一个plugin的实例，参数都通过构造函数传入。
---

# 怎么配置单页应用？怎么配置多页应用？
  * 单页应用可以理解为webpack的标准模式，直接在entry中指定单页应用的入口即可，这里不再赘述
  * 多页应用的话，可以使用webpack的 AutoWebPlugin来完成简单自动化的构建，但是前提是项目的目录结构必须遵守他预设的规范。 多页应用中要注意的是：
    - 每个页面都有公共的代码，可以将这些代码抽离出来，避免重复的加载。比如，每个页面都引用了同一套css样式表
    - 随着业务的不断扩展，页面可能会不断的追加，所以一定要让入口的配置足够灵活，避免每次添加新页面还需要修改构建配置
---

# npm打包时需要注意哪些？如何利用webpack来更好的构建？  
  * NPM模块需要注意以下问题：
    1. 要支持CommonJS模块化规范，所以要求打包后的最后结果也遵守该规则。
    2. Npm模块使用者的环境是不确定的，很有可能并不支持ES6，所以打包的最后结果应该是采用ES5编写的。并且如果ES5是经过转换的，请最好连同SourceMap一同上传。
    3. Npm包大小应该是尽量小（有些仓库会限制包大小）
    4. 发布的模块不能将依赖的模块也一同打包，应该让用户选择性的去自行安装。这样可以避免模块应用者再次打包时出现底层模块被重复打包的情况。
    5. UI组件类的模块应该将依赖的其它资源文件，例如.css文件也需要包含在发布的模块里。

  * 基于以上需要注意的问题，我们可以对于webpack配置做以下扩展和优化：
    1. CommonJS模块化规范的解决方案： 设置output.libraryTarget='commonjs2'使输出的代码符合CommonJS2 模块化规范，以供给其它模块导入使用
    2. 输出ES5代码的解决方案：使用babel-loader把 ES6 代码转换成 ES5 的代码。再通过开启devtool: 'source-map'输出SourceMap以发布调试。
    3. Npm包大小尽量小的解决方案：Babel 在把 ES6 代码转换成 ES5 代码时会注入一些辅助函数，最终导致每个输出的文件中都包含这段辅助函数的代码，造成了代码的冗余。解决方法是修改.babelrc文件，为其加入transform-runtime插件
    4. 不能将依赖模块打包到NPM模块中的解决方案：使用externals配置项来告诉webpack哪些模块不需要打包。
---

# 前端为什么要进行打包和构建？
  1. 代码层面：
    * 体积更小（Tree-shaking、压缩、合并），加载更快
    * 编译高级语言和语法（TS、ES6、模块化、scss）
    * 兼容性和错误检查（polyfill,postcss,eslint）
  2. 研发流程层面：
    * 统一、高效的开发环境
    * 统一的构建流程和产出标准
    * 集成公司构建规范（提测、上线）
---

# 什么是bundle，什么是chunk，什么是module
  * bundle： 最终打包完成的文件，一般就是和chunk一一对应的关系，bundle就是对chunk进行编译压缩打包等处理后的产出。
  * chunk： 表示代码块，一个chunk可以由多个模块组成。
  * module： 开发中的每一个文件都可以看作是module，模块不局限于js，也包含css，图片等。
---

# babel和webpack的区别
  * babel JS新语法编译工具，只关心语法，不关心模块化
  * webpack -打包构建工具，是多个Loader plugin的集合
---

# babel-polyfill babel-runtime 区别
  * babel-polyfill 会污染全局
  * babel-runtime 不会污染全局，产出第三方lib时要用babel-runtime
---

# babel的缓存是怎么实现的
---

# webpack怎么配置mock转发代理，mock的服务，怎么拦截转换的
---

# 自己有没有写过ast, webpack通过什么把公共的部分抽出来的，属性配置是什么
---

# webpack如何做到页面不刷新也就就自动更新的
---

# Webpack 五个核心概念分别是什么？
1. Entry
  * 入口（Entry）指示 Webpack 以哪个文件为入口起点开始打包，分析内部构件依赖图
2. Output
  * 输出（Output）指示 Webpack 打包后的资源 bundles 输出到哪里去，以及如何命名
3. Loader
  * Loader 能让 Webpack 处理非 JavaScript/json 文件（Webpack 自身只能处理 JavaScript/json ）
4. Plugins
  * 插件（Plugins）可以用于执行范围更广的任务，包括从打包优化和压缩到重新定义环境中的变量
5. Mode
  * 模式(Mode)指示 Webpack 使用相应模式的配置，只有development（开发环境）和production（生产环境）两种模式
---

# 如何利用webpack来优化前端性能？
  * 开发环境下：
    1. 开启HMR功能，优化打包构建速度
    2. 配置 devtool: ‘source-map’，优化代码运行的性能
  * 生产环境下：
    1. oneOf 优化
      - 默认情况下，假设设置了7、8个loader，每一个文件都得通过这7、8个loader处理（过一遍），浪费性能，使用 oneOf 找到了就能直接用，提升性能
    2. 开启 babel 缓存
      - 当一个 js 文件发生变化时，其它 js 资源不用变
    3. code split 分割
      - 将js文件打包分割成多个bundle，避免体积过大
    4. 懒加载和预加载
    5. PWA 网站离线访问
    6. 多进程打包
      - 开启多进程打包，主要处理js文件（babel-loader干的活久），进程启动大概为600ms，只有工作消耗时间比较长，才需要多进程打包，提升打包速度
    7. dll 打包第三方库
      - code split将第三方库都打包成一个bundle，这样体积过大，会造成打包速度慢
      - dll 是将第三方库打包成多个bundle，从而进行速度优化
---

# hash、chunkhash、contenthash三者的区别？
浏览器访问网站后会强缓存资源，第二次刷新就不会请求服务器（一般会定个时间再去请求服务器），假设有了bug改动了文件，但是浏览器又不能及时请求服务器，所以就用到了文件资源缓存（改变文件名的hash值）
  1. hash：不管文件变不变化，每次wepack构建时都会生成一个唯一的hash值
  2. chunkhash：根据chunk生成的hash值。如果打包来源于同一个chunk，那么hash值就一样
    - 问题：js和css同属于一个chunk，修改css，js文件同样会被打爆
  3. contenthash：根据文件的内容生成hash值。不同文件hash值一定不一样
---

# webpack 打包文件的 hash 值怎么设置？hash 值长度如何设置，用什么分割？
---

# hash 除了用文件内容外还可以用哪些来计算？
---

# webpack 注入变量给 js 调用如何做？
---

# mode 注入的环境值是 webpack 读取还是 node 读取？
---

# 在哪里配置 webpack 启动的命令？
---

# webpack-dev-server 和 http服务器的区别
webpack-dev-server使用内存来存储webpack开发环境下的打包文件，并且可以使用模块热更新，比传统的http服务对开发更加有效。
---

# 什么是长缓存？在webpack中如何做到长缓存优化？
  * 浏览器在用户访问页面的时候，为了加快加载速度，会对用户访问的静态资源进行存储，但是每一次代码升级或者更新，都需要浏览器去下载新的代码，最方便和最简单的更新方式就是引入新的文件名称。

  * 在webpack中，可以在output给出输出的文件制定chunkhash，并且分离经常更新的代码和框架代码，通过NameModulesPlugin或者HashedModulesPlugin使再次打包文件名不变。
---

# 什么是Tree-sharking?CSS可以Tree-shaking吗?
Tree-shaking是指在打包中去除那些引入了，但是在代码中没有被用到的那些死代码。在webpack中Tree-shaking是通过uglifySPlugin来Tree-shakingJS。Css需要使用Purify-CSS。
---

# tree-sharking的原理是什么？
  * ES6 Module引入进行静态分析，故而编译的时候正确判断到底加载了那些模块
  * 静态分析程序流，判断那些模块和变量未被使用或者引用，进而删除对应代码
---

# 你知道sourceMap是什么吗？生产环境怎么用？
  * source map 是将编译、打包、压缩后的代码映射回源代码的过程。打包压缩后的代码不具备良好的可读性，想要调试源码就需要 soucre map。
  * map文件只要不打开开发者工具，浏览器是不会加载的。

  * 线上环境一般有三种处理方案：
    1. hidden-source-map：借助第三方错误监控平台 Sentry 使用
    2. nosources-source-map：只会显示具体行数以及查看源代码的错误栈。安全性比 sourcemap 高
    3. sourcemap：通过 nginx 设置将 .map 文件只对白名单开放(公司内网)

  * 注意：避免在生产中使用 inline- 和 eval-，因为它们会增加 bundle 体积大小，并降低整体性能。
---

# 模块打包原理知道吗？
Webpack 实际上为每个模块创造了一个可以导出和导入的环境，本质上并没有修改代码的执行逻辑，代码执行顺序与模块加载顺序也完全一致。
---

# 文件监听原理呢？
* 在发现源码发生变化时，自动重新构建出新的输出文件。
* Webpack开启监听模式，有两种方式：
  1. 启动 webpack 命令时，带上 --watch 参数
  2. 在配置 webpack.config.js 中设置 watch:true
  缺点：每次需要手动刷新浏览器

* 原理：轮询判断文件的最后编辑时间是否变化，如果某个文件发生了变化，并不会立刻告诉监听者，而是先缓存起来，等 aggregateTimeout 后再执行。

# 如何对bundle体积进行监控和分析？
  * VSCode 中有一个插件 Import Cost 可以帮助我们对引入模块的大小进行实时监测，还可以使用 webpack-bundle-analyzer 生成 bundle 的模块组成图，显示所占体积。
  * bundlesize 工具包可以进行自动化资源体积监控。
---

# 是否写过Loader？简单描述一下编写loader的思路？
  * Loader 支持链式调用，所以开发上需要严格遵循“单一职责”，每个 Loader 只负责自己需要负责的事情。
  * 思路
    1. Loader 运行在 Node.js 中，我们可以调用任意 Node.js 自带的 API 或者安装第三方模块进行调用
    2. Webpack 传给 Loader 的原内容都是 UTF-8 格式编码的字符串，当某些场景下 Loader 处理二进制文件时，需要通过 exports.raw = true 告诉 Webpack 该 Loader 是否需要二进制数据
    3. 尽可能的异步化 Loader，如果计算量很小，同步也可以
    4. Loader 是无状态的，我们不应该在 Loader 中保留状态
    5. 使用 loader-utils 和 schema-utils 为我们提供的实用工具
    6. 加载本地 Loader 方法
      - Npm link
      - ResolveLoader
---

# 是否写过Plugin？简单描述一下编写plugin的思路？
  * webpack在运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在特定的阶段钩入想要添加的自定义功能。Webpack 的 Tapable 事件流机制保证了插件的有序性，使得整个系统扩展性良好。

  * 思路：
  1. compiler 暴露了和 Webpack 整个生命周期相关的钩子
  2. compilation 暴露了与模块和依赖有关的粒度更小的事件钩子
  3. 插件需要在其原型上绑定apply方法，才能访问 compiler 实例
  4. 传给每个插件的 compiler 和 compilation对象都是同一个引用，若在一个插件中修改了它们身上的属性，会影响后面的插件
  5. 找出合适的事件点去完成想要的功能
    - emit 事件发生时，可以读取到最终输出的资源、代码块、模块及其依赖，并进行修改(emit 事件是修改 Webpack 输出资源的最后时机)
    - watch-run 当依赖的文件发生变化时会触发
  6. 异步的事件需要在插件处理完任务时调用回调函数通知 Webpack 进入下一个流程，不然会卡住
---

# dev-server的原理是什么？描述一下他的具体流程
  * webpack-dev-server 可以作为命令行工具使用，核心模块依赖是 webpack 和 webpack-dev-middleware。webapck-dev-server 负责启动一个 express 服务器监听客户端请求；实例化 webpack compiler；启动负责推送 webpack 编译信息的 webscoket 服务器；负责向 bundle.js 注入和服务端通信用的 webscoket 客户端代码和处理逻辑。webapck-dev-middleware 把 webpack compiler 的 outputFileSystem 改为 in-memory fileSystem；启动 webpack watch 编译；处理浏览器发出的静态资源的请求，把 webpack 输出到内存的文件响应给浏览器。

  * 每次 webpack 编译完成后向客户端广播 ok 消息，客户端收到信息后根据是否开启 hot 模式使用 liveReload 页面级刷新模式或者 hotReload 模块热替换。hotReload 存在失败的情况，失败的情况下会降级使用页面级刷新。

  * 开启 hot 模式，即启用 HMR 插件。hot 模式会向服务器请求更新过后的模块，然后对模块的父模块进行回溯，对依赖路径进行判断，如果每条依赖路径都配置了模块更新后所需的业务处理回调函数则是 accepted 状态，否则就降级刷新页面。判断 accepted 状态后对旧的缓存模块和父子依赖模块进行替换和删除，然后执行 accept 方法的回调函数，执行新模块代码，引入新模块，执行业务处理代码。
---

# dev-server是怎么跑起来的？
  1. 启动http服务
  2. webpack构建输出bundle到内存，http服务从内存中读取bundle文件
  3. 监听文件变化，重新执行第二步操作
---

# 请说一下DllPlugin和DllReferencePlugin的工作原理，为什么不直接使用压缩版本的js
---

# Webpack 怎么提取公共模块
---

# Webpack 打包出来的文件如何拆包？
---

# 使用webpack开发时，你用过哪些可以提高效率的插件？
  * webpack-dashboard：可以更友好的展示相关打包信息。
  * webpack-merge：提取公共配置，减少重复配置代码
  * speed-measure-webpack-plugin：简称 SMP，分析出 Webpack 打包过程中 Loader 和 Plugin 的耗时，有助于找到构建过程中的性能瓶颈。
  * size-plugin：监控资源体积变化，尽早发现问题
  * HotModuleReplacementPlugin：模块热替换
---

# 文件指纹是什么？怎么用？
  * 文件指纹是打包后输出的文件名的后缀。
    - Hash：和整个项目的构建相关，只要项目文件有修改，整个项目构建的 hash 值就会更改
    - Chunkhash：和 Webpack 打包的 chunk 有关，不同的 entry 会生出不同的 chunkhash
    - Contenthash：根据文件内容来定义 hash，文件内容不变，则 contenthash 不变

  * JS的文件指纹设置
    - 设置 output 的 filename，用 chunkhash。

  * CSS的文件指纹设置
    - 设置 MiniCssExtractPlugin 的 filename，使用 contenthash。

  * 图片的文件指纹设置
    - 设置file-loader的name，使用hash。
---

# 在实际工程中，配置文件上百行乃是常事，如何保证各个loader按照预想方式工作？
可以使用 enforce 强制执行 loader 的作用顺序，pre 代表在所有正常 loader 之前执行，post 是所有 loader 之后执行。
---

# 你刚才也提到了代码分割，那代码分割的本质是什么？有什么意义呢？
  * 代码分割的本质其实就是在源代码直接上线和打包成唯一脚本main.bundle.js这两种极端方案之间的一种更适合实际场景的中间状态。用可接受的服务器性能压力增加来换取更好的用户体验。
  * 源代码直接上线：虽然过程可控，但是http请求多，性能开销大。
  * 打包成唯一脚本：一把梭完自己爽，服务器压力小，但是页面空白期长，用户体验不好。  
---

# 什么是JavaScript模块化开发，有哪些可遵循的规范
模块是指为了完成某种功能所需的程序或者子程序，模块是系统中职责单一且可替换的部分。模块化开发是指如何开发新的模块和复用已用的模块来实现应用的功能。JavaScript可遵循的主流规范有：commonJS，AMD和ES6 module。
---

# 怎么实现webpack的按需加载？什么是神奇注释?
在webpack中，import不仅仅是ES6 module的模块导入方式，还是一个类似require的函数，我们可以通过import('module')的方式引入一个模块，import()返回的是一个Promise对象；使用import（）方式就可以实现webpack的按需加载。在import（）里可以添加一些注释，如定义该chunk的名称，要过滤的文件，指定引入的文件等等，这类带有特殊功能的注释被称为神器注释。
---

# babel怎么做polyfill，polyfill的最佳实践是什么？
  * babel只负责进行语法转换，即将es6语法转换成es5语法，但如果是在es5中，有些对象，方法实际在浏览器中可能是不支持的，例如：promise，array.prototype.includes，这时候就需要@babel/polyfill来做模拟。
  * @babel/polyfill虽然可以解决模拟浏览器不存在对象方法的事情，但是有以下两个问题：
    - 直接修改内置的原型，造成全局污染；
    - 无法按需引入，webpack打包时，会把所有的polyfill都加载进来，导致产出文件过大。
  * 为了解决这个问题，babel社区又提出了@babel/runtime的方案，@babel/runtime不再修改原型，而是采用替换的方式，比如我们用promise，使用@babel/polyfill会产生一个window.promise对象，而@babel/runtime则会生成一个_Promise（示例而已）来替换掉我们代码中用到的Promise.另外@babel/runtime还支持按需引入。

# babel怎么针对不同的浏览器打包不同的适配代码
browserslist就是帮助我们来设置目标浏览器的工具。它实际上就是声明了一段浏览器的集合，我们的工具可以根据这段集合描述，针对性的输出兼容性代码。
---

# webpack钩子函数
---

# pageage.json 中版本号 ^ 和 ~ 有什么区别吗? 
---

#  webpack如何处理es module， 现代浏览器怎么写esmodule
---

# webpack怎么处理图片
---

# 打包时是怎么把不同router路径的js分别打包到多个trunk里的
---

# webpack中怎么设置压缩代码
---

# 我看到你的webpack配置用到webpack.optimize.UglifyJsPlugin这个插件，有没有觉得压缩速度很慢，有什么办法提升速度。
---

# webpack是怎么处理模块循环引用的情况的？
---

# 介绍下webpack的整个生命周期

---
