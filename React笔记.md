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
export default const MyContext = React.createContext() // 创建上下文对象

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

```react
<MyContext.Provider value={{commonData: '123'}}>
    <OtherContext.Provider value={{otherData: 'aaa'}}>
    	<Children {...obj}/>
    </OtherContext.Provider>
</MyContext.Provider>

// 子组件
import MyContext from './context.js'
import OtherContext from './other.js'
// 关键：一个子组件只能指定一个contextType, 其他context就通过Consumer获取
Children.contextType = MyContext 

export class Children extends Component {
    render(){
        console.log(this.context)
        return (
        	<div>
                {/* 通过Consumer获取otherContext的数据 value通过回调函数传进来*/}
            	<OtherContext.Consumer>
                    {
                        value => <h2>{value.otherData}</h2>
                    }
                </OtherContext.Consumer>
            </div>
        )
    }
}
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

![image-20221101141720292](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20221101141720292.png)

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

**本质是通过forwardRef函数做了一个ref的转发**

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

