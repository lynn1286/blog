---
title: vue.config配置
date: 2020-09-01 11:48:54
categories: Vue
tags: vue.config TS
---

###  代码规范配置

使用vue created 命令搭建一个基本的架子出来 , 把vuex router等都勾选上.

接下来打开项目开始进行配置: 

我们先来配置一下这个代码规范的问题，在这里我们需要使用到的工具有:vetur，eslint，prettier，首先我们要在项目中安装一个包：@vue/prettier

```
    yarn add -D @vue/eslint-config-prettier
```

安装好之后，在 .eslintrc.js 给它加上：

```
    "extends": [
      "plugin:vue/essential",
      "eslint:recommended",
      "@vue/prettier"
    ],
```

现在我们执行 npm run lint 的时候，可以看到eslint已经帮我们启动了代码规范， 但是很多东西都不能按照我们的想法来执行的，这个时候我们还需要做一些配置：

在项目的根目录下创建一个.prettierrc.js文件，然后在其中加入：

```
    module.exports = {
        semi: false,  //行位是否使用分号，默认为true
        singleQuote: true, //  是否使用单引号
        // bracketSpacing: true, //对象大括号直接是否有空格，默认为true，效果：{ foo: bar }
        // "printWidth": 80, //一行的字符数，如果超过会进行换行，默认为80
        // "tabWidth": 2, //一个tab代表几个空格数，默认为80
        // "useTabs": false, //是否使用tab进行缩进，默认为false，表示用空格进行缩减
        // "trailingComma": "none", //是否使用尾逗号，有三个可选值"<none|es5|all>"
        // parser: "babylon" //代码的解析引擎，默认为babylon，与babel相同。
    }
```

到了这里，即时有上千个VUE文件，我们也可以通过npm run lint 处理我们的错误规范代码

配置到这里我们已经可以实现代码规范了， 但是为了能够在vscode编辑器看到我们的错误规范，我们还需要装一个插件，这个插件就叫Eslint。

安装好eslint后，因为eslint并不认识我们vue文件里面包含了js语法，所以我们还需要打开我们的vscode 配置文件，这里的配置只是配置我们个人的文件，并没有达到团队的配置效果，所以我们要在项目中创建一个.vscode文件夹，但是这里需要注意一个问题：.vscode 这个文件夹在.gitignore文件里面，所以千万要记得把.vscode删除

做好上面的一步之后，在.vscode里面创建文件settings.json文件，添加如下代码：

```
    {
        "eslint.autoFixOnSave": true,
        "eslint.validate": [
            "javascript",
            "javascriptreact",
            {
                "language": "vue",
                "autoFix": true
            }
        ],
    }
```

上面的配置就是在保存的时候校验并修改我们的代码，它会自动帮我们整理代码规范

配置到这里其实已经结束了，但是因为我们安装了很多插件，例如Prettier ， 因为我们不只开发vue项目，可能还有其它类型的js项目特别是传统js项目，需要用到prettier进行美化，而prettier的一些功能是会和eslint相冲突的，比如说我们在全局设置了prettier的formatOnSave，这个功能就会和eslint的autoFixOnSave打架，为了避免这个矛盾，我们通常还会在本项目的settings.json文件里再多加几个选项，类似于这样：

```
    "editor.tabSize": 2,
    "editor.formatOnSave": false,
    "prettier.semi": false,
    "prettier.singleQuote": true
```

有了这些设置，基本上prettier就不会和eslint打架了。

###  配置git hook 以及提交规范

项目级安装:

