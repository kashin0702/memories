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



## 输出语句占位符

```java
public class Demo {
    public static void main() {
        System.out.printf("%s,你好啊", "张三"); // %s就是后面张三的占位符
    }
}
```





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

### JAVA内存结构

栈内存：方法执行的地方(main方法等)，基本数据类型都保存在栈内存

堆内存：new关键词创建的引用数据类型保存的地方

方法区：各种字节码文件保存的地方, 一个类想要被使用，这个类的字节码文件就要加载到方法区中临时存储



### 1、基本数据类型

```java
//定义long,float类型要加后缀标识L,F
long b = 9999999L
float z = 10.33F

```



**基本类型(整数/浮点/布尔/字符)**

数据值存储在变量自己的内存空间,在栈内存中开辟一个独立空间

 变量保存的是真实值(栈内存)

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

### 初始化值

 int默认值是0

double默认0.0

char默认'/u0000' (空格)

布尔默认false

引用类型默认null（String是引用类型） 

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

### 数组打乱

```java
// 打乱数据 本质也是元素交换
    int[] arr = {1,2,3,4,5};
    Random ran = new Random();
    for (int i = 0; i < arr.length; i++) {
        int rIndex = ran.nextInt(arr.length);
        int temp = arr[i]; // 临时变量保存数据，方便下面互换元素
        arr[i] = arr[rIndex];
        arr[rIndex] = temp;
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
// javabean类
public class Student {
    // 成员变量 完整格式:修饰符 数据类型 变量名 = 初始化值 一般不指定初始化值，在创建实例时赋值
    String name;
    double height;
    
    // 成员方法(行为) 
    public void study() {}
    public void sleep() {}
}

public class Demo {
    public static void main(String[] args) {
        // main中创建类
        Student stu = new Student();
        stu.name = "david";
        stu.height = "1.75";
    }
}
```



### main方法

main方法是java程序的主入口

**因为main方法是静态的, 所以测试类中其他方法也必须静态(静态方法只能访问静态方法和变量)**

```java
public class xxSystem { 
    // main方法其实也是属于xxSystem类中的一个静态方法, 所以main调用其他方法也必须是静态的
    public static void main(String[] args) {
        // xxx...
    }
}
```





### 封装

**对象代表什么,就要封装对应的数据,并提供数据对应的行为**



### 成员变量和局部变量

成员变量和局部变量重名,不使用this就会触发就近原则, 谁离得近就用谁

**形参也是局部变量**

使用this使用成员变量

若没有局部变量, 不使用this也会查找成员变量(成员变量作用域在整个类中有效)

this作用: 区分成员变量和局部变量

this本质: 方法调用者的内存地址

Friend f = new Friend();  此时f获得一个Friend对象的内存地址

f.xxFn()  xxFn中的this就是f的内存地址

```java
public class Friend {
    private int age; // 成员变量
    public void xxFn() {
        int age = 10; // 局部变量
        System.out.println(age) // 直接使用,触发就近原则(我认为本质还是作用域)
        System.out.println(this.age) // 打印成员变量
    }
}
```

![image-20230704115804563](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230704115804563.png)



## 全部权限修饰符

权限范围由小到大:  private < 空着不写 < protected < public

实际开发中常用private和public

![image-20230707162000404](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230707162000404.png)



### public修饰符

被jvm调用, 权限修饰符 访问权限足够大



### private修饰符

权限修饰符, 可以修饰成员变量和方法,**被修饰的成员只能在本类中访问**,需提供get/set方法让外部访问

```java
// javabean类
public class Friend {
    private String name;
    private int age;

    // private权限修饰符的属性只能在本类中访问,需提供对应方法,外部才能访问
    // 对应get/set方法获取和设置属性
    public void setAge(int age) {
        if (age >= 18 || age < 50) {
            this.age = age;
        } else {
            System.out.println("年龄不合法!");
        }
    }
    public int getAge() {
        return age;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
// 测试类
public class FriendTest {
    public static void main(String[] args) {
        Friend david = new Friend();
        david.setAge(35);
        david.setName("大卫");
        david.setAge(40);
        System.out.print(david.getAge() + " " + david.getName());
    }
}
```







### 构造方法

也叫构造函数

作用: 创建对象时给成员变量进行赋值

规则: 方法名和类名相同,大小写也要一致, 没有返回值, 没有void

**构造方法在创建对象时由虚拟机自动调用,每次创建都会调用一次**

