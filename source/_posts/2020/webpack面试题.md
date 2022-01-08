---
title: webpack面试题
date: 2020-12-09 11:29:43
categories: 面试题
tags: webpack
---

## webpack与grunt、gulp的不同

  三者都是前端构建工具，grunt和gulp在早期比较流行，现在webpack相对来说比较主流，不过一些轻量化的任务还是会用gulp来处理，比如单独打包CSS文件等。
  grunt和gulp是基于任务和流（Task、Stream）的。类似jQuery，找到一个（或一类）文件，对其做一系列链式操作，更新流上的数据， 整条链式操作构成了一个任务，多个任务就构成了整个web的构建流程。
  webpack是基于入口的。webpack会自动地递归解析入口所需要加载的所有资源文件，然后用不同的Loader来处理不同的文件，用Plugin来扩展webpack功能。
  
## 与webpack类似的工具还有哪些？谈谈你为什么最终选择（或放弃）使用webpack

  同样是基于入口的打包工具还有以下几个主流的：
    1. webpack
    2. rollup
    3. parcel

  从应用场景上来看：
    1. webpack适用于大型复杂的前端站点构建
    2. rollup适用于基础库的打包，如vue、react
    3. parcel适用于简单的实验性项目，他可以满足低门槛的快速看到效果

## 有哪些常见的Loader?你用过哪些Loader?

  1. raw-loader： 加载文件原始内容（utf-8）
  2. file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 (处理图片和字体)
  3. url-loader：与 file-loader 类似，区别是用户可以设置一个阈值，大于阈值会交给 file-loader 处理，小于阈值时返回文件 base64 形式编码 (处理图片和字体)
  4. source-map-loader：加载额外的 Source Map 文件，以方便断点调试
  5. svg-inline-loader：将压缩后的 SVG 内容注入代码中
  6. image-loader：加载并且压缩图片文件
  7. json-loader 加载 JSON 文件（默认包含）
  8. handlebars-loader: 将 Handlebars 模版编译成函数并返回
  9. babel-loader：把 ES6 转换成 ES5
  10. ts-loader: 将 TypeScript 转换成 JavaScript
  11. awesome-typescript-loader：将 TypeScript 转换成 JavaScript，性能优于 ts-loader
  12. sass-loader：将SCSS/SASS代码转换成CSS
  13. css-loader：加载 CSS，支持模块化、压缩、文件导入等特性
  14. style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS
  15. postcss-loader：扩展 CSS 语法，使用下一代 CSS，可以配合 autoprefixer 插件自动补齐 CSS3 前缀
  16. eslint-loader：通过 ESLint 检查 JavaScript 代码
  17. tslint-loader：通过 TSLint检查 TypeScript 代码
  18. mocha-loader：加载 Mocha 测试用例的代码
  19. coverjs-loader：计算测试的覆盖率
  20. vue-loader：加载 Vue.js 单文件组件
  21. i18n-loader: 国际化
  22. cache-loader: 可以在一些性能开销较大的 Loader 之前添加，目的是将结果缓存到磁盘里

## 有哪些常见的Plugin?你用过哪些Plugin?

  1. define-plugin：定义环境变量 (Webpack4 之后指定 mode 会自动配置)
  2. ignore-plugin：忽略部分文件
  3. html-webpack-plugin：简化 HTML 文件创建 (依赖于 html-loader)
  4. web-webpack-plugin：可方便地为单页应用输出 HTML，比 html-webpack-plugin 好用
  5. uglifyjs-webpack-plugin：不支持 ES6 压缩 (Webpack4 以前)
  6. terser-webpack-plugin: 支持压缩 ES6 (Webpack4)
  7. webpack-parallel-uglify-plugin: 多进程执行代码压缩，提升构建速度
  8. mini-css-extract-plugin: 分离样式文件，CSS 提取为独立文件，支持按需加载 (替代extract-text-webpack-plugin)
  9. serviceworker-webpack-plugin：为网页应用增加离线缓存功能
  10. clean-webpack-plugin: 目录清理
  11. ModuleConcatenationPlugin: 开启 Scope Hoisting
  12. speed-measure-webpack-plugin: 可以看到每个 Loader 和 Plugin 执行耗时 (整个打包耗时、每个 Plugin 和 Loader 耗时)
  13. webpack-bundle-analyzer: 可视化 Webpack 输出文件的体积 (业务组件、依赖第三方模块)

