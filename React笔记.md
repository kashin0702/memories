### 开发依赖

React开发必须依赖3个库

**react**: react核心代码

**react-dom**: react渲染在不同平台所需要的核心代码

**babel**: 将jsx转换成react代码的工具



### 创建根组件

```react
<div id="root"></div>

// react18之前使用render方法  2个入参，1.渲染内容 2.被选择的根组件
const root = ReactDOM.render(<h2></h2>, document.querySelector('#root'))

// react18开始使用createRoot方法 分2步 1：选择根组件 2.插入要渲染的内容
const root = ReactDOM.createRoot(document.querySelector('#root'))
root.render(<h2>hello react</h2>)
```



### 组件化开发

**render函数除了传入元素，还可以传入组件, react有2种组件类型**

1.类组件

2.函数式组件



### 类组件

1.定义一个class类(类名，组件名称必须大写，小写会被认为html元素)，并且继承自**React.component**

2.实现当前组件的render内容

```react
<div id="root"></div>

class App extends React.component {
    // 组件数据
    constructor(){
        super() // 必须调用super执行父类的constructor
        this.state = { // state内定义组件的响应数据，不能定义为其他值
            message: 'hello david'
        }
        // 重点！提前进行this绑定,否则回调事件执行时，this是undefined
        this.changeText = this.changeText.bind(this)
    }
    // 组件方法
    changeText() {
        // setState继承自React.component
        // setState做2件事：1.修改state数据 2.重新执行render
        this.setState({ 
            message: 'hello gyf'
        })
    }
    // 若使用箭头函数，则不需要进行bind绑定
    changeText2 = () => {
        this.setState({
            message: 'hello gyf2'
        })
    }
    // 组件渲染内容 固定语法：必须用render返回要渲染的内容
    render() {
        return (
            // 必须有一个根元素
            <div>
            	<h2>{this.state.message}</h2>
                {/*重点！react绑定事件本质是执行了click = App.changeText,把方法赋值给了click
                此时独立执行click,this即默认绑定(普通模式this指向window)
                es6默认是严格模式，在严格模式下this=undefined*/}
                <button onClick={this.changeText}>修改内容</button>
            </div>
        )
    }
}
const root = ReactDOM.createRoot(document.querySelector('#root'))
// 渲染类组件
root.render(<App/>)
```

### setState原理

react必须通过调用setState修改数据更新页面

当setState调用时，react就会重新执行render函数刷新页面, 这也会导致若没有改变时调用了setState也会触发render函数

优化方案1：shouldComponentUpdate回调中进行判断 返回true or false

优化方案2：PureComponent

vue中，页面刷新是通过数据劫持触发的render函数 (所以vue可以通过直接赋值的方式触发页面更新)

#### 参数传入回调函数

setState除了直接传入对象，还可以传入回调函数

```react
// 好处1：可以在回调中编写新的state逻辑
// 好处2：回调函数能接收到之前的state和props参数
xxFn(){
    this.setState((prevState, props) => {
        // 编写一些对新state的处理逻辑 内聚性更强
        return {
            name: 'david' // 返回新state
        }
    })
}
```

#### setState是异步调用

1.setState设计成异步，可提升性能

因为每次调用setState,render都会执行，界面重新渲染，效率很低

所以当获取到多个更新后，进行批量更新

2.如果是同步更新state,但还没执行render，state和props就不能保持同步，会存在不一致性

```react
// 若想在设置后立即获得最新的值，可以传入第2个参数，也是一个回调函数
xxFn() {
    this.setState({name: 'david'}, () => {
        console.log('name:', this.state.name) // 这个回调就可以拿到最新的state值
    })
    console.log('name:', this.state.name) // 并不会打印最新的值
}
```



### JSX

jsx是js的一种扩展语法，也可称之为JavaScript XML， 因为看起来就像XML语法，可以和js融合在一起使用

所以React开发也叫 **All in JS**

**JSX语法本质上就是转化为了React.createElement()生成虚拟DOM**

基本语法:(**在html中使用必须在script标签上添加：type="text/babel"**)

```react
// 在js中这段代码会报错，而在jsx中这段代码会被babel进行转化
// 本质上就是转化为了React.createElement('div',null, '哈哈哈')
let element = <div>哈哈哈</div>
```

#### jsx绑定变量规则({}内的内容)

1.当变量是**number,string,array**时，可以直接显示

2.当变量是**null,undefined,Boolean**时，内容为空(要显示这些内容就需要转成string)

**3.Object对象不能作为子元素嵌入**

```react
// jsx内可以插入变量、表达式、函数等各种类型的js代码，非常自由
class App extends React.Component{
    constructor(){
        super()
        this.state = {
            message: '哈哈哈',
            age: 24,
            firstName: 'kobe',
            lastName: 'brant',
            movies: ['星际穿越', '星际探索', '蝙蝠侠']
        }
    }
    render(){
        const {message, age, firstName, lastName} = this.state
        const fullName = firstName + ' ' + lastName
        return (
            <div>
            	<div>{message}</div>
                <div>{age >= 18 ? 'men' : 'children'}</div>
                <div>{fullName}</div>
                {/*直接插入jsx代码*/}
                <ul>{this.state.movies.map(item => <li key={item}>{item}</li>)}</ul>
                {/*插入函数调用*/}
                <ul>{this.getMovie()}</ul>
            </div>
        )
    }
    getMovie(){
        return this.state.movies.map(item => <li key={item}>{item}</li>)
    }
}

```

#### jsx绑定属性

```react
class App extends React.Component{
    constructor(){
        super()
        this.state = {
            imgUrl: 'http://xxxx.com',
            href: 'http://www.baidu.com',
            isActive: true
        }
    }
    render(){
        const {imgUrl, href, isActive} = this.state
        // class绑定固定+动态样式
        // 方法1. 模板字符串拼接，动态绑定xxstyle active
        const classNames = `xxstyle ${isActive} ? 'active' : 'other'`
        // 方法2. 转成数组后添加
        const classList = ['xxstyle']
        if (isActive) classList.push('active')
        // 方法3: 第三方库classNames
        return (
        	<div>
            	<image src={imgUrl}></image>
                <a href={href}>百度一下</a>
                {/*绑定class, js中class是保留字，所以要用className*/}
                <h2 className={classNames}></h2>
                {/*因为是数组，渲染后class="xxstyle,active" 多了1个逗号，所以要用join去掉*/}
                <h2 className={classList.join(' ')}></h2>
                
                {/*绑定style 内层{}表示style对象，jsx不支持font-size写法*/}
                <h2 style={{color:'red';fontSize: '14px'}}></h2>
            </div>
        )
    }
}
```

#### jsx事件绑定(this绑定)

```react
class App extends React.Component{
    constructor(){
        super()
    }
    btnClick1() {
        this.setState({})
    }
    btnClick2 = () => {
        this.setState({})
    }
    render(){
        return (
        	<div>
                {/*方法1：直接bind绑定*/}
            	<button onClick={this.btnClick1.bind(this)}></button>
                {/*方法2：定义时使用箭头函数，无须bind绑定*/}
                <button onClick={this.btnClick2}></button>
                {/*方法3：在外层使用箭头函数包裹(推荐，方便传参)
                	箭头函数不绑定this,执行时等于执行了内部的this.btnClick()
                	this.btnClick()是隐式绑定, 所以this指向了当前实例
                */}
                <button onClick={() => this.btnClick1()}></button>
            </div>
        )
    }
}
```

#### jsx参数传递

```react
class App extends React.Component {
      constructor() {
        super()
        this.state = {
            movies: ['星际穿越', '指环王', '盗梦空间'],
            currentIndex: null
        }
      }
      // 和原生js一样，默认接收一个事件对象参数
      btnClick (event) {
        console.log('event:', event)
        console.log('this:', this)
      }
      // 接收多个参数
      btnClick2 (event, name, age) {
        console.log('event:', event)
        console.log('name, age:', name, age)
      }
      // 保存点击时的index
      movieClick (index) {
          this.setState({
              currentIndex: index
          })
      }
      render () {
        const {currentIndex} = this.state
        return (
          <div>
            {/* 不使用箭头函数，执行时默认接收event */}
            <button onClick={this.btnClick}>按钮1</button>

            {/* 使用箭头函数，直接从外层把事件对象传进去 */}
            <button onClick={(e) => this.btnClick(e)}>按钮2</button>
			{/* 传递多个参数时也一样*/}
            <button onClick={(e) => this.btnClick2(e,'david',34)}>按钮3</button>
                
             {/* 点击时触发对应元素样式改变 */}
            <ul>
            	{
                    this.movies.map((item,index) => {
                        return <li key={item}
                                   onClick={() => this.movieClick(index)}
                                   className={currentIndex === index ? 'active' : ''}>
                            {item}
                        </li>
                    })
                }
            </ul>
          </div>
        )
      }
    }
```



#### jsx条件渲染

```react
class App extends React.Component {
    constructor() {
        super()
        this.state = {
            isShow: true
            friend: null,
            showMessage: false
            message: '一些文本'
        }
    }
    render() {
        const {isShow, friend, showMessage, message} = this.state
        let showElement = null
        // 方法1：if else条件判断
        if (isShow) {
            showElement = <h2>准备好了!</h2>
        } else {
            showElement = <h2>没准备好！</h2>
        }
        
        // 切换隐藏回调事件
        btnClick () {
            this.setState({
                showMessage: !this.state.showMessage
            })
        }
        return (
        	<div>
            	<div>{showElement}</div>
                {/* 方法2: 三元运算符 */}
                <div>{isShow ? <h2>准备好了！</h2> : <h2>没准备好！</h2>}</div>
                
                {/* 方法3：逻辑与&&条件判断 当数据是网络请求时比较有用*/}
                <div>{friend && <div>{friend.name + ' ' +friend.age}</div>}</div>
                
                {/* 案例：点击按钮切换隐藏文本 */}
                <button onClick={() => this.btnClick()}>切换隐藏</button>
                <div>{showMessage && <h2>{message}</h2>}</div>
            </div>
        )
    }
}
```

#### 购物车案例

**重点：修改state要先拷贝原数据，对拷贝数据修改后，再调用setState修改原数据**

