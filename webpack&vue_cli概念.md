## webpack知识点

vue项目要打包的东西

​	1.js打包   es6转es5  ts代码转化

2. css处理  sass,less等预处理器的处理
3. 资源文件img,font  图片Img文件的加载  字体font文件加载
4. html资源处理  打包html资源文件
5. 处理vue项目的sfc文件  .vue文件

### 全局安装

通过npm安装 (安装node后自带npm)

npm install webpack webpack-cli -g  (安装2个: webpack和webpack-cli)

命令行 全局打包命令: webpack

### 局部安装

不同的项目依赖的webpack版本不同，推荐每个项目使用局部安装webpack

npm install webpack webpack-cli -D (--save-dev)  生成node_modules文件

**命令行局部打包命令: **

**方式1： ./node_modules/.bin/webpack**

**方式2： npx webpack**

**方式3： package.json文件中---> "script"添加脚本: "build": "webpack"**

修改打包文件入口和出口 (webpack默认入口是src/index.js)

npx webpack --entry ./src/main.js --output-path ./dist

### npm init 

创建package.json文件 (包依赖项文件)

### import方法懒加载  

其实就是在build时对文件进行分包，懒加载的文件会打包成chunk.xxx.js

```js
// import是webpack的方法，返回一个promise对象 可使用语法:import('./xxx').then()
const routes = [
    										// magic comment魔法注释 给分包自定义名
    {path: '/home', component: () => import(/* webpackChunkName: "home-chunk "*/'../views/home.vue')}
]
```

### 配置文件webpack.config.js

```js
const path = require('path')
module.exports = {
    entry: './src/main.js',
    // output必须是绝对路径
    output: {
        // path.resolve对路径进行拼接 __dirname是当前文件路径
        path: path.resolve(__dirname, './dist'),
        filename: 'bundle.js' //打包后文件名
    },
    // 模块配置
    module: {
        // 模块规则, 是一个数组
        rules: [
            {
                test: /\.css$/, // 用正则表达式匹配
                use: [
                   // {loader: 'css-loader'} // 完整写法
                    "style-loader", // 注意：loader从下往上执行,先写style-loader
                    "css-loader",
                    {   // 添加Postcss需要用完整写法，配置option属性
                        loader: "postcss-loader",
                        options: {
                            postcssOptions: {
                                plugins: [
                                    require('autoprefixer')
                                ]
                            }
                        }
                    }
                ]
            },
            {
                test: /\.less$/,
                use:[// less-loader只是转化，依然要使用style,css-loader进行加载
                    "style-loader",
                    "css-loader",
                    "less-loader"
                ]
            }
        ]
    }
}
```

### css-loader

命令行安装: npm install css-loader -D

### style-loader

命令行安装: npm install style-loader -D

### less-loader

npm install less less-loader -D

### postcss 

postcss其实是一个平台，利用插件实现很多css转化和适配

npm install postcss postcss-cli -D

安装postcss的插件 autoprefixer

npm install autoprefixer -D   自动添加浏览器前缀插件

 //--use 指定使用的插件 -o输出demo.css 输入test.css

npx postcss --use autoprefixer -o demo.css test.css   

```css
.title{
    /*自动添加前缀插件*/
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}
```

### postcss-preset-env

包括了autoprefixer功能的插件，一般用这个插件

npm install postcss-preset-env -D

```js
{   // 添加Postcss需要用完整写法，配置option属性
    loader: "postcss-loader",
    options: {
        postcssOptions: {
            plugins: [
                require('postcss-preset-env')
            ]
        }
    }
}
```

### file-loader 

```js
{
    test: /\.(jpg|png|gif|svg)$/,
    use:[
        "file-loader"
    ]
}
```

### 使用相对路径导入图片不显示

```js
const el = document.createElement('img')
// 这种导入方式打包后，图片是无法显示的，因为这个src会被认为是字符串
el.src = '../assets/xxx.png'  

// 解决方案: 使用模块化方式导入，当成模块资源使用， import或者require 打包时才能正确解析资源路径
el.src = require('../assets/xxx.png')
// 或者
import xxpng from '../assets/xxx.png' 
el.src = xxpng
```







## 　各文件作用

1、build/dev-server.js  文件 项目node的启动文件，这里面做了webpack配置和node操作，

