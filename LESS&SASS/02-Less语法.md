### Less的语法

#### 1. 注释语法

##### 1.1  /**/  (多行注释)

这种注释是css的注释,编译以后,会保留显示在css文件中

##### 1.2   //    (单行注释)

// 这种代码注释css并不识别,编译后会影藏,不会显示在css文件中



#### 2. 变量(重点)

在less 中,可以使用变量,来统一设置一类可以重复使用的值,方便后期CSS代码维护



##### 2.1 普通变量的使用

使用@符号定义变量,

语法:

> @变量名: 变量值;

注意:变量值一定要符合CSS属性值的规范

在CSS选择器中后面是用变量,如: 属性的名称: @变量

```less
// 定义变量
@baseColor: #369;

// 使用变量
.box{
    color: @baseColor;
    border: 1px solid @baseColor;
}
```



##### 2.2 在字符串中使用变量

 如果需要变量名和其他字符串拼接,

使用一下语法

> "@{变量名}字符串"

```less
@images: "../img";

body{
    background:url("@{images}/1.png")
}
```



##### 2.3  选择器使用变量

语法

> @{变量名}{
>
> }

```less
@wuwei: #wrap;

@{wuwei}{
    color:red;
}
```



##### 2.4 属性变量

语法

> 选择器{
>
> ​	@{变量没}: 值
>
> }	

```less
@cc: color;
.wrap{
    @{cc}: red;
}
```





##### 2.5 导入其他的less

语法

> @变量名: 地址;
>
> @import "@{变量名}";

```less
@src: "./header.less";
@import "@{src}";
```

注意,就是将其他的less 引入到自己的less中,最后统一编译成一个css文件



##### 2.6 变量作用域

每个元素的css样式的{}都是一个独立的作用于, 按照js函数作用于来理解

语法

```less
@color: red;
.wrap{
    color:@color; // .wrap自己作用于内没有color变量,所以用上一次作用域重点变量
}
.box{
    @color: skyblue;
    color:@color; // 因为.box自己作用于中有同名变量,就会先用自己作用于内变量的值
}
```

注意变量会被预解析

就是使用后定义也没关系

```js
// 先使用变量
.wrap{
    color:@color; 
}

// 后定义变量
@color: red;
```



##### 2.7 变量的计算

变量的值可以参与运算

```less
@width: 300px;
.box{
    width:@width - 100;
    height:@width;
    background: skyblue;
}
```







#### 3. 混合(重点)

Mixins 有点像函数,在定义后,可以通过名称调用.(也支持动态传参)

混合可以将一个定义好的class A 轻松 的引入到另一个class B 中,从而简单实现class B继承class A 中的所有属性,我们还可以带参数地调用,就像使用函数一样



##### 3.1 什么是Mixins混合

简单理解就是函数,可以封装CSS代码,在别的选择器中调用,提高代码的重用性和可维护性



##### 3.2 定义混入

语法:

> 1.无参数混入定义
>
> .混入名 () {
>
> ​	封装的CSS代码
>
> }
>
>  
>
> 2.有参数混入
>
> .混入名(@参数: 默认值){
>
> ​	封装的CSS代码
>
> }

```less
// 定义混入
.error(@color: #999, @size: 16px) {
  background-color: @color;
  font-size: @size;
}
```





##### 3.3 调用混入

语法

> 选择器{
>
> ​	混入名(@参数)
>
> }

```less
// 使用混入
@baseColor: #369;
.box p {
  .error(@baseColor, 30px)
}
```



##### 3.4 如果混入没有参数可以不用加括号

```less
.wrapProp(){
    width:200px;
    height:200px;
    border:1px solid red;
}

.wrap{
    .wrapProp() // 加括号 可以执行mixin
}
.box{
    background: skyblue;
    .wrapProp;  // 没有参数不加括号也可以执行mixin
    width:100px;
}
```

如果mixin需要动态传参数, 则必须加括号传参

```less
.wrapProp(@wi,@height,@color){
    width:@wi;
    height:@height;
    border:1px solid @color;
}

@width:200px;
@height:200px;
@color:red;

.wrap{
    .wrapProp(@width,@height,@color);
}
.box{
    background: skyblue;
    .wrapProp(300px,100px,blue);
    // width:100px;
}
```





