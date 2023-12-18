###  浏览器运行环境

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

**path.join()**  将多个路径片段拼接成一个完整的路径字符串,  **推荐拼接路径都是用join，不要用+号**

**path.basename()**  从字符串中将文件名解析出来

```js
const path = require('path')
const newpath = path.join(__dirname, './files/aa.txt') // 路径为： __dirname/files/aa.txt

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



#### 加载机制

1.模块首次加载会放到缓存中，如引用了多次同一个模块，模块中的代码只会执行一次

2.内置模块优先级最高，如第三方模块和内置模块重名，优先加载内置模块

3.require()加载的模块不写后缀名，会自动补全，优先补全为js，如果没有xx.js这个文件，补全为xx.json

4.require()加载的是文件夹，优先加载文件夹内package.json中指定的main入口文件，若没有package.json，则加载index.js

查找机制： 如果加载模块不是内置模块，也没有路径标识，node会从当前模块的父目录开始尝试从/node_modules中加载第三方模块，如果没有则移动到再上一层，直到系统根目录



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



**发布包流程：**

1.npm官网注册账号

2.终端内执行**npm login** 依次输入用户名、密码、邮箱后登录成功

3.将终端切换到包的根目录，执行**npm publish**,即将包发布到了npm(注意包名必须唯一)





### express

基于nodejs的一个web开发框架，用来创建web服务器（代替内置模块http，更高效），本质也是一个第三方模块

express最常见的使用：

创建web网页资源服务器，创建api接口服务器



#### 安装

npm install express@4.17.1  安装指定版本

#### 使用

```js
// 导入模块
const express = require('express')
// 创建web服务器
const app = express()

app.listen(80, () => {
    console.log('express server is running at http://127.0.0.1')
})

// 监听get请求
app.get('请求URL', function(req, res) {/* 处理函数 */})
// 监听post请求
app.post('请求URL', function() {/* 处理函数 */})

// 返回数据
app.get('/user', function(req, res) {
    // send方法可以响应一个JSON对象， 也可以响应文本
    res.send({name: 'david', age: 35})
})


// 获取url中的参数  127.0.0.1/?name=david&age=35
app.get('/', function(req, res) {
    // req.query获取url上的参数
    console.log(req.query) // {name: 'david', age: 35}
})

// 获取动态参数 请求地址127.0.0.1/user/123
app.get('/user/:id', function(req, res) {
    // req.params匹配动态参数
    console.log(req.params) // {id: 123}
})
```



#### 托管静态资源

app.use(express.static(’目录‘))   对外提供静态资源

```js
/* 
	指定public目录为静态资源文件夹，如images,css,js都是public文件夹内的资源，就都可以访问到了
	访问的路径不需要添加/public路径，默认访问的就是public路径下的资源
	localhost:3000/images/xx.jpg
	localhost:3000/css/style.css
	localhost:3000/js/xx.js
*/
app.use(express.static('./public')) // 当前文件相对路径
app.use(express.static('./files')) // 可以托管多个静态资源目录


// 挂载路径前缀
app.use('/resource', express.static('./public')) // 需要访问localhost:3000/resource/js/xx.js 才能访问资源
```



#### 路由

express中，路由是指客户端请求与服务器处理函数的**映射关系**

express中路由分3部分组成：请求类型，请求的URL，处理函数

```js
// 挂载路由，匹配get请求， 请求url为/user
app.get('/user', (req, res) => {
    res.send('hello get')
})

// 挂载路由，匹配post请求，请求url为/
app.post('/', (req, res) => {
    res.send('hello post')
})
```



#### 路由模块化

不推荐把路由直接挂载到app上，推荐抽离到单独模块管理

```js
// router.js文件内，创建路由实例
const express = require('express')
// 创建路由实例
const router = express.Router()
// 挂载路由
router.get('/user/list', (req, res) => {res.send('get list')})
router.post('/user/add', (req, res) => {res.send('post add')})