```react
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    body {
      box-sizing: border-box;
    }
    .my-table {
      border: 1px solid black;
      padding: 10px 14px;
      border-collapse: collapse;
    }
    td {
      padding: 10px 14px;
      border: 1px solid black;
      
    }
  </style>
</head>
<body>
  <div id="root"></div>
  <script src="lib/react.js"></script>
  <script src="lib/react-dom.js"></script>
  <script src="lib/babel.js"></script>


  <script type="text/babel">
    class App extends React.Component {
      constructor () {
        super()
        this.state = {
          books: [
            {name: '《计算机科学与技术》', date: '2005-10-02', author: 'xxx', price: '48', count: 0},
            {name: '《网络概论》', date: '2012-10-05', author: 'zzz', price: '55', count: 0},
            {name: '《深入浅出node》', date: '2007-12-04', author: 'yyy', price: '66', count: 0}
          ],
          rowHeads: ['书名', '出版日期', '作者', '价格', '数量', '操作'],
          totalPrice: 0
        }
      }
      formatPrice (price) {
        return '¥' + Number(price).toFixed(2)
      }
      // 价格计算
      handleCount (index, count) {
        // 重点！浅拷贝state数据 修改state前需要先拷贝原数据修改，再使用setState修改
        const list = [...this.state.books]
        list[index].count += count
        this.setState({
          books: list
        })
      }
      // 数组归并算总价
      getTotalPrice () {
        return this.state.books.reduce((prev, cur) => {
          return prev + cur.count * cur.price
        }, 0)
      }
      removeItem (index) {
        const list = [...this.state.books]
        list.splice(index, 1)
        this.setState({
          books: list
        })
      }
      render () {
        if (this.state.books.length > 0) {
          return (
            <div>
              <table className={'my-table'}>
                <thead>
                    <tr>
                      {this.state.rowHeads.map(item => <th key={item}>{item}</th>)}
                    </tr>
                </thead>
                <tbody>
                  {this.state.books.map((item, index) => {
                    return (
                      <tr key={index}>
                        <td>{item.name}</td>
                        <td>{item.date}</td>
                        <td>{item.author}</td>
                        <td>{this.formatPrice(item.price)}</td>
                        <td>
                          <button disabled={item.count === 0 ? true : false} onClick={() => this.handleCount(index, -1)}>-1</button>
                            {item.count}
                          <button onClick={() => this.handleCount(index, 1)}>+1</button>
                        </td>
                        <td>
                          <button onClick={() => this.removeItem(index)}>删除</button>
                          </td>
                      </tr>
                    )
                  })}
                </tbody>
              </table>
              <h2>总价: {this.formatPrice(this.getTotalPrice())}</h2>
            </div>
          ) 
        } else {
          return <h2>购物车为空~</h2>
        }
      }
    }

    const root = ReactDOM.createRoot(document.querySelector('#root'))
    root.render(<App/>)
  </script>
</body>
</html>
```



### React脚手架

官方脚手架: create-react-app(CRA)

1. 安装脚手架 **npm install create-react-app -g** (全局安装)
2. cd到要创建的目录执行：**create-react-app 项目名称**  （项目名称不能有大写）

项目运行: 和vue一样，react也是通过npm进行包管理，所以到**package.json**文件查看运行命令

一般是npm run start

#### react项目文件解释

public

​	favicon.ico //应用程序顶部的icon图标

​	index.html //应用的index.html入口文件

​	manifest.json //web App相关配置(一般用在移动端配置成桌面app图标)

#### PWA是什么

**pwa表示渐进式web应用**，  一般用在移动端，当使用手机打开该web，可以把网站配置桌面成图标，增加离线缓存等功能， 这种类似native App的形式也称为**web App**

脚手架创建的react工程，一般配置了pwa相关的文件(manifest.json, serviceWorker.js)

#### webpack配置查看

react脚手架默认隐藏webpack配置，如何查看react项目的webpack配置

1.npm run start执行的是react-scripts 文件 ，所以配置在node_modules/react-scripts/config/webpack.config.js查看

2.npm run eject：弹出webpack的配置到项目目录下, 会生成config和scripts文件

#### 安装craco(webpack配置工具)

因为脚手架创建的工程隐藏了webpack配置，可以通过安装craco对webpack进行配置

**npm install @craco/craco@alpha** (支持cra5.0的最新版本)

**安装craco后，package.json中的script命令必须改成craco start | craco build,  craco配置才会生效**

#### craco-less

less样式文件配置工具（类似less-loader配置）

**npm install craco-less@alpha**



### 组件类型

根据组件内部是否有状态需要维护分为：1.无状态组件 2.有状态组件

根据组件的不同职责分为： 1.展示型组件 2.容器型组件 

根据定义方式分为：**1.函数式组件** **2.类组件**

函数组件特点(不使用hooks情况下)：

**1.没有生命周期(hooks模拟生命周期) 2.没有this关键字指向组件实例 3.没有内部状态**

一般情况下，函数式组件都是无状态组件，类组件都是有状态组件(维护state)

(函数式组件也可以通过hooks维护状态)

```js
// 函数组件
function App() {}
// 类组件
class App extends React.Component {}
```

#### 函数组件传参

```react
// 接收参数
function MyCpn(props) {
    return <h2>{props.message}</h2>
}
export default MyCpn

// 父组件调用即传参
import MyCpn from './MyCpn'
class App extends PureComponent{
    constructor(){
        super()
        this.state = {
            message: 'hello world'
        }
    }
    render() {
        const {message} = this.state
        return (
            <div>
                <MyCpn message={message}></MyCpn>
            </div>
        )
    }
}


```



### render函数可以返回的类型

类组件render的返回值和函数式组件的返回值是一致的

```react
function HelloWorld() {
    return <h2>hello</h2>
}
class App extends React.Component {
    // 伪代码:列举render可以返回的类型
    render() {
        //1.返回一个组件
        return <HelloWolrd/> 
        // 2.返回jsx对象，本质是React.createElement元素
        return <h2>hello david</h2> 
        // 3.返回数组
        return [
            <div>元素1</div>,
            <div>元素2</div>,
            <div>元素3</div>
        ]
        // 4.字符串或数字类型
        return 'david' 
    }
}
```

### 生命周期函数

![image-20221017145449532](C:\Users\yoki\Desktop\memories\assets\image-20221017145449532.png)

React内部为了告诉我们当前处于哪个阶段，会对组件内部实现的某些函数进行回调，这些函数就是生命周期函数

**componentDidMount**函数：组件已经挂载的DOM上时，就会回调

**componentDidUpdate**函数：组件已经发生了更新时，就会回调

**componentWillUnmount**函数：组件即将被移除时，就会回调

```react
class App extends React.Component {
    render () {}
    // 组件挂载后执行的生命周期
    componentDidMount () {
        // 依赖DOM的操作、网络请求、添加订阅(如添加事件总线监听，必须在componentWillUnmount取消订阅)
    }

    // 组件更新后回调，该回调传3个参数进来(更新前的Props,state,snapshot, snapshot是getSnapshotBeforeUpdate回调函数的返回值,会作为第3个参数传进来)
    componentDidUpdate (prevProps, prevState, snapshot) {}
    
    // 组件即将卸载前的回调
    componentWillUnmount () {
        // 清除timer、取消网络请求、清除componentDidMount中创建的订阅
    }
    
    // 当发生更新后，根据返回值控制render函数是否重新执行
    shouldCoponentUpdate () {
        return true || false
    }
    
    // DOM更新前的一个回调函数，可以获取DOM更新前的一些信息(如滚动条位置scrollPosition)
    getSnapshotBeforeUpdate () {
        return something //该返回值会传给update回调函数的第3个参数
    }
}
```

### super(props)传参问题

为了让 React.Component 构造函数能够初始化 this.props，将 props 传入 super 是必须的：

```javascript
// React 內部
class Component {
  constructor(props) {
    this.props = props;
    // ...
  }
}
```

这几乎就是真相了 — 确然，它是 这样做 的。

但有些扑朔迷离的是，即便你调用 super() 的时候没有传入 props，你依然能够在 render 函数或其他方法中访问到 this.props。（如果你质疑这个机制，尝试一下即可）

那么这是怎么做到的呢？事实证明，React 在调用构造函数后也立即将 props 赋值到了实例上

```javascript
// React 内部
const instance = new YourComponent(props);
instance.props = props;
```

因此即便你忘记了将 props 传给 super()，React 也仍然会在之后将它定义到实例上。这么做是有原因的。

当 React 增加了对类的支持时，不仅仅是为了服务于 ES6。其目标是尽可能广泛地支持类抽象。当时我们 不清楚 `ClojureScript，CoffeeScript，ES6，Fable，Scala.js，TypeScript` 等解決方案是如何成功的实践组件定义的。因而 React 刻意地没有显式要求调用 super() —— 即便 ES6 自身就包含这个机制。

这意味着你能够用 super() 代替 super(props) 吗？

最好不要，毕竟这样写在逻辑上并不明确确然，React 会在构造函数执行完毕之后给 this.props 赋值。但如此为之会使得 this.props 在 super 调用一直到构造函数结束期间值为 undefined。

```javascript
// React 內部
class Component {
  constructor(props) {
    this.props = props;
    // ...
  }
}
// 你的程式碼內部
class Button extends React.Component {
  constructor(props) {
    super(); // ? 我们忘了传入 props
    console.log(props);      // ✅ {}
    console.log(this.props); // ? 未定义
  }
  // ...
}
```

如果在构造函数中调用了其他的内部方法，那么一旦出错这会使得调试过程阻力更大。这就是我建议开发者一定执行 super(props) 的原因，即使理论上这并非必要：

```javascript
class Button extends React.Component {
  constructor(props) {
    super(props); // ✅ 传入 props
    console.log(props);      // ✅ {}
    console.log(this.props); // ✅ {}
  }
  // ...
}
```

确保了 this.props 在构造函数执行完毕之前已被赋值。

最后，还有一点是 React 爱好者长期以来的好奇之处。

你会发现当你在类中使用 Context API （无论是旧版的 contextTypes 或是在 React 16.6 更新的新版 contextTypes）的时候，context 是作为第二个参数传入构造函数的。

那么为什么我们不能转而写成 `super(props, context)` 呢？我们当然可以，但 context 的使用频率较低，因而并没有掘这个坑。



### 组件通信

父传子： 

1.父组件通过**属性=值**传递给子组件， 子组件通过**props**获取父组件传递过来的数据

2.解构传递{...obj}

子传父:    通过回调函数实现

```react
// 父传子
class Father extends Component {
    constructor(){
        super()
        this.state = {
            obj: {name:'david', age:34}
        }
    }
    render(){
        const {name} = obj
        return (
            // 传递单个数据
        	<Children name={name} />
            // 传递整个对象时，可以使用解构语法 子组件通过this.props获得整个对象
            <Children {...obj} />
        )
    }
}

// 传递一个对象
class Children extends Component {
    // 子组件通过constructor函数接收父组件数据
    //(若不需要维护state,constructor可以不写，默认会执行super(props))
    constructor(props){
        super(props) //同时执行super(props), 把props保存到实例中
    }
    render () {
        const {name} = this.props // 解构获得父组件传过来的name
    }
}
-----------------------------------
// 子传父 父组件
class Father extends Component {
    btnClick(count) {
        console.log(count)
    }
    render() {
        return (
        	<div>
                {/*把函数作为Props传给子组件, 接收子组件回调时传入参数count*/}
            	<Children btnClick={(count) => this.btnClick(count)}></Children>
            </div>
        )
    }
}
// 子组件
class Children extends Component {
    handleClick(count) {
        // 执行父组件传过来的回调函数，传入参数
        this.props.btnClick(count)
    }
    render() {
        <div>
        	<button onClick={() => this.handleClick(10)}>+10</button>
        </div>
    }
}
```



### 组件prop类型校验

通过propTypes进行类型校验

```react
// 导入类型校验模块
import PropTypes from 'prop-types'

class Cpn extends Component {
    render () {
        const {title, list} = this.props
    }
}
// 增加prop类型校验
Cpn.propTypes = {
    title: PropTypes.string, 
    list: PropTypes.array.isRequired // 必传prop
}
// 默认值
Cpn.defaultProps = {
    title: '默认标题',
    list: []
}
```



### React实现插槽

react官方没有插槽的说法，但可以实现类似vue插槽的功能

有2种方法

1.直接在子组件中插入元素，会自动被传入到子元素的**props.children**属性中

