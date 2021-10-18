### 基本类型检测 

变量屁股后跟上 :类型

```typescript
let a:number = 1  //ok
let b:string = 1 //type error

```

### 数组类型检测

```typescript
// 两种写法等价
let arr:number[] = [1,2,3] //ok 推荐写法
let brr:Array<number> = [4,5,6]  //ok 数组泛型:Array<元素类型>  不推荐写法(jsx中有冲突)
```

### 对象类型检测

```typescript
// 传入一个point形参对象，包含x,y两个属性
function foo(point: {x: number, y: number}){
    //code
}
// 调用
foo({x: 123, y: 456})
```



### 类型别名

有时候类型检测太长可以使用别名, 提高代码阅读性

```typescript
// 用关键词type定义类型别名
type IdType = string|number|boolean
// 定义一个对象类型别名
type pointType = {
    x: number,
    y: number,
    z?: number
}
// 直接使用类型别名给参数定义类型
function foo(id: IdType) {
    //code
}

function printPoint(point: pointType) {
    //code
}
```

### interface定义对象类型

```typescript
interface xxxType {
    name: string,
    age: number,
    other?: string
}
```

### interface定义索引类型

```typescript
const obj: ObjType = {
    0: 'HTML',
    1: 'CSS',
    2: 'JAVASCRIPT',
    // 这样写就会报错
    'xxx': 'VUE'
}
//当想定义一个key是数值,value是字符串的类型对象时，可以定义一个索引类型
interface ObjType {
    //index是形参可以随便写,[]表示索引
    [index: number]: string
}
```

### interface定义函数类型

```typescript
interface calcFnType {
    // 固定写法 不用箭头用冒号，冒号左边是参数 右边是返回值
    (n1: number, n2: number): number
}
function calc(num1: number, num2: number, calcFn: calcFnType){
    return calcFn(num1, num2)
}
const add: calcFnType = (n1, n2) {
    return n1 + n2
}
calc(10,20,add)
```





### 函数类型检测

函数很少定义类型检测，可以不写

```typescript
//1.直接定义写法 可读性差
const foo: (() => void) = () => {}

//2.定义写法： 关键词type 定义一个类型别名
type MyFunction = () => void
// 给foo函数一个myFunction类型
const foo: MyFunction = () => {}

//3. 写法3 有参数的情况
const add: (num1: number,num2: number) => number = (num1: number, num2: number) {
    return num1 + num2
}
//或者
const addType = (num1: number, num2: number) => number
cosnt add: addType = (num1: number, num2: number) {
    return num1 + num2
}
```

### 函数作为参数

```typescript
function foo(){}
//函数作为参数传入
function bar(fn: () => void){
    fn()
}

bar(foo)
```





### Tuple元组类型检测

元组类型适用于已知个数和元素类型的数组

```js
let X:[number,string];
x = [1,'hello'] //ok
x = [1,2]  //type error

```

### class类

```typescript
class Person {
    //没有放在constructor，需要给一个初始化值
    name: string = ''
    age: number = 0
	fn() {}
}
class Person {
    name: string
    age: number
    // 执行构造器constructor 在内部初始化
    constructor(name: string, age: number){
        this.name = name,
        this.age = age
    },
    fn(){}
}
const p = new Person('david', 18)
```



### class类的继承

```typescript
class Person{
    name: string 
    age: number
    constructor(name: string, age: number){
        this.name = name
        this.age = age
    }
    personFn(){}    
}
// Student类继承了Person的所有属性和方法
class Student extends Person{
    other: string
    constructor(name: string,age: number,other: string){
        //super调用父类构造器 保存name,age	
        super(name,age)
        this.other = other
    }
    studying(){}
}
```

### class类的修饰符

```typescript
class Person{
    //此处有默认修饰符public(不用写) 让name属性在任意地方都可以被访问
    public name: string = 'david'
    // 私有修饰符private 仅在内部可以访问
    private age: number = 30
    getAge(){
        return this.age //这里可以访问age 暴露给外部
    }
    //仅内部和子类可以访问
    protected name2: string = 'kashin'
}
const p = new Person()
p.name // david

class Student extends Person {
    getName2(){
        //子类可以访问父类的name2属性 暴露给外部使用
        return this.name2
    }
}
p.name2 //kashin
```



### class类实现接口implements