// 导出路由
module.exports = router
```

app模块内导入路由

```js
// 导入路由模块
const useRouter = require('./router.js')
// 注册路由模块
app.use(useRouter) // app.use 用来注册全局中间件, 路由本质上也是一个中间件

// 给路由添加统一前缀
app.use('/api', useRouter) // 访问时路径前要加/api
```





#### nodemon

监听代码，修改node代码后自动重启服务，无需手动重启

安装：npm i nodemon -g

使用：nodemon xx.js     使用nodemon运行的js代码，就可以监听代码改动，并自动重启项目



#### 中间件

express中间件概念：

业务处理环节中的中间过程，必须有输入和输出，本质是一个函数

当请求到达服务器后，可以连续调用多个中间件，从而对请求进行预处理

客户端请求==》中间件1===》中间件2===》....中间件N===》处理完毕响应(路由)===》客户端接收响应

next函数作用：是多个中间件连续调用的关键，表示把流转关系转交给下一个**中间件**或**路由**

```js
// 官方示例：这是一个中间件函数，它的形参中必须包含一个next函数， 这是和路由函数的区别
app.get('/', function(req, res, next) {
    next() // 处理完毕，转交处理给下一个中间件或路由
})
```

##### 全局中间件

```js
const express = require('express')
const app = express()

// 定义中间件函数
const mw = function(req, res, next) {
    console.log('我是中间件')
    const reqTime = new Date().getTime()
    req.reqTime = reqTime // 给req对象添加属性，流转到下一个函数中使用
    next() // 必须调用，流转req和res给下一个函数
}
// 注册全局中间件
app.use(mw)

app.get('/', function(req, res) {
    console.log(req.reqTime) // 获取中间件添加的属性
})
app.listen(80, () => {console.log('server is running')})
```

##### 局部中间件

不使用app.use的就是局部中间件

```js
const mw = function(req, res, next) {
    console.log('中间件函数')
    next()
}
const mw2 = function() {}
/*
	挂载局部生效的中间件，可以同时挂载多个，会先调用中间件函数，再调用路由函数
*/
app.get('/', mw, mw2, function(req, res) {
    res.send('home')
})
```

##### 路由级别中间件

```js
const app = express()
const router = express.Router()

// 路由上注册中间件
router.use(function(req, res, next) {
    next()
})
app.use(router)
```

##### 错误级别中间件

```js
app.get('/', function(req, res) {
    throw new Error('some error') // 模拟错误并抛出
})

// 错误级别的中间件多一个形参err，包含了错误信息, 捕获错误防止程序崩溃, ！必须注册在所有路由之后！
app.use(function(err, req, res, next) {
    console.log('错误信息:', err.message )
    res.send('error==>', err.message) // 响应错误信息
})
```

##### 内置中间件

node官方提供的中间件

```js
// express.static 托管静态资源的中间件

// express.json  解析json格式的请求数据
app.use(express.json())
// express.urlencoded  解析url-encoded格式的请求数据
app.use(express.urlencoded({ extended: false }))
```

获取请求体演示：

```js
const express = require('express')
const app = express()

// 关键：配置解析json的中间件
app.use(express.json())

app.post('/user', function(req, res) {
    // req.body可以获取客户端发送的请求体数据，默认情况若不配置解析数据的中间件res.body = undefined
    console.log(req.body)
    res.send('ok')
})

app.listen(80, function() {console.log('server is running')})
```



##### 第三方中间件

body-parser ：解析请求体数据中间件

安装 **npm install body-parser**，require导入， app.use注册中间件

```js
const parser = require('body-parser')

app.use(parser.urlencoded({extended: false}))

app.post('user', function(req, res) {
    console.log(req.body) // 可以解析url-encoded格式数据
})
```



##### 自定义中间件

模拟express.urlencoded中间件

```js
// 0.创建服务器 略

// node内置模块querystring, 转换数据格式用
const qs = require('querystring')

