### 浏览器运行环境

浏览器本身也是一个运行环境，也就是js的前端运行环境，chrome浏览器的运行环境主要由2部分组成：

**1.V8引擎，解析和执行JS代码。 **

**2.内置API，自带了BOM,DOM，canvas, XMLHttpRequest等API**

我们编写的js代码可以调用浏览器内置的API（这些API只能在浏览器所在的运行环境中执行），最后通过浏览器的V8引擎解析和执行

**浏览器的顶层对象是window**





### node.js是什么

**概念：基于chrome V8的JavaScript运行环境**

相当于js的后端运行环境，**不能调用浏览器内置的API**

node环境中一些常用内置API：fs/path/http/querystring...

应用场景：基于express框架，开发web应用；基于electron，开发跨平台桌面应用；基于restify，构建API接口项目

**node的顶层对象是global**



### 执行js文件

在文件位置打开cmd或powershell（shift+右键）， 执行命令 **node** xx.js

powershell相当于新版本的终端，功能更多



### fs文件系统模块

nodejs官方提供，用来操作文件的模块

**fs.readFile()  读取指定文件中的内容**

**fs.writeFile() 向指定文件中写入内容**

语法：

```javascript
// 只要安装了node环境，就可以直接通过require方法获取到fs模块
const fs = require('fs')

// 读取文件 参数 fs.readFile(path[, options], callback) // 必传：path文件路径和回调函数, 可选：options 指定编码格式
fs.readFile('./files/aa.txt', function(err, dataStr) {
    console.log(err) // 读取失败时的错误信息，是一个对象, 读取成功时为null
    console.log('----')
    console.log(dataStr) // 读取成功的数据， 读取失败时为undefined
})

// 写入内容 参数：fs.writeFile(path,data[,options], callback) // data: 要写入的内容
fs.writeFile('./files/aa.txt', 'abc', function(err) {
    console.log(err) // 写入失败时的错误信息，是一个对象， 写入成功时为null
})
```



#### fs读取文件路径问题

fs在读取文件时，**会基于node命令时所处路径**，动态拼接文件名，组成完整路径

**若执行了cd ../ 改变了路径，容易出现找不到文件的错误**，解决方案：使用绝对路径，从盘符开始写完整路径(移植性差，不利于维护，**推荐使用__dirname**)

```js
const fs = require('fs')
// 当前node所处路径为c:/users/desktop 则完整路径是c:/users/desktop/files/11.txt
fs.readFile('./files/11.txt', function(){})
```



#### __dirname（node内置成员）

__dirname表示当前文件所处的目录，通过__dirname拼接完整路径

```js
const fs = require('fs')
fs.readFile(__dirname + '/files/aa.txt',  function(){})
```





### path路径模块

node官方提供，提供处理路径的一系列方法和属性

**path.join()**  将多个路径片段拼接成一个完整的路径字符串, 拼接路径推荐都是用join，不要用+号

**path.basename()**  从字符串中将文件名解析出来

```js
const path = require('path')
const newpath = path.join(__dirname, './files/aa.txt') // 当前文件目录/file/aa.txt

// 读取文件案例最终写法，通过join
fs.readFile(path.join(__dirname, './files/aa.txt'), function(){})


// 获取文件名
const filepath = '/a/b/c/index.html'
const name = path.basename(filepath) // index.html
```



### 案例-处理html文件

读取一个包含<style>和<script>标签代码的html文件，提取里面的style, script部分到单独的css, js文件中存放

最终形成.css .js  .html 3个文件单独存放，html中通过外链引用.css和.js 的形式

```js
// 创建读取script和style的正则
const regStyle = /<style>[\S\s]*<\/style>/ //匹配任意空白和非空白字符 *表示任意次
const regJs = /<script>[\s\S]*<\/script>/

// resolveCSS方法，提取html文件中的<style></style>部分代码，放到单独样式文件中
function resolveCSS(htmlStr) {
    // 用正则截取style部分代码
    const res = regStyle.exec(htmlStr)
    const newCSS = res[0].replace('<style>', '').replace('</style>', '')
    
    // 写入新文件
    const fs = require('fs')
    const path = require('path')
    fs.writeFile(path.join(__dirname, './css/index.css'), newCSS, function(err) {
        if (err) console.log('CSS写入失败')
        console.log('CSS写入成功')
    })
}

// 提取js代码，和css处理逻辑一样
function resolveJS(htmlStr) {}

// 处理html文件，把包含的script和style部分代码去除，替换为link形式
function resolveHTML(htmlStr){
    const newHTML = htmlStr.replace(regStyle, '<link rel="stylesheet" href="./css/index.css" />')
    					   .replace(regJs, '<script src="./js/index.js"></script>')
    // 写入新的index.html文件
    const fs = require('fs')
    const path = require('path')
    fs.writeFile(path.join(__dirname, './index.html'), newHTML, function(err){
        if(err) console.log('写入html失败')
        console.log('写入html成功')
    })
}
```