## 那你再说一说Loader和Plugin的区别?

  Loader 本质就是一个函数，在该函数中对接收到的内容进行转换，返回转换后的结果。 因为 Webpack 只认识 JavaScript，所以 Loader 就成了翻译官，对其他类型的资源进行转译的预处理工作。
  Plugin 就是插件，基于事件流框架 Tapable，插件可以扩展 Webpack 的功能，在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

  Loader 在 module.rules 中配置，作为模块的解析规则，类型为数组。每一项都是一个 Object，内部包含了 test(类型文件)、loader、options (参数)等属性。
  Plugin 在 plugins 中单独配置，类型为数组，每一项是一个 Plugin 的实例，参数都通过构造函数传入。

## Webpack构建流程简单说一下

  Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：
    1. 初始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数；
    2. 开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译；
    3. 确定入口：根据配置中的 entry 找出所有的入口文件；
    4. 编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理；
    5. 完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系；
    6. 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会；
    7. 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。

  在以上过程中，Webpack 会在特定的时间点广播出特定的事件，插件在监听到感兴趣的事件后会执行特定的逻辑，并且插件可以调用 Webpack 提供的 API 改变 Webpack 的运行结果。

  简单说：
    初始化：启动构建，读取与合并配置参数，加载 Plugin，实例化 Compiler
    编译：从 Entry 出发，针对每个 Module 串行调用对应的 Loader 去翻译文件的内容，再找到该 Module 依赖的 Module，递归地进行编译处理
    输出：将编译后的 Module 组合成 Chunk，将 Chunk 转换成文件，输出到文件系统中

## 使用webpack开发时，你用过哪些可以提高效率的插件?

  1. webpack-dashboard：可以更友好的展示相关打包信息。
  2. webpack-merge：提取公共配置，减少重复配置代码
  3. speed-measure-webpack-plugin：简称 SMP，分析出 Webpack 打包过程中 Loader 和 Plugin 的耗时，有助于找到构建过程中的性能瓶颈。
  4. size-plugin：监控资源体积变化，尽早发现问题
  5. HotModuleReplacementPlugin：模块热替换

## source map是什么?生产环境怎么用?

  source map 是将编译、打包、压缩后的代码映射回源代码的过程。打包压缩后的代码不具备良好的可读性，想要调试源码就需要 soucre map。
  map文件只要不打开开发者工具，浏览器是不会加载的。

  线上环境一般有三种处理方案：
    1. hidden-source-map：借助第三方错误监控平台 Sentry 使用
    2. nosources-source-map：只会显示具体行数以及查看源代码的错误栈。安全性比 sourcemap 高
    3. sourcemap：通过 nginx 设置将 .map 文件只对白名单开放(公司内网)

  注意：避免在生产中使用 inline- 和 eval-，因为它们会增加 bundle 体积大小，并降低整体性能。

## 模块打包原理知道吗?

  Webpack 实际上为每个模块创造了一个可以导出和导入的环境，本质上并没有修改 代码的执行逻辑，代码执行顺序与模块加载顺序也完全一致。

## 文件监听原理呢?

  在发现源码发生变化时，自动重新构建出新的输出文件。
  Webpack开启监听模式，有两种方式：
    1. 启动 webpack 命令时，带上 --watch 参数
    2. 在配置 webpack.config.js 中设置 watch:true

  缺点：每次需要手动刷新浏览器
  原理：轮询判断文件的最后编辑时间是否变化，如果某个文件发生了变化，并不会立刻告诉监听者，而是先缓存起来，等 aggregateTimeout 后再执行。
  
## 是否写过Loader？简单描述一下编写loader的思路

  Loader 支持链式调用，所以开发上需要严格遵循“单一职责”，每个 Loader 只负责自己需要负责的事情。

  1. Loader 运行在 Node.js 中，我们可以调用任意 Node.js 自带的 API 或者安装第三方模块进行调用
  2. Webpack 传给 Loader 的原内容都是 UTF-8 格式编码的字符串，当某些场景下 Loader 处理二进制文件时，需要通过 exports.raw = true 告诉 Webpack 该 Loader 是否需要二进制数据
  3. 尽可能的异步化 Loader，如果计算量很小，同步也可以
  4. Loader 是无状态的，我们不应该在 Loader 中保留状态
  5. 使用 loader-utils 和 schema-utils 为我们提供的实用工具
  6. 加载本地 Loader 方法
    6.1. Npm link
    6.2. ResolveLoader

