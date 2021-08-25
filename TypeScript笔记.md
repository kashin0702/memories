### 基本类型检测 

变量屁股后跟上 :类型

```typescript
let a:number = 1  //ok
let b:string = 1 //type error

```

### 数组类型检测

```typescript
let arr:number[] = [1,2,3] //Number型数组ok
let brr:Array<number> = [4,5,6]  //ok 数组泛型:Array<元素类型>
```

### 元组Tuple类型检测

元组类型适用于已知个数和元素类型的数组

```js
let X:[number,string];
x = [1,'hello'] //ok
x = [1,2]  //type error

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



### 枚举

`enum`类型是对JavaScript标准数据类型的一个补充。 使用枚举类型可以为一组数值赋予友好的名字。

```typescript
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
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



### 类型断言 

类型断言的意义：先做好一个假设，使得编译通过，**类型断言更像是类型的选择，而不是类型转换**

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