若没写构造方法,程序也会自动创建一个空参构造方法

建议: 创建类时有参构造和无参构造都要写, 方便使用 ,  若只写一个有参构造,不传参时就会报错

```java
public class Student {
    private int age;
    private String name;
    
    // 空参构造方法
    public Student() {} 
    
    // 带全部参数构造方法
    public Student(String name, int age) {
        this.name = name
        this.age = age
    }
}

// 测试类中创建
public class Demo {
    public static void main(String[] args) {
        Student s = new Student("david", 35); // 利用构造方法直接对成员变量赋值
    }
}
```



### 标准javabean类

1.成员变量使用private修饰

2.提供空参构造和全参构造2个构造方法

3.提供每一个成员变量的get/set方法

```java
package com.david.demo02;

// IDEA中使用alt+insert可以快速创建构造方法和get/set方法

public class Goods {
    private String id;
    private String name;
    private double price;
    private int count;

    public Goods() {
    }

    // 创建对象时, 选中方法的括号按ctrl+p 查看方法接收的参数
    public Goods(String id, String name, double price, int count) {
        this.id = id;
        this.name = name;
        this.price = price;
        this.count = count;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    public int getCount() {
        return count;
    }

    public void setCount(int count) {
        this.count = count;
    }
}

```



### 键盘接收方法区别

**nextInt(), nextDouble(), next() 在遇到空格/制表符/回车就会停止接收, 这些符号后面的数据会被自动放到第二个next接收**

**nextLine() 也接收字符串, 遇到空格也能接收, 不建议和上面混用**

```java
Scanner sc = new Scanner(System.in)

int num1 = sc.nextInt(); // 接收整数 如果输入123 456  num1=123 num2=456
int num2 = sc.nextInt(); 
double num3 = sc.nextDouble(); // 接收小数
String str = sc.next(); // 接收字符串



```



### 复杂对象数组处理

```java
// javabean-student类
public class Student {
    String id;
    String name;
    int age;

    public Student() {
    }

    public Student(String id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

}


// 测试类
public class StudentTest {
    public static void main(String[] args) {
        // 创建student类对象
        Student[] arr = new Student[3];
        Student stu1 = new Student("001", "zhangsan", 15);
        Student stu2 = new Student("002", "lisi", 14);
        Student stu3 = new Student("003", "wangwu", 17);

        arr[0] = stu1;
        arr[1] = stu2;
        arr[2] = stu3;
        // 需求1: 再添加一个学生对象,添加时要求Id唯一
        Student stu4 = new Student("007", "zhaoliu", 20);
        //校验Id
        if (contains(arr, stu4)) {
            System.out.println("当前学生id已存在,请重新录入!");
        } else {
            // 可添加2种情况 1. 原数组已存满 2.原数组未存满
            int count = checkArr(arr);
            // 已存满
            if (count == arr.length) {
                // 建新数组 原数组长度+1
                Student[] newArr = new Student[arr.length + 1];
                for (int i = 0; i < arr.length; i++) {
                    newArr[i] = arr[i];
                }
                newArr[count] = stu4;
                System.out.println("已存满====>");
                printArr(newArr);
            } else {
                // 未存满, 直接添加即可
                arr[count] = stu4; // count也代表最后一个空着的索引值
                System.out.println("未存满=====>");
                printArr(arr);
            }
        }
        // 需求2: 删除指定id的学生
        // 查找id对应的索引
        int idx = getIndex(arr, "002");
        if (idx >= 0) {
            arr[idx] = null; // 删除索引对应的数据
            System.out.println();
            printArr(arr);
        } else {
            System.out.println("没找到对应Id的学生");
        }

    }

    // 定义校验id的方法,返回布尔
    public static boolean contains(Student[] arr, Student stu) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] != null) {
                if (arr[i].getId() == stu.getId()) {
                    return true;
                }
            }
        }
        return false;
    }
    // 计算数组是否存满,并返回长度
    public static int checkArr(Student[] arr) {
        int count = 0;
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] != null) {
                count++;
            }
        }
        return count;
    }
    public static void printArr(Student[] arr) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] != null) {
                System.out.println(arr[i].getId()+" "+arr[i].getName()+" "+arr[i].getAge());
            }
        }
    }
    public static int getIndex(Student[] arr, String id) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i].getId() == id) {
                return i;
            }
        }
        return -1; // 没找到则返回-1
    }
}

```



## 面向对象进阶

### static修饰符

静态属性修饰符static  