2、build/webpack.base.conf.js   webpack基本配置文件

3、build/webpack.dev.conf.js  开发环境webpack配置

4、build/webpack.prod.conf.js  正式环境的webpack配置

5、build/build.js   执行打包的配置文件

6、config/index.js    开发、线上环境的文件输出、打包等一些配置



## **1、build/dev-server.js**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```js
// 检查 Node 和 npm 版本
require('./check-versions')()

// 获取 config/index.js 的默认配置
var config = require('../config')

// 如果 Node 的环境无法判断当前是 dev / product 环境
// 使用 config.dev.env.NODE_ENV 作为当前的环境
if (!process.env.NODE_ENV) {
  process.env.NODE_ENV = JSON.parse(config.dev.env.NODE_ENV)
}

// 一个可以强制打开浏览器并跳转到指定 url 的插件
var opn = require('opn')
// 使用 NodeJS 自带的文件路径工具
var path = require('path')
// 使用 express
var express = require('express')
// 使用 webpack
var webpack = require('webpack')
// 使用 proxyTable 热更新
var proxyMiddleware = require('http-proxy-middleware')
// 使用 dev 环境的 webpack 配置
var webpackConfig = require('./webpack.dev.conf')
// 使用axios请求
var axios = require('axios')

// 如果没有指定运行端口，使用 config.dev.port 作为运行端口
var port = process.env.PORT || config.dev.port


var autoOpenBrowser = !!config.dev.autoOpenBrowser

// 使用 config.dev.proxyTable 的配置作为 proxyTable 的代理配置
var proxyTable = config.dev.proxyTable
// 使用 express 启动一个服务，URL改一下，这里是做服务转发，还可以对
var app = express()

var apiRoutes = express.Router()

apiRoutes.get('/getDiscList', function (req, res) {
  var url = 'https://c.y.qq.com/splcloud/fcgi-bin/fcg_get_diss_by_tag.fcg'
  axios.get(url, {
    headers: {
      referer: 'https://c.y.qq.com/',
      host: 'c.y.qq.com'
    },
    params: req.query
  }).then((response) => {
    res.json(response.data)
  }).catch((e) => {
    console.log(e)
  })
})

apiRoutes.get('/lyric', function (req, res) {
  var url = 'https://c.y.qq.com/lyric/fcgi-bin/fcg_query_lyric_new.fcg'

  axios.get(url, {
    headers: {
      referer: 'https://c.y.qq.com/',
      host: 'c.y.qq.com'
    },
    params: req.query
  }).then((response) => {
    var ret = response.data
    if (typeof ret === 'string') {
      var reg = /^\w+\(({[^()]+})\)$/
      var matches = ret.match(reg)
      if (matches) {
        ret = JSON.parse(matches[1])
      }
    }
    res.json(ret)
  }).catch((e) => {
    console.log(e)
  })
})

app.use('/api', apiRoutes)

var compiler = webpack(webpackConfig)
// 启动 webpack-dev-middleware，将 编译后的文件暂存到内存中
var devMiddleware = require('webpack-dev-middleware')(compiler, {
  publicPath: webpackConfig.output.publicPath,
  quiet: true
})
// 启动 webpack-hot-middleware，也就是我们常说的 Hot-reload
var hotMiddleware = require('webpack-hot-middleware')(compiler, {
  log: () => {}
})
// force page reload when html-webpack-plugin template changes
compiler.plugin('compilation', function (compilation) {
  compilation.plugin('html-webpack-plugin-after-emit', function (data, cb) {
    hotMiddleware.publish({ action: 'reload' })
    cb()
  })
})

// 将 proxyTable 中的请求配置挂在到启动的 express 服务上
Object.keys(proxyTable).forEach(function (context) {
  var options = proxyTable[context]
  if (typeof options === 'string') {
    options = { target: options }
  }
  app.use(proxyMiddleware(options.filter || context, options))
})

// 使用 connect-history-api-fallback 匹配资源，如果不匹配就可以重定向到指定地址
app.use(require('connect-history-api-fallback')())

// 将暂存到内存中的 webpack 编译后的文件挂在到 express 服务上
app.use(devMiddleware)

// 将 Hot-reload 挂在到 express 服务上
app.use(hotMiddleware)