```typescript
class Animal {
    
}
// 接口类型ISwim 有一个函数对象的swimming
interface ISwim {
    swimming: () => void
}
interface IEat {
    eating: () => void
}
// 继承：Fish类继承Animal类，只能单继承
// 实现：实现接口，可以实现多接口
class Fish extends Animal implements ISwim, IEat {
    swimming() {
        console.log('fish swimming')
    }
	eating() {
        //code
    } 
}
// Person类也实现了ISwim接口
class Person implements ISwim {
    swimming() {
        console.log('person swimming')
    }
}

//编写一些公共API: 面向接口编程
function swimAction(swimable: ISwim) {
    swimable.swimming()
}
// 所有实现了接口的类对应的对象，都可以传入
swimAction(new Fish())
swimAction(new Person())

swimAction({swimming: function() {}})
```



### 问号(?) 叹号(!)用法

#### **？用法：** 

1.可选属性的含义是: 使用这个属性时，要么这个属性名不存在，要么必须符合属性的类型定义

2.当使用A对象属性A.B时，如果无法确定A是否为空，则需要用A?.B，表示当A有值的时候才去访问B属性，没有值的时候就不去访问，如果不使用?则会报错

```typescript
//?用法 由于函数参数可选，因此parma无法确定是否拥有，所以无法正常使用parma.x，使用parma?.x向编译器假设此时parma不为空且为IDemo类型，同时parma?.x无法保证非空，因此使用parma?.x!来保证了整体通过编译
interface IDemo {
    x: number
}

let y:number

const demo = (parma?: IDemo) => {
    y = parma?.x!
    console.log(parma?.x)   // 只是单纯调用属性时就无需!    
    return y
}
    
// 如果使用y = parma!.x!是会报错的，因为当parma为空时，是不拥有x属性的，会报找不到x的错误

//但是?用法只能读操作而不能写操作，对一个可能为空的属性赋值是不会被编译通过的，此时还需用用到类型断言

interface IDemo {
    x: number
}

// 编译报错，不能赋值给可选属性
const demo = (parma?: IDemo) => {
    parma?.x = 1    
}
    
// 使用类型断言  
const demo = (parma?: IDemo) => {
    let _parma = parma as IDemo
    _parma.x = 1
}
```



#### **！用法：**

非空类型判断

用在变量前表示取反，用在赋值的内容后时，使**null**和**undefined**类型可以赋值给其他类型并通过编译

```typescript
let y:number

y = null        // 无法通过编译
y = undefined   // 无法通过编译

y = null!
y = undefined! 

// 由于x是可选的，因此parma.x的类型为number | undefined，无法传递给number类型的y，因此需要用x!
interface IDemo {
    x?: number // x要么不存在undefined，要么为number型
}

let y:number

const demo = (parma: IDemo) => {
    y = parma.x!
    return y
}



//如果存在空情况的判断并赋具体值时，可以不用!，但是如果要想令y存在等于undefined的情况还是需要用!
interface IDemo {
    x?: number
}

let y:number

const demo = (parma: IDemo) => {
    y = parma.x || 1    // 如果为undefined，返回y=1，如果不为undefined，则返回parma.x的值
    return y
}




```

### 联合类型

参数可以是string或者number或者boolean类型

```typescript
function sum(num1: string | number | boolean) {
    //code
}
function sum2(nums: (string|number)[]){
    //code
}
```



### Void类型

当函数没有返回值时，就是void类型，可以不写，ts会自动推导

```typescript
function sum(num1: number, num2: number): void {
    // code...
}
```



### enum枚举类型

枚举其实就是将一组可能出现的值，一个个例举出来定义在一个类型中

枚举允许开发者定义一组命名常量，常量可以是数字、字符串类型

定义的LEFT，RIGHT等常量打印出来本质上是0,1,2,3这样的数字
可以在定义时修改 LEFT = 100, RIGHT = 200 数字初始化

LEFT = 'LEFT' RIGHT = 'RIGHT' 字符串初始化

```typescript
enum Direction {
    LEFT,
    RIGHT,
    TOP,
    BOTTOM
}
// 定义函数 传入类型限制为枚举类型
function turnDirection(direction: Direction){
    
}
//调用方法，传入枚举值
turnDirection(Direction.LEFT)
```





### any类型

有时候不希望tsc太严格，对于部分变量或者数据开个后门，就可以声明Any 任意类型

```js
// var重复定义
var a:any = 1
var a:any = 2

// 定义any数组
let arr:number[] = [1,2,3]
let brr:string[] = ['hh','xx','ccc']
let crr:any[] = ['hh',123,'ccc',44]
```

### 泛型

类型的参数化

在定义这个函数时不决定参数的类型，而是让调用者以参数形式告知类型