// 1. 定义中间件
app.use((req, res, next) => {
    // 1.1监听req的data事件, 当服务端接收到数据时就会出发这个事件
    // 当数据很大时，不会一次接收完，可能多次触发事件，定义一个变量存储所有数据
    let str = ''
    req.on('data', (chunk) => { // chunk就是每次触发事件时接收到的数据
        str += chunk
    })
    // 1.2 监听req的end事件， 请求发送完毕后触发
    req.on('end', () => {
        console.log(str) // 完整的数据，是一个字符串
        // 1.3 把字符串数据转换成对象
       	const body = qs.parse(str)
        // 1.4 把body挂载到req.body上，给下游中间件或路由使用
        req.body = body
        next() // 别忘记调用next
    })
})

// 下游路由
app.post('/user',  function(req, res) {
    res.send(req.body) // 获取body数据并响应回去
})
```

**封装成模块**

```js
// custom-parser.js 文件名
const qs = require('querystring')

function bodyParser(req, res, next) {
    let str = ''
    req.on('data', (chunk) => {
        str += chunk
    })
    req.on('end', () => {
       	const body = qs.parse(str)
        req.body = body
        next()
    })
}
module.exports = bodyParser

// 使用模块
const customParser = require('custom-parser')
app.use(customParser)
```



##### 注意事项

1.多个中间件共享req和res对象

2.中间件必须定义在路由前面（错误级别中间件定义在最后）

3.必须调用next函数

4.next函数执行后不要再写代码



#### 编写接口

```js
// app.js 
const express = require('express')
const app = express()

// 导入路由
const router = require('./router.js')

// 注册解析请求体中间件
app.use(express.urlencoded({extended: false}))

// 指定/api为统一路由前缀， 注册全局路由
app.use('/api', router)
// 启动服务
app.listen(80, function() {})


// router.js
const express = require('express')
const router = express.Router()

// 挂载get请求路由  前端请求路径为: /api/user
router.get('/user', function(req, res) {
    // 获取查询参数
    const query = req.query
    
    res.send({
        code: '000000',
        msg: 'get请求成功',
        data: query // 把请求查询参数原样返给前端
    })
})

// 挂载post请求路由  请求路径：/api/other
router.post('/other', function(req, res) {
    // 获取请求体数据
    const body = req.body 
    res.send({
        code: '000000',
        msg: 'post请求成功',
        data: body
    })
})
```

#### 跨域

cors解决跨域

安装cors中间件: npm install cors

**原理：跨域的响应是被浏览器阻止的，在服务端配置响应头：Access-Control-Allow-*， 客户端识别到这种响应头就不会被拦截了**

```js
// 导入模块
const cors = require('cors')
// 在所有路由前注册cors中间件
app.use(cors())

const router = require('./router.js')

app.use('/api', router)
```



#### cors响应头类型

**Access-Control-Allow-Origin: <origin> | ***       origin的值指定了允许访问该资源的外域URL， *就是通配符，允许任何外域请求

**Access-Control-Allow-Headers  ：**  默认情况cors仅支持9种客户端请求头（Accept, Accept-Language, Content-Language, DPR, Downlink, Save-Data, Viewport-Width, Width, Content-Type (值仅限于text/plain，multipart/form-data , application/x-www-form-urlencoded 三者之一) ）

如果客户端发送额外的请求头，则服务端需要对额外请求头进行声明，否则请求会失败

**Access-Control-Allow-Methods:**   默认情况cors仅支持get,post,head请求， 如果客户端希望通过put,delete方式请求服务端，需要配置这个响应头

```js
// 设置只允许来自指定域名的请求
res.setHeader('Access-Control-Allow-Origin', 'http://xx.com')

// 声明允许客户端发送Content-Type和X-Custom-Header请求头
res.setHeader('Access-Control-Allow-Headers', 'Content-Type, X-Custom-Header')