// 拼接 static 文件夹的静态资源路径
var staticPath = path.posix.join(config.dev.assetsPublicPath, config.dev.assetsSubDirectory)
// 为静态资源提供响应服务
app.use(staticPath, express.static('./static'))

var uri = 'http://localhost:' + port

var _resolve
var readyPromise = new Promise(resolve => {
  _resolve = resolve
})

console.log('> Starting dev server...')
devMiddleware.waitUntilValid(() => {
  console.log('> Listening at ' + uri + '\n')
  // 如果不是测试环境，自动打开浏览器并跳到我们的开发地址
  if (autoOpenBrowser && process.env.NODE_ENV !== 'testing') {
    opn(uri)
  }
  _resolve()
})
// 让我们这个 express 服务监听 port 的请求，并且将此服务作为 dev-server.js 的接口暴露
var server = app.listen(port)

module.exports = {
  ready: readyPromise,
  close: () => {
    server.close()
  }
}
```

## **2、build/webpack.base.conf.js**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```js
var path = require('path')
var utils = require('./utils')
var config = require('../config')
// .vue 文件配置 loader 及工具 (autoprefixer)
var vueLoaderConfig = require('./vue-loader.conf')
// 拼接我们的工作区路径为一个绝对路径的函数
function resolve(dir) {
  return path.join(__dirname, '..', dir)
}

// webpack 配置，输入、输出、模块、插件
module.exports = {
  entry: {
    app: './src/main.js'
  },
  output: {
      // 编译输出的根路径
    path: config.build.assetsRoot,
    // 编译输出的文件名
    filename: '[name].js',
    // 正式发布环境下编译输出的发布路径
    publicPath: process.env.NODE_ENV === 'production'
      ? config.build.assetsPublicPath
      : config.dev.assetsPublicPath
  },
  resolve: {
      // 自动补全的扩展名
    extensions: ['.js', '.vue', '.json'],
    // 默认路径代理，即起别名，例如 import Vue from 'vue'，会自动到 'vue/dist/vue.common.js'中寻找
    alias: {
      '@': resolve('src'),
      'common': resolve('src/common'),
      'components': resolve('src/components'),
      'base': resolve('src/base'),
      'api': resolve('src/api')
    }
  },
  module: {
    rules: [
          // 需要处理的文件及使用的 loader
      {
        test: /\.(js|vue)$/,
        loader: 'eslint-loader',
        enforce: 'pre',
        include: [resolve('src'), resolve('test')],
        options: {
              // eslint 代码检查配置工具
          formatter: require('eslint-friendly-formatter')
        }
      },
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: vueLoaderConfig
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        include: [resolve('src'), resolve('test')]
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('img/[name].[hash:7].[ext]')
        }
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
        }
      }
    ]
  }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

## **3、build/webpack.dev.conf.js**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```js
// 使用一些小工具
var utils = require('./utils')
var webpack = require('webpack')
// 同样的使用了 config/index.js 的预设配置
var config = require('../config')
// 使用 webpack 配置合并插件
var merge = require('webpack-merge')
// 加载 webpack.base.conf
var baseWebpackConfig = require('./webpack.base.conf')
// 使用 html-webpack-plugin 插件，这个插件可以帮我们自动生成 html 并且注入到 .html 文件中
var HtmlWebpackPlugin = require('html-webpack-plugin')
// 一个友好的错误提示插件
var FriendlyErrorsPlugin = require('friendly-errors-webpack-plugin')