```typescript
// Type用来接收参数类型
function sum<Type>(num: Type): Type {
    return num
}

//调用方式1：明确传入类型
//调用的时候传入参数类型
sum<number>(100)
//对象类型
sum<{name: string}>({name: 'david'})
//数组类型
sum<any[]>(['123','321'])

//调用方式2：不传类型，会进行类型推导 
sum(50) // 推导为字面量类型 非number类型，但也满足条件

//传多个类型
function sum2<T, E>(num1: T, num2: E){
    //code
}
sum2<number, string>(10,'abc')
```

### 接口泛型

```typescript
interface Person<T1, T2> {
	name: T1,
    age: T2
}
// 在外部定义类型
const p1: Person<string, number> = {
    name: 'david',
    age: 18
}

//设置默认类型
interface Person<T1 = string, T2 = number> {
	name: T1,
    age: T2
}
```



### 类泛型

person 类实现了 GenericInterface<T>，而此时 T 表示 Number 类型，因此等价于该类实现了 GenericInterface<Number> 接口；
而对于 GenericInterface<U> 接口来说，类型变量 U 也变成了 Number。这里我有意使用不同的变量名，以表明类型值沿链向上传播，且与变量名无关

```typescript
interface GenericInterface<U> {
    value: U
    genericFn: () => U
}
class Person<T> implements GenericInterface<T> {
    value: T
    constructor(value: T){
        this.value = value
    }
    
    genericFn(): T {
        return this.value
    }
}

const p = new Person<number>(33)
const p2 = new Person<string>('david')
```



### 类型约束

```typescript
interface Length {
    length: number
}
//泛型继承Length接口类型，进行类型约束
function getLength<T extends Length>(arg: T){
    return arg.length
}

getLength(123) //报错，没有length属性
getLength('abc') //ok, 字符串有length属性
getLength({length: '100'}) //ok 对象中也有length属性
```





### 类型断言 as

类型断言的意义：有时候TS无法获取具体的类型信息，就要使用断言

比如通过doucment.getElementById, TS只知道该函数返回**HTMLElement**,并不知道具体的类型

```typescript
//实例1 不使用断言只能返回HTMLElement类型，太广了
//用断言把对象设置为HTMLImageElement类型， 才能使用src属性
const el = document.getElementById('david') as HTMLImageElement
// 此时给el的src元素赋值不会报错
el.src = 'xxx.png'

// 实例2
class Person {
    
}
// Student继承Person， Person也是Student的父类
class Student extends Peroson {
    studying() {
        
    }
}
function sayHello(p: Person) {
    //直接使用p.studying()是调用不了的，因为编译器判断传入的参数是一个person类型，
    //所以要使用类型断言成student才能调student的方法
    (p as Student).studying()
}
const stu = new Student()
sayHello(stu) //student继承了person，所以stu也能传入该函数
```

```js
类型断言的2种方式
<类型>值

// 或者

值 as 类型
```

```js
//写法1
let a:number = 1
let b:any = <number>a //赋值
//写法2
let a:number = 1
let b:any = (a as number)
```

### 非空类型断言!

```typescript
function fn(message?: string) {
    // 此处不加!会编译报错，因为message可能是undefined
    // 加了! 表示message一定会传进来，不会有空的情况，保证编译通过
    console.log(message!.length)
}
```

### 字面量类型

```typescript
// 'hello world', 123 其实也是一个类型，叫做字面量类型
const message: 'hello world' = 'hello world'
let num: 123 = 123

// 字面量类型单独使用时没有任何意义，必须结合联合类型使用
type alignType = 'left'|'center'|'right'
let align:alignType = 'left'
algin = 'center' //ok
align = 'xxx' //报错
```



### 类型断言实例

```typescript
// 定义Method为字面量类型
type Method = 'GET'|'POST'
function request(url: string, method: Method) {
    //code
}
const options = {
    //此处的method是一个string类型
    method: 'POST',
    url: 'http://xxxx.com/abc'
}
// 此处要加类型断言把类型缩小定义为Method，才不会报错
request(options.url, options.method as Method)
```

### TS类型申明 .d.ts文件

**.d.ts**的文件 :  TS类型申明文件(declare),仅用来做类型检测，告诉TS我们有哪些类型

来自3个地方：

1.内置类型申明(ts自带，声明了js运行时需要的类型 Math Date window等对象)

 2.外部定义类型申明 (axios这种第三方库自带的)

3.自己定义类型申明

