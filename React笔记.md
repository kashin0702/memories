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

![image-20221017145449532](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20221017145449532.png)

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



### 组件通信

父传子： 父组件通过**属性=值**传递给子组件， 子组件通过**props**获取父组件传递过来的数据

子传父:    通过回调函数实现

```react
// 父传子
<Children banners={faterData}></Children>

class Children extends Component {
    // 子组件通过constructor函数接收父组件数据
    //(若不需要维护state,constructor可以不写，默认会执行super(props))
    constructor(props){
        super(props) //同时执行super(props), 把props保存到实例中
    }
    render () {
        const {banners} = this.props // 解构获得父组件传过来的banners
    }
}

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