静态属性

1. 对象可共享该属性,
2. 不属于对象, 属于类
3. 随类加载而加载,优先于对象存在, 对象在new以后才会存在

静态方法

1.多用在测试类或工具类中, javabean类中很少用

2**.静态方法只能访问静态变量和静态方法, 静态方法中没有this关键字**

**非静态方法可以访问静态方法和变量, 也可以访问非静态的成员方法和变量(访问所有内容)**

调用: 属性和方法都推荐用类名.调用 

**当给静态变量赋值时, 内存中, 会在堆内存中单独创建静态变量的储存区**

```java
public class Student {
    private String name;
    private int age;
    public static String teacherName; // 所有实例对象共享该属性 1.直接用类名调用Student.teacher 2.用对象调用
}

// 测试类
public class StudentTest {
    public static void main () {
        Student.teacherName = "david"; // 静态变量赋值, 此时会在堆内存单独创建一个静态变量区保存
        Student stu = new Student();
    }
}
```





### final修饰符

final可以修饰方法, 类, 变量

修饰方法: 最终方法,不能被重写

修饰类: 最终类, 不能被继承

修饰量: 变成常量, 只能被赋值一次(常用)

```java
class Father{
    public final void show(){
        
    }
}
class Son extends Father {
    @Override
    public void show(){} // 此处报错, 不能被重写
}

// 不能被继承
public final class Person {
    
}
// final修饰的基本类型,数据值不能修改, 引用类型的内存地址不能修改,但是内部数据可以修改
final int num = 123;
num = 456; // 报错, 不能修改
final Student S = new Student("david", 25);
S.setName("张三") // 不会报错, 内存地址没变
```







### 工具类

帮助我们做一些事情,但不描述任何事物的类

```java
public class ArrUtil{
    private ArrUtil(){} // 关键: 私有化工具类本身,不让外部调用new创建对象, 工具类没有必要创建对象
   
    // 提供各种静态方法
    public static int getMax(){}
    public static int getMin(){}
}
```







## String字符串

String类是java定义好的一个类,定义在java.lang包中,是java的核心包, 使用时不需要导包

java中所有的字符串都被实例为此类对象

**字符串创建后,它的值不会改变**

String name = "david";

name = "kashin";  // 创建了一个新的字符串类赋给了name,之前的字符串没有变

### 字符串创建

```java
// 直接赋值创建
String name = "david";
String lastName = "king";
System.out.print(name + lastName); // 字符串拼接后会产生一个新的字符串,对原来的字符串没有影响

// new关键词创建
String str = new String("abc");

// 传字符数组创建
char[] chs = {'a', 'b', 'c'};
String str2 = new String(chs); // abc

// 传字节数组创建
byte[] bytes = {97,98,99};
String str3 = new String(bytes) //abc 在asc码表中查对应数字的asc码
```



### String中内存关系

**使用直接赋值创建的字符串,如果值相等,是会复用的, 变量记录的是字符串池的地址**

**使用new关键词创建的字符串,每次创建是新的对象,内存地址不同, 变量记录的是堆内存中的地址**

```java
String name = "abc";
String name2 = "abc";
System.out.print(name == name2) // true 直接赋值的字符串指向串池地址, 地址相同

// new创建的内存地址在堆内存中, 地址不同
String name3 = new String("abc");
String name4 = new String("abc");
System.out.print(name3 == name4) // false  new方式创建会开辟新的内存空间,所以内存地址不相等
```



### 字符串比较

**基本类型使用==号比较的是值**

**引用类型使用==号比较的是内存地址**

```java
// 要比较字符串值, 使用equals()方法
String name = "abc";
String name2 = "abc";
System.out.print(name.equals(name2)) // true 方法返回一个布尔值, 比较值是否相等
```

### 登录字符串校验

```java
// 登录测试
import java.util.Scanner;

public class userLogin {
    public static void main(String[] args) {
        // 已知正确的用户名密码, 验证输入的用户名密码,3次机会
        String userName = "david";
        int userPwd = 123456;
        Scanner sc = new Scanner(System.in);
        for (int i = 0; i < 3; i++) {
            System.out.println("请输入用户名");
            String name = sc.next();
            System.out.println("请输入密码");
            int pwd = sc.nextInt();
            // equals方法比较字符串值是否相等
            if (userName.equals(name) && userPwd == pwd) {
                System.out.println("登录成功!");
                break;
            } else {
                if (i == 2) {
                    System.out.println("错误次数过多,账号已锁定!");
                } else {
                    System.out.println("用户名或密码错误,你还有"+(2-i)+"次机会");
                }
            }
        }
    }
}

```