##### 3.5 less可以直接继承其他选择器中的样式

```less
.wrap{
    width:200px;
    height:200px;
}
.box{
    .wrap; // 这里将会继承.wrap的样式
    color:red;
}

// Mixin里除了可以写样式,选择器, 还可以写变量
.mixin() {
  @width:  100%;
  @height: 200px;
}

.caller {
  .mixin();
  width:  @width;
  height: @height;
}

// Mixins 里除了返回变量,还可以返回Mixins
#box(@color){
    .wrap(){
        width:500px;
        height:100px;
        background:@color;
    }
}

.box{
    #box(red);  // 执行第一个Mixin是为了能够使用里面的Mixin
    .wrap();   // 第二个执行Mixin里面的Mixin
}
```



##### 3.6 可以在Mixin中使用选择器

```less
.hover(@color){
    &:hover{
        background:@color;
    }
}
.wrap{
    width: 100px;
    height: 100px;
    background: red;
    // &:hover{
    //     background:skyblue;
    // }
    .hover(skyblue)
    
}


.box{
    width: 200px;
    height: 200px;
    margin-top:20px;
    background: skyblue;

    // &:hover{
    //     background:red;
    // }
    .hover(red)
}

```





##### 3.7 Mixins的命名空间

说白了  就有点类似于js中的作用于的问题,

```less
.wrap{
    width:300px;
    .inner{
        font-size:30px;
        color:pink;
    }
    #box(){
        width:200px;
        height:200px;
    }
}

.box{
    .wrap .inner;
    .wrap #box();
    // .inner
    // #box()  // 这两种写法是错误的
}
/*
 如果.box要继承 .wrap里面 .inner 或者#boxMixin函数的样式, 直接是找不的, 必须通过先找到.wrap 然后在通过.wrap 去找Mixin 或inner样式
*/
```





##### 3.8 Mixin的默认值与不定参

###### 1. 默认值

Less 可以使用默认参数，如果没有传参数，那么将使用默认参数。 

如果不是默认值, 在没有传参的情况下, Mixin参数没有值就会出错,所以实参和形成的个数必须保持一致

如果希望Mixin可以在更多场合复用, 可以使用默认值,如果没有传参,则使用默认值

```less
#box(@width : 100px, @height : 100px, @color:red) {
    width: @width;
    height: @height;
    background-color: @color;
}

.box{
    #box(50px,50px,pink) ; // 这里使用自己传递的参数
    width:200px;
}

.wrap{
    #box;   // 这里使用默认值
}
```





###### 2. 不定参

不确定参数的个数



```less
// ... 就是@reset 剩余参数, 会将剩余的所有参数都加到@arguments里 
.boxShadow(...){
          box-shadow: @arguments;
 }

.box{
    .boxShadow(1px,4px,30px,red);
}

// @arguments是处理第一个实参外的所有实参的集合
.boxShadow(@width,...){
          box-shadow: @arguments;
 }
.box{
    .boxShadow( 50px,1px,4px,30px,red);
}
```





##### 3.9 Mixins的判断条件

Less没有if / else 但是less具有一个when，and，not与“，”语法。

###### 1. when 表示 在使用Mixin的时候必须满足 when后面的条件

```less
#box(@width, @height, @color) when (@width > 100px){
    width: @width;
    height: @height;
    background-color: @color;
}

.box{
    #box(101px,50px,pink)
}
```



###### 2. 如果有两个必须同时满足的条件,使用 and

```less
// 这里必须满足传递的宽度和颜色必须同时同满足条件才能执行Mixin
#box(@width, @height, @color) when (@width > 100px) and (@color = pink){
    width: @width;
    height: @height;
    background-color: @color;
}

.box{
    #box(101px,50px,skyblue)
}
```





###### 3. 如果需要排除某个条件才能使用Mixin,使用not

```less
// 这里排除颜色为pink ,除了pink颜色值都可以运行Mixin
#box(@width, @height, @color) when  not (@color = pink){
    width: @width;
    height: @height;
    background-color: @color;
}

.box{
    #box(101px,50px,skyblue)
}
```



###### 4. 如果需要多个条件只要满足一个就执行Mixin,使用 逗号, 