如果想要在全局安装, 可以看这篇[文章](https://juejin.im/post/5afc5242f265da0b7f44bee4)

```
    npm install -D commitizen cz-conventional-changelog
```

然后在 package.json 中配置:

```
    "script": {
    "commit": "git-cz",
    },
    "config": {
        "commitizen": {
            "path": "node_modules/cz-conventional-changelog"
        }
    }
```

如果全局安装过 commitizen, 那么在对应的项目中执行 git cz 或者 npm run commit 都可以。

注意： 如果没有全局安装 commitizen ， git cz 是无法执行的。

走完上述操作后其实已经可以实现我们提交的规范了, 但是 Angular 这一套规范我们可能不太习惯, 那么可以通过指定 Adapter cz-customizable 指定一套符合自己团队的规范.

项目级安装 cz-customizable

```
    npm i -D cz-customizable
```

然后修改 package.json 中的 config 为:

```
    "config": {
        "commitizen": {
            "path": "node_modules/cz-customizable"
        }
    }
```

然后在项目目录下创建 .cz-config.js 文件 , 注明: 配置出自 GitHub： [Leo Hui](https://gist.github.com/leohxj/7bc928f60bfa46a3856ddf7c0f91ab98)

```
     'use strict';

    module.exports = {

        types: [
            {
                value: 'WIP',
                name : '💪  WIP:      Work in progress'
            },
            {
                value: 'feat',
                name : '✨  feat:     A new feature'
            },
            {
                value: 'fix',
                name : '🐞  fix:      A bug fix'
            },
            {
                value: 'refactor',
                name : '🛠  refactor: A code change that neither fixes a bug nor adds a feature'
            },
            {
                value: 'docs',
                name : '📚  docs:     Documentation only changes'
            },
            {
                value: 'test',
                name : '🏁  test:     Add missing tests or correcting existing tests'
            },
            {
                value: 'chore',
                name : '🗯  chore:    Changes that don\'t modify src or test files. Such as updating build tasks, package manager'
            },
            {
                value: 'style',
                name : '💅  style:    Code Style, Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)'
            },
            {
                value: 'revert',
                name : '⏪  revert:   Revert to a commit'
            }
        ],

        scopes: [],

        allowCustomScopes: true,
        allowBreakingChanges: ["feat", "fix"]
    };
```

接下来我们再配置 校验 message 是否合法 , 如果不合法则拒绝提交.

项目级安装：

```
    npm i -D @commitlint/config-conventional @commitlint/cli
```

同时需要在项目目录下创建配置文件 .commitlintrc.js, 写入[示例配置](https://github.com/conventional-changelog/commitlint/blob/master/@commitlint/config-conventional/index.js):

```
    module.exports = {
        extends: [
            '@commitlint/config-conventional'
        ],
        rules: {
            
        }
    };
```

如果您是自定义的 Adapter 那么您可能需要针对自定义的 Adapter 进行 Lint：

上述配置是使用了 符合 Angular团队规范 的Adapter , 而要根据我们自定义的 commitizen adapter , 则需要安装:

```
    npm i -D commitlint-config-cz @commitlint/cli
```

.commitlintrc.js 中写入:

```
    module.exports = {
        extends: [
            'cz'
        ],
        rules: {}
    };
```

结合 Husky: 

校验 commit message 的最佳方式是结合 git hook, 所以需要配合 [Husky](https://github.com/typicode/husky).

这个插件是可以让我们在git commit 之前 都执行一次 hook 脚本

```
    npm install husky --save-dev
```

package.json 中添加:

```
    "husky": {
        "hooks": {
            "pre-commit": "npm run lint",
            "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
        }
    },
```

或者 创建 .huskyrc.js 添加:

```
    module.exports = {
        hooks: {
            'pre-commit': 'npm run lint',
            'commit-msg': 'commitlint -E HUSKY_GIT_PARAMS'
        }
    };
```

配置后在后续的每一次git commit 之前，都会执行一次对应的 hook 脚本npm run lint 。其他hook同理

standard-version: 自动生成 CHANGELOG

通过以上工具的帮助, 我们的工程 commit message 应该是符合 Angular团队那套, 这样也便于我们借助 [standard-version](https://github.com/conventional-changelog/standard-version) 这样的工具, 自动生成 CHANGELOG, 甚至是 语义化的版本号([Semantic Version](https://semver.org/lang/zh-CN/)).

安装使用:

```
    npm i -S standard-version
```

package.json 配置:

```
    "scirpt": {
        "release": "standard-version"
    }
```

### vue国际化配置：

我们使用vue-i18n来实现国际化。

首先当然是安装啦：

```
    npm install vue-i18n -S
```

安装完毕后，写配置文件，我们先来完成语言包吧：

```
    //新建中文语言包：zh.js
    export default {
        input:{
            placeholder:'请输入用户名',
            password:'请输入密码'
        }
    }
```

然后再新建英文语言包：

```
    // en.js
    export default {
        input:{
            placeholder:'Please enter a user name',
            password:'Please enter your password'
        }
    }
```

这是demo ，所以就弄这两种语言试试水。

接下来新建一个i18n.js配置文件。

```
    // i18n.js
    import Vue from 'vue'
    import VueI18n from 'vue-i18n'
    import elementEnLocale from 'element-ui/lib/locale/lang/en' // element-ui lang
    import elementZhLocale from 'element-ui/lib/locale/lang/zh-CN'// element-ui lang
    import enLocale from './lang/en'
    import zhLocale from './lang/zh'

    Vue.use(VueI18n)

    const messages = {
    en: {
        ...enLocale,
        ...elementEnLocale
    },
    zh: {
        ...zhLocale,
        ...elementZhLocale
    }
    }

    const i18n = new VueI18n({
    locale: localStorage.getItem('locale') || 'zh', // set locale
    messages // set locale messages
    })

    export default i18n
```

然后我们再来配置mian.js.

```
    import Vue from 'vue';
    import App from './App.vue';
    import router from './router';
    import store from './store';
    import './registerServiceWorker';
    // 引入element ui框架
    import ElementUI from 'element-ui';
    // 引入element ui 样式文件
    import 'element-ui/lib/theme-chalk/index.css';

    // 引入阿里icon
    import '../public/img/icons/iconfont.css';
    // 如果是使用svg就要引入iconfont.js这个文件
    import '../public/img/icons/iconfont';
    // 国际化
    import i18n from './common/i18n.js'  



    //全局注册组件
    import './components/index';

    //全局使用插件
    Vue.use(ElementUI, {
    size: 'medium', // set element-ui default size
    i18n: (key, value) => i18n.t(key, value)
    })

    Vue.config.productionTip = false;

    new Vue({
    router,
    store,
    i18n, // 国际化
    render: h => h(App),
    }).$mount('#app');
```

入口文件这里其他的我就不解释了，主要讲国际化，把我们的国际化配置文件引进来，引进来之后，注入到vue的实例里面去，这里有一个很重要的点，为了让element的内部组件语言也改变，所以下面这段代码不能少：

```
    //全局使用插件
    Vue.use(ElementUI, {
    size: 'medium', // set element-ui default size
    i18n: (key, value) => i18n.t(key, value)
    })
```

具体原因别问我，我不懂，因为我也踩了好久的坑，才百度到这个。

接下来就是使用了。

```
    <template>
  <div class="login">
    <el-input :placeholder="$t('input.placeholder')"
              prefix-icon="icon-tubiao-15"
              v-model="input21" />
    <el-input :placeholder="$t('input.password')"
              prefix-icon="icon-tubiao-15"
              v-model="input" />
    <div class="lang">
      <el-dropdown @command="handleCommand"
                   size="small">
        <span class="el-dropdown-link">
          {{locale}}<i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item command="zh-CN"
                            :disabled='locale === "中文" '>中文</el-dropdown-item>
          <el-dropdown-item command="en-US"
                            :disabled='locale === "English" '>English</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
     <el-date-picker
      v-model="value1"
      type="datetime"
      :placeholder="$t('input.placeholder')">
    </el-date-picker>
  </div>
</template>

<script>
export default {
  name: 'login',
  data () {
    return {
      input21: '',
      input: '',
      value1:''
    }
  },
  computed: {
    locale () {
      if (this.$i18n.locale === 'zh') {
        return '中文'
      } else {
        return 'English'
      }
    }
  },
  methods: {
    handleCommand (command) {
      if (command == 'zh-CN') {
        localStorage.setItem('locale', 'zh')
        this.$i18n.locale = localStorage.getItem('locale')
        console.log(this.$i18n.locale);
        this.$message({
          message: '切换为中文！',
          type: 'success'
        })
      } else if (command == 'en-US') {
        localStorage.setItem('locale', 'en')
        this.$i18n.locale = localStorage.getItem('locale')
        console.log(this.$i18n.locale);
        this.$message({
          message: 'Switch to English!',
          type: 'success'
        })
      }
    }
  }
}
</script>

<style>
</style>
```

使用的话就是：

```
    <el-input :placeholder="$t('input.placeholder')"
              prefix-icon="icon-tubiao-15"
              v-model="input21" />  // 属性里面这样用
    <span>{{$t('input.placeholder')}}</span> // 标签内这样用。
```

这里面的值是我们写的语言包对象，key是input，value是placeholder，

至于我们如何来根据事件更换语言包呢？

直接改变它的值：this.$i18n.locale 就可以实现了，具体就是当你触发点击事件的时候，把这个值改变成你相应的语言包名字就可以了，注意，每次更换一点要把本地存储的值给覆盖哦。

```
    handleCommand (command) {
      if (command == 'zh-CN') {
        localStorage.setItem('locale', 'zh')
        this.$i18n.locale = localStorage.getItem('locale')
        // console.log(this.$i18n.locale);
        this.$message({
          message: '切换为中文！',
          type: 'success'
        })
      } else if (command == 'en-US') {
        localStorage.setItem('locale', 'en')
        this.$i18n.locale = localStorage.getItem('locale')
        // console.log(this.$i18n.locale);
        this.$message({
          message: 'Switch to English!',
          type: 'success'
        })
      }
    }
```

这样国际化就实现啦~~~

这里再提一个bug，但是我想不清楚应该怎么解决，只能临时找到个比较bug的方式

假如，我们有一个数组，也是需要配置国际化的，例如下拉框，看代码如下：

```
    <el-select v-model="value"
               :placeholder="$t('input.placeholder')">
        <el-option v-for="item in options"
                    :key="item.id"
                    :label="item.label"
                    :value="item.id">
        </el-option>
    </el-select>
    
    data () {
        return {
        options: this.$t('input.options')
        }
    },
```

我们都知道，vue更改数组是要使用this.$set()方法才能更新视图的，但是，这里是直接从语言包里面拿到一个数据，当你切换语言包的时候，视图并不会更新，需要手动刷新一次才能够显示，我这里暂时的解决方案是直接在切换语言的时候在那个事件上调用:

```
    this.$router.go(0)
```

但是用户体验真心不好，哪位大哥知道更好的方法，求加微信：294999978 指教，本人小白一枚，真心感谢

```
    handleCommand (command) {
      if (command == 'zh-CN') {
        localStorage.setItem('locale', 'zh')
        this.$i18n.locale = localStorage.getItem('locale')
        console.log(this.$i18n.locale);
        this.$router.go(0)// 刷新页面，为的就是触发视图
        this.$message({
          message: '切换为中文！',
          type: 'success'
        })
      } else if (command == 'en-US') {
        localStorage.setItem('locale', 'en')
        this.$i18n.locale = localStorage.getItem('locale')
        console.log(this.$i18n.locale);
        this.$router.go(0)  // 刷新页面，为的就是触发视图
        this.$message({
          message: 'Switch to English!',
          type: 'success'
        })
      }
    }
```


### 最后奉上项目的整体配置 , 以及[源码的地址](https://github.com/ChaliceLee92/vue_admin)

```
const path = require('path')
let LodashModuleReplacementPlugin = require('lodash-webpack-plugin')
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer')
    .BundleAnalyzerPlugin

// 配置别名
function resolve(dir) {
    return path.join(__dirname, dir)
}

// gzip压缩
const CompressionWebpackPlugin = require('compression-webpack-plugin')

// 代码压缩
const UglifyJsPlugin = require('uglifyjs-webpack-plugin')

// 是否为生产环境
const isProduction = process.env.NODE_ENV !== 'development'

// 本地环境是否需要使用cdn
const devNeedCdn = false

// cdn链接
const cdn = {
    // cdn：模块名称和模块作用域命名（对应window里面挂载的变量名称）
    externals: {
        vue: 'Vue',
        vuex: 'Vuex',
        'vue-router': 'VueRouter',
        'element-ui': 'ELEMENT',
        axios: 'axios'
    },
    // cdn的css链接
    css: ['https://unpkg.com/element-ui/lib/theme-chalk/index.css'],
    // cdn的js链接
    js: [
        'https://cdn.bootcss.com/vue/2.6.10/vue.min.js',
        'https://cdn.bootcss.com/vuex/3.1.1/vuex.min.js',
        'https://cdn.bootcss.com/vue-router/3.1.3/vue-router.min.js',
        'https://unpkg.com/element-ui/lib/index.js',
        'https://cdn.bootcss.com/axios/0.19.0-beta.1/axios.min.js'
    ]
}

// 获取baseurl
function getProxy(path, type) {
    if (path === '/api') {
        switch (type) {
            case 'localhost':
                return 'http://106.52.38.245:9000/sys'
            case 'service':
                return 'http://106.52.38.245:9000/sys'
        }
    }
}

module.exports = {
    productionSourceMap: false,

    chainWebpack: config => {
        config.resolve.alias
            .set('@', resolve('src'))
            .set('@assets', resolve('src/assets'))
            .set('@components', resolve('src/components'))
            .set('@router', resolve('src/router'))
            .set('@views', resolve('src/views'))
            .set('@store', resolve('src/store'))
            .set('@utils', resolve('src/utils'))
            .set('@interface', resolve('src/http/interface'))

        // ============压缩图片 start============
        config.module
            .rule('images')
            .use('image-webpack-loader')
            .loader('image-webpack-loader')
            .options(
                {
                    mozjpeg: { progressive: true, quality: 65 },
                    optipng: { enabled: false },
                    pngquant: { quality: [0.65, 0.9], speed: 4 },
                    gifsicle: { interlaced: false }
                    // webp: { quality: 75 }
                },
                { bypassOnDebug: true }
            )
            .end()
        // ============压缩图片 end============

        // ============注入cdn start============
        config.plugin('html').tap(args => {
            // 生产环境或本地需要cdn时，才注入cdn
            if (isProduction || devNeedCdn) args[0].cdn = cdn
            // 修复 Lazy loading routes Error
            args[0].chunksSortMode = 'none'
            return args
        })
        // ============注入cdn end============

        // 修复HMR
        config.resolve.symlinks(true)

        // 移除 prefetch 插件不会加载其他路由文件，只加载当前路由文件
        config.plugins.delete('prefetch')
        // preload插件作用，暂时未知
        config.plugins.delete('preload')

        // 配置TypeScript
        config.resolve.extensions
            .add('.ts')
            .add('.tsx')
            .end()
            .end()
            .module.rule('typescript')
            .test(/\.tsx?$/)
            .use('babel-loader')
            .loader('babel-loader')
            .end()
            .use('ts-loader')
            .loader('ts-loader')
            .options({
                transpileOnly: true,
                appendTsSuffixTo: ['\\.vue$'],
                happyPackMode: false
            })
            .end()
    },
    // 如果你不需要使用eslint，把lintOnSave设为false即可
    lintOnSave: true,
    css: {
        sourceMap: false, // 开启 CSS source maps
        extract: true, // 是否使用css分离插件 ExtractTextPlugin
        requireModuleExtension: true, // false 会导致样式失效
        loaderOptions: {
            sass: {
                prependData: `@import "@/assets/styles/varibles.scss";`
            }
        }
    },
    devServer: {
        // historyApiFallback: true  // history 异步路由没有缓存在页面中，第一次进入页面会找不到 , 这个解决
        // open: true,
        // host: 'localhost',
        // port: 8081,
        // https: false,
        // overlay: {
        //     warnings: true,
        //     errors: true
        // },
        // proxy: {
        // 这里配置了 /sys 相当于就是 服务器的地址 . 例如: sys/login = http:localhost:8081/api/login
        // '/sys': {
        //     target: getProxy('/api', process.env.VUE_APP_TYPE),
        //     ws: true,
        //     changOrigin: true, //允许跨域
        //     pathRewrite: {
        //         '^/sys': ''
        //     }
        // }
        // }
    },
    configureWebpack: config => {
        // 生产环境相关配置
        if (isProduction) {
            // 代码压缩
            config.plugins.push(
                new UglifyJsPlugin({
                    uglifyOptions: {
                        warnings: false, // 若打包错误，则注释这行
                        //生产环境自动删除console
                        compress: {
                            drop_debugger: true,
                            drop_console: true,
                            pure_funcs: ['console.log']
                        }
                    },
                    sourceMap: false,
                    parallel: true
                })
            )

            // gzip压缩
            const productionGzipExtensions = ['html', 'js', 'css']
            config.plugins.push(
                new CompressionWebpackPlugin({
                    filename: '[path].gz[query]',
                    algorithm: 'gzip',
                    test: new RegExp(
                        '\\.(' + productionGzipExtensions.join('|') + ')$'
                    ),
                    threshold: 10240, // 只有大小大于该值的资源会被处理 10240
                    minRatio: 0.8, // 只有压缩率小于这个值的资源才会被处理
                    deleteOriginalAssets: false // 删除原文件
                })
            )

            // 公共代码抽离  构建优化上我们使用了 happypack 来利用多核CPU 加快打包的速度。
            config.optimization = {
                // https://webpack.js.org/plugins/split-chunks-plugin/
                splitChunks: {
                    // minSize: 1000000, // 单个文件的最小size
                    // maxSize: 2000000, // 单个文件最大的size
                    // minChunks: 2, // 最小被引用
                    // maxAsyncRequests: 5, // 首页加载资源
                    // maxInitialRequests: 3,
                    // automaticNameDelimiter: '~', // 打包文件自定义的链接符
                    // name: true,
                    // chunks: 'async', // initial(初始块)、async(按需加载块)、all(默认，全部块)
                    // 这里需要注意的是如果使用initial 会将首页需要的依赖和项目本身的依赖打包2次增大文件体积
                    cacheGroups: {
                        default: false,
                        vendors: {
                            name: 'chunk-vendors',
                            test: /[\\/]node_modules[\\/]/,
                            chunks: 'initial',
                            priority: 2,
                            reuseExistingChunk: true,
                            enforce: true
                        },
                        common: {
                            name: 'chunk-common',
                            chunks: 'initial',
                            minChunks: 2,
                            maxInitialRequests: 5,
                            minSize: 0,
                            priority: 1,
                            reuseExistingChunk: true,
                            enforce: true
                        },
                        elementUI: {
                            name: 'chunk-elementui',
                            test: /[\\/]node_modules[\\/]element-ui[\\/]/,
                            chunks: 'all',
                            priority: 3,
                            reuseExistingChunk: true,
                            enforce: true
                        },
                        echarts: {
                            name: 'chunk-echarts',
                            test: /[\\/]node_modules[\\/](vue-)?echarts[\\/]/,
                            chunks: 'all',
                            priority: 4,
                            reuseExistingChunk: true,
                            enforce: true
                        },
                        vue: {
                            test(module) {
                                let path = module.resource
                                if (!path) return false
                                path = path.replace(/\\/g, '/')
                                // return path && path.indexOf('node_modules') > -1 && path.indexOf('vuetify') > -1
                                return path && /node_modules\/vue/.test(path)
                            },
                            name: 'chunk-vuetify',
                            priority: 9,
                            enforce: true
                        },
                        styles: {
                            name: 'styles',
                            test: /\.(sa|sc|c)ss$/,
                            chunks: 'all',
                            enforce: true
                        },
                        runtimeChunk: {
                            name: 'manifest'
                        }
                    }
                }
            }

            // 可视化包大小
            config.plugins.push(
                new BundleAnalyzerPlugin({
                    analyzerMode: 'server',
                    analyzerHost: '127.0.0.1',
                    analyzerPort: 8889,
                    reportFilename: 'report.html',
                    defaultSizes: 'parsed',
                    openAnalyzer: false,
                    generateStatsFile: false,
                    statsFilename: 'stats.json',
                    statsOptions: null,
                    logLevel: 'info'
                })
            )
        }
        config.plugins.push(new LodashModuleReplacementPlugin())

        // 用cdn方式引入，则构建时要忽略相关资源
        if (isProduction || devNeedCdn) config.externals = cdn.externals

        // 取消webpack警告的性能提示
        config.performance = {
            hints: 'warning',
            //入口起点的最大体积
            maxEntrypointSize: 50000000,
            //生成文件的最大体积
            maxAssetSize: 30000000,
            //只给出 js 文件的性能提示
            assetFilter: function(assetFilename) {
                return assetFilename.endsWith('.js')
            }
        }

        // 配置TS
        // config.resolve = { extensions: ['.ts', '.tsx', '.js', '.json'] }
        // config.module = {
        //     rules: [
        //         {
        //             test: /.tsx?$/,
        //             loader: 'ts-loader',
        //             exclude: /node_modules/,
        //             options: {
        //                 appendTsSuffixTo: [/.vue$/]
        //             }
        //         }
        //     ]
        // }
    },
    // 第三方插件配置
    pluginOptions: {}
}


```
