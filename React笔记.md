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

基本语法:(**在html中使用必须在script标签上添加：type="text/babel"**)

```react
// 在js中这段代码会报错，而在jsx中这段代码会被babel进行转化
// 会被babel转化为类似React.createElement('div',null, '哈哈哈')
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
        // class的一些绑定方法
        // 方法1. 模板字符串拼接，动态绑定xxstyle active
        const classNames = `xxstyle isActive ? 'active' : ''`
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
        this.state = {}
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
      render () {
        return (
          <div>
            {/* 不使用箭头函数，执行时默认接收event */}
            <button onClick={this.btnClick}>按钮1</button>

            {/* 使用箭头函数，直接从外层把事件对象传进去 */}
            <button onClick={(e) => this.btnClick(e)}>按钮2</button>
			{/* 传递多个参数时也一样*/}
            <button onClick={(e) => this.btnClick2(e,'david',34)}>按钮3</button>
          </div>
        )
      }
    }
```