```less
// 虽然不满足宽度大于等于100px,但是满足为了颜色是skyblue 所以Mixin会执行
#box(@width, @height, @color) when (@width >= 100px),(@color = skyblue){
    width: @width;
    height: @height;
    background-color: @color;
}

.box{
    #box(50px,50px,skyblue)
}
```





##### 3.10 Less循环

Less循环采用的类似于js的递归调用

```less
.wuwei(@length,@i:1) when (@i  <= @length){
    .item-@{i}{
        width: @i * 50px;
        height:50px;
        border:1px solid red;
    }
	
    // 递归调用的同时,改变循环变量@i
    .wuwei(@length, (@i+1));
};

.wuwei(6);
```





##### 3.11  模式匹配 

根据条件进行样式显示,类似JS中的switch

有些情况下,我们想根据传入的参数来改变混合的默认呈现,

比如

相当于定义一个相同的混合名称,根据传入其他混合名称的不同,执行不同混合分支

但是有一个公共的分支,任何一个分支都会调用

@_ 表示匹配所有,通常用于定义公共部分



语法:

模式匹配的定义

> 公共部分
>
> .fun (@_, @color){
>
> ​      // 任何分支都会执行的公共部分
>
> }
>
> .fun( s1,@color){
>
> ​      // s1 分支独有代码
>
> }



模式匹配的使用

> h1{
>
> ​	.fun(s2, @color)
>
> }



```less
// 三角形公用代码
.sjx(@_, @color, @size) {
  width: 0;
  left: 0;
  display: block;
  border: @size solid transparent;
}

// 左三角形
.sjx(l, @color, @size) {
  border-left-color: @color;
}

// 右三角形
.sjx(r, @color, @size) {
  border-right-color: @color;
}

// 上三角形
.sjx(t, @color, @size) {
  border-top-color: @color;
}

// 下三角形
.sjx(b, @color, @size) {
  border-bottom-color: @color;
}

.trangle {
  .sjx(b, red, 20px)
}
```



#### 4. 颜色函数

CSS预处理器一般都会内置一些颜色处理函数,用来对颜色值进行处理,,例如: 加亮,变暗,颜色梯度等

色彩三要素:  色相(颜色的名称,)     饱和度(鲜艳程度)    明度(亮度,明暗程度)

语法:

> lighten(@color, 10%)  比@color 亮 10% 的颜色 (亮度)  50%以后的都是黑色了
>
> darken(@color, 10%) 比@color 暗 10%
>
>  
>
> saturate(@color, 10%) 比@color 浓 10%   (饱和度)
>
> desaturate(@color, 10%)  比 @color 淡 10%
>
>  
>
> spin(@color., 10)   色相值增加10    (色相,就是颜色的名称)
>
> spin(@color, -10)   色相值减少10
>
>  
>
> mix(@color1, @color2)    混合两种颜色





#### 5.  嵌套(重点)

##### 5.1 什么是嵌套

具有层级关系的CSS样式,CSS的层级是有HTML的结构决定的



##### 5.2 嵌套的好处: 

1. 用在具有后代关系和父子 关系的选择器中
2. 减少代码量,
3. 代码结构更加清晰



##### 5.3  使用

语法:

> 父选择器{
>
> ​	父属性样式
>
> 
>
> ​	子选择器A{
>
> ​		子选择器A的样式
>
> ​	}
>
> 	子选择器B{
>
> ​		子选择器B的样式
>
> ​	}
>
> }



##### 5.4 &符号的使用

使用场景: 父子,兄弟,紧邻,伪类 选择器使用时

$符号表示父元素

```less
.box{
    width: 100px;
    height: 100px;
    .wrap{
        font-size: 16px;
        $:hover{
            color: red;
        }
    }
    
}
```





#### 6.  运算符

运算符的作用: 可以对角度,颜色,高度等进行运算

运算符的分类: 加 +,   减  - ,   乘   *,    除    /

```less
@w1:200px;
@w2:300px;
@c1: #333;
@c2: #666;

li {
    width: @w1 + @w2;
}

// 16进制的颜色值,只能在#000000 ~ #ffffff之间,如果超出了,就会自动使用最大值
li {
   background: @c1 + @c2;
}
```





例子:

```less
.button(@size){
    width: 100px * @size;
 	height: 40px * @size;
    font-size: 14px * @size;
}

// 使用
.button {
    .button(3)
}
```