// 将 Hol-reload 相对路径添加到 webpack.base.conf 的 对应 entry 前
Object.keys(baseWebpackConfig.entry).forEach(function (name) {
  baseWebpackConfig.entry[name] = ['./build/dev-client'].concat(baseWebpackConfig.entry[name])
})
// 将我们 webpack.dev.conf.js 的配置和 webpack.base.conf.js 的配置合并
module.exports = merge(baseWebpackConfig, {
  module: {
      // 使用 styleLoaders
    rules: utils.styleLoaders({ sourceMap: config.dev.cssSourceMap })
  },
  // 使用 #eval-source-map 模式作为开发工具，此配置可参考 DDFE 往期文章详细了解
  devtool: '#cheap-module-eval-source-map',
  plugins: [
    // definePlugin 接收字符串插入到代码当中, 所以你需要的话可以写上 JS 的字符串
    new webpack.DefinePlugin({
      'process.env': config.dev.env
    }),
    // definePlugin 接收字符串插入到代码当中, 所以你需要的话可以写上 JS 的字符串
    // HotModule 插件在页面进行变更的时候只会重回对应的页面模块，不会重绘整个 html 文件
    new webpack.HotModuleReplacementPlugin(),
    // 遇到报错，跳过还行，并提示错误信息
    new webpack.NoEmitOnErrorsPlugin(),
    // 将 index.html 作为入口，注入 html 代码后生成 index.html文件
    new HtmlWebpackPlugin({
      filename: 'index.html',
      template: 'index.html',
      inject: true
    }),
    // 使用这个插件，更好的输出错误信息
    new FriendlyErrorsPlugin()
  ]
})
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

## **4、build/webpack.prod.conf.js** 

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```js
var path = require('path')
var utils = require('./utils')
var webpack = require('webpack')
var config = require('../config')
// 加载 webpack 配置合并工具
var merge = require('webpack-merge')
// 加载 webpack.base.conf.js文件
var baseWebpackConfig = require('./webpack.base.conf')
var CopyWebpackPlugin = require('copy-webpack-plugin')
// 一个可以插入 html 并且创建新的 .html 文件的插件
var HtmlWebpackPlugin = require('html-webpack-plugin')
// 一个 webpack 扩展，可以提取一些代码并且将它们和文件分离开
// 如果我们想将 webpack 打包成一个文件 css js 分离开，那我们需要这个插件
var ExtractTextPlugin = require('extract-text-webpack-plugin')
var OptimizeCSSPlugin = require('optimize-css-assets-webpack-plugin')

var env = config.build.env

// 合并 webpack.base.conf.js
var webpackConfig = merge(baseWebpackConfig, {
  module: {
      // 使用的 loader
    rules: utils.styleLoaders({
      sourceMap: config.build.productionSourceMap,
      extract: true
    })
  },
  // 是否使用 #source-map 开发工具，更多信息可以查看 DDFE 往期文章
  devtool: config.build.productionSourceMap ? '#source-map' : false,
  output: {
      // 编译输出目录
    path: config.build.assetsRoot,
    // 编译输出文件名
    // 我们可以在 hash 后加 :6 决定使用几位 hash 值
    filename: utils.assetsPath('js/[name].[chunkhash].js'),
    // 没有指定输出名的文件输出的文件名
    chunkFilename: utils.assetsPath('js/[id].[chunkhash].js')
  },
  plugins: [
    // definePlugin 接收字符串插入到代码当中, 所以你需要的话可以写上 JS 的字符串
    new webpack.DefinePlugin({
      'process.env': env
    }),
    // 压缩 js (同样可以压缩 css)
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      },
      sourceMap: true
    }),
    // 将 css 文件分离出来
    new ExtractTextPlugin({
      filename: utils.assetsPath('css/[name].[contenthash].css')
    }),
    // Compress extracted CSS. We are using this plugin so that possible
    // duplicated CSS from different components can be deduped.
    new OptimizeCSSPlugin({
      cssProcessorOptions: {
        safe: true
      }
    }),
    // 输入输出的 .html 文件
    new HtmlWebpackPlugin({
      filename: config.build.index,
      template: 'index.html',
      // 是否注入 html
      inject: true,
      // 压缩的方式
      minify: {
        removeComments: true,
        collapseWhitespace: true,
        removeAttributeQuotes: true
      },
      chunksSortMode: 'dependency'
    }),
    // 没有指定输出文件名的文件输出的静态文件名
    new webpack.optimize.CommonsChunkPlugin({
      name: 'vendor',
      minChunks: function (module, count) {
        // any required modules inside node_modules are extracted to vendor
        return (
          module.resource &&
          /\.js$/.test(module.resource) &&
          module.resource.indexOf(
            path.join(__dirname, '../node_modules')
          ) === 0
        )
      }
    }),
    // 没有指定输出文件名的文件输出的静态文件名
    new webpack.optimize.CommonsChunkPlugin({
      name: 'manifest',
      chunks: ['vendor']
    }),
    // copy custom static assets
    new CopyWebpackPlugin([
      {
        from: path.resolve(__dirname, '../static'),
        to: config.build.assetsSubDirectory,
        ignore: ['.*']
      }
    ])
  ]
})
// 开启 gzip 的情况下使用下方的配置
if (config.build.productionGzip) {
  // 加载 compression-webpack-plugin 插件
  var CompressionWebpackPlugin = require('compression-webpack-plugin')

  webpackConfig.plugins.push(
      // 使用 compression-webpack-plugin 插件进行压缩
    new CompressionWebpackPlugin({
      asset: '[path].gz[query]',
      algorithm: 'gzip',
      test: new RegExp(
        '\\.(' +
        config.build.productionGzipExtensions.join('|') +
        ')$'
      ),
      threshold: 10240,
      minRatio: 0.8
    })
  )
}

if (config.build.bundleAnalyzerReport) {
    // 懒加载插件
  var BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin
  webpackConfig.plugins.push(new BundleAnalyzerPlugin())
}

module.exports = webpackConfig
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

## **5、build/build.js**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```js
// 检查 Node 和 npm 版本
require('./check-versions')()
// 当node无法判断是本地还是线上时，这里默认写上线上
process.env.NODE_ENV = 'production'
// 一个很好看的 loading 插件
var ora = require('ora')
var rm = require('rimraf')
var path = require('path')
var chalk = require('chalk')
var webpack = require('webpack')
var config = require('../config')
// 加载 webpack.prod.conf文件
var webpackConfig = require('./webpack.prod.conf')
// 使用 ora 打印出 loading + log
var spinner = ora('building for production...')
// 开始 loading 动画
spinner.start()