```react
// 父组件
<Navbar1>
{/* 多个子元素就是多个插槽，会被放到子组件的props.children中 */}
  <button>我是button</button>
  <span>我是span</span>
  <i>我是斜体</i>
</Navbar1>

// 子组件Navbar
export class Navbar1 extends Component {
  render() {
 // 父组件传过来的插槽，会被传到props.children中, 如果children只有1个就是对象，有1个以上就是数组
    const {children} = this.props
    return (
      <div className='nav-bar'>
        <div className="left">{children[0]}</div>
        <div className="center">{children[1]}</div>
        <div className="right">{children[2]}</div>
      </div>
    )
  }
}
```

2.直接通过props传递(推荐)

```react


// 父组件
<Navbar2 
    leftSlot={<button>按钮</button>} 
    rightSlot={<span>文本</span>}
    // 传递回调函数，实现作用域插槽 父组件甚至可以通过判断item的类型返回不同的组件!
    centerSlot={item => <button>{item}</button>}
/>
// 根据类型返回不同的组件，直接在组件里替换这个方法
getItemType (item) {
    if (item === 'XXX') return <span>{item}</span>
    if (item === 'AAA') return <strong>{item}</strong>
}

// 子组件Navbar
export class Navbar2 extends Component {
  render() {
    const {leftSlot, rightSlot, centerSlot} = this.props
    const data = 'Scope Data'
    return (
      <div className='nav-bar'>
        <div className="left">{leftSlot}</div>
            {/* 作用域插槽 通过回调实现 */}
        <div className="center">{centerSlot(data)}</div>
        <div className="right">{rightSlot}</div>
      </div>
    )
  }
}
```

### 非父子组件通信的三种方式

1.Context

2.EventBus

3.Redux

### 跨组件数据通信(Context)

1. 通过**React.createContext**创建context
2. 父组件使用**context.provider**包裹子组件给后代提供数据
3. 子组件挂载**contextType** 指定要引用的context
4. 最后子组件就可以通过this.context拿到数据

```react
// 公共文件定义context
import React from 'react'
const MyContext = React.createContext()
export default MyContext // 创建上下文对象

// 父组件
import MyContext from './context.js'
export class App extends Component {
    constructor(){
        super()
        this.state = {
            obj: {name: '', age: ''}
        }
    }
	render() {
        const {obj} = this.state
        return (
        	<div>
      {/* 通过MyConetxt包裹子组件，就可以跨组件传递数据,数据使用value进行传递 */}
            	<MyContext.Provider value={{commonData: '123'}}>
                	<Children {...obj}/>
                </MyContext.Provider>
            </div>
        )
    }    
}
// 子组件
import MyContext from './context.js'
// 关键：可能存在多个context,所以指定要引用的context
Children.contextType = MyContext 

export class Children extends Component {
    render(){
        console.log(this.context) // 此时就可以拿到父组件传递的context
    }
}
```

#### 函数式组件使用context

函数组件通过**Consumer**获取context数据

```react
import MyContext from './context.js'
function Children2() {
    return (
    	<div>
            {/* value通过回调函数的形式传进来 */}
            <MyContext.Consumer>
                {
                    value => {
                        return <h2>{value.commonData}</h2>
                    }
                }
            </MyContext.Consumer>
        </div>
    )
}
```

#### 多个Context嵌套

```jsx
<MyContext.Provider value={{commonData: '123'}}>
    <OtherContext.Provider value={{otherData: 'aaa'}}>
    	<Children {...obj}/>
    </OtherContext.Provider>
</MyContext.Provider>

// 子组件
import MyContext from './context.js'
import OtherContext from './other.js'

export class Children extends Component {
    render(){
        console.log(this.context)
        return (
        	<div>
                {/* 通过Consumer获取otherContext的数据 value通过回调函数传进来*/}
            	<MyContext.Consumer>
                    {
                        value => {
                            return (
                            	<div>
                                	<h2>{value.commonData}</h2>
                                    {/*回调函数中嵌套回调函数,获取context*/}
                                    <OtherContext.Consumer>
                                        {value => <h2>value.otherData</h2>}
                                    </OtherContext.Consumer>
                                </div>
                            )
                        }
                    }
                </MyContext.Consumer>
            </div>
        )
    }
}
// 关键：一个子组件只能指定一个contextType, 其他context就通过Consumer获取
Children.contextType = MyContext 
```



### 事件总线

一般使用第三方库实现(使用较多库的是**events**)

个人理解：

事件总线的本质其实也是发布订阅模式

创建一个全局事件对象实例 **const eventBus = new EventBus()**