### http模块

node官方提供，创建web服务器的模块

创建web服务器步骤

```js
// 1.导入模块
const http = require('http')
// 2.创建web服务器实例
const server = http.createServer()

// 3.给服务器绑定request事件
// 只要有客户端请求，就会触发request事件
 server.on('request', (req, res) => {
     // 触发事件后的回调
     console.log('someone visit webserver')
     /*
     	req请求对象包含了客户端的相关属性，例如：
     	req.url是客户端请求的url地址， 请求地址从端口号后面开始，若请求的是根地址，则url就是'/'
     	req.method 客户端请求类型  默认是get
     */
     
     /*
     	res是响应对象， 通过res.end()传要响应的内容
     	res.setHeader 设置响应头各种字段 中文一定要设置否则乱码
     */ 
     res.setHeader('Content-Type', 'text/html; charset=utf-8')
     res.end('我是node server') // 被插入到页面的body标签中
 })

// 4.启动服务器 指定端口号和回调函数
server.listen(80, () => {
    console.log('http server running at http://127.0.0.1')
})
```



**根据不同的请求地址返回对应内容**

```js
server.on('request', (req, res) => {
    const url = req.url // 获取请求地址
    let content = '<h1>not found</h1>'
    if (url === 'index.html') {
        content = '<h1>首页</h1>'
    }
    if (url === 'about.html') content = '<h1>关于</h1>'
    res.setHeader('Content-Type', 'text/html; charset=utf-8')
    // 把响应内容返给客户端
    res.end(content)
})
```



#### 读取文件内容返回给客户端

```js
/*
	思路：前端请求地址映射为服务器的资源储存地址，读取文件内容后返回给客户端
	例如请求/index.html 服务端映射资源路径为 资源完整路径+/index.html 通过__dirname拼接
*/ 
const http = require('http')
const server = http.createServer()
const fs = require('fs')
const path = require('path')

 server.on('request', (req, res) => {
     const url = req.url
     // 把请求路径映射到资源路径
     // 用户请求/index.html 后台资源路径实际为: __dirname+/web/index.html
     fs.readFile(path.join(__dirname, '/web', url), 'utf8', function(err, dataStr) {
         if(err) console.log('请求失败')
         res.end(dataStr) // 把读取成功后的内容相应给客户端
     })
 })

// 4.启动服务器 指定端口号和回调函数
server.listen(80, () => {
    console.log('http server running at http://127.0.0.1')
})
```



### 模块化

优点：提升代码复用性，提升代码可维护性，实现按需加载

nodeJs遵循CommonJS模块化规范

nodejs的模块分类：

1.内置模块（由nodeJs官方提供，如fs, path, http）

2.自定义模块（用户创建的每个js文件都是自定义模块）

3.第三方模块（由第三方开发的模块，也叫做包）



#### 加载模块

调用require()方法即加载模块

**require()加载模块时会执行模块中的代码**

```js
const fs = require('fs') // 加载内置模块
const moment = require('moment') // 加载第三方模块和内置模块差不多

const custom = require('./costom.js') // 加载自定义模块需要指定路径
```

**模块有自己的作用域，不会共享变量和属性**

```js
// a.js
const name = 'david'

// b.js
const m = require('./a.js')
console.log(m) // {}  打印的是一个空对象，模块作用域，模块成员不共享
```



#### module对象

在每个.js自定义模块中都有一个module对象，储存了和当前模块有关信息，如id,path,filename,exports等等

##### exports向外共享成员

exports是module对象上的一个属性，默认值是一个空对象

module.exports 可以将模块内成员共享出去，供外界调用

**外部使用require()方法得到的就是module.exports所指向的对象**



### npm

全球最大的包共享平台，也就是第三方模块共享平台，可以从官方服务器下载所需包，通过npm包管理工具下载包

npm init -y  创建package.json包管理配置文件， -y表示采用系统默认的基本配置信息

npm install xx   安装包



#### 自己发布npm包

创建一个npm项目，创建入口文件，创建README.md 说明文档，创建package.json 包配置文件

关键步骤：package.json中配置入口文件main: index.js，创建src文件夹，将对应功能写到独立的js文件中再导出

index.js主要用来暴露模块，暴露包中要提供的各种功能

```js
// index.js 入口文件
const date = require('./src/dateFormat.js')
const escape = require('./src/htmlEscape.js')

// 暴露功能模块
module.exports = {
    ...date, // 展开对象挂载到模块上, 因为date导出的对象也挂载了多个方法
    ...escape
}

// 使用方式
const myUtils = require('david-pacakge') // package.json中配置的包名
myUtils.formatDate(new Date())
```

