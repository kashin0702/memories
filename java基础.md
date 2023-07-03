# java基础

## java版本

javaSE: java标准版，用于桌面应用开发

javaME: 嵌入式设备，小型移动设备应用开发(早期塞班系统的应用，已凉)

javaEE(重点): java企业版 web方向网站开发(浏览器+服务器) 服务器开发领域no1

## cmd常用命令

**盘符+冒号**  盘符切换 

**dir**   查看当前路径下内容

**cd 目录名**  进入单级目录

**cd..**    作用：回退到上级目录

**cd 目录1\目录2\...**    进入多级目录

**cd \ **  回退到盘符目录

**cls **  清屏

**exit ** 退出命令提示符窗口

**环境变量作用：在任意目录下打开指定的软件**

配置环境变量: 高级系统设置==》环境变量==》系统变量==》选择path==》点击编辑==》点击新建保存程序路径



## 安装jdk

下载官网：www.oracle.com

1.路径不要包含中文

2.开发工具放统一目录

**java17安装时会自动配置4个环境变量(java,javac,javaw,jshell)**

**一些重要文件：**

jdk/bin/javac 用来编译文件的工具

jdk/bin/java 用来执行文件的工具

```java
public class HelloWorld{
    // public static void main是程序主入口
    public static void main(String[] args) {
        System.out.println("helloworld");
    }
}
```

如何运行java程序

1、在要运行的文件目录下打开cmd  执行javac  xxx.java  编译源文件，会生成一个class文件

2、同目录下执行java xxx (不加后缀名) 即可看到运行结果

**jdk和jre**

jdk包含了jvm,核心类库，开发工具(javac,java,jhatjdb)等调试工具

jre:只需要运行，不需要开发时使用,就是java运行环境，包含了jvm和核心类库这些运行必备环境，去除了jdk中的开发工具的环境包



## java环境变量配置

1、新建系统变量JAVA_HOME，变量值设置为jdk安装路径如E:\develop\jdk

2、系统变量Path内增加一条引用：%JAVA_HOME%\bin (%%表示引用) 



## 编译型与解释型语言区别

1、编译型语言整体编译后产生一个新文件，交给机器执行（c++/c）

2、解释型语言利用解释器按行解释，交给机器执行，不会产生新文件(python,  js)

java属于混合型语言，java编译后产生的class文件是运行在java虚拟机环境上的(实现了java跨平台的能力)





## class

用于定义/创建一个类， 类是java最基本的组成单元





## 字面量类型

字面量：数据在程序中的书写格式

java中有6种字面量类型：整数，小数，字符串，字符，布尔，空

![image-20230628153909801](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230628153909801.png)



## 制表符 \t

打印时把前面字符串长度补齐到8，或8的整数倍。最少补1个空格，最多补8个空格

![image-20230628154307751](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230628154307751.png)



## 变量定义格式

数据类型 变量名= 数据值

```java
int a = 10;
double b = 10.2;
```

## 不同进制的表现形式

二进制：代码中以0b开头

十进制：不加任何前缀

八进制：代码中以0开头

16进制：代码中以0x开头



## 进制转换

![image-20230628163008744](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230628163008744.png)

![image-20230628163351983](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230628163351983.png)



## 计算机存储过程

**任意数据都以二进制存储**

**计算机存储数据大致分为3类，文本，图片，音频， （视频其实就是许多图片+音频的集合）**

**1、文本text(数字，字母，汉字等)**

数字==》直接转成二进制

'a'英文字符==》查找ASCII码表对应的数字==》转成二进制

汉字 ==》查找unicode码表对应的数字==》转成二进制

**2、图片image**

图片由像素点组成，像素点由3原色组成，3原色可以用(255,255,255)数字表示

**3、声音sound**

用数字记录采样点，音质越高，采样点越多



## 数据类型

### 1、基本数据类型

```java
//定义long,float类型要加后缀标识L,F
long b = 9999999L
float z = 10.33F

```



**基本类型(整数/浮点/布尔/字符)**

数据值存储在变量自己的内存空间, 变量保存的是真实值(栈内存)