**导入第三方库时没有类型声明文件时就会报错，如lodash**

解决方案：

1.在自己库中编写.d.ts文件

```typescript
// src根目录下创建lodash.d.ts
//申明lodash模块
declare module 'lodash' {
    // 申明一个join的函数类型
    export function join(arg: any[]): void
    
}
 //声明变量/函数/类
declare let name: string
declare let age: number
declare function foo(): void

declare class Person {
    name: string
    age: number
    constructor(name: string, age: number)
}

//声明文件
declare module '*.jpg'
declare module '*.png'
```

2.通过社区的公有库DefinitelyTyped存放类型声明文件

安装声明文件**npm install @types/lodash -dev**





### 函数形参 剩余参数写法

```typescript
//表示传入名为nums的数组，类型是number，可传入任意个
function sum(...nums: number[]){
    let total = 0
    for(let num of nums){
        total += num
    }
    return total
}
sum(1,2,3)
sum(1,2,3,4,5,6)

```



### 函数重载(ts独有)

定义：1.函数名一样，传入的参数不一样

2. 函数名和函数体分开定义

```typescript
//函数重载&联合类型 案例
//实现方式1 联合类型
function getLength(args: string|any[]){
    return args.length
}
getLength('abc') //3
getLength([123,321,123]) //3

//实现方式2 函数重载  单独申明函数、入参、函数返回类型
function getLength(args: string):number
function getLength(args:any[]):number
//函数体定义 入参范围扩大
function getLength(args: any){
    return args.length
}

```





### (es11特性)可选链?.

```typescript
type Person = {
    name: string,
    // 可选类型friend
    friend?: {
        name: string,
        age: number
    }
}
const info: Person = {
    name: 'david',
    friend: {
        name: 'kobe'
    }
}
//此处使用?. 可选链，当没有friend属性属性时会短路，不执行后续代码
console.log(info.friend?.name)
```



### ES11特性 ??

当操作符左侧是null或者undefined时，返回右侧，否则返回左侧

比条件运算符更简洁

```js
let message: string|null = null
const content = message ?? '我是默认值'
//等价于条件运算符
const content = message ? message : '我是默认值'
```







### 函数参数解构赋值

函数的参数在类型监测的时候，用解构赋值，需要冒号  ' : '

```typescript
function f([first,second]:[number,number]){
    console.log(first)
    console.log(second)
}
```



### 函数返回的嵌套对象解构

```js
function zoo(){
    return {
        type: 'animal',
        items: 100,
        func: {
            func1: 1,
            func2: 2
			}
    }
}
let {type,items, func:{func1,func2}} = zoo() //嵌套的对象用冒号解构
```



### ts组件传参

（1）子组件中定义要传过来的参数，该参数要有一个类型约定（interface），这里要导出是因为父组件要用到该接口：

```typescript
export interface ColumnProps {
  id: number;
  title: string;
  avatar: string;
  description: string;
}
```

（2）子组件中的props属性如何定义该传过来的参数：

```typescript
props:{
   list: Array as PropType<ColumnPros[]>,
   required:true  
}
import { defineComponent, PropType } from 'vue'
```

说明：

- 这个props中不能直接使用Array as ColumnProps[]，因为Array是一个数组的构造函数，不是一个类型，所以不能断言成一个类型
- vue中**PropType**的用法：
  - 接受一个泛型，可以将一个array的构造函数返回传入的泛型类型
  - 可以把一个构造函数断言成一个类型



### element-plus按需引入

```typescript
// global/register-element.ts 按需导入的执行文件
import {App} from 'vue'
import 'element-plus/lib/theme-chalk/base.css'
import {
    ElButton,
    ElForm,
    ElInput,
    ElRadio
} from 'element-plus'

const components = [
    ElButton,
    ElForm,
    ElInput,
    ElRadio
]
// 遍历所有导入的组件，进行组件注册
export default function(app: App): void {
    for(const component of components){
        app.component(component.name, component)
    }
}



// global/index.ts
import { App } from 'vue'
import registerElement from './register-element'

export function registerApp(app: App): void {
    registerElement(app)
}


//main.ts 入口文件
import {registerElement} from './global'
import {App} from './App.vue'
import {createApp} from 'vue'
const app = createApp(App)
// 这里注册引入的element组件
registerElement(app)
```



### 拥有构造函数实例的类型

```typescript
// typeof Person先获取构造函数的类型，再通过InstanceType获取拥有构造函数的实例
type Bar = InstanceType<typeof Person>
const p = ref<Bar>()
```