## 是否写过Plugin？简单描述一下编写Plugin的思路？

  webpack在运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在特定的阶段钩入想要添加的自定义功能。Webpack 的 Tapable 事件流机制保证了插件的有序性，使得整个系统扩展性良好。

  1. compiler 暴露了和 Webpack 整个生命周期相关的钩子
  2. compilation 暴露了与模块和依赖有关的粒度更小的事件钩子
  3. 插件需要在其原型上绑定apply方法，才能访问 compiler 实例
  4. 传给每个插件的 compiler 和 compilation对象都是同一个引用，若在一个插件中修改了它们身上的属性，会影响后面的插件
  5. 找出合适的事件点去完成想要的功能:
    5.1 emit 事件发生时，可以读取到最终输出的资源、代码块、模块及其依赖，并进行修改(emit 事件是修改 Webpack 输出资源的最后时机)
    5.2 watch-run 当依赖的文件发生变化时会触发
  异步的事件需要在插件处理完任务时调用回调函数通知 Webpack 进入下一个流程，不然会卡住

## webpack的热更新是如何做到的？说明其原理

  Webpack 的热更新又称热替换（Hot Module Replacement），缩写为 HMR。 这个机制可以做到不用刷新浏览器而将新变更的模块替换掉旧的模块。

  HMR的核心就是客户端从服务端拉去更新后的文件，准确的说是 chunk diff (chunk 需要更新的部分)，实际上 WDS 与浏览器之间维护了一个 Websocket，当本地资源发生变化时，WDS 会向浏览器推送更新，并带上构建时的 hash，让客户端与上一次资源进行对比。客户端对比出差异后会向 WDS 发起 Ajax 请求来获取更改内容(文件列表、hash)，这样客户端就可以再借助这些信息继续向 WDS 发起 jsonp 请求获取该chunk的增量更新。

  后续的部分(拿到增量更新之后如何处理？哪些状态该保留？哪些又需要更新？)由 HotModulePlugin 来完成，提供了相关 API 以供开发者针对自身场景进行处理，像react-hot-loader 和 vue-loader 都是借助这些 API 实现 HMR。

## 如何利用webpack来优化前端性能？（提高性能和体验）

  用webpack优化前端性能是指优化webpack的输出结果，让打包的最终结果在浏览器运行快速高效。
  
    1. 压缩代码。删除多余的代码、注释、简化代码的写法等等方式。
    2. 利用CDN加速。在构建过程中，将引用的静态资源路径修改为CDN上对应的路径。可以利用webpack对于output参数和各loader的publicPath参数来修改资源路径
    3. 删除死代码（Tree Shaking）。将代码中永远不会走到的片段删除掉。可以通过在启动webpack时追加参数--optimize-minimize来实现
    4. 提取公共代码。