| 数据类型 | 关键字  |
| -------- | ------- |
| 整数     | byte    |
| 整数     | short   |
| 整数     | int     |
| 整数     | long    |
| 浮点数   | float   |
| 浮点数   | double  |
| 字符     | char    |
| 布尔     | boolean |

![image-20230628170513944](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230628170513944.png)



### 2、引用数据类型

除了基本4个类型的,都是引用数据类型

1. **new关键词创建的都是引用数据类型(变量保存的是内存地址,数据存在堆内存中)**




## 开发工具IDEA

目前java市场占有率最高的集成开发工具

http://www.jetbrains.com/idea/

**idea项目结构**

idea开发目录按以下结构创建

项目-模块-包(目录)-类

project-->modules-->package-->class



## 导入工具类

```java
import java.util.Scanner; // 导入Scanner类,该类的作用是记录键盘选择的数字

public class myImport{
	public static void main(String[] agrs){
		Scanner sc = new Scanner(System.in);
		System.out.println("请输入第一个数字");
		int number1 = sc.nextInt(); // 接收键盘选择的数字
		
		System.out.println("请输入第二个数字");
		int number2 = sc.nextInt();
		System.out.println(number1+number2); // 打印之和
	}
}
```



## 算术运算符

注意点:

### 算术运算符+号

1.char字符型会转换为ascii码对应数字,再进行计算

2.byte,short,char类型参与计算,会统一提升为Int类型再参与计算

3.任意类型与string字符串型相加都会变成字符串型(字符串拼接, +号理解为拼接符, 字符串只有加操作,没有减乘除)

取值范围从小到大: **byte < short < int < long <float < double** 

隐式转换: 不同数据类型相加, 取值范围小的会自动变大(自动类型提升)

强制转换: 把大的范围类型赋值给小的范围,需要强制转换 (可能导致数据错误)



隐式转换底层: 如int提升为long 底层是在int(4字节32位)前补32个0, 变成long(8个字节64位)

强制转换底层: 如int降低为byte 底层是把int(32位)前的24位直接去掉, 变成byte(1个字节8位)

```java
public class Arithmetic{
	public static void main(String[] agrs){
        System.out.println(10/3) // 整数参与计算,返回整数3
        System.out.println(10.0/3) // 小数参与计算,返回3.333333333333335
        int a =5
        double b = a
        System.out.println(b) // double型5.0 自动提升为double型
        char c = 'a';
        int z = 12;
        System.out.println(c+z) //int型109 字符型会转换为ascii码对应数字,再进行计算
        byte b1 = 10;
        byte b2 = 20;
        System.out.println(b1+b2) // 统一转为int型,30
        // 强制转换
        int a = 300;
        byte b = (byte) a; // 结果为负数,int范围超过了byte
        
        // 字符串拼接
        System.out.println(1 + 2 + "abc" + 2 + 1) // 3abc21  从左往右先加法1+2,碰到字符串拼接,后面即都为字符串拼接
	}
}
```





### 赋值运算符

=, +=, -=, *=, /=, %=

```java
public class Arithmetic {
    int a = 10;
    double b = 20.0;
    a += b; // 等同于a = (int) (a + b)  +=,-=,*=,/=,%=底层都隐藏了一个强制类型转换 
    System.out.println(b) // 30 b是double型,会强制转为a的int类型
}
```

### 关系运算符

关系运算符的结果都为Boolean型

<  >= <=

== 判断两边**值**是否相等

!= 判断两边**值**是否不等



### 逻辑运算符(不常用)

**&| : 不管左侧true false 右侧都会执行**

& 逻辑与(且),  并且,两边都为真,结果才为真

| 逻辑或, 或者,两边都为假,结果才为假

^ 逻辑异或 相同为false,不同为true

! 逻辑非  取反 (java中只用一个!  不要用!!)

### 短路逻辑运算符

**含义:左侧表达式能确定最终结果,右侧就不会执行**

&&  短路与  结果和&相同,但具有短路效果, 左侧为false时,右侧不执行 效率更高

||   短路或  结果和|相同, 但具有短路效果, 左侧为true时,右侧不执行 效率更高