### 字符串获取长度

字符串通过length()方法获取长度, 和数组的length属性直接获取有区别

```java
import java.util.Scanner;

// 字符串统计
public class StringTest {
    public static void main(String[] args) {
        // 根据输入的字符串,统计出现了多少个大写字母,小写字母,数字
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入一个字符串");
        String str = sc.next();
        int strLen = str.length(); // 获取字符串长度
        int smallCount = 0, bigCount = 0, numCount = 0;
        for (int i = 0; i < strLen; i++) {
            char c = str.charAt(i); // 根据索引获取对应的字符
            if (c >= 'a' && c <= 'z') { // 字符型在参与计算时会查asc码表,转成int再进行比较
                smallCount++;
            } else if (c >= 'A' && c <= 'Z') {
                bigCount++;
            } else if (c >= '0' && c <= '9') { // 注意 比较的是字符的0~9 转成对应的asc值
                numCount++;
            }
        }
        System.out.println("小写字母有" + smallCount + "个");
        System.out.println("大写字母有" + bigCount + "个");
        System.out.println("数字有" + numCount + "个");
    }
}

```

### 字符串反转

```java
public class StringTest02 {
    public static void main(String[] args) {
        // 字符串反转
        String str = reverse("abcdefg");
        System.out.println(str);
    }
    public static String reverse(String str) {
        int len = str.length();
        // 从末位索引遍历
        String turnStr = "";
        for (int i = len - 1; i >= 0; i--) {
            char c = str.charAt(i);
            turnStr += c;
        }
        return turnStr;
    }
}
```



### 数字转大写汉字

```java
import java.util.Scanner;

// 8324 ---> 零佰零拾零万捌仟叁佰贰拾肆元
public class StringTest03 {
    public static void main(String[] args) {
        // 字符串数字转大写汉字数字， 汉子要求7位，不足的补零，每位要显示汉字金额单位
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入金额");
        int money = sc.nextInt();
        if (money >= 0 && money <= 9999999) {
            String res = "";
            while(true) {
                int ge = money % 10;
                money = money / 10;
                String c = strToCapital(ge);
                // 1、数字转汉字
                res = c + res; // 上一次结果res放在右边，因为数字是从右边开始获取
                if (money == 0) {
                    break;
                }
            }
            System.out.println(res);
            // 2、金额前补零
            int len = res.length();
            for (int i = 0; i < 7 - len; i++) {
                res = "零" + res;
            }
            System.out.println(res);
            // 3、插入汉字单位
            String[] arr = {"佰","拾","万","仟","佰","拾","元"};
            int resLen = res.length();
            String finalMoney = "";
            for (int i = 0; i < resLen; i++) {
                char c = res.charAt(i); // 获取每个索引对应的字符
                finalMoney = finalMoney + c + arr[i];
            }
            System.out.println(finalMoney);
        } else {
            System.out.println("输入金额不合法！");
        }
    }

    // 根据索引返回对应的大写汉字
    public static String strToCapital(int index) {
        String[] caps = {"零","壹","贰","叁","肆","伍","陆", "柒", "捌", "玖"};
        return caps[index];
    }
}
```



### substring

substring(int beginIndex, int endIndex) 包头不包尾, 返回一个截取后的字符串,不修改原字符串, 和js一样

### 身份证截取

```java
// 身份证信息截取
public class StringP02 {
    // 截取身份证生日和性别, 输出信息
    // 7~14位生日 17位性别(男奇数,女偶数)
    public static void main(String[] args) {
        String id = "330102198807021816";
        String birth = id.substring(6, 14);
        char gender = id.charAt(16);
        // 字符"1"-->数字1
        // 利用字符参与计算转成asc码对应的数值
        System.out.println('0' + 0); // 48 查'0'对应的acs值
        System.out.println(gender + 0); // 字符'1'对应49, 所以49-48就能得到数字1
        int num = gender - 48; // 字符转成数字
        char cGender = num % 2 == 0 ?  '女' : '男';
        System.out.println("当前用户生日为" + birth + "性别为:" + cGender);
    }
}
```



### replace

replace(旧值, 新值)



### startsWith

startsWith("xx")   以某个字符串开头, 返回布尔值



### StringBuilder