// 声明允许的请求方式
res.setHeader('Access-Control-Allow-Methods', 'POST, GET, PUT, HEAD, DELETE') // 允许这5个方法 或*允许所有
```



#### cors请求的分类

客户端在请求CORS接口时，根据请求方式和请求头不同，可将CORS请求分为2类

**简单请求只发生一次请求，预检请求发生二次**



**简单请求**

满足以下2个条件就是简单请求

1.请求方式是GET/POST/HEAD

2.HTTP头部信息不超过cors默认支持的9个类型



**预检请求**

**预检请求概念：在浏览器和服务端正式通信前，浏览器会发送OPTION请求进行预检，以获知服务器是否允许该实际请求，这次OPTION请求被称为预检请求。服务器成功响应预检请求后，才会发送真正的请求并携带真实数据。**

符合以下条件之一，就需要执行预检请求

1.GET,POST,HEAD之外的请求类型

2.请求头中包含自定义头部字段

3.发送的数据类型是application/json



#### JSONP接口

jsonp概念：浏览器通过script标签的src属性，请求服务端数据，同时服务端返回一个函数的调用，这种请求方式就是jsonp，**jsonp仅支持get请求， 不是真正的AJAX请求，因为没有使用XMLHttpRequest对象**

实现步骤：

1. 获取客户端发送过来的回调函数名字
2. 拼接一个函数调用的字符串
3. 把字符串响应给客户端的<script>标签进行解析

```js
// 创建jsonp接口， 放在cors中间件前防止cors冲突， 放后面会被被cors处理成跨域接口
app.get('/api/jsonp', function(req, res) {
    // 获取回调函数的名字
    const funcName = req.query.callback
    // 传给回调函数的数据
    const data = { name: 'david', age: 35}
    // 发送的是一个函数调用，拼接成字符串返回给客户端
    const scriptStr = `${funcName}(${JSON.stringify(data)})`  // funcName(data) 就是这种样式
    res.send(scriptStr) // 响应给客户端的<script>标签进行解析
})

const cors = require('cors')
app.use(cors())
const router = require('./router.js')
app.use('/api', router)
```

**客户端用jquery进行jsonp接口请求**

```js
// 绑定事件触发请求
$('#button').on('click', function() {
    $.ajax({
        method: 'GET',
        url: 'http://127.0.0.1/api/jsonp',
        dataType: 'jsonp', // 必须指定为jsonp请求
        success: function (res) {
            console.log(res) // {name: 'david', age: 35}
        }
    })
})
```



### mySQL

#### 安装

mySQL sever  数据库服务软件

mySQL workbench 可视化操作工具



#### 项目中操作mySQL

1.安装mySQL模块

2.通过mySql模块连接数据库

3.通过mySql模块执行SQL语句

**安装模块**

npm install mysql

**建立数据库连接**

```js
const mysql = require('mysql')

// 建立与mysql数据库的连接
const db = mysql.createPool({
    host: '127.0.0.1', // 数据库地址
    user: 'root',
    password: 'admin123',
    database: 'my_db_01' // 要操作哪个数据库
})


// 查询users表数据
const sqlStr = 'select * from  users'
db.query(sqlStr, (err, res) => { // 执行完毕的回调函数
    // 查询失败
    if (err) console.log(err.message)
    // 查询成功
    console.log(res)
})

// 插入数据 
const user = { name: 'david', password: '123456' }
// ?表示占位符
const insertSql = 'insert into users(name, password) VALUES(?, ?)'
// 方法中按顺序传入占位符的数据， 通过affectedRows判断是否执行成功
db.query（insertSql, [user.name, user.password], (err, res) => {
    if (err) console.log('执行失败')
    if (res.affectedRows === 1) console.log('执行成功')
}）