### 运算符优先级

![image-20230630164354777](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230630164354777.png)

## 原码 补码 反码

计算机的最小储存单位是byte, 即8位 所以1个字节的储存范围是**-128~127**

正整数 :
	原反补码相同，都是正整数的二进制表示
负整数 :
 原码 :
  数值的二进制
 反码:
 	原码各个位取反
 补码:
  反码的基础上加1

原码: 十进制数据的二进制表现,最左边是符号位,0为正, 1为负

原码对正数计算没问题,但计算负数时,实际运算结果与预期是相反的

反码:解决原码不能计算负数的问题

反码计算规则: 正数的反码不变,负数的反码在原码的基础上,符号位不变,数值取反,0变1,1变0

反码中0有两种表现形式, 负数跨0计算时会产生1的误差(正数计算不会产生误差)

补码: 在反码的基础上+1 错开一位, 屏蔽误差

补码产生一个特殊值-128, 该数据在一个字节下,没有原码和反码

**数字的存储和计算都是以补码的形式来操作的**

![image-20230630172727088](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230630172727088.png)



### 循环for和while使用场景

for循环：知道循环次数或者循环的范围

**循环体语句执行后会执行条件控制语句，再进入下一次条件判断**

for (初始化语句；条件判断语句；条件控制语句) ｛

​	循环体语句；

｝

while循环：不知道循环次数或者范围，只知道循环结束的条件

初始化语句；

while (条件判断语句) ｛

​	循环体语句；

​	条件控制语句；

｝

### 回文数判断

从左往右和从右往左读是一样的整数，121是回文数

```java
import java.util.Scanner;

public class ChargeNumber {
    public static void main(String[] args) {
        // 思路：把数字倒过来和原来数字进行比较
        Scanner sc = new Scanner(System.in);
            System.out.println("请输入一个整数");;
        int inNumber = sc.nextInt();
        int temp = inNumber; // 保存输入值
        int x = 0;
        while(inNumber != 0) {  // 当截取输入数字为0时结束循环
            // 每次循环获取输入数字的末位，进行拼接
            int last = inNumber % 10; // 获取当前数的末位
            inNumber = inNumber / 10; // 循环结束前截掉末位，给下次循环时使用
            x = x * 10 + last // 把当前数拼到最右侧
        }
        System.out.println(x == temp);
    }
}
```



### 生成随机数

```java
import java.util.Random;

public class random {
    public static void main(String[] args) {
        Random r = new Random();
        for (int i = 0; i < 10; i++) {
            int number = r.nextInt(100); // 随机数从0开始，不含末位 100表示随机数范围是0~99
            System.out.println("随机数是:" + number);
        }
    }
}
```

### 猜数字1~100

```java
public class random {
    public static void main(String[] args) {
        // 猜1~100数字
        Random r2 = new Random();
        int number2 = r2.nextInt(100); // 生成一个随时数
        Scanner sc = new Scanner(System.in);
        while (true) { // 利用无限循环比较猜的数字和随机数
            System.out.println("请输入你要猜的数字"); // 每次进入循环重新输入数字
            int guess = sc.nextInt();
            if (guess > number2) {
                System.out.println("太大了");
            } else if (guess < number2) {
                System.out.println("太小了");
            } else if (guess == number2) {
                System.out.println("猜对了！");
                break;
            }
        }
    }
}
```



## 数组

数组是一种容器，**在存储数据时要考虑存在隐式转换**，所以容器的类型和存储类型最好保持一致

数组定义的2种格式

格式1：数据类型[] 数组名

格式2：数据类型 数组名[]

静态初始化完整格式：数据类型[] 数组名 = new 数据类型[]{元素1，元素2，...};

动态初始化：数据类型[] 数组名 = new 数据类型[数组长度];

 int默认值是0，double默认0.0， char默认'/u0000'空格， 布尔默认false，引用类型默认null（String是引用类型） 

静态初始化指定数组元素，系统根据元素个数计算数组长度

动态初始知道数组长度，系统给出默认初始化值