字符串创建容器, 提升字符串各种操作效率(拼接,反转)

**实例化后打印sb对象,打印的是属性值,不是地址值(java底层处理过)**

![image-20230705092724087](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230705092724087.png)

```java
StringBuilder sb = new StringBuilder("abc");
// 添加内容
sb.append(123);
sb.append(true); //abc123true

sb.reverse(); // 反转内容
int len = sb.length() // 获取长度
    
String str = sb.toString() // sb对象转换为String
```



### StringJoiner

jdk8后出现, 实际作用和Stringbuilder一样, 代码编写比StringBuilder更简洁

![image-20230705095054242](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230705095054242.png)

```java
StringJoiner sj = new StringJoiner(", ", "[", "]"); // 指定间隔符,开始符(可选), 结束符(可选)
sj.add("aaa").add("ccc").add("bbb") // [aaa, ccc, bbb]
```



### 字符串数字变罗马数字

```java
import java.util.Scanner;
import java.util.StringJoiner;

public class stringbuilder {
    public static void main(String[] args) {
        // 录入一个字符串,返回对应的罗马数字, 罗马数字内没有0,0可以变成""
        // 长度小于等于9, 只能是数字
        Scanner sc = new Scanner(System.in);
        StringBuilder sb = new StringBuilder();
        char c;
        String str;
        while (true) {
            System.out.println("请输入一个数字");
            String strNum = sc.next();
            boolean flag = true;
            for (int i = 0; i < strNum.length(); i++) {
                c = strNum.charAt(i);
                if (c < '0' || c > '9') {
                    System.out.println("存在非数字字符,请重新输入");
                    flag = false;
                    break;
                } else {
                    // 字符数字->数字
                    int num = c - 48; // 48->字符'0' 就是把字符数字转化为数字
                    String luoma = toLuoma(num);
                    sb.append(luoma + ", ");
                }
            }
            if (flag) {
                str = sb.toString();
                System.out.println(str);
                break;
            }
        }
    }
    public static String toLuoma (int number) {
        String[] luoma = {"","I","II","III","IV","V","VI","VII","VIII","IX"};
        return luoma[number];
    }
}

```

### 字符串比较

```java
public class stringP03 {
    public static void main(String[] args) {
        // ab字符串比较
        // 把a字符串头位放到末位,执行若干次后,如果能和b字符串一样,返回true,否则返回false
        String strA = "abcde";
        String strB = "eabcd";
        boolean result = checkStr(strA, strB);
        System.out.println("是否可以一样:" + result);
    }


    //定义一个旋转字符串的方法, 返回旋转后的字符串
    public static String rotate (String str) {
        char first = str.charAt(0);
        String last = str.substring(1);
        String newStr = last + first;
        return newStr;
    }

    public static boolean checkStr (String str1, String str2) {
        // 思路: 最大循环次数就是字符串自己的长度,循环结束后不满足相等条件,则返回false
        for (int i = 0; i < str1.length(); i++) {
            str1 = rotate(str1); // 重点: 每次执行旋转后赋值给自己,带入下次循环
            if (str1.equals(str2)) {
                return true;
            }
        }
        return false;
    }
}

```

### toCharArray

字符串转成字符数组

```java
String str = "abcd";
char[] chs = str.toCharArray(); // 返回一个字符数组, 可以用new String(chs) 再变回字符串
```



### 字符串数字相乘

```java
public class stringP04 {
    public static void main(String[] args) {
        // 给定两个非负字符串表示的num1,num2 ,返回num1,num2的成绩, 乘积也用字符串表示
        // 用已学知识完成
        // 字符串--> 整数数字
        String str1 = "345";
        String str2 = "666";
        // 1.转成字符数组
        int num1 = stringToInt(str1);
        int num2 = stringToInt(str2);
        int sum = num1 * num2;
        String sumStr = sum + ""; // 任意数字+字符串 都会转成字符串
        System.out.println(sumStr);
    }
    public static int stringToInt(String str) {
        char[] chs = str.toCharArray();
        int num = 0;
        for (int i = 0; i < chs.length; i++) {
            num = num * 10 + (chs[i] - 48); // 关键语句: 字符转数字再利用*10拼成整数
        }
        return num;
    }
}

```



## 集合

和数组区别:

1. 数组长度固定, **集合长度可变**
2. 数组可以存基本数据类型和引用数据类型, 集合只能存引用数据类型,**要存基本类型必须是包装类**



### 包装类

