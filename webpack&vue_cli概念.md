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

npm install 不写-g就是局部安装

**不同的项目依赖的webpack版本不同，推荐每个项目使用局部安装webpack**

npm install webpack webpack-cli -D (--save-dev)  生成node_modules文件

**命令行局部打包命令: **

**方式1： ./node_modules/.bin/webpack**

**方式2： npx webpack**

**方式3： package.json文件中---> "script"添加脚本: "build": "webpack"**

修改打包文件入口和出口 (webpack默认入口是src/index.js)

npx webpack --entry ./src/main.js --output-path ./dist

### npm init 

创建package.json文件 (依赖项文件)



### 配置文件webpack.config.js

__dirname是[node.js](https://so.csdn.net/so/search?q=node.js)的一个全局变量，用于指向当前执行脚本（dirname.js）所在的目录路径，而且是绝对路径

```js
const path = require('path')
module.exports = {
    entry: './src/main.js',
    // output必须是绝对路径
    output: {
        // path.resolve对路径进行拼接 __dirname是当前文件路径， ./dist打包后的文件夹
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

作用：只解析.css文件， 并不负责把CSS插入到页面

命令行安装: npm install css-loader -D

### style-loader

作用： 把解析后的css样式插入到元素中

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
// 打包图片
{
    test: /\.(jpg|png|gif|svg)$/,
    use:[
        {
            loader: 'file-loader',
            options: {
                outputPath: 'img', //打包后输出路径在img文件夹内
                // []内表示文件名格式，hash:6表示6位的哈希值，ext表示文件后缀名
                name: '[name]_[hash:6].[ext]'  
            }
        }
    ]
}
// 打包字体图标
{
    test: /\.(eot|ttf|woff|woff2?)$/,
    use:[
        {
            loader: 'file-loader',
            options: {
                name: 'font/[name]_[hash:6].[ext]' // 路径也可以直接写在name属性内
            }
        }
    ]
}
```

### url-loader

和file-loader工作方式相似，可以将小文件转成base64URI

区别：可以对小的图片进行base64格式打包，打包后不会生成图片，会被编码到打包后的js文件中

优点： 减少请求服务器的次数 (图片请求也会消耗服务器资源)

```js
{
    test: /\.(jpg|png|gif|svg)$/,
    use:[
        {
            loader: 'url-loader',
            options: {
                outputPath: 'img', //打包后输出路径在img文件夹内
                name: '[name]_[hash:6].[ext]',
                limit: 100 * 1024 // 100kb以下的图片才进行base64格式打包
            }
        }
    ]
}
```

### asset module type(资源模块类型)

webpack5新特性, 替代file-loader/url-loader等

```js
// asset打包图片, 效果同上面的url-loader
{
    test: /\.(jpg|png|gif|svg)$/,
    type: 'asset',
        parser:{
            dataUrlCondition: {
                maxSize: 100 *1024
            }
        }
}
// asset打包字体图标
{
    test: /\.(eot|ttf|woff|woff2?)$/,
    type: 'asset/resource',
        generator: {
            filename: 'font/[name]_[hash:6][ext]' // asset不需要加.
        }
}
```





### 使用相对路径导入图片不显示的问题

```js
const el = document.createElement('img')
// 这种导入方式通过webpack打包后，图片是无法显示的，因为这个src会被认为是字符串
el.src = '../assets/xxx.png'  

// 解决方案: 使用模块化方式导入，当成模块资源使用， import或者require 打包时才能正确解析资源路径
el.src = require('../assets/xxx.png')
// 或者
import xxpng from '../assets/xxx.png' 
el.src = xxpng
```



## webpack插件-plugin

webpack的另一个核心就是插件，用来实现打包优化，资源管理、环境变量注入等功能

### cleanWebpackPlugin

cleanWebpackPlugin： 每次重新打包前自动删除dist文件夹

安装: npm install clean-webpack-plugin -D

```js
const path = require('path')
// 该模块会返回一个对象，包含一个cleanWebpackPlugin类
const {CleanWebpackPlugin} = require('clean-webpack-plugin')
module.exports = {
    entry: './src/main.js',
    // output必须是绝对路径
    output: {
        // path.resolve对路径进行拼接 __dirname是当前文件路径
        path: path.resolve(__dirname, './dist'),
        filename: 'bundle.js' //打包后文件名
    },
    // 插件配置
    plugins: [
        new CleanWebpackPlugin()
    ]
}
```

### htmlWebpackPlugin

htmlWebpackPlugin： 打包后生成一个index.html入口文件

```js
const path = require('path')
const {CleanWebpackPlugin} = require('clean-webpack-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
// webpack内置插件 无需安装
const {DefinePlugin} = require('webpack')
module.exports = {
    entry: './src/main.js',
    // output必须是绝对路径
    output: {
        // path.resolve对路径进行拼接 __dirname是当前文件路径
        path: path.resolve(__dirname, './dist'),
        filename: 'bundle.js' //打包后文件名
    },
    // 插件配置
    plugins: [
        new CleanWebpackPlugin(),
        // htmlwebpackplugin可以传入一个对象，用来定义index.html的模板
        new HtmlWebpackPlugin({
        	template: './public/index.html' //使用public下面的index.html作为打包后的模板
        }),
        // 定义index.html模板内EJS语法的BASE_URL的值
        new DefinePlugin({
            BASE_URL: "'./'"  // 双引号内单引号，定义字符串， 一个双引号会作为变量去寻找对应的值
        })
    ]
}
```

### copyWebpackPlugin

copyWebpackPlugin：把public内的文件复制到打包后的dist文件夹内

```js
const copyWebpackPlugin = require('copy-webpack-plugin')
plugins:[
    new CopyWebpackPlugin({
        patterns: [
            {
                from: 'public', //从哪里拷贝
                to: './',   // 可不填，不填默认拷贝到output地址
                globOptions: {
                    ignore: [
                        '**/index.html' // 要忽略的文件
                    ]
                }
            }
        ]
    })
]
```

### mode和devtool

```js
module.exports = {
    mode: 'development',  // 打包模式:production和devlopment，区别代码是否压缩
    devtool: 'source-map'  // 建立js映射文件， 方便调试代码错误，返回具体的错误文件和位置
} 
```



## Babel

将ES6+/TS/JSX 转化为普通javascript代码的工具

**@babel/core** 核心库

**@babel/cli** 能在命令行使用babel

安装： npm install @babel/core @babel/cli  -D

使用：npx babel demo.js -- out-dir dist     //把demo.js转化为普通js, 输出到dist文件夹下

### 转化箭头函数

转化特定的语法 需要使用babel特定插件，也需要安装

安装插件: npm install @babel/plugin-transform-arrow-functions -D

// 命令行 --plugins跟上插件

使用： npx babel demo.js --out-dir dist --plugins=@babel/plugin-transform-arrow-functions

### Babel预设@babel/preset-env

预设了一些常用的转换

安装: npm install @babel/preset-env -D

使用： npx babel demo.js --out-dir dist --presets=@babel/preset-env

通过webpack使用babel

```js
{
    test: /\.js$/,
        use: {
            loader: 'babel-loader',
            options: {
                // 写法1： 逐个写入插件
                plugins: [
				"@babel/plugin-transform-arrow-functions", //转化箭头函数插件
                 "@babel/plugin-transform-block-scoping"  // 转化const作用域插件
                ],
                // 写法2： 直接使用预设
                presets: [
				"@babel/preset-env"
                ],
                
                presets: [
                    // 写法3： preset有额外参数时,写在数组中，额外参数跟在后面的{}
                    ["@babel/preset-env", {}]
                ]
            }
        }
}
```

### 单独写babel配置文件

babel.config.json (或.js .cjs .mjs)

.babelrc.json (或.babelrc  .js .cjs)

```js
// babel.config.js
module.exports = {
    presets: [
    	"@babel/preset-env"
    ]
}


// webpack.config.js 
{
    test: /\.js$/,
    use: {
        loader: 'babel-loader'  //直接写babel-loader即可
    }
}

```

### npm下载的vue不同版本解析

**vue的版本主要分为1. 运行时+编译器 或 2.仅运行时**

通过npm install vue@next 下载下来的vue代码，放在node_modules内

其中dist文件夹下有很多版本

**vue.global.js:**  通过<script src=''>进行引用或cdn引用的就是这个版本

esm表示es module

**vue.esm-browser.js:**  浏览器中通过<script type="module"> 引用的是这个版本

**vue(.runtime).esm-bundler.js:**  用于webpack等构建工具的版本, 有2个版本 runtime-compiler和runtime only

runtime only 不能解析template,   runtime-compiler可以

**所以如果app.vue文件(打包入口文件)含有template模板，打包vue项目时要指定vue.esm-bundler.js 版本**



### vue开发的3种方式

1. template模板方式
2. render函数， 使用h函数来编写渲染的内容 (h函数返回虚拟vnode节点)
3. sfc方式编写： 即.vue文件中的template来编写模板 （.vue文件会通过vue-loader 解析）



### .vue就是sfc文件

.vue文件本质就是sfc(single file components(单文件组件))

vscode安装插件支持sfc文件 

1. vetur  vscode支持的vue插件
2. volar 官方推荐 



### vue项目打包

### vue-loader

解析.vue文件

npm install默认会安装vue2的loader， 需要使用@next安装最新版

安装: npm install vue-loader@next -D  开发时依赖

同时安装: npm install @vue/compiler-sfc   （sfc文件解析插件）

引入vue-loader的插件: VueLoaderPlugin

```js
const {VueLoaderPlugins} = require('vue-loader/dist/index')
{
    test: /\.vue$/,
    use: {
        "vue-loader"
    }
}
plugins: [
    new VueLoaderPlugins(),
    // 额外打包配置，写在definePlugin内
    new DefinePlugin({
        __VUE_OPTIONS_API: true, // 是否支持vue2的optionsAPI, 设置false打包后包会更小
        __VUE_PROD_DEVTOOLS__: false // 生产环境是否支持devtools调试工具
    })
]
```



### webpack搭建本地服务

#### webpack-dev-server（基于express框架的本地服务器）

**没有本地服务时，可使用live-server插件实现本地服务，但没有热更新功能**

作用： 监听文件变化，具备live reloading（实时重新加载）的功能

1.安装： npm install webpack-dev-server -D

2.配置脚本： package.json 内配置serve脚本

```json
"script":{
    "build": "webpack",
    "serve": "webpack serve"  //运行npm run serve时 自动执行该命令 启动本地服务器
}
```

### npm run serve 后的打包文件

本质上也是有一个打包文件产生，但是不会输出任何打包后的文件到目录上，而是**存在了内存**里供本地服务使用 



### devServer配置 

```js
const path = require('path')
//完整配置目录
module.exports = {
    target: 'web|node', // 打包环境 一般都是web
    mode: 'development',
    devtool: 'source-map',
    entry: './src/main.js',
    output: {}, //打包出口
    module: {}, // 各种loader配置
    plugins: [], // 各种插件配置
    devServer: { // devServer配置
        // 开发环境使用, 生产环境仍要使用CopyWebpackPlugin插件，因为资源都要部署到服务器上
        contentBase: { //如果某些资源未被webpack打包，则会从contentBase中加载
            "./public"   //public文件夹内的资源会被webpack加载
        },
        // webpack中一般把一个文件当成一个模块， 不开启默认使用live reloading
        hot: true, // hmr(模块热替换) 应用运行中替换、添加、删除模块，无需刷新整个页面 
        // localhost本质是一个域名，会被解析成127.0.0.1
        host: "localhost | 0.0.0.0",
        port: 8090,
        // 是否对请求的数据进行压缩，压缩除html外的文件。一般采用gzip压缩
        compress: true,   // 响应头response-headers的content-encoding变成gzip
        // 最常用的配置，开发阶段本地跨域配置
        proxy: {
            "/api":{
                target: "http://xxxx.cn:8888",
                pathRewrite: {
                    "^/api": ''     //重写路径 去除/api路径
                },
                secure: false,  // 默认true 没有证书时，不接受转发到https服务器上
                changeOrigin: true // 请求源修改，设为true会放到header中，避免服务器安全校验
            }
        }
    },
    // 模块解析 注意和devServer同级
    resolve: {
        // 扩展名配置
        extensions: [".vue", '.ts'],  //默认有.js .json  所以import引入js json可以不写后缀名
        // 路径别名配置
        alias: {
            "@": path.resolve(__dirname, './src') // __dirname表示当前项目路径
        }
    }
}
```

```js
// main.js 文件
import './js/xxxx.js' 
// 需要加个判断，指定要开启热替换的模块（ vue-loader默认实现了热替换，无需手动写）
if(module.hot){
    module.hot.accept('./js/xxxx.js') // 要开启热替换的模块
}
```

### devServer热更新原理

webpackDevServer会创建2个服务:

1.静态资源服务(express) 2.socket服务(长链接)

express server直接提供静态资源的服务(打包后的资源直接被浏览器请求和解析)

socket server 长链接服务，建立连接后服务器可以直接发送文件到客户端

当服务器监听到对应的模块发生变化时，会生成2个文件.json描述文件，.js更新文件

通过长链接直接将2个文件主动发给客户端(浏览器)

浏览器拿到2个文件后，通过hmr runtime机制加载2个文件实现模块热更新



### localhost和0.0.0.0的区别

127.0.0.1是一个回环地址，意思是自己主机发出去的包，被自己接收

正常数据库包发送经过应用层-传输层-网络层-数据链路层-物理层

而回环地址在网络层就被捕获，不经过数据链路层-物理层

因此当监听127.0.0.1时，在同一网段下的主机中，通过ip地址是不能访问的

0.0.0.0： 监听ipv4上所有的地址，再根据端口找到不同的应用程序

当监听0.0.0.0时，即监听ipv4上所有的地址，在同一网段的其他主机就能通过ip地址进行访问



### resolve 模块解析

```js
 // 模块解析 注意和devServer同级
    resolve: {
        // 扩展名配置
        extensions: [".vue", '.ts'],  //默认有.js .json  所以import引入js json可以不写后缀名
        // 路径别名配置
        alias: {
            "@": path.resolve(__dirname, './src') // __dirname表示当前项目路径
        }
    }
```

resolve用于设置模块如何被解析

可以帮助webpack从每个require/import语句中找到需要引入的合适的模块代码

webpack能解析的3种文件路径

1.绝对路径 不需要进一步解析

2.相对路径

​	使用import/require引入的资源文件所处的目录，被认为是上下文目录

​	在Import/require中给定的相对路径，会拼接此上下文路径，来生成模块的绝对路径

3.模块路径

​	在resolve.modules中指定的所有目录检索模块

​	默认值：node_modules, 默认会从node_modules中查找模块

确认是文件还是文件夹

如果是文件：

如果文件有扩展名，则直接打包，否则使用resolve.extensions作为文件扩展名解析

如果是文件夹：

resolve.mainFiles配置选项中指定的文件顺序查找， mainFiles默认值是index

再根据resolve.extensions来解析扩展名

**所以/common/index.js这种目录结构时，可直接import xxx from  './common'  省略路径index.js**



### 环境分离（重要）

```js
//第一步 package.json配置： 不同环境运行不同的webpack配置
"scripts": { // --config 指定配置文件
    "build": "webpack --config ./config/webpack.prod.config.js",
    "serve": "webpack serve --config ./config/webpack.dev.config.js"
}
```

```js
// 创建一个config文件夹，存放不同环境的webpack配置
// webpack.common.config.js 公共配置 （开发和生产都需要的配置）
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const {DefinePlugin} = require('webpack')
const {VueLoaderPlugin} = require('vue-loader/dist/index')
module.exports = {
    target: 'web',
    entry: './src/main.js',
    output: path.resolve(__dirname, './dist'),
    filename: 'js/bundle.js',
    resolve: {
        extensions: [],
        alias: {}
    },
    module: {},
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html',
            title: 'xxx'
        }),
        new DefinePlugin({
            BASE_URL: "'./'",
            __VUE_OPTIONS_API__: true,
            __VUE_PROD_DEVTOOLS: false
        }),
        new VueLoaderPlugin()
    ]
}
// webpack.dev.config.js 开发环境配置
module.exports = {
    mode: 'development',
    devtool: 'source-map',
    devServer: {},
    
}
// webpack.prod.config.js 生产环境配置
const {CleanWebpackPlugin} = require('clean-webpack-plugin')
const CopyWebpackPlugin = require('copy-webpack-plugin')
module.exports = {
    mode: 'production',
    plugins: [
        new CleanWebpackPlugin(), // 生产环境才需要的插件
        new CopyWebpackPlugin({
            patterns: [
                {
                    from: "public",
                    to: './', 
                    globOptions: {
                        ignore: ["**/index.html"]
                    }
                }
            ]
        })
    ]
}
```

### merge 合并

webpack-merge也是一个插件，用来合并配置文件， 需要安装

安装: npm install webpack-merge -D

把webpack.common.config合并到dev和prod的config文件中

```js
// merge是一个函数， 入参就是2个要合并的对象
const {merge} = require('webpack-merge')
const commonConfig = require('./webpack.common.config')

const {CleanWebpackPlugin} = require('clean-webpack-plugin')
const CopyWebpackPlugin = require('copy-webpack-plugin')

// 通过merge函数合并commonConfig和 {}内的prod配置对象
module.exports = merge(commonConfig, {
    mode: 'production',
    plugins: [
        new CleanWebpackPlugin(), // 生产环境才需要的插件
        new CopyWebpackPlugin({
            patterns: [
                {
                    from: "public",
                    to: './',
                    globOptions: {
                        ignore: ["**/index.html"]
                    }
                }
            ]
        })
    ]
})
```







## vue-cli知识点

作用： 预设很多webpack的基本配置

全局安装: npm install @vue/cli -g

通过vue-cli创建vue项目：  vue create 项目名称

### vue-cli创建项目的目录结构

public: 存放一些项目资源，打包时会被复制到dist文件夹

src: 所有源码

.browserslistrc: 设置要适配的浏览器范围

.gitignore:  git的忽略文件

babel.config.js:   babel配置文件

package.json:   项目的管理文件

### vue-cli脚本

```js
// vue-cli和webpack的运行原理是一样的，就是运行了vue-cli的对应命令
"script": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build"
}
```

### vue-cli-service原理（难点）

运行npm run serve 就是执行vue-cli-service serve命令

这个命令会去**node_modules/.bin**文件下寻找**vue-cli-service**文件执行

而这个vue-cli-service其实是一个软连接，真实执行的代码位置在node_modules/@vue/cli-service目录下

**node_modules/@vue/cli-service/package.json**文件:

```json
// 关键脚本 真实执行的命令: vue-cli-service
"bin":{
    "vue-cli-service": "bin/vue-cli-service.js" //真正被执行的js
}
```

部分源码 (配置所有webpack配置)

```js
// node_modules/@vue/cli-service/lib/service.js
// 获取webpack的所有配置
const webpackConfig = api.resolveWebpackConfig()

// 同目录pluginAPI.js
resolveWebpackConfig(chainableConfig){
    return this.service.resolveWebpackConfig(chainableConfig)
}
```

部分基础配置源码

```js
// config/base.js  css.js prod.js
webpackconfig  // 基于第三方库，采用了链式调用
	.mode('development')
	.context(api.service.context)
	.entry('app')
	.add('./src/main.js')
	.end()
	.output
		.path(api.resolve(options.outputDir))
		.filename(isLegacyBundle ? '[name]-legacy.js' : '[name].js')
	// ....
```



### vue.config.js(vue-cli配置)

自己的vue-cli配置文件， 最后会和vue-cli内的webpack配置进行合并

```js
module.exports = {
    // 根据源码，两种key的写法都可以
    configureWebpack: {},
    chainWebpack: {}
}
```





## Vite

官网: cn.vitejs.dev

思想: 基于最新浏览器支持es module解析，把ts,vue等文件都转化为module，省去了模块解析这一步

预打包： 如果项目有基于第三方库，第一次打包会对这些库进行预打包，放在node_modules/.vite下，第二次打包会直接拿这些包进行打包，所以速度更快

```js
// main.js
import {fn} from './utils.js'
console.log(fn('123'))
```

```html
<html>
    <!--最新浏览器使用type="module" 支持es module语法， 能够解析esModule模块-->
    <script src="./main.js" type="module"></script>
</html>
```

局部安装:  npm install vite -D 

运行： npx vite (运行**node_modules/.bin**文件夹内的vite)



### vite对css的支持

先npm init 安装package.json 管理项目依赖

vite 可以直接解析css模块 不需要css-loader

vite不能解析less,还是需要安装: npm install less -D

安装postcss：  npm install postcss -D

安装postcss对应插件：npm install postcss-preset-env -D

写对应配置

```js
// postcss.config.js
module.exports = {
    plugins: [
        require('postcss-preset-env')
    ]
}
```

### vite对ts的支持

vite直接支持ts 不需要额外配置



### vite对vue的支持

安装插件：

vue3： @vitejs/plugin-vue

vue3jsx： @vitejs/plugin-vue-jsx

vue2： underfin/vite-plugin-vue2



### vite.config.js(vite配置文件)

```js
const vue = require('@vitejs/plugin-vue')
module.exports = {
    plugins:[
         vue()
    ]
}
```



### 正式打包

npx vite build



### 真实开发可使用script命令

```js
// package.json 配置脚本
"script":{
    "serve": "vite",
    "build": "vite build",
    "preview": "vite preview"  // 预览打包后的项目
}
```



### vite脚手架

vite实际上有2个工具：

vite: 相当于一个构建工具，类似webpack， 从0开始搭建项目

@vitejs/create-app: 相当于脚手架，类似vue-cli， 预设很多vite配置

使用方法（推荐， 非官方init方法）：

1.全局安装脚手架: npm install @vitejs/create-app -g

2.创建项目:  create-app 项目名称













### 

### 

### 

### 

### 　vue-cli各配置文件作用

1、build/dev-server.js  文件 项目node的启动文件，这里面做了webpack配置和node操作，

2、build/webpack.base.conf.js   webpack基本配置文件

3、build/webpack.dev.conf.js  开发环境webpack配置

4、build/webpack.prod.conf.js  正式环境的webpack配置

5、build/build.js   执行打包的配置文件

6、config/index.js    开发、线上环境的文件输出、打包等一些配置



### **1、build/dev-server.js**

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

### **2、build/webpack.base.conf.js**

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

 

### **3、build/webpack.dev.conf.js**

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

 

### **4、build/webpack.prod.conf.js** 

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

 

### **5、build/build.js**

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

 

### **6、config/index.js**

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