```java

public class array01 {
    public static void main(String[] args) {
        // 数组静态初始化
        // 定义数组写法1
        int arr1[] = new int[]{11,22,33,44};
        for (int i = 0; i < arr1.length; i++) {
            System.out.println(arr1[i]);
        }
        // 写法2
        String[] arr2 = new String[]{"david", "kashin"};
        for (int i = 0; i < arr2.length; i++) {
            System.out.println(arr2[i]);
        }
        // 右侧new省略写法
        String arr3[] = {"KING", "BILLY"};
        System.out.println(arr3[1]);
        
        // 动态初始化
        int[] arr4 = new int[10]; // 仅指定长度10，不指定具体初始化值
    }
}

```

### 取最值

```java
// 获取数组中的最大值
public class array02 {
    public static void main(String[] args) {
        int[] arr = {11,34,56,100,92,234};
        // 把第一个值作为初始化比较的值
        int max = arr[0];
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i]; // 循环所有值，只要比上一个大就记录
            }
        }
        System.out.println(max);
    }
}
```

### 数组元素交换

```java
// 数组12345变成54321
// 思路：利用两个变量循环头和尾，遍历数组进行交换
public class looptest01 {
    public static void main(String[] args) {
        // 把数组12345变成54321
        int[] arr = {1,2,3,4,5};
        int[] newArr = new int[5];
        // 定义2个索引,从头尾同时遍历,进行数据互换 实现倒排
        for(int i = 0, j = arr.length - 1; i == j || j > i; i++, j--) {
            int temp = arr[i];
            newArr[i] = arr[j];
            newArr[j] = temp;
        }
        for(int i = 0; i < newArr.length; i++) {
            System.out.print(newArr[i] + " ");
        }
    }
}
```



### 二维数组

定义方式

```java
// 静态初始化
int[][] arr = new int[][]{{元素1， 元素2}, {元素1，元素2}}
// 简化
int[][] arr = {{元素1，元素}，｛元素1，元素}}

// 动态初始化
int[][] arr = new int[m][n] //m表示可以存放多少个一维数组 n表示每个一维数组可以存放多少个元素
    
// 特殊动态初始化 
int[][] arr = new int[2][] // 一维数组长度不定义 好处是一维数组长度可以不一样
int[] arr2 = {11,22,33}
int[] arr3 = {44,55}
arr[0] = arr2
arr[1] = arr3

// 特殊情况2 定义了二维数组中一维数组的长度为2
int[][] arr = new int[2][2]
int[] arr2 = {11,22,33}
int[] arr3 = {44,55}
arr[0] = arr2
arr[1] = arr3 // 本质是arr3内存地址给了arr[1], 原来初始化时的内存地址被替换
```





## 方法

形参: 方法定义中的参数

实参: 方法调用中的参数

**return含义:**  

**必须写在方法中**

**方法没有返回值时可省略,不省略的话不能跟返回值, 表示结束方法**

**方法有返回值时不能省略, 表示结束方法和返回结果**

```java
public class module01{
    public static void main(String[] args) {
        
    }
    
    // 方法定义和主程序main是平级关系
    //定义方法格式: 无参数
    public static void getData() {}
    // 带参数
    public static void getSum(int num1,int num2) {
        int res = num1 + num2;
        System.out.print(res);
    }
    // 带返回值 定时写上返回值类型
    public static int getSum2(int num1, int num2) {
        int res = num1 + num2;
        return res; // 返回数据
    }
}


```

### 方法重载

**重载的含义: 同一个类中,方法名相同,参数不同的方法, 与返回值无关**

不用定义太多名字不同的方法, 方便调用

```java
public class Demo {
    public staic void main(Stirng[] args) {
        compare(10, 20) // 第一个compare方法会被调用
        compare((byte)10, (byte) 20) // 第3个方法会被调用
    }
    
    // 这3个就是方法的重载,在同一个类,方法名相同,参数不同
    public static void compare(int num1, int num2) {
        System.out.print(num1 == num2);
    }
    public static void compare(double num1, double num2) {
        System.out.print(num1 == num2);
    }
    public static void compare(byte num1, byte num2) {
        System.out.print(num1 == num2);
    }
}
```