集合添加基本类型,要使用包装类, 泛型中传入包装类

```java
ArrayList<Integer> list = new ArrayList<>();
ArrayList<Short> list2 = new ArrayList<>();
```



![image-20230705145708622](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230705145708622.png)

### ArrayList

```java
package com.david.demo03;

import java.util.ArrayList;

public class ArrayListTest1 {
    public static void main(String[] args) {
        // 定义一个集合 集合必须传入泛型,定义集合的类型
        ArrayList<String> list = new ArrayList<>();
        // 增add
        list.add("david");
        list.add("billy");
        list.add("king");

        // 删remove
//        String el = list.remove(0); // 返回被删除的元素
//        System.out.println(el);

        // 改set
        list.set(0, "leon");
        // 查get
        String item = list.get(2); // 根据索引查询
        // 获取长度 size() 用于遍历
        for (int i = 0; i < list.size(); i++) {
            System.out.print(list.get(i));
        }
        System.out.println();
        System.out.println(list); // ArrayList底层有处理不会返回地址值, 返回的是数据内容
    }
}

```



### 集合查数据

```java
package com.david.demo03;

import java.util.ArrayList;

public class ArrayListTest02 {
    public static void main(String[] args) {
        // 定义一个集合存入3个用户对象, 用户属性为id,username,password
        // 要求:定义一个方法,根据id查找对应的用户信息, 存在返回true 不存在返回false

        // 泛型传入User类
        ArrayList<User> userList = new ArrayList<>();
        User u1 = new User("001", "david", 123456);
        User u2 = new User("002", "kashin", 123456);
        User u3 = new User("003", "leon", 123456);
        userList.add(u1);
        userList.add(u2);
        userList.add(u3);

        // 判断集合中是否有该id对应的用户
        System.out.println(hasUser(userList,"001")); // true
        System.out.println(hasUser(userList,"004")); // false
    }
    public static boolean hasUser(ArrayList<User> arr, String id) {
        for (int i = 0; i < arr.size(); i++) {
            String uid = arr.get(i).getId();
            if (uid.equals(id)) {
                return true;
            }
        }
        return false;
    }
}
```



### break跳出外层循环

循环内嵌套switch语句时, switch使用默认Break, 退出的是switch语句,并不能退出外层while循环

```java
// 给循环指定一个名字
loop: while(true) {
    // xxx
    switch(value) {
        case val1 -> 
        case val2 -> 
            //...
        case val5: -> {
            break loop; // 跳出外层循环loop
        } 
            
        }
        default:
    }
}
```



## IDEA快捷键

定义还没创建的方法后, 选中方法alt+回车, 快速创建

alt+insert可以快速创建构造方法和get/set方法

创建对象时, 选中方法的括号按ctrl+p 查看方法接收的参数





## 继承

**1.java只能单继承，一个类只能继承一个直接父类**

**2.java不能多继承，但支持多层继承， （可以继承父类的父类）**

**3.java中所有类都直接或间接继承于Object类**

子类可以得到父类的属性和方法

子类可以在父类基础上增加功能



### 继承的范围

继承和调用概念不要混淆， 继承是拷贝了一份父类的方法或属性，复制到了子类中

构造方法: 非私有/private都不能

成员变量: 非私有/private都能 **(private會被继承，但不能直接调用，一样要用set/get方法调用)**

成员方法: 非私有能, private不能  **只有虚方法会被继承(非private/非static/非final就是虚方法)**

```java
// 子类 extends父类
pubic class Student extends Person{}
```



### super

super()就是执行父类的无参构造函数

super可以直接调用父类的变量和方法

```java
class Student extends Person {
    String name;
    int age;
    
    public Stundent() {
        super(); //子类创建时默认调用父类的无参构造，程序默认会加在第一行 写不写都行
    }
    public Student(String name, int age) {
        super(name, age); // 想给父类构造传值，就要手动写在子类带参构造函数中
    }
}
```



### 空参构造附初始值

```java
class Student extends Person{
    String name;
    int age;
    String school;
    
    public Student(){
    	this(null, 0, "xx大学")  // school赋了初始值，this()表示调用本类其他构造方法
    }
    public Student(String name, int age, String school) {
        this.name = name;
        this.age = age;
        this.school = school;
    }
}

// 测试类创建Student
public class Demo{
    public static void main(){
        Student stu = new Student(); // 调用的是空参构造，但是有一个school的初始化值
    }
}
```







### 成员变量访问特性