例如：当触发某btnClick时绑定事件总线btnClick={eventBus.emit(//code)}

1.**eventBus.emit('xxFn', value1, value2)**  在eventBus实例上注册一个事件，并发送要传递的数据value1,value2

2.在任意组件导入eventBus，比如在componentDidMount生命周期中，通过**eventBus.on('xxFn', (value1, value2) => {//接收value1,value的回调函数})** //监听xxFn, 并传如一个回调函数，当emit事件执行时，也就会执行该回调函数

**注意：若组件会被销毁，必须要在组件销毁的生命周期内移除监听的回调，不然可能会导致内存溢出**

```react
// 把回调函数单独抽出来方便移除
callback(value1,value2){
    console.log(value1, value2)
    console.log(this)
},
// 绑定监听
componentDidMount() {
    eventBus.on('xxFn', this.callback.bind(this)) // 传函数进去要绑定this,否则内部this不正确
}
// 移除监听
componentWillUnMount() {
	eventBus.off('xxFn', this.callback)
}
```



### render性能优化(SCU优化)

当setState内的数据没有发生改变时，render也会执行。

比如父组件更新了数据，但子组件没有更新，此时子组件也会执行render, 可以对其进行性能优化

```react
// 利用shouldComponentUpdate(SCU)生命周期函数优化render执行 （默认返回true）
shouldComponentUpdate(nextProps, nextState) {// 该生命周期有2个参数，最新的state和props
    // 之前的值和最新的值不相等时，返回true,执行render, 否则返回false不执行render
    if (this.state.counter !== nextState.counter) {
        return true 
    }
    return false
}
```

#### 类组件SCU优化(PureComponent)

让**类组件**继承PureComponent就能实现上面的SCU优化，react默认帮我们处理了render执行优化

**PureComponent只比较第一层数据(浅层比较)**

```react
import React, {PureComponent} from 'react'
class App extends PureComponent {
    
}
```

#### 函数组件SCU优化(memo)

函数组件没有维护状态，没有继承，所以不能通过PureComponen优化

通过react提供的memo方法包裹函数组件，该方法会返回一个function, 再导出即可优化

```react
import {memo} from 'react'
const cpn1 = memo(function(props) {
    return <h2>{props.message}</h2>
})
export default cpn1
```

#### PureComponent源码

![image-20221101141720292](C:\Users\yoki\Desktop\memories\assets\image-20221101141720292.png)

### 不可变数据(state)

```jsx
class App extends PureComponent {
    constructor(){
        super()
        this.state = {
            books: [
                {name: 'vue高级设计', price: 88, count: 1},
                {name: 'react高级设计', price: 77, count: 3}
            ]
        }
    }
    addNewBook() {
        let newBook = {name: 'angular高级设计', price: 66, count: 1}
        // 如果直接在state上修改对象类型数据，在PureComponent内不会重新渲染
        // 因为PureComponent内会用新的state和旧state比较，但是setState设置的还是原来的books，PureComponent只比较第一层对象，所以页面不会重新渲染
        this.state.books.push(newBook)
        this.setState({books: this.state.books})
        // 正确做法：永远不要直接修改state中的对象类型数据，先浅拷贝state内的数据,修改浅拷贝数据后setState
        // 原理：浅拷贝后，数据的第一层对象不一样(内存地址)，所以render会重新渲染
        let books = [...this.state.books]
        books.push(newBook)
        this.setState({books})
    }
    render() {
        const {books} = this.state
        return (
        	<ul>
            	{books.map(item => <li>{item.name}-{item.price}</li>)}
            </ul>
        )
    }
}
```



### ref操作DOM

react中有3种ref方式可以操作DOM

```jsx
import React, {PureComponent, createRef} from 'react'
class App extends PureComponent{
    constructor(){
        super()
        this.state = {}
        // 方法2：createRef创建ref（官方推荐）
        this.title2 = createRef()
        // 方法3
        this.title3 = null
    }
    getDOM() {
        // 1.refs属性获取
        console.log(this.refs.title)
        // 2.current属性获取
        console.log(this.title2.current) 
        // 3.ref传入回调函数获得
        console.log(this.title3)
    }
    render() {
        return (
        	<div>
                {/*方法1：通过ref属性赋值获取*/}
            	<h2 ref="title">我是标题</h2>
                {/*方法2：通过引入函数createRef创建*/}
                <h2 ref={this.title2}>我是标题2</h2>
             {/*方法3：通过给ref传入回调函数获得，参数el就是ref, render执行时会自动执行该回调函数*/}
                <h2 ref={el => this.title3 = el}>我是标题3</h2>
                <button onClick={() => this.getDOM()}>获取DOM</button>
            </div>
        )
    }
}
```



### ref获取类组件实例

```jsx
import {PureComponent, CreateRef} from 'react'
class Home extends PureComponent{
    constructor(){
        super()
    }
    test(){
        console.log('123')
    }
}
class App extends PureComponent{
    constructor(){
        super()
        // 创建一个ref对象
        this.homeRef = createRef()
    }
    getComponent(){
        // 通过ref.current调用子组件的test方法
        this.homeRef.current.test()
    }
    render() {
        return (
        	<div>
                {/* 绑定ref给子组件实例*/}
            	<Home ref={this.homeRef}></Home>
                <button onClick={() => this.getComponent()}>调用子组件方法</button>
            </div>
        )
    }
}
```

### ref获取函数组件(forwardRef)

函数组件没有实例，所以不能直接绑定ref获取实例, 需要通过**forwardRef**函数进行绑定

**本质是通过forwardRef函数做了一个ref的转发，也就是高阶组件**

```jsx
import {forwardRef} from 'react'
// 回调函数内接收props,ref两个参数，其中ref就是父组件传进去的ref
const Home = forwardRef(function(props, ref){
    return (
    	<div>
            {/*函数组件的ref只能单独绑定在返回的元素上*/}
        	<h2 ref={ref}>我是标题</h2>
        </div>
    )
})
// 父组件
class App extends PureComponent{
    constructor(){
        super()
        this.homeRef = createRef()
    }
    render(){
        return (
        	<div>
            	<Home ref={this.homeRef}></Home>
            </div>
        )
    }
}
```



### 受控组件

react中的表单类元素如果绑定了value, 就属于受控组件(input, checkbox, select)等

当绑定value属性时, 必须同时绑定change事件，否则内容是不会被修改的

```jsx
class App extends PureComponent{
    constructor(){
        super()
        this.state = {
            userName: ''
        }
    }
    inputChange(event) {
        // 通过事件修改input的value
        this.setState({
            userName: event.target.value
        })
    }
    render() {
        const {userName} = this.state
        return (
        	<div>
                {/*绑定value后必须同时绑定事件，否则受控组件的内容不会被修改*/}
            	<input value={userName} onChange={(e) => this.inputChange(e)}></input>
                {/* 不绑定value,非受控组件*/}
                <input type='text'/></input>
            </div>
        )
    }
}
```



#### 受控组件的form表单提交

![image-20221102165646594](C:\Users\yoki\Desktop\memories\assets\image-20221102165646594.png)

```jsx
import React, { PureComponent } from 'react'

class InputCpn extends PureComponent {
  constructor(){
    super()
    this.state = {
      username: '',
      password: '',
      isChecked: false,
      checks: [
        {value: '1', label: '游泳', isChecked: false},
        {value: '2', label: '健身', isChecked: true},
        {value: '3', label: '摔跤', isChecked: false}
      ],
      fruit: 'banana',
      fruit2: []
    }
  }
  handleSubmit(event) {
    event.preventDefault() // 阻止默认的表单提交行为
  }
  handleInput(event) {
    // 根据event.target.name动态设置input的内容
    const key = event.target.name
    this.setState({
      [key]: event.target.value
    })
  }
  handleCheck(event) {
      this.setState({
          isChecked: event.target.checked // 拿的是event.target.checked属性
      })
  }
    
  handleMultiCheck(event, index) {
    // 浅拷贝state数据
    const checks = [...this.state.checks]
    checks[index].isChecked = event.target.checked
    this.setState({ checks })
  }
    
  handleSelect(event) {
    this.setState({fruit: event.target.value})
  }
    
  handleSelectMultiple(event) {
    console.log('selectedOptions===>', event.target.selectedOptions)
    const options = event.target.selectedOptions 
    // selectedOptions是一个类数组，需要转化后才能使用数组方法
    const arr = Array.from(options) 
    const values = arr.map(item => item.value)
    this.setState({
      fruit2: values
    }, () => {
      console.log(this.state.fruit2)
    })
  }
  render() {
    const {username,password,isChecked} = this.state
    return (
      <div>
        {/* 原生表单提交监听事件onSubmit */}
        <form onSubmit={(e) => this.handleSubmit(e)}>
          {/* 1.表单默认方式提交时(action提交)必须绑定name属性  2.受控组件必须绑定onChange事件*/}
            账号:
          <input type="text" name='username'
              value={username} 
              onChange={(e) => this.handleInput(e)} 
          />
            密码:
          <input type="password" name='password'
              value={password} 
              onChange={(e) => this.handleInput(e)} 
          />
          <h2>多选框测试</h2>
          <input type="checkbox" 
              checked={isChecked}
              onChange={e => this.handleCheck(e)}
          />XX协议
            
            <div>多选列表</div>
          {/* 先定义or获取数据，用map函数渲染 */}
          {
            this.state.checks.map((item, index) => {
              return (
                <label htmlFor={item.value} key={item.value}>
                  <input type='checkbox'
                         id={item.value}
                         checked={item.isChecked}
                         onChange={e => this.handleMultiCheck(e, index)} 
                  />
                  {item.label}
                </label>
              )
            })
          }
          <h2>select下拉控件</h2>
          <select value={this.state.fruit} onChange={(e) => this.handleSelect(e)}>
            <option value="apple">苹果</option>
            <option value="banana">香蕉</option>
            <option value="pear">梨</option>
          </select>
          <h2>select多选情况</h2>
          <select 
              value={this.state.fruit2} 
              onChange={(e) => this.handleSelectMultiple(e)}
              multiple
          >
            <option value="apple">苹果</option>
            <option value="banana">香蕉</option>
            <option value="pear">梨</option>
          </select>
          {/* button type设置submit类型 */}
          <button type='submit'>表单提交</button>
        </form>
      </div>
    )
  }
}
```



### 非受控组件

不绑定value的就是非受控组件，通过设置**defaultValue、defaultChecked**可以绑定默认值

```jsx
class App extends PureComponent{
    constructor(){
        super()
        this.state = {
            message: '哈哈哈'
        }
        this.inputRef = createRef()
    }
    handleClick() {
        // 获取非受控组件的值
        console.log(this.inputRef.current.value)
    }
    render() {
        const {message} = this.state
        return (
            {/*defaultValue可以给input绑定一个默认值，且不变成受控组件*/}
        	<div>
            	<input type="text" defaultValue={message} ref={this.inputRef} />
                <button onClick={e => this.handeClick(e)}>获取结果</button>
            </div>
        )
    }
}
```



### 高阶组件(设计模式)

高阶组件是一种设计模式，本身是一个函数，接收组件作为参数，返回一个新组件

高阶组件的目的：对组件拦截，返回的新组件可以对原组件进行扩展和增强

```jsx
function hoc(Cpn) {
    class newCpn extends PureComponent{
        constructor(){
            super()
            this.state = {
                userInfo: {
                    name:'david',
                    age: '34'
                }
            }
        }
        render(){
            // 拦截组件，传入新的属性
            return <Cpn {...this.state.userInfo}/>
        }
    }
    return newCpn
}
class Home extends PureComponent{
    render(){
        const {name, age} = this.props.userInfo
        // 获得注入属性
        return <div>{name}-{age}哈哈哈</div>
    }
}
class App extends PureComponent{
    render() {
        // 使用高阶组件拦截Home组件，返回一个处理后的新组件
        const newHome = hoc(Home)
        return (
        	<newHome/>
        )
    }
}
```

#### 高阶组件应用1

```jsx
//拦截组件，导入通用数据
function enhancedCpn(OriginComponent) {
    class newCpn extends PureComponent {
        constructor(){
            super()
            this.state = {
                message: 'some message'
            }
        }
        render() {
            return <OriginComponent {...this.state}/>
        }
    }
    return newCpn
}

// 直接把函数式组件放到高阶组件中，返回的新组件即携带了通用数据,通过props传了进来
const Home = enhancedCpn(function(props) {
    return <div>Home: {props.message}</div>
})
const Profile = enhancedCpn(function(props) {
    return <div>Profile: {props.message}</div>
})
const Other = enhancedCpn(function(props) {
    return <div>Other: {props.message}</div>
})
// 父组件App使用增强后的组件
class App extends PureComponent {
    render() {
        return (
        	<div>
            	<Home/>
                <Profile/>
                <Other/>
            </div>
        )
    }
}
```



#### 高阶组件应用2

```jsx
// 父组件
import MyContext from './MyContext'
import Home from './Home'
import Other from './Other'
class App extends PureComponent{
    render(){
        return (
        	<MyContext.provider value={{name: 'david'}}>
            	<Home/>
				<Other/>
            </MyContext.provider>
        )
    }
}

// 子组件文件夹
// 定义高阶组件
import MyContext from './MyContext'
function hoc(originComponent) {
    return props => {
        return (
        	<MyContext.consumer>
            	{
                    // 给传入的组件注入props和value
                    (value) => <originComponent {...props} {...value}/>
                }
            </MyContext.consumer>
        )
    }
}
class Home extends PureComponent {
    render() {
        return <h2>接收高阶组件传的数据：{this.props.name}</h2>
    }
}
// 导出用hoc处理过的home,能直接获得context传入的数据
export default hoc(Home)
```





### Portals

作用：将元素挂载到根组件之外的指定元素上。

接收2个参数，1.任意可渲染的react子元素 2.要插入的DOM元素

```jsx
// index.html入口文件 定义一个非root元素, portal创建的元素会挂载到这里
<div id="david"></div>

import { PureComponent } from 'react'
import {createPortal} from 'react-dom'
export class Modal extends PureComponent {
  render() {
    // 获取插入的子元素，并插入到#david的DOM元素上，createPortal返回一个虚拟dom
    return createPortal(this.props.children, document.querySelector('#david'))
  }
}
export default Modal

//父元素
export class App extends PureComponent {
    render() {
        return (
        	<Modal>
            	<h2>我是modal子元素</h2>
            </Modal>
        )
    }
}
```



### Fragment

作用：不想render函数包裹根节点时，可以使用Fragment，渲染后就不存在div这样的根节点

和vue中的template标签类似

```jsx
import {PureComponent, Fragment} from 'react'
export class App extends PureComponent {
    render() {
        return (
        	<Fragment>
            	<h2>我是标题</h2>
                <p>我是内容</p>
            </Fragment>
        )
    }
}
// 语法糖写法：直接用空标签 (如果是遍历需要绑定key,不能使用该写法)
render() {
    return (
        <>
            <h2>我是标题</h2>
            <p>我是内容</p>
        </>
    )
}
```



### StrictMode严格模式

1.不会渲染任何可见UI

2.为后代元素触发额外的检查和警告(使用过时的API或生命周期函数时)

3.严格模式仅在开发模式下运行，不影响生产环境构建

**4.constructor和render会被执行2次，看看是否有副作用（仅开发模式）**

```jsx
import {PureComponent, Fragment, StrictMode} from 'react'
export class App extends PureComponent {
    render() {
        return (
            {/*被StrictMode包裹的组件即进入严格模式*/}
            <StrictMode>
            	<Home/>
            </StrictMode>
            <Other/>
        )
    }
}
```



### react-transition-group 过渡动画的第三方库

实现组件显示或消失的过渡动画

安装：**npm install react-transition-group --save**

该库主要包含4个组件

**Transition**：和平台无关的组件(不一定要结合CSS)

**CSSTransition**: 前端开发中常用

**SwitchTransition**: 两个组件显示和隐藏切换时使用

**TransitionGroup**: 多个动画组件包裹进去，一般用于列表中元素的动画



**CSSTransition 有3个状态:  appear|enter|exit**

3个状态需要定义对应的CSS样式

开始状态：-appear | -enter |-exit

执行动画：-appear-active |  -enter-active | -exit-active

结束动画：-appear-done | -enter-done | -exit-done

```css
/* style.css */
/* 首次进入动画 */
.david-appear {
  transform: translateX(-200px);
}
.david-appear-active {
  transform: translateX(0);
  transition: transform 2s ease;
}
/* 进入动画 */
.david-enter {
  opacity: 0;
}
.david-enter-active {
  opacity: 1;
  transition: opacity 2s ease;
}

/* 退出动画 */
.david-exit {
  opacity: 1;
}
.david-exit-active {
  opacity: 0;
  transition: opacity 2s ease;
}
/* switch切换动画 */
.switch-enter {
  transform: translateX(100px);
  opacity: 0;
}
.switch-enter-active {
  transform: translateX(0);
  opacity: 1;
  transition: all 1s ease;
}
.switch-exit {
  transform: translateX(0);
  opacity: 1;
}
.switch-exit-active {
  transform: translateX(-100px);
  opacity: 0;
  transition: all 1s ease;
}
```

```jsx
import React, { PureComponent } from 'react'
import {CSSTransition} from 'react-transition-group'
import './style.css' // 引入样式文件
export class Animation extends PureComponent {
  constructor() {
    super()
    this.state = {
      isShow: true
    }
  }
  render() {
    const {isShow} = this.state
    return (
      <div>
        <button onClick={() => this.setState({isShow: !isShow})}>显示or隐藏</button>
        {/* in: 控制显示和隐藏 unmountOnExit: 动画退出后卸载组件 classNames：要绑定的class类 timeout: 动画执行时间 appear: 首次进入动画 in属性必须同时为true*/}
        <CSSTransition 
            in={isShow} 
            unmountOnExit={true} 
            classNames='david' 
            timeout={2000} 
            appear
         >
          <h2>动画效果测试</h2>
        </CSSTransition>
            
        {/* switchTransition测试  mode: 动画模式 out-in 先离开，后进入*/}
        <SwitchTransition mode='out-in'>
          <CSSTransition 
              {/*必须有2个不一样的key,当key改变时才会在2个内容之间执行切换动画*/}
              key={isLogin ? 'exit' : 'login'} 
              classNames="switch" 
              timeout={1000}
          >
            <button 
            onClick={() => this.setState({isLogin: !isLogin})}>{isLogin ? '退出' : '登录'}			  </button>
          </CSSTransition>
        </SwitchTransition>
            
        {/* TransitionGroup测试 component：把包裹的TransitionGroup替换为指定的元素，默认是div */}
        <TransitionGroup component='ul'>
          {
            this.state.books.map((item, index) => {
              return (
                // 必须绑定唯一key才能正确的显示动画,特别是删除操作的时候
                <CSSTransition key={item.id} classNames='books' timeout={1000}>
                  <li>{item.name}-{item.price}</li>
                </CSSTransition>
              )
            })
          }
        </TransitionGroup>
        <button onClick={() => this.addBook()}>添加书籍</button>
      </div>
    )
  }
}
```



### CSS in React解决方案

#### 内联样式

style接收小驼峰命名属性的JS对象，不是CSS字符串，并且可以引用state中的状态来设置样式

#### 独立css文件，import引入

缺点：Import引入的普通css文件都是全局的，不管是父子组件还是兄弟组件，都会互相覆盖

#### CSS module (基于webpack配置环境)

react脚手架已内置了css module配置，只需把**.css/.scss文件改成.module.css/.module.scss等**

缺点：不支持my-title这种连字符写法，不支持动态样式

```css
/*app.module.css文件*/
.title {
    color: red;
    font-size: 20px;
}
```

```jsx
import appstyle from './app.module.css'

export class App extends PureComponent {
    render() {
        return <h2 className={appstyle.title}>我是标题</h2>
    }
}
```



#### CSS in JS (推荐)

优点：没有样式冲突、非常灵活、可以插入state动态值

利用第三方库实现, 目前流行的库： styled-components / emotion / glamorous

**npm install styled-components**

PS：vscode也要安装styled-components插件，编写时会有高亮提示

##### 第三方库styled-components

```jsx
// style.js
import styled from 'styled-components'
import {smallSize} from './variables.js' // 导入全局公共变量

// es6的模板字符串调用函数方式style.div``本质就是styled.div()，会返回一个组件
// 对这个函数传参，传递的内容就是它包裹的元素的css样式
// 获取动态值，通过props回调函数获得
export const wrapper = styled.div`
	.section {
		.title {
			color:blue;
			border: 1px solid #fff;
			font-size: ${smallSize}px;
		}
		.content {
			color: ${props=> props.color};
			font-size: ${props => props.size}px
		}
	}
` 
// 通过attrs给标签模板字符串提供属性
// attrs可以传对象也可以传函数，传入的函数用于判断是否传了color,没有则设置为blue
export const otherWrapper = styled.div.attrs(props => {
    return {
        textColor: props.color || 'blue'
    }
})`
	.title {
		color: ${props => props.textColor}
}`

// App.jsx 导入wrapper
import {wrapper} from './style.js'
export class App extends PureComponent {
    constructor() {
        super()
        this.state = {
            color: 'purple',
			size: 20            
        }
    }
    render() {
        return (
            {/* 1.用wrapper包裹要修改样式的内容*/}
            {/* 2.因为wrapper本身也是组件，所以可直接对wrapper传参，使用state中的动态值*/}
        	<wrapper color={this.state.color} size={this.state.size}>
            	<div className="section">
                	<h2 className="title">标题</h2>
                    <div className="content">内容</div>
                    <div className="foot">尾部</div>
                </div>
            </wrapper>
        )
    }
}

// variables.js 定义全局css变量
const smallSize = 16
const middleSize = 18
const largeSize = 20
export {
	smallSize,
    middleSize,
    largeSize
}
```

##### 样式继承

通用样式可以用继承实现

```js
import styled from 'styled-components'
const myButton = styled.button`
	border: 1px solid red;
	border-radius: 5px;
`
// 继承myButton的样式，不用重复编写
const myBtton2 = styled(myButton)`
	color: #fff;
`
```

##### 动态添加class

```jsx
export class App extends PureComponent {
    constructor() {
        super()
        this.state = {
            isbbb: true
        }
    }
    render() {
        const {isbbb} = this.state
        // 使用数组
        let classList = []
        if (isbbb) classList.push('bbb')
        let className = classList.join(' ')
        return (
            {/* 1.使用三元运算符动态添加class */}
        	<h2 className={`aaa ${isbbb ? 'bbb' : ''}`}>哈哈哈</h2>
            {/* 2.使用数组,join转化为字符串 */}
    		<div className={className}></div>
        )
    }
}
```

##### 第三方库classnames

以上的动态添加class都比较麻烦，使用第三方库classnames就可以实现和vue一样的动态添加

**npm install classnames**

```jsx
// 导入库
import classnames from 'classnames'
export class App extends PureComponent {
    constructor() {
        super()
        this.state = {
            isbbb: true,
            isccc: false
        }
    }
    render() {
        const {isbbb, isccc} = this.state
        return (
            // 使用classnames()函数传入参数
            <h2 className={classnames('aaa', {bbb: isbbb, ccc: isccc})}>哈哈哈</h2>
            // 数组形式,本质就是把数组最终转换成字符串传给className
            <h2 className={classnames(['aaa', {bbb: isbbb, ccc: isccc}])}></h2>
        )
    }
}
```



### Redux

#### createStore

初始化store过程：当 store 创建后，**Redux 会 dispatch 一个 action 到 reducer 上，用初始的 state 来填充 store。**你不需要处理这个 action。但要记住，如果第一个参数也就是传入的 state 是 undefined 的话，reducer 应该返回初始的 state 值

初始化createStore(reducer) 做了什么？

源码：createStore最后会调用**dispatch({type: actionTypes.INIT})**  派发一个初始化state的action

dispatch(action)源码内会执行**reducer(currentState, action)**  , 因为我们定义的reducer内没有INIT这个type,所以会返回默认的**initialState**，也就实现state的初始化

#### store

想跟踪变化的初始数据，定义在store中

```js
const initialState = {
    friends: [
        {name:'david', age: 34},
        {name: 'kashin', age: 28}
    ]
}
```

#### action

所有数据变化，必须通过派发action来实现

action是一个普通js对象，用来描述这次更新的type和content

```js
const action1 = {type: 'ADD_FRIEND', info: {name: 'david', age: 34}}
const action2 = {type: 'INC_AGE', index: 0}
```

#### reducer

将传入的state和action结合在一起，返回一个新state

**reducer是一个纯函数**

```js
// reducer实例 传入state和action,返回新的state
// 返回值：reducer的返回值会作为store之后储存的state
// 第一次执行时传进来的state肯定是undefined,所以必须给state一个初始值
function reducer(state = initialState, action) {
    switch(action.type) {
        case 'ADD_FRIEND':
            return {...state, friends: [...state.friends, action.info]}
        case 'INC_AGE':
            return {
                ...state, friends: state.friends.map((item, index) => {
                    if (index === action.index) {
                        return {...item, age: item.age + 1}
                    }
                    return item
                })
            }
            // 未匹配到任何action(初始化执行时), 返回state
        default:
            return state
    }
}
```



#### store的4个方法

dispatch: 派发action

getState: 获取state

subscribe: 订阅state回调

replaceReducer

#### redux的主要代码结构

store/index.js ===> 创建store对象: createStore(reducer)

store/reducer.js ===> action-type&action-state结合， 返回新的state

store/actionCreators.js ===> action封装

store/constants.js  ===>action.type常量定义

#### redux核心流程

![image-20221110203530324](C:\Users\yoki\Desktop\memories\assets\image-20221110203530324.png)



#### react中使用redux

**安装 npm install redux**



#### react-redux（高阶组件库）

**安装 npm install react-redux**

这个库的目的就是将react和redux建立连接，结合在一起

使用方法：

```jsx
// 1.index.js根文件引入store, 这样就不必每个页面都引入store了
import App from './App'
import {Provider} from 'react-redux'
import store from './store'

const root = ReactDOM.createRoot(document.querySelector('#root'))
root.render(
	<Provider store={store}>
    	<App/>
    </Provider>
)

// 2.要使用store的页面，引入connect高阶组件
import {connect} from 'react-redux'
```

#### redux中进行异步请求

![image-20221111170932877](C:\Users\yoki\Desktop\memories\assets\image-20221111170932877.png)

本质就是在action中编写网络请求相关代码，在组件中直接调用，实现代码解耦

action派发一个函数，在该函数中进行网络请求和dispatch操作



#### redux中间件

redux中间件目的：**在dispatch的action和最终达到的reducer之间，扩展一些自己的代码**

比如日志记录，调用异步接口，添加代码调试等功能

**中间件redux-thunk**

store.dispatch默认只能传入object类型，要想使dispatch能接收函数类型，必须使用thunk中间件增强store

这个中间件可以让dispatch(action函数), action可以是一个函数，该函数会被调用，并且会接收一个dispatch函数和getState函数

**若派发的是函数则会被执行，若派发是对象，则会被传给reducer执行**

disapatch用于再次派发action

getState可以获取之前的一些状态

****

**安装： npm install redux-thunk**

```js
import {createStore, applyMiddleware} from 'redux'
import thunk from 'redux-thunk' // 安装中间件thunk，使store.dispatch可以支持函数派发
import reducer from './reducer'

// createStore接收的第二个参数是enhancer，可以对store进行增强，传入要增强的中间件thunk
const store = createStore(reducer, applyMiddleware(thunk))
export default store
```



### react调试工具

需要安装2个： react-devtool / redux-devtool (chrome市场或github中下载)

注意：redux-devtool默认是关闭，打开操作如下

```js
// store根目录:index.js
import {createStore,applyMiddleware, compose} from 'redux'
import thunk from 'redux-thunk'
import reducer from './reducer'

// compose是一个合并函数, 用来合并中间件
//这段意思就是用reduxdevTool合并中间件配置,若没有则使用redux自己的compose函数
const composeEnhancer = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose
// 使用composeEnhancer增强store, 支持redux调试工具
const store = createStore(reducer, composeEnhancer(applyMiddleware(thunk)))
export default store
```



### store模块化，合并reducer

拆分store后，对多个reducer进行合并操作, 使用**combineReducer** API

```js
import {createStore, combineReducer} from 'redux'
import homeReducer from './home'
import counterReducer from './other'

// 合并reducer
const reducer = combineReducer({
    home: homeReducer,
    counter: counterReducer 
})
const store = createStore(reducer)
```

### combineReducer原理

```js
// 第一次执行时state为空，所以home和counter接收到的值是undefined，就会返回自己的默认值
function combineReducer(state = {}, action) {
    
    // 返回一个对象
	return {
        // 初始化执行时state.home和state.counter都是undefined，就会去拿自己的默认值
        home: homeReducer(state.home, action),
        counter: counterReducer(state.counter, action)
    }
}
```



### Redux toolkit

redux官方的工具包

目的：解决redux编写繁琐的问题，标准化redux编写规范

**安装： npm install @reduxjs/toolkit react-redux**  (依然要安装react-redux)

**toolkit核心API：**

**configureStore**: 包装createStore，简化配置，自动组合拆分的reducer, 默认包含了redux-thunk中间件，并启用了redux DevTools Extension

```js
// 替换了createStore，configureStore默认包含了thunk配置和redux-devtool配置
import {configureStore} from '@reduxjs/toolkit'
import counterReducer from './modules/counter'

const store = configureStore({
  reducer: {
    counter: counterReducer
  }
})

export default store
```

**createSlice**: 接收reducer函数的对象，切片名称和初始值，并自动生成切片reducer,并带有响应的actions，自动返回一个对象

createSlice接收的参数： 

​	name:用于标记slice的名称，在redux-devtool中显示对应的名称 (action中的type根据name生成)

​	initialState: 第一次初始化的值

​	reducers: 相当于之前的reducer函数，但是是对象类型，可以添加很多的函数, 函数类似原来reducer中的一个case语句，函数的参数: 	参数1:state  参数2：调用这个action时，传递的action参数

​	extraReducers: 异步action

```js
import {createSlice} from '@reduxjs/toolkit'

// createSlice作用：把之前actionCreators/reducer/结合到一起
const counterSlice = createSlice({
  name: 'counter',  // reducer的名字 action中的type根据这个name生成
  initialState: {
    counter: 365
  },
  reducers: { // 原来reducer中的switch语句的case判断，直接拆分到每个函数中
    addNumber(state, action) {
      console.log('addNumber Action', action) // action对象携带payload和type两个属性
      // 此处不用使用{...state}浅拷贝方式创建新的state,toolkit内部会自动创建一个新的state
      // toolkit底层通过immerjs库实现了新state创建
      state.counter = state.counter + action.payload 
    },
    subNumber(state, action) {
      console.log('subNumber===>')
      state.counter = state.counter - action.payload
    }
  }
})
// actions对象中保存了reducers中的所有方法, 导出对应的action
export const {addNumber, subNumber} = counterSlice.actions 
// 注意：返回的是这个对象的reducer属性，用来和store进行合并
export default counterSlice.reducer 
```

**createAsyncThunk**: 接收一个动作类型字符串和一个返回承诺的函数。并生成一个pending/fulfilled/rejected基于该承诺分派动作类型的thunk

```js
// 异步action需要单独写一个方法调用
// createAsyncThunk有2个参数，第一个参数定义一个name，就是action中type的值， 第二个参数是一个回调函数，会进行异步执行
// 第二个回调函数有3种状态，pending/fulfilled/rejected 类似promise, 需要在extraReducer监听3种状态再进行操作
// 第二个回调函数的第二个参数会传入store对象，也可以直接在这个action中进行disptach操作，不走extraReducer
export const fetchHomeDataAction = createAsyncThunk('fetch/homeData', async (extraInfo, store) => {
  console.log('extraInfo', extraInfo)
  console.log('store',store)
  const res = await axios.get('http://123.207.32.32:8000/home/multidata')
  return res.data // 返回即表示fulfilled
})

export const homeSlice = createSlice({
    name: 'home',
    initialState: {
        banners: []
    },
    // 异步请求action在这里处理
    extraReducers: {
        [fetchHomeDataAction.fulfilled](state, action) {
            state.banners = action.payload.data.data.list
        }
    }
})
```

### connect源码实现

```js
// 导入context，传递store对象
import StoreContext from './StoreContext.js'
// 思路：接收2个函数，返回一个高阶组件函数, 并且把接收到props传给高阶组件进行增强返回
export default function connect(mapStateToProps, mapDispatchToProps) {
    return function(OriginComponent) {
        class NewComponent extends PureComponent {
            constructor(props, context) { // constructor传入第二个参数context
                super(props)
                this.state = mapStateToProps(context.getState())
            }
            componentDidMount() {
                this.unSubscribe = this.context.subscribe(() => {
                    this.setState({this.context.getState()}) // 调用了setState，就必须设置constructor
                })
            }
    		componentWillUnmount() {
                this.unSubscribe()
            }
            render() {
                let stateObj = mapStateToProps(this.context.getState())
	        	let dispatchObj = mapStateToProps(this.context.dispatch)
                return <OriginComponent {...this.props} {...stateObj} {...dispatchObj}/>
            }
        }
        NewComponent.contextType = StoreConetxt // NewComponent的静态属性上挂载contextType
        return NewComponent
    }
}
```



### 中间件实现原理

#### 日志打印中间件实现

需求：store.dispatch派发前打印atcion日志，派发后打印state日志

思路：对dispatch方法进行拦截和修改

```js
// index.js
import {createStore, applyMiddleware} from 'redux'
import thunk from 'redux-thunk'
import reducer from './reducer'

const store = createStore(reducer, applyMiddleware(thunk))

// 给store.dispatch添加打印的功能，通过中间件实现
function printLog(store) {
    const next = store.dispatch // 保存原来的dispatch方法
    
    // 内部再定义一个新的函数，用来覆盖原来的dispatch方法, 在这里添加日志打印,这种操作称之为:monkey patch
    function myDispatch(action) {
        console.log('派发的action:', action)
        next(action) // 执行原来的store.dispatch
        console.log('派发后的state:', store.getState())
    }
    store.dispatch = myDispatch // 核心把该方法覆盖到原方法上， 之后调用store.dipatch都是通过这个方法
}

printLog(store) // 导出前执行函数
export default store
```

#### thunk中间件实现

```js
// index.js
import {createStore} from 'redux'
import reducer from './reducer'

const store = createStore(reducer)

// 使disptach能接收函数作为参数
function thunk(store) {
    const next = store.dispatch
    function dispatchThunk(action) {
        if (typeof action === 'function') {
            // 注意：这个dispatch是修改后的新disptach,因为需要支持函数, next是原dispatch
            action(store.dispatch, store.getState) // action接收dipatch和getState2个方法作为参数
        } else {
            next(action)
        }
    }
    store.dispatch = dispatchThunk // 覆写原disptach方法
}
thunk(store)
export default store
```

#### 合并中间件

```js
// 合并多个中间件，统一导出，入参接收store和多个中间件，中间件使用不定参数写法
function applyMiddleware(store, ...fns) {
    fns.forEach(fn => {
        fn(store)
    })
}

function thunk(store) {
    const next = store.dispatch
    function dispatchThunk(action) {
        if (typeof action === 'function') {
            action(store.dispatch, store.getState)
        } else {
            next(action)
        }
    }
    store.dispatch = dispatchThunk
}

function printLog(store) {
    const next = store.dispatch
    
    function myDispatch(action) {
        console.log('派发的action:', action)
        next(action)
        console.log('派发后的state:', store.getState())
    }
    store.dispatch = myDispatch
}

export default applyMiddleware
export {thunk, printLog}
```

```js
// 调用applyMiddleware
import {createStore} from 'redux'
import reducer from './reducer'
import {thunk, printLog}, applyMiddleware from './middleware'

const store = createStore(reducer)
applyMiddleware(store, printLog, thunk) //传入中间件和store, 内部依次执行中间件

export default store
```



### react-router

**安装**： **npm install react-router-dom**

react-router提供2种路由模式：BrowserRouter和HashRouter

**路由核心概念：映射关系 path => component **

#### 路由根页面

```jsx
import React, { PureComponent } from 'react'
import {Route, Routes, Link, NavLink, Navigate} from 'react-router-dom'
import Home from './views/home/Home'
import Profile from './views/profile/Profile'
import NotFound from './views/notFound/NotFound'
import HomeRecommends from './views/home/HomeRecommends'
import HomeBanners from './views/home/HomeBanners'
import ProfileBanner from './views/profile/ProfileBanner'
import ProfileDetail from './views/profile/ProfileDetail'
import User from './views/user/User'
export class App extends PureComponent {
  render() {
    return (
      <div>
        <div className="header">
          <h2>header</h2>
          {/* 路由跳转*/}
          <Link to='/home'>首页</Link>
          <Link to='/profile'>资料</Link>
          {/* NavLink 作用是在选中时给该<a>元素自动添加一个'active'的className 可以用来定义样式 style接收一个函数，会传入一个对象包含isActive属性 */}
          <NavLink to='/home' style={({isActive}) => ({color: isActive ? 'red' : ''})}>navLink</NavLink>
          {/* 动态改样式名 */}
          <NavLink to='/home' className={({isActive}) => isActive ? 'my-active' : ''}>navLink2</NavLink>
          
          {/* 跳转时传查询字符串 */}
          <Link to='/user?name=guyufeng&age=34'>用户</Link>
          <hr />
        </div>
        <div className='content'>
          {/* 路由映射关系 path => component */}
          <Routes>
            {/* Navigate标签实现重定向 */}
            <Route path='/' element={<Navigate to='/home'/>}/>
            <Route path='/home' element={<Home/>}>
              {/* 二级路由重定向：当匹配到/home时，默认匹配二级路由的/home/recomend */}
              <Route path='/home' element={<Navigate to='/home/recommend'/>}/>
              {/* home的二级路由 嵌套Route即可实现 二级路由需要配置占位符Outlet*/}
              <Route path='/home/recommend' element={<HomeRecommends/>}/>
              <Route path='/home/banner' element={<HomeBanners/>}/>
            </Route>
            <Route path='/profile' element={<Profile/>}>
              <Route path='/profile/banner' element={<ProfileBanner/>}/>
              {/* 传动态路由 */}
              <Route path='/profile/detail/:id' element={<ProfileDetail/>}/>
            </Route>
            {/* 传查询字符串 */}
            <Route path='/user' element={<User/>}/>
            {/* 路径不存在时，匹配这个页面 */}
            <Route path='*' element={<NotFound/>}/>
          </Routes>
        </div>
        <div className="footer">
          <hr />
          <h2>footer</h2>
        </div>
      </div>
    )
  }
}

export default App
```

#### useNavigate(编程式跳转hook)

Link标签跳转都默认渲染成a标签， 想使用其他标签进行跳转就要使用编程式跳转，给标签绑定事件

**useNavigate是一个hook，会返回navigate跳转的方法，但只能在函数式组件或自定义hooks中使用，不能在类组件中使用**

```jsx
// Home.jsx 改写为函数式组件 才能使用useNavigate hook
import {Outlet, Link, useNavigate} from'react-router-dom'
export function Home(props) {
  // 注意：hooks必须在顶层执行
  const navigate = useNavigate() // useNavigate是一个hook, 返回一个跳转方法
  // 定义跳转方法
  const navigateTo = (path) => {
    navigate(path)
  }
  return (
    <div>
      <h2>Home</h2>
       {/* 1.标签式跳转 */}
      <Link to='/home/recommend'>recomend页面</Link>
      <Link to='/home/banner'>banner页面</Link>
       {/* 2.编程式跳转 */}   
      <button onClick={e => navigateTo('/home/recommend')}>button跳转1</button>
      <button onClick={e => navigateTo('/home/banner')}>button跳转2</button>
      {/* 路由占位符 */}
      <Outlet/>
    </div>
  )
}
export default Home
```



#### ProfileBanner 携带动态参数跳转到==> ProfileDetail

```jsx
import React, { PureComponent } from 'react'
import withRouter from '../hoc/withRouter'
// ProfileBanner.jsx
export class ProfileBanner extends PureComponent {
  constructor(props) {
    super(props)
    this.state = {
      list: [
        {name: '热门歌单', id: '111'},
        {name: '最新歌单', id: '222'},
        {name: '畅销歌单', id: '333'}
      ]
    }
  }
  navigateTo(id) {
    const {navigate} = this.props.router
    navigate('/profile/detail/' + id)
  }
  render() {
    return (
      <div>
        <h2>profile-二级页面-banner</h2>
        <ul>
          {
            this.state.list.map((item, index) => {
              return <li key={index} onClick={e => this.navigateTo(item.id)}>{item.name}</li>
            })
          }
        </ul>
      </div>
    )
  }
}
export default withRouter(ProfileBanner) 

// ProfileDetail.jsx
import React, { PureComponent } from 'react'
import withRouter from '../hoc/withRouter'

export class ProfileDetail extends PureComponent {
  render() {
    const {params} = this.props.router // 获得navigate传递的url参数
    return (
      <div>
        <h2>profile-detail</h2>
        <span style={{color: 'red'}}>id: {params.id}</span>
      </div>
    )
  }
}
export default withRouter(ProfileDetail)
```



#### router hooks(封装高阶组件)

```js
import {useNavigate,useParams,useLocation,useSearchParams} from 'react-router-dom'

function withRouter(OriginComponent) {
	return function(props) {
        const navigate = useNavigate() // 顶层调用hook
        const params = useParams() // 获取动态路由参数 类似/home/:id
        const location = useLocation() // 获取查询字符串
        const [searchParams] = useSearchParams() // 获取URLSearchParams对象
        const query = Object.fromEntries(searchParams) // 把查询字符串转成obj对象
        const router = {navigate, params, location, query} // 包裹到router对象中
        return <OriginComponent {...props} router={router} /> // 把增强方法传给原类组件
    }    
}
```

#### 路由配置型写法

```jsx
// react-router 5.x 需要react-router-config库才支持配置写法
// react-router 6.x 开始不需要三方库即支持
// router/index.js 单独路由配置文件
import React from 'react'
import {Navigate} from 'react-router-dom'
import Home from '../views/home/Home'
import HomeBanners from '../views/home/HomeBanners'
import HomeRecommends from '../views/home/HomeRecommends'
import NotFound from '../views/notFound/NotFound'
import Profile from '../views/profile/Profile'
import ProfileBanner from '../views/profile/ProfileBanner'
import ProfileDetail from '../views/profile/ProfileDetail'
// import User from '../views/user/User'
// 路由懒加载写法 import是webpack的方法，返回一个Promise对象
const User  = React.lazy(() => import('../views/user/User')) // 懒加载组件会被单独打包成一个js文件
const routes = [
  {
    path: '/',
    element: <Navigate to='/home' />
  },
  {
    path: '/home',
    element: <Home/>,
    children: [
      {
        path: '/home',
        element: <Navigate to='/home/recommend'/>
      },
      {
        path: '/home/recommend',
        element: <HomeRecommends/>
      },
      {
        path: '/home/banner',
        element: <HomeBanners/>
      }
    ]
  },
  {
    path: '/profile',
    element: <Profile/>,
    children: [
      {
        path: '/profile/banner',
        element: <ProfileBanner/>
      },
      {
        path: '/profile/detail/:id',
        element: <ProfileDetail/>
      }
    ]
  },
  {
    path: '/user',
    element: <User/>
  },
  {
    path: '*',
    element: <NotFound/>
  }
]

export default routes
      
      
// App.jsx内调用路由配置
import { Link, NavLink, useRoutes} from 'react-router-dom'
// 导入配置文件
import routes from './router'

export function App() {
  return (
    <div>
      <div className="header">
        <h2>header</h2>
        {/* 路由跳转*/}
        <Link to='/home'>首页</Link>
        <Link to='/profile'>资料</Link>
        <Link to='/user?name=guyufeng&age=34'>用户</Link>
        <hr />
      </div>
      <div className='content'>
        {/* useRoutes调用路由配置文件 */}
        {useRoutes(routes)}
      </div>
      <div className="footer">
        <hr />
        <h2>footer</h2>
      </div>
    </div>
  )
}

export default App
```



### React-hooks

**目的：帮助函数式组件实现类组件的功能**

**规则：**

**1.必须在函数组件的顶层使用**

**2.不能在普通函数中使用**

**3.可以在自定义Hook中使用（use开头定义函数）**



#### useState

useState => 钩入state,  它与class里的this.state提供的功能完全相同

useState接受唯一参数，在第一次被调用时使用作为初始化值（不传则初始化值为undefined）

**重点：useState的set方法是异步的，修改后的值在下次调用才会生效！和闭包陷阱概念有关**

**重点：useState的初始化值只有在组件第一次渲染时有效，组件更新后的第二次渲染不会再被设置**

```js
// 参数也可以是一个函数，该函数的返回值就是useState的初始化值（逻辑较长时可以使用）
const [data, setData] = useState(() => {
    return JSON.parse(localStorage.getItem(key))
})
```



useState返回一个数组，通过解构数组来完成赋值

##### useState储存对象类型

```jsx
import {useState} from 'react'
function App () {
    const [person, setPerson] = useState({name: 'david', age: 34})
    const modPerson = () => {
        setPerson({
            ...person, // 修改对象类型时，必须浅拷贝后修改，否则视图不会更新
            age: person.age + 1
        })
    }
    return (
    	<div>
        	<h2>姓名: {person.name}</h2>
            <h2>年龄: {person.age}</h2>
            <button onClick={() => modPerson()}>年龄+1</button>
        </div>
    )
}
```

##### useState函数式更新

如果新的 state 需要通过使用先前的 state 计算得出，那么可以将函数传递给 `setState`。**该函数将接收先前的 state，并返回一个更新后的值**

```js
// useState的两种更新方式
function Counter() {
  const [count, setCount] = useState(0);
  function handleClick() {
    setCount(count + 1)
  }
  function handleClickFn() {
    setCount((prevCount) => {
      return prevCount + 1
    })
  }
  return (
    <>
      Count: {count}
      <button onClick={handleClick}>+</button>
      <button onClick={handleClickFn}>+</button>
    </>
  );
}
```



#### useEffect

useEffect => 钩入生命周期

**重点1：useEffect可以存在多个**

好处：每个useEffect有自己的逻辑，实现自定义hook，从而进行抽取复用

**重点2：useEffect内回调函数执行顺序：**

**1.首次渲染时会执行useEffect内的回调函数**
**2.当数据更新或卸载时先执行return的返回函数**
**3.视图更新后，执行useEffect的回调**

```jsx
import {memo} from 'react'
import {useState, useEffect} from 'react'

const App = memo(() => {
    const [count, setCount] = useState(10)
    
    // useEffect接收函数作为参数，该函数会在组件首次渲染或更新后执行
    useEffect(() => {
        // 网络请求、DOM操作、事件监听、redux监听
        store.subscribe() // 执行redux的监听
        
        // 返回一个函数，用来取消监听， 在组件更新或卸载时执行
        return () => {
            // 取消redux监听
            store.unsubscribe()
        }
    })
    return (
    	<div>
            <h2>当前计数: {count}</h2>
        	<button onClick={() => setCount(count + 1)}>+1</button>
        </div>
    )
})
export default App
```

##### useEffect执行机制

useEffect接收第二个参数，是一个数组，决定useEffect在哪些state发生变化时才重新执行(受谁的影响)

不传则只要有更新就会执行

如果一个函数不希望依赖任何的内容时，可传入一个空数组(只有首次渲染或卸载才执行，模拟生命周期)

```jsx
import {useState, useEffect, memo} from 'react'

const App = memo(() => {
    const [counter, setCounter] = useState(100)
    useEffect(() => {
        
    }, [])
    return (
    	<div>
        	<h2>当前计数:{counter}</h2>
            <button onClick={e => setCounter(counter + 1)}>+1</button>
        </div>
    )
})
```



#### useContext

组件依赖的context数据发生改变时，该组件也会重新渲染

```jsx
// 使用context的组件
import React, { memo, useContext } from 'react'
import { counterContext, messageContext } from '../context/context'

const Context = memo(() => {
  // useContext的返回值可以直接获取到传入的context的值
  const counter = useContext(counterContext)
  const message = useContext(messageContext)
  return (
    <div>
      <h2>counterContext: {counter.height}</h2>
      <h2>messageContext: {message.name + ' ' + message.age}</h2>
    </div>
  )
})
export default Context

// App.js
import React from 'react'
import Context from './pages/Context'

function App() {
  return (
    <div>
      <h2>useContext测试</h2>
      <Context/>
    </div>
  )
}

export default App

// index.js根文件
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import { messageContext, counterContext } from './context/context';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <messageContext.Provider value={{name: 'david', age: 34}}>
    <counterContext.Provider value={{height: '1.75'}}>
      <App />
    </counterContext.Provider>
  </messageContext.Provider>
);

```



#### useReducer(不常用)

userReducer并不是redux中reducer的替代，仅仅是useState的一种替代方案

```jsx
import React, { memo } from 'react'
import { useState } from 'react'
import { useReducer } from 'react'

// !注意useReducer虽然和redux内的reducer类似，但不能替代redux, 是useState的一种替代方案（定义和操作复杂数据时使用）
// 定义一个reducer函数，传给useReducer 
function reducer(state, action) {
  switch (action.types) {
    case 'increment':
      return {...state, counter: state.counter + 1}  
    case 'decrement':
      return {...state, counter: state.counter - 1}
    case 'add_number':
      return {...state, counter: state.counter + action.num}
    case 'sub_number':
      return {...state, counter: state.counter - action.num}
    default:
      return state
  }
}
const reducerTest = memo(() => {
  // useReducer接收2个参数，一个是定义好的reducer函数，一个是初始化的state值
  // useReducer返回的也是一个数组，第一个返回值是state,第二个返回值是dispatch方法
  const [state, dispatch] = useReducer(reducer, {counter: 100, friends: [], user: {}}) // 复杂数据可以定义在一个useReducer中

  // 使用useState就需要分开定义3个useState
  // const [counter, setCounter] = useState(100)
  // const [friends, setFriends] = useState([])
  // const [user, setUser] = useState({})
  return (
    <div>
      <h2>useReducer:</h2>
      <h2>当前计数: {state.counter}</h2>
      {/* disptach接收一个action对象 */}
      <button onClick={() => dispatch({types: 'increment'})}>+1</button>
      <button onClick={() => dispatch({types: 'decrement'})}>-1</button>
      <button onClick={() => dispatch({types: 'add_number', num: 5})}>+5</button>
      <button onClick={() => dispatch({types: 'sub_number', num: 5})}>-5</button>
    </div>
  )
})

export default reducerTest
```



#### useCallback

useCallback替代普通的函数声明，普通函数声明每次重新渲染都会重新声明

useCallback性能优化的意义：

**当需要将一个函数传递给子组件时，使用useCallback进行优化，将优化后的函数传给子组件，子组件就不会被重新渲染**

因为props属性发生改变时，组件本身会被重新渲染（必须包裹在memo函数内，否则子组件都会渲染）

```jsx
const App = memo(function() {
    const [count, setCount] = useState(100)
    // 当点击按钮时，页面每次重新渲染，increment函数就会反复的重新声明(旧的函数没有引用会被销毁)
    function increment() {
        setCount(count + 1)
    }
    // 使用useCallback能让每次调用的都是同一个函数(本质是闭包)
    const increment2 = useCallback(function() {
        setCount(count + 1)
    }, [count]) // 第二个参数表示依赖对象，当这个值改变时，callback才会重新赋值，否则会产生闭包陷阱导致每次count都一样
    
    return (
        <div>
        	<h2>当前计数：{count}</h2>
            <button onClick={increment}>+1</button>
            <button onClick={increment2}>+1</button>
        </div>
    )
})
```

**useCallback应用场景**

```jsx
// 将方法传递给子组件 若每次获取的都是新的increment那么子组件每次都会重新渲染
const Cpn1 = memo(function(props) {
    const {increment} = props
    console.log('子组件被渲染')
    return (
    	<div>
        	<button onClick={increment}>+1</button>
        </div>
    )
})

const App = memo(function() {
    const [count, setCount] = useState(100)
    const [message, setMessage] = useState('hello world')
    
    // useCallback定义函数 非count依赖时,返回的increment函数不会被更新
    const increment = useCallback(function() {
        setCount(count + 1)
    }, [count])
    
    // 普通函数声明 如果把这个函数传给子组件cpn1,那么每次父组件其他数据更新时，increment也是新声明的，子组件也会被更新
    //const increment = function(){
      //  setCount(count + 1)
    //}
    return (
        <div>
        	<h2>当前计数：{count}</h2>
            <button onClick={increment}>+1</button>
      {/* 关键：把useCallback方法传递给子组件，当调用setMessage时increment不会改变，所以子组件不会每次都更新 */}
            <Cpn1 increment={increment}/>
            
            <h2>当前文本：{message}</h2>
            <button onClick={e => setMessage(Math.random())}>改变文本</button>
        </div>
    )
})
```



#### useRef

返回一个ref对象，这个对象在组件的整个生命周期保持不变

常见用法：

1.引入DOM元素（或者Class组件） 

2.保存一个数据，这个对象在整个生命周期中可以保持不变

```jsx
// useRef创建的ref对象，在组件整个生命周期都不会改变
// 操作DOM实例
const App = memo(function(props) {
    const countRef = useRef()
    const getDom = function() {
        console.log(countRef)
    }
    return (
    	<div>
        	<h2 ref={countRef}>我是标题</h2>
            <button onClick={() => getDom()}>查看DOM</button>
        </div>
    )
})
```

**利用不变特性对useCallback进一步优化**

```jsx
// 对useCallback做进一步优化=>依赖数据发生改变时，子组件也不会重新渲染
const Cpn1 = memo(function(props) {
    const {increment} = props
    console.log('子组件被渲染')
    return (
    	<div>
        	<button onClick={increment}>+1</button>
        </div>
    )
})

const App = memo(function() {
    const [count, setCount] = useState(100)
   
    // useRef会返回一个ref对象
    const countRef = useRef()
    countRef.current = count // 保存count
    
    // 重点！setCount内传入countRef.current, 因为组件每次渲染的countRef是同一个对象，所以cuurent属性会自增
    const increment = useCallback(function() {
      setCount(countRef.current + 1)
    }, []) // 这里设置为空数组，即increment不依赖任何数据，保证increment不会被更新
    return (
    	<div>
        	<h2>当前计数：{count}</h2>
            <button onClick={increment}>+1</button>
            {/*子组件调用父组件传入方法increment修改count时，子组件自己也不会重新渲染*/}
            <Cpn1 increment={increment}/>
        </div>
    )
})
```



#### useMemo

```jsx
// useMemo和useCallback区别
function fn() {}
// useCallback优化的是传入的函数，返回值是一个函数，会根据依赖的数据更新而重新执行
let increment1 = useCallback(fn, deps)
// useMemo优化的是返回值(返回值可以是函数执行结果或一个函数)，会根据依赖的数据更新而重新执行
let increment2 = useMemo(() => fn, deps)
// 这两种写法是等效的 increment1 === increment2
```



#### useImperativeHandle

子组件对父组件传入的ref进行处理(控制子组件暴露出去的方法和属性等)

```jsx
import React, { memo, useImperativeHandle } from 'react'
import { forwardRef, useRef } from 'react'

// forwardRef获取父组件传入的ref
const InputCpn = memo(forwardRef((props, ref) => {
  // 定义私有ref绑定到要暴露出去的DOM上
  const privateRef = useRef()
  // 接收2个参数，1.要控制的ref对象，2.一个回调函数，回调函数的返回值就是暴露给父组件的ref对象
  useImperativeHandle(ref, () => {
    return {
        // 暴露给父组件的myFocus方法
      myFocus() {
        console.log('哈哈哈，我是暴露出去的focus')
        privateRef.current.focus() // 通过自己的ref实现focus功能
      }
    }
  })
  // 绑定自己的ref, 帮助实现myFocus内的逻辑
  return <input type='text' ref={privateRef} />
}))

const ImperativeHandle = memo(() => {
  const inputRef = useRef()
  function handleInput() {
    inputRef.current.myFocus() // 父组件只能调用useImperativeHandle暴露出来的对象
    inputRef.current.value = 'david无敌' // 这种操作无效，useImperativeHandle的目的就是控制父组件操作子组件ref时的权限
  }
  return (
    <div>
      <InputCpn ref={inputRef}/>
      <button onClick={handleInput}>父组件操作Input输入框</button>
    </div>
  )
})

export default ImperativeHandle
```



#### useLayoutEffect

和useEffect区别：

useEffect会在渲染的内容更新到DOM上后执行，不会阻塞DOM的更新

useLayoutEffect会在渲染的内容更新到DOM上之前执行，会阻塞DOM的更新

较少使用，官方推荐使用useEffect



#### 自定义Hooks

react中的自定义hook必须用"use"开头，否则会报错

```jsx
import {useEffect} from 'react'

// 自定义hook: 在组件加载或卸载时打印日志
function useLogLife(cpnName) {
  useEffect(() => {
    console.log(cpnName + '已加载')
    return () => {
      console.log(cpnName + '已卸载')
    }
  }, []) // 传空数组模拟生命周期，仅加载和卸载时执行
}

export default useLogLife
```

```js
import { useContext } from 'react'
import { tokenContext } from '../context/context'

// 获取公共的contextToken数据
export function useToken() {
  const token = useContext(tokenContext)
  return token
}
```

```js
import { useState,useEffect } from 'react'

export function useScrollPosition() {
  // 给scrollX scrollY设置初始化值
  const [scrollX, setScrollX] = useState(0)
  const [scrollY, setScrollY] = useState(0)
  // 把监听事件放在useEffect中，卸载组件时要移除监听
  useEffect(() => {
    function handleScroll() {
      setScrollX(window.scrollX) // 滚动值发生改变时调用第二个方法设置数据，一调用setScroll组件就会刷新
      setScrollY(window.scrollY)
    }
    window.addEventListener('scroll', handleScroll)
    return () => {
      window.removeEventListener('scroll', handleScroll)
    }
  }, []) // 仅在组件加载卸载时执行
  return [scrollX, scrollY]
}
```

```js
import { useEffect, useState } from "react";

export function useLocalStorage(key) {
  // 把key的value作为state的初始化值 让两者产生联系
  const [data, setData] = useState(JSON.parse(localStorage.getItem(key)))
  // 监听当data改变时，调用setItem设置localStorage
  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(data))
  }, [data])
  return [data, setData]
} 
```



### redux-hooks

redux7.1开始提供hooks方式,免去编写connect以及对应的映射函数



#### useSelector

作用：将state映射到组件中(类似connect函数中的mapStateToProps)

参数1：将state映射到需要的数据中；

参数2：可以进行比较来决定是否组件重新渲染(useSelector默认比较返回的两个对象是否相等)

```js
import {useSelector} from 'react-redux'

const App = memo((props) => {
    // 返回一个对象包含counter, 直接解构
    // useSelector接收一个函数，函数的参数就是state,返回值就是要映射的state数据
    // 优点：比connect/mapStateToProps写法更简洁
    const {count} = useSelector((state) => {
        return {
            count: state.counter.count
        }
    })
})
```



#### useDispatch

作用：直接获取dispatch函数，在组件中直接使用即可

```jsx
import {useSelector, useDispatch} from 'react-redux'
import {addNumAction} from 'store/modules/counter'

const App = memo((props) => {
    const {count} = useSelector((state) => {
        return {
            count: state.counter.count
        }
    })
    const disptach = useDispatch()
    function addCount(num) {
        // 直接派发action 省去了mapDisptachToProps的写法
        dispatch(addNumAction(num))
    }
    return (
    	<div>
        	<h2>当前计数: {count}</h2>
            <button onClick={() => addCount(5)}>+5</button>
        </div>
    )
})
```



#### useSelector性能优化

```jsx
import {useSelector, useDispatch, shallowEqual} from 'react-redux'
import {addNumAction, changeMessageAction} from 'store/modules/counter'

const App = memo((props) => {
    const {count} = useSelector((state) => {
        return {
            count: state.counter.count
        }
    }, shallowEqual) // 传入第2个参数shallowEqual进行浅层比较，当返回值相等时不会重新渲染组件
    const disptach = useDispatch()
    function addCount(num) {
        // 直接派发action 省去了mapDisptachToProps的写法
        dispatch(addNumAction(num))
    }
    return (
    	<div>
        	<h2>当前计数: {count}</h2>
            <button onClick={() => addCount(count+5)}>+5</button>
            <!--引入Home组件，当修改count时，Home组件也会重新渲染，可通过shallowEqual性能优化-->
            <Home/>
        </div>
    )
})


const Home = memo((props) => {
    const {message} = useSelector((state) => {
        return {
            message: state.counter.message
        }
    }, shallowEqual)
    // home组件渲染
    console.log('Home render!')
    const dispatch = useDispatch()
    function changeMessage() {
        dispatch(changeMessageAction('hello,david!'))
    }
	return (
    	<div>
        	<h2>home: {message}</h2>
            <button onClick={() => changeMessage()}>修改message</button>
        </div>
    )    
})
```



#### useId（SSR使用）

当需要动态生成的id时，useId可保证服务端和客户端id一致

```jsx
import {memo, useState, useId} from 'react'

//服务端也会运行这段代码，不管执行几次id一旦生成都不会变
const App = memo((props) => {
    const [count, setCount] = useState(0)
    const id = useId()
    return (
    	<div>
        	<button onClick={() => setCount(count+1)}>+1</button>
            <!--保证客户端和服务器端ID是唯一的-->
            <label htmlFor={id}>
            	用户名: <input id={id} type="text"></input>
            </label>
        </div>
    )
})
export default App
```



#### useTransition(延迟更新1)

返回一个布尔值pending和一个函数，把要延迟更新的函数放在这个函数中回调



#### useDeferredValue(延迟更新2)

```jsx
import {memo, useState, useDeferredValue} from 'react'
import namesArray from './data'

const App = memo(() => {
    const [names, setNames] = useState(namesArray)
    // 创建一个数组副本,这个副本会被延迟更新
    const deferredArray = useDeferredValue(names)
    
    function handleInput(event) {
        const keywords = event.target.value
        let filters = namesArray.filter(item => item.includes(keywords))
        setNames(filters)
    }
    return (
        <!--效果就是输入框的文字会立刻更新，js执行完后再更新列表，否则输入框和列表更新是同步的，会有卡顿的感觉-->
    	<div>
        	<input type="text" onInput={handleInput}></input>
            <h2>用户名列表：</h2>
            <ul>
            	{
                    deferredArray.map((item, index) => {
                        return (
                        	<li key={index}>{item}</li>
                        )
                    })
                }
            </ul>
        </div>
    )
})

```



### 项目(airbnb)

#### 配置

1.配置icon, 在public文件夹下

2.配置title

3.配置jsconfig.json, 方便vscode有更好的提示，如引用路径提示

4.配置别名路径（使用craco.config.js配置）

npm install @craco/craco

5.支持less(使用craco.config.js配置)

npm install craco-less

6.样式重置--> normalize.css

安装：npm install normalize.css

index.js引入：import 'normalize.css'

#### 路由

npm install react-router-dom

#### redux

npm install @reduxjs/toolkit react-redux

#### axios封装





### material UI 三方库

官方地址：mui.com

**安装： npm install @mui/material @emotion/react @emotion/styled** 



### antd 三方库

**安装: npm install antd**