### 数组拷贝

```java
public class Demo {
    public static void main(String[] args) {
        int[] arr = {1,2,3,4,5,6,7,8};
        int[] copyArr = copyOfRange(arr, 3, 7); // 获取索引值3~7的数组
    }
    // 根据指定索引返回新数组
    public static int[] copyOfRange(int[] arr, int from, int to) {
        if (to > arr.length) to = arr.length;
        int len = to - from;
        int[] newArr = new int[len]; // 计算新数组长度
        int idx = 0;
        for (int i = from; i < to; i++) {
            newArr[idx] = arr[i];
            idx++;
        }
        return newArr;
    }
}
```



### 质数判断

```java

public class Demo {
    public static void main(String[] agrs) {
        int result = getPrimeNum();
        System.out.print("101~200一共有" + result + "个质数");
    }
    // 判断质数个数并列出
    public static int getPrimeNum() {
        int count = 0;
        for (int i = 101; i <= 200; i++) {
            boolean flag = true;
            // 判断质数,如101 循环2~100能否整除101
            for (int j = 2; j < i; j++) {
                if (i % j == 0) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                count++;
                System.out.print(i + " ");
            }
        }
        return count;
    }
}

```

### 随机验证码生成

```java
public static void getRandomCode() {
    // 思路: 利用acs码表把数字强转成字母,创建一个52位字母的数组, 再利用随机索引实现字母的随机
    char[] codes = new char[52];
    for (int i = 0; i < codes.length; i++) {
        // 大写字母asc 65~90
        if (i <= 25) {
            codes[i] = (char)(i + 65); // 数字强制转换为字符
        } else {
            // 小写字母 97~122
            codes[i] = (char)(i + 71);
        }
    }
    // 第二步 生成前4个随机字母
    Random ran = new Random();
    String checkCode = ""; // 验证码
    for (int i = 0; i < 4; i++) {
        int idx = ran.nextInt(52);
        checkCode += codes[idx];
    }
    // 生成最后一位数字
    int num = ran.nextInt(10);
    checkCode += num;
    System.out.println(checkCode);
}
```

### 数字加密

```java
public class encodingPwd {
    public static void main(String[] args) {
        int res = changeNum(1983);
        System.out.println();
        System.out.println("加密后数字为:" + res);
    }

    // 数字加密
    // 规则：1、每位数字+5 2、取10余数 3、数字反转
    public static int changeNum(int num) {
        // 1、先计算Num的个数 用于创建数组长度
        int count = 0;
        int tempNum = num;
        while(num != 0) {
            num /= 10;
            count++;
        }
        // 2、创建数组并对每位数字赋值
        int[] arr = new int[count];
        int idx = count - 1;
        while (tempNum != 0) {
            int ge = tempNum % 10;
            tempNum /= 10;
            arr[idx] = ge;
            idx--;
        }
        // 3、执行加密规则
        for (int i = 0; i < arr.length; i++) {
            arr[i] += 5;
            arr[i] %= 10;
        }
        // 4、反转数组
        for (int i = 0, j = arr.length - 1; i < j; i++, j--) {
            int arrTemp = arr[i];
            arr[i] = arr[j];
            arr[j] = arrTemp;
        }
        // 5、数组转回数字
        int newNum = 0;
        for (int i = 0; i < arr.length; i++) {
            newNum = newNum * 10 + arr[i];
        }
        return newNum;
    }
}
```



## 面向对象

javabean类：用来面熟一类事物的类叫javabean类； javabean类中不写main方法

测试类：编写main方法的类叫测试类，**在测试类中可以创建javabean类的对象并进行赋值调用**

一个java文件内可以定义多个类，但只能有一个public类， pubic修饰的类名必须和文件名一样。

 

```java
// 定义类
public class Student {
    // 成员变量 完整格式:修饰符 数据类型 变量名 = 初始化值 一般不指定初始化值，在创建实例时赋值
    String name;
    double height;
    
    // 成员方法(行为) 
    public void study() {}
    public void sleep() {}
}
```