遵循就近原则，局部变量--》成员变量--》父类变量

**个人理解: 变量重名不会被继承重写, 父类变量和子类变量都存在, 通过不同方式调用**

```java
public class Extends01 {
    public static void main(String[] args) {
        Son s = new Son();
        s.show();
    }
}


class Father{
    String name = "father";
}
class Son extends Father {
    String name = "son";
    public void show(){
        String name = "show";
        System.out.println(name); // show
        System.out.println(this.name); // son
        System.out.println(super.name); // father
    }

}
```



### 方法重写 Override

子类中出现和父类名字一样的方法，就是重写方法 重写的方法必须和父类保持一致，形参，方法名,返回值（小于等于父类）

**不被继承的方法不能重写**

@override 重写注解，加在重写方法后面，让虚拟机校验重写语法是否正确

**个人理解:方法重写是重写从父类虚方法表中复制过来的方法, 不会影响父类的方法**



## 多态

表现形式：

**子类对象赋值给父类类型 => 父类类型 名称 = 子类对象;**

前提：

1、必须是继承关系 

2、必须有方法重写 

```java
public class duotaiTest {
    public static void main(String[] args) {
        Student stu = new Student();
        stu.setName("大卫");
        stu.setAge(35);
        register(stu); // 调用register 并传学生

        Teacher t = new Teacher();
        t.setName("顾老师");
        t.setAge(45);
        register(t);// 调用register 并传老师

        Admin ad = new Admin();
        ad.setName("管理员大佬");
        ad.setAge(27);
        register(ad);// 调用register 并传admin
    }

    // 该方法需要接受student,teacher,admin3个类型参数，需要使用多态传父类作为参数，否则就要写3个方法
    public static void register(Person p){
        p.show(); // 多态根据传入对象, 分别调用对应对象中的show方法
    }
}



```

```java
public class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
    public void show(){
        System.out.println("我是person,我的名字是" + name);
    }
}


public class Admin extends Person{
    @Override
    public void show(){
        System.out.println("我是admin，我的名字是" + this.getName());
    }

}

public class Student extends Person{
    @Override
    public void show(){
        System.out.println("我是student，我的名字是" + this.getName());
    }
}

```



### 多态创建子类

成员变量==> 堆内存中的一个对象中,会分别创建一个父类变量和一个子类变量, 当用父类类型调用时,查的就是父类的变量

成员方法==> 编译看左边, 运行看右边, 运行被重写的方法, 也就是成员方法

![image-20230707113119742](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230707113119742.png)

```java
public class duotai {
    public static void main(String[] args) {
        // 用多态Animal父类类型创建一个子对象
        Animal a = new Dog();
        System.out.println(a.name); // 动物  调用变量,调用的是父类变量
        a.show(); // dog==>狗 调用方法 调用的是子类方法
    }
}

class Animal {
    String name = "动物";
    public void show(){
        System.out.println("animal====>"+ name);
    }
}
class Dog extends Animal {
    String name = "狗";
    @Override
    public void show(){
        System.out.println("dog===>" + name);
    }
}
class Cat extends Animal {
    String name = "猫";

    @Override
    public void show(){
        System.out.println("cat===>" + name);
    }
}

```





### 多态优点

1,多态形式下,实现右边对象解耦,便于扩展和维护

```java
Person p = new Teacher(); // 比如想调用 student.work 直接把new teacher改成new Student即可
p.work() // 业务逻辑改变时,后续代码无需修改
```



2.**重要:**定义方法的时候,使用父类型作为形参,可以接收所有子类对象,体现多态扩展性

```java
ArrayList<Person> list = new ArrayList<>(); // 把父类作为对象类型传给集合, 集合可接收所有子类
```





### 多态缺点

当多态定义的对象调用的方法在父类中不存在时,就会报错

此时就要使用强制类型转换转成子类,再调用

```java
class Animal {
    
}
class Dog extends Animal{
    public void show(){}
}

Animal a = new Dog();
Dog d = (Dog) a; // animal强制转换成dog类型, 调用show方法才不会报错
a.show();
```



### instancof

类型判断

```java
class Animal {
    
}
class Dog extends Animal{
    public void show(){}
}

Animal a = new Dog();
if (a instanceof Dog d) { // 新特性 a是不是Dog类型, 是的话转换并赋值变量d 
    // Dog d = (Dog) a 老写法
}
```



### 多态应用