// 删除打包后的文件夹，重新生成打包后的文件
rm(path.join(config.build.assetsRoot, config.build.assetsSubDirectory), err => {
  if (err) throw err
  //  开始 webpack 的编译
  webpack(webpackConfig, function (err, stats) {
      // 编译成功的回调函数
    spinner.stop()
    if (err) throw err
    process.stdout.write(stats.toString({
      colors: true,
      modules: false,
      children: false,
      chunks: false,
      chunkModules: false
    }) + '\n\n')

    console.log(chalk.cyan('  Build complete.\n'))
    console.log(chalk.yellow(
      '  Tip: built files are meant to be served over an HTTP server.\n' +
      '  Opening index.html over file:// won\'t work.\n'
    ))
  })
})
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

## **6、config/index.js**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```js
// see http://vuejs-templates.github.io/webpack for documentation.
var path = require('path')
module.exports = {
  // production 环境
  build: {
      // 使用 config/prod.env.js 中定义的编译环境
    env: require('./prod.env'),
    port: 9000,
    index: path.resolve(__dirname, '../dist/index.html'),
    // 编译输出的静态资源根路径
    assetsRoot: path.resolve(__dirname, '../dist'),
    // 编译输出的二级目录
    assetsSubDirectory: 'static',
    // 编译发布上线路径的根目录，可配置为资源服务器域名或 CDN 域名
    assetsPublicPath: '',
    // 是否开启 cssSourceMap
    productionSourceMap: true,
    // 是否开启 gzip
    productionGzip: false,
    // 需要使用 gzip 压缩的文件扩展名
    productionGzipExtensions: ['js', 'css'],
    // 插件叫做bundleAnalyzerReport，上面有几行注释，讲的是只要在打包的时候输入npm run build --report,就可以在打包的同时查看每个打包生成的js，里面的库的构成情况，方便我们进行相关的删减，比如有的库太大了，是否可以自己实现，有的库是否有必要，可否删除之类
    bundleAnalyzerReport: process.env.npm_config_report
  },
  // 使用 config/dev.env.js 中定义的编译环境
  dev: {
    env: require('./dev.env'),
    // 运行测试页面的端口
    port: 8080,
    // 是否自动打开浏览器
    autoOpenBrowser: true,
    // 编译输出的二级目录
    assetsSubDirectory: 'static',
    // 编译发布上线路径的根目录，可配置为资源服务器域名或 CDN 域名
    assetsPublicPath: '/',
    // 需要 proxyTable 代理的接口（可跨域）
    proxyTable: {},
    // 是否开启 cssSourceMap
    cssSourceMap: false
  }
}
```