## 如何优化 Webpack 的构建速度？

  1. 使用高版本的 Webpack 和 Node.js
  2. 多进程/多实例构建：HappyPack(不维护了)、thread-loader
  3. 压缩代码：
    3.1 多进程并行压缩：
      3.1.1 webpack-paralle-uglify-plugin
      3.1.2 uglifyjs-webpack-plugin 开启 parallel 参数 (不支持ES6)
      3.1.3 terser-webpack-plugin 开启 parallel 参数
    3.2 通过 mini-css-extract-plugin 提取 Chunk 中的 CSS 代码到单独文件，通过 css-loader 的 minimize 选项开启 cssnano 压缩 CSS。
  4. 图片压缩：
    4.1 使用基于 Node 库的 imagemin (很多定制选项、可以处理多种图片格式)
    4.2 配置 image-webpack-loader
  5. 缩小打包作用域：
    5.1 exclude/include (确定 loader 规则范围)
    5.2 resolve.modules 指明第三方模块的绝对路径 (减少不必要的查找)
    5.3 resolve.mainFields 只采用 main 字段作为入口文件描述字段 (减少搜索步骤，需要考虑到所有运行时依赖的第三方模块的入口文件描述字段)
    5.4 resolve.extensions 尽可能减少后缀尝试的可能性
    5.5 noParse 对完全不需要解析的库进行忽略 (不去解析但仍会打包到 bundle 中，注意被忽略掉的文件里不应该包含 import、require、define 等模块化语句)
    5.6 IgnorePlugin (完全排除模块)
    5.7 合理使用alias
  6. 提取页面公共资源：
    6.1 基础包分离：
      6.1.1 使用 html-webpack-externals-plugin，将基础包通过 CDN 引入，不打入 bundle 中
      6.1.2 使用 SplitChunksPlugin 进行(公共脚本、基础包、页面公共文件)分离(Webpack4内置) ，替代了 CommonsChunkPlugin 插件
  7. DLL：
    7.1 使用 DllPlugin 进行分包，使用 DllReferencePlugin(索引链接) 对 manifest.json 引用，让一些基本不会改动的代码先打包成静态资源，避免反复编译浪费时间。
    7.2 HashedModuleIdsPlugin 可以解决模块数字id问题
  8. 充分利用缓存提升二次构建速度：
    8.1 babel-loader 开启缓存
    8.2 terser-webpack-plugin 开启缓存
    8.3 使用 cache-loader 或者 hard-source-webpack-plugin
  9. Tree shaking：
    9.1 打包过程中检测工程中没有引用过的模块并进行标记，在资源压缩时将它们从最终的bundle中去掉(只能对ES6 Modlue生效) 开发中尽可能使用ES6 Module的模块，提高tree shaking效率
    9.2 禁用 babel-loader 的模块依赖解析，否则 Webpack 接收到的就都是转换过的 CommonJS 形式的模块，无法进行 tree-shaking
    9.3 使用 PurifyCSS(不在维护) 或者 uncss 去除无用 CSS 代码:
      9.3.1 purgecss-webpack-plugin 和 mini-css-extract-plugin配合使用(建议)
  10. Scope hoisting
    10.1 构建后的代码会存在大量闭包，造成体积增大，运行代码时创建的函数作用域变多，内存开销变大。Scope hoisting 将所有模块的代码按照引用顺序放在一个函数作用域里，然后适当的重命名一些变量以防止变量名冲突
    10.2 必须是ES6的语法，因为有很多第三方库仍采用 CommonJS 语法，为了充分发挥 Scope hoisting 的作用，需要配置 mainFields 对第三方模块优先采用 jsnext:main 中指向的ES6模块化语法
  11. 动态Polyfill： 建议采用 polyfill-service 只给用户返回需要的polyfill，社区维护。 (部分国内奇葩浏览器UA可能无法识别，但可以降级返回所需全部polyfill)

## 如何对bundle体积进行监控和分析?

  VSCode 中有一个插件 Import Cost 可以帮助我们对引入模块的大小进行实时监测，还可以使用 webpack-bundle-analyzer 生成 bundle 的模块组成图，显示所占体积。bundlesize 工具包可以进行自动化资源体积监控。

## 文件指纹是什么？怎么用？

  文件指纹是打包后输出的文件名的后缀。
    1. Hash：和整个项目的构建相关，只要项目文件有修改，整个项目构建的 hash 值就会更改
    2. Chunkhash：和 Webpack 打包的 chunk 有关，不同的 entry 会生出不同的 chunkhash
    3. Contenthash：根据文件内容来定义 hash，文件内容不变，则 contenthash 不变

  JS的文件指纹设置: 设置 output 的 filename，用 chunkhash。
  CSS的文件指纹设置: 设置 MiniCssExtractPlugin 的 filename，使用 contenthash。
  图片的文件指纹设置: 设置file-loader的name，使用hash。

## 代码分割的本质是什么？有什么意义呢？

  代码分割的本质其实就是在源代码直接上线和打包成唯一脚本main.bundle.js这两种极端方案之间的一种更适合实际场景的中间状态。

  「用可接受的服务器性能压力增加来换取更好的用户体验。」

  源代码直接上线：虽然过程可控，但是http请求多，性能开销大。

  打包成唯一脚本：一把梭完自己爽，服务器压力小，但是页面空白期长，用户体验不好。
  
## 聊一聊Babel原理吧

  大多数JavaScript Parser遵循 estree 规范，Babel 最初基于 acorn 项目(轻量级现代 JavaScript 解析器) Babel大概分为三大部分：
    1. 解析：将代码转换成 AST
      1.1 词法分析：将代码(字符串)分割为token流，即语法单元成的数组
      1.2 语法分析：分析token流(上面生成的数组)并生成 AST
    2. 转换：访问 AST 的节点进行变换操作生产新的 AST
      2.1 Taro就是利用 babel 完成的小程序语法转换
    3. 生成：以新的 AST 为基础生成代码