```java
package com.david.demoDuotai04;

// 多态应用场景
// 需求 调用keepPet输出饲养员信息,饲养的动物信息,喂的东西信息
public class duotai02 {
    public static void main(String[] args) {
        Person p1 = new Person("大卫", 35);
        Tiger tiger = new Tiger("黄", 3);
        p1.keepPet(tiger, "骨头"); // 定义时用的是多态父类, 可以直接传入不同类型的子类

        Person p2 = new Person("老李", 50);
        Fish fish = new Fish("蓝", 5);
        p2.keepPet(fish, "饲料");
    }
}

// 动物类-父类
class Dongwu {
    private String color;
    private int age;

    public void eat(String food) {
        System.out.println("动物在吃" + food);
    }
    public Dongwu() {
    }

    public Dongwu(String color, int age) {
        this.color = color;
        this.age = age;
    }

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
// 动物子类
class Tiger extends Dongwu {

    public Tiger() {
    }

    // 子类自己的构造函数
    public Tiger(String color, int age) {
        super(color, age); // 有参构造, 执行父类构造函数
    }

    @Override
    public void eat(String food) {
        System.out.println("一只"+getColor()+"颜色的"+getAge()+"岁的老虎在抱着"+food+"猛吃");
    }
}
// 动物子类2
class Fish extends Dongwu{

    @Override
    public void eat(String food) {
        System.out.println("一只"+getColor()+"颜色的"+getAge()+"岁的鱼在吃"+food);
    }
    public Fish() {
    }

    public Fish(String color, int age) {
        super(color, age);
    }
}
// 饲养员类
class Person {
    private String name;
    private int age;

    // 重点方法: 利用多态,形参传父类, 只写一个keepPet方法就行
    public void keepPet(Dongwu a, String food) {
        // 强制类型转换 把父类转成子类, 这样就可以调用子类的成员变量
        if (a instanceof Tiger t) {
            System.out.println(age+"岁的"+name+"养了一只"+t.getColor()+"颜色"+t.getAge()+"岁的老虎");
        } else if (a instanceof Fish f) {
            System.out.println(age+"岁的"+name+"养了一只"+f.getColor()+"颜色"+f.getAge()+"岁的鱼");
        }
        a.eat(food); // 直接调用子类重写的方法
    }
    public Person() {
    }
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

```



## 抽象类

当使用多态时, 子类不重写父类方法,父类就无法调用, 所以定义抽象类和抽象方法可以让子类强制重写抽象方法

**抽象类不能创建对象**

**有抽象方法的类一定是抽象类, 抽象类可以没有抽象方法**

**抽象类可以有构造方法**

子类继承抽象类时需满足2个要求其中1个

  1.重写抽象类的所有抽象方法

2. 自己也定义成一个抽象类

```java
// 定义抽象类
public abstract class Person{
    // 抽象方法, 子类必须强制重写
    public abstract void work(); // 不写方法体,只需要形参
}

// 继承抽象类
class Student extends Person{
    @Override
    public void work(){ // 必须重写抽象方法
        
    } 
}
```







## 导包

包就是文件夹,用来管理不同功能的java类

写法: 域名反写+包的作用, 全部小写

不需要导包的情况

1.使用同一个包中的类

2.使用java.lang包中的类

**同时使用两个包中相同名字的类时, 要使用全类名**

```java
package com.david.demoDuotai04; // 导包

// 全类名
com.david.demoDuotai04.Person p1 = new com.david.demoDuotai04.Person();
```



## 代码块

局部代码块(已淘汰, 没必要)

构造代码块(不灵活, 建议把构造代码块内容抽取到独立方法中, 在构造方法中再调用)

静态代码块(重点, static修饰, 随类加载而加载, 并且自动触发, **只执行一次, new创建不会再执行**)

静态代码块作用: 在类加载中, 做一些数据初始化

```java
public class Demo{
    public class void main(){
        // 局部代码块 a只能在局部代码块中使用, 运行结束后a就会被销毁
        {
            int a = 0;
            System.out.println(a);
        }
    }
}

public class Person{
    String name;
    int age;
    // 构造代码块 写在成员位置,在构造函数前 作用: 把构造函数中重复代码提取出来 执行时机:创建类时先执行构造代码块,再执行构造方法
    {
        System.out.println("先执行构造代码块");
    }
    
    // 静态代码块 类加载时执行一次
    static {
        // 做一些数据初始化处理
    }
    
    public Student() {}
    public Student(String name, int age) {}
    
    
}
```