// 更新数据
const user2 = { id:7, name: 'kashin', password: '000' }
// 也要使用?作为占位符
const updateSql = 'update users SET name=?, password=? WHERE id=?'
// 按顺序传入占位符对应数据， 备注：sql中如果只有一个占位符，可以省略[]，直接传数据
db.query(updateSql, [user2.name, user2.password, user2.id], (err, res) => {
    if (res.affecedRows === 1) console.log('成功')
})
```



### web开发模式

大致分为2种：

1.服务端渲染： 服务端发送html页面给客户端，服务端通过字符串拼接动态生成

​	优点：前端响应快，只需渲染页面，利于SEO

​	缺点：占用服务端资源，不利于前后端分离，对于前端复杂的项目开发效率低

2.前端渲染： 前端专注UI， 后端专注API

​	优点：AJAX可实现局部渲染，用户体验好， 减轻服务端渲染压力

​	缺点：不利于SEO（Vue，react的SSR能解决该问题）



### Session

**http协议是无状态性的，每次http请求都是独立的**

Cookie在身份认证中的作用：

客户端第一次请求时，服务端通过响应头的形式，向客户端发送一个身份认证的Cookie,客户端会自动将Cookie保存到浏览器中。

随后当客户端每次请求时，浏览器会自动将该Cookie通过请求头发送给服务器，服务器即可验证该客户端身份

**Cookie不具备安全性：** 浏览器发送的Cookie可能伪造，需要服务端认证通过, 见以下session认证



#### express-session中间件

```js
const express = require('express')
const app = express()

const session = require('express-session')
// 注册session中间件.传入一个配置对象
app.use(session({
    secret: 'abc123',
    resave: false,
    svaUninitialized: true
}))


// 在session上保存数据
app.post('/api/login', (req, res) => {
    if (req.body.username !== 'david' && req.body.password !== '123456') {
        return res.send({status: 1, msg: '登陆失败'})
    }
    // 配置了express-session, 才能获取到req.session属性
    req.session.user = req.body // 把用户信息保存到session中
    req.session.isLogin = true  // 把登录状态保存到session中
    res.send({stuats: 0, msg: '登录成功'})
})


// 从session上获取数据
app.get('/api/userInfo', (req, res) => {
    if (!req.session.isLogin) { // 用户未登录
        return res.send({status: 1, msg: 'fail'})
    }
})

// 清空session
app.post('/api/logout', (req, res) => {
    req.session.destroy() // 只会清空当前用户的session
    res.send({status: 0, msg: '退出登录成功'})
})
```



#### session的局限性

session基于cookie实现，cookie默认不支持跨域，当涉及前端跨域请求接口时需要很多额外配置。



### JWT

JSON Web Token：跨域认证解决方案

JWT由3部分组成：header.payload.signature      其中payload部分是真正的用户信息加密后生成的字符串

![image-20231218175505452](D:\typora-img\image-20231218175505452.png)



#### express中使用jwt

安装2个包：npm install jsonwebtoken express-jwt 

jsonwebtoken：用于生成jwt字符串

express-jwt：用于将jwt字符串解析还原成JSON对象

```js
const express = require('express')
const app = express()
const jwt = require('jsonwebtoken')
const expressJWT = require('express-jwt')

// 定义秘钥，一个字符串，用来加密和解密jwt字符串
const secretKey = 'hello david'

// 定义全局中间件，解析请求里传过来的token, 还原为json对象  配置正则：/api开头的请求不需要token认证
app.use(expressJWT({secret: secretKey}).unless({path: [/^\/api\//]}))

// 1.生成jwt
app.post('/api/login', (req, res) => {
    if (req.body.username === 'david' && req.body.password === '123456') {
        // 传入username和secretky,生成jwt字符串
        const token = jwt.sign({username: req.body.username}, secretKey, {expiresIn: '60s'})
        
        res.send({
            status: 0,
            msg: '登陆成功',
            token: token
        })
    }
})

// 2.解析jwt
app.post('/admin/userinfo', (req, res) => {
    res.send({
        msg: 'success'
        data: req.user // {username: 'david', exp: xxxxx} 只有添加了expressJWT中间件，才能在req.user属性上获取到解析后的JWT中的username信息
    })
})

app.listen(80, () ={})
```





