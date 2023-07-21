

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



## 工具类Scanner

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
        
        // nextLine() 无视空格接受所有输入，直到回车结束 建议所有键盘接收事件都用nextLine()方法
        String str = sc.nextLine() // 输入123 123 可以全部被接收到
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



### 位运算符（也可参与逻辑运算但不常用）

**&| : 不管左侧true false 右侧都会执行**

&： 二进制数按位与，两个位都为1，结果为1；逻辑运算时：两边都为真,结果才为真

| ：二进制数按位或，两个位都为0，结果为0； 逻辑运算时：两边都为假,结果才为假

^ ：两个位相同为0，不同为1；逻辑运算时： 相同为false,不同为true

! 逻辑非  取反 (java中只用一个!  不要用!!)



### 短路逻辑运算符

**含义:左侧表达式能确定最终结果,右侧就不会执行**

&&  短路与  结果和&相同,但具有短路效果, 左侧为false时,右侧不执行

||   短路或  结果和|相同, 但具有短路效果, 左侧为true时,右侧不执行 



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



### 权限修饰符范围

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
    private ArrUtil(){} // 关键: 私有化构造函数,不让外部调用new创建对象, 工具类没有必要创建对象
   
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

### ArrayList底层原理

集合的底层也是数组

1.用空参构造创建ArrayList时，底层默认创建一个长度为0的数组

2.当添加第一个元素时，底层创建一个新的长度为10的数组

3.集合存满时，自动扩容为1.5倍(每次存满都会自动扩容)

4.若一次性存多个元素存满，自动扩容为当前实际长度

集合每添加一个元素，size++

集合的size方法有2层含义：1.集合的元素个数  2.下次存入的位置





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



## 集合进阶

集合体系结构

集合分为两类：单列集合（一次添加一个数据），双列集合（一次添加一对数据）

单列集合顶层接口是**Collection**，子类是List和Set

List和Set区别：

List：添加元素有序（指存放和获取有序），可重复，有索引

Set：添加元素无序，不重复，无索引

![image-20230720141356522](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230720141356522.png)

### Collection集合

```java
package com.david.demo06Collection;

import java.util.ArrayList;
import java.util.Collection;

public class CollectionTest {
    public static void main(String[] args) {
        // 多态方式创建ArrayList对象
        Collection<String> coll = new ArrayList<>();
        ArrayList<String> list = new ArrayList<>();
        // add 返回一个布尔值表示添加是否成功，List一定true，如果是Set集合，添加相同元素就会返回false
        coll.add("david");
        coll.add("superMan");
        System.out.println(coll);

        // clear 清空集合
//        coll.clear();
//        System.out.println(coll);

        // remove 删除指定元素
        coll.remove("david");
        System.out.println(coll);

        // contains 判断元素是否包含
        System.out.println(coll.contains("david"));

        // 获取长度
        int size = coll.size();
        System.out.println(size);
    }
}

```

### Collection遍历

有3种遍历方式：迭代器、增强for、Lambda表达式遍历

常用成员方法：add,clear,remove,contains,isEmpty,size

在遍历中想删除元素，使用迭代器

仅想遍历元素使用增强for和Lambda表达式



#### 迭代器

特点：迭代器不依赖索引

```java
package com.david.demo06Collection;

import java.util.ArrayList;
import java.util.Iterator;

public class IteratorTest {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("david");
        list.add("king");
        list.add("kashin");

        // 注意1：迭代器遍历完后指针不会复位，若要再次遍历就要再次创建对象
        Iterator<String> it = list.iterator();// 返回一个迭代器对象，默认指向0索引
        while (it.hasNext()) { // hasNext() 返回布尔值，判断当前位置是否有元素
            // 注意2：在循环中next只能使用一次，不能多次调用，因为next会移动指针，指针越界就会报错
            // 注意3：迭代器中不能使用集合的方法增删元素，会报错，删除要用迭代器方法删it.remove()，迭代器不能增加元素
            System.out.println(it.next()); // 获取当前位置元素，并将指针指向下一个对象
        }
    }
}
```



#### 增强for遍历

底层也是Iterator原理，简化迭代器书写

**单列集合和数组才能用增强for遍历**

格式：for(元素数据类型 变量名：数组or集合) {}

```java
for (String s : list) { 
	System.out.println(s)  // s就是集合中的每个元素
}
```



#### Lambda表达式遍历

用forEach方法遍历

forEach(Consumer) // 接收一个Consumer接口，要传实现类

```java
Collection<String> coll = new ArrayList<>();
    coll.add("LEON DE ORIAD");
    // lambda表达式遍历 接收一个Consumer接口，传匿名类实现该接口
    coll.forEach(new Consumer<String>() {
        @Override
        public void accept(String s) { // 方法forEach会把集合中的每个元素传给accept方法
            System.out.println("===>" + s);
        }
    });


	// Lambda表达式简写
	coll.forEach(s -> {
        System.out.println("===>" + s);
    })
```

### List集合

list也是接口，继承了Collection所有方法

List集合独有方法：

**void add(int index, E element)**  在指定位置插入指定元素

**E remove(int index)** 删除指定索引元素，返回被删除的元素

**E set(int index, E elment)** 修改指定索引元素，返回被修改的元素

**E get(int index)** 返回指定索引元素

```java
list.add(1);
list.add(2);
// remove是一个重载方法，还有个父类方法remove(Object o), 这里优先调用实参和形参一致的方法，即remove(int index)
list.remove(1);
```



#### List遍历

List集合支持Collection的所有遍历方式，还支持**普通for循环遍历**和**列表迭代器遍历**

```java
// 列表迭代器ListIterator
ListIterator<String> it = list.listIterator();
while(it.hasNext()) {
    String str = it.next();
    if("bbb".equals(str)) {
        it.add("qqq") // 列表迭代器支持add方法（普通迭代器只能删除，无法添加）
    }
}
```



#### LinkedList集合

**底层数据结构是双向链表**，是查询慢，增删快的模型 （操作首尾元素极快，含特有操作首尾元素的API）

```java
// LinkedList部分源码
public class LinkedList<E> extends AbstractSequentialList<E> implements .... {
    // 初始化操作
    transient int size = 0;
    transient Node<E> first; // 单独维护头结点，是一个Node类型， 这个Node是一个内部类
    transient Node<E> last;  // 单独维护尾结点
    
    //other Code....
    
    // 内部类Node，表示结点对象，内部维护自己数据、前一个结点地址、后一个结点地址
    private static class Node<E> {
        E item;
        Node<E> next; // 前一结点地址值
        Node<E> prev; // 后一结点地址值
        
        // 构造函数
        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.prev = prev;
            this.next = next;
        }
    }
}

// 链表添加元素
LinkedList<String> list = new LinkedList<>();
list.add("david");

// 添加元素内部逻辑
public boolean add(E e) {
    modCount++; // 集合每变化一次就自增一次，和并发修改异常相关（迭代器会同步这个modCount数并校验）
    linkLast(e); // add调用linkLast传入元素
    return true
}

void linkLast(E e) {
    final Node<E> l = last; // 获取上一个结点地址值
    // 调构造函数创建结点，传入3个参数:上一个结点地址值，当前添加的元素，下一个结点地址值默认null
    final Node<E> newNode = new Node<>(l, e, null); 
    last = newNode; // 把当前结点记录为上一个结点
    if (l == null) { // l==null表示没有上一个结点，是首结点，直接赋值给first保存
        first = newNode;
    } else { // 非首结点，把当前结点赋值给上一个结点的next属性，即上一个结点指向下一个的地址值
        l.next = newNode;
    }
    size++;
    modCount++;
}
```



### Set集合





## IDEA快捷键

定义还没创建的方法后, 选中方法alt+回车, 快速创建

alt+insert 可以快速创建构造方法和get/set方法

ctrl+p  选中方法的括号按查看方法接收的参数

ctrl+b 选中方法, 跟进方法

ctrl+alt+M 把选中的内容抽取成一个方法

**ctrl+n  全局搜索, 可以搜索查看类的源码**

​	**ctrl+f12  查看类的构造方法(一个类可能有很多个不同参数的构造方法)**

alt+回车 重写接口方法



## 第三方包引入

引入的第三方jar包一般放在lib文件夹中, 右键 jar包选择: **add as libary** 即可展开



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

**子类对象赋值给父类类型 , 格式: 父类类型 名称 = 子类对象;**

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

**多态不能访问子类的特有方法**

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

抽象类的好处：强制重写子类，也就统一了子类的方法名参数和返回，方便统一维护

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



## 接口

 接口是一种规则， 是对行为的抽象，使用implements关键词表示

接口不能实例化

接口的子类（实现类）

1.要么重写接口中的所有抽象方法

2.要么自己也是抽象类

```java
// 定义接口
public interface swim{
    public abstract void swim(); // 定义抽象方法
}
public interface play{
    public abstract void play();
}

// 接口和类之间是实现关系，通过implements关键词表示 学生拥有了swim这个接口，实现了swim的功能
public class Student implements swim {
    @Override
    public void swim(){}; // 重写接口的抽象方法
}
// 接口和类可以多实现
public class Student implements swim, play{
    
}
// 实现类在继承一个类的同时实现多个接口
public class Student extends Person implements swim, play{
    
}

```



### 接口中成员的特点

成员变量：只能是常量，用public static final修饰

构造方法:  没有

成员方法：jdk7之前：只能是抽象方法，默认用public abstract修饰。  

 jdk8后可以添加默认方法（有方法体）**不需要强制重写，但如果重写需要去掉default关键词**

jdk9后可以添加私有方法（普通私有，静态私有）

目的：默认方法中有重复代码，不想让外部调用又想抽取，定义到私有方法中

```java
// 接口中允许定义默认方法，解决接口升级时，实现类全部要重写的兼容性问题
public interface inter{
    // 定义默认方法 必须用default修饰， 表示默认方法
    public default void show() {
        
    }
    // 定义静态方法 不能重写 只能通过接口名调用，不能通过实现类或者对象名调用
    public static void show2(){
        
    }
    // 定义普通私有方法（服务默认方法）
    private void show3(){
        
    }
    // 静态私有方法（服务静态默认方法）
    private static void show4(){
        
    }
}


```



### 接口的多态

当一个方法的参数是接口时，可以传递接口所有实现类的对象，这种方式就叫接口多态

```java
// 接口
public interface inter1{
    
}
// 实现类1
public class Person implements inter1{
    
}
// 实现类2
public class Company implements inter1{
    
}

// 定义一个方法，接收一个接口类型参数
public void methods(inter1 inter){
    
}
Person p = new Person()
Company c = new Company()

//方法参数可以传实现类
methods(p);
methods(c);
```





### 接口和类之间的关系

类和类：继承关系，只能单继承，但可以多层继承

类和接口：实现关系，可以单实现也可以多实现，还可以在继承一个类同时实现多个接口

接口和接口：继承关系，可以单继承，也可以多继承

```java
// 接口继承
public interface inter1{
    public abstract void methods1();
}
public interface inter2{
    public abstract void methods2();
}
public interface inter3 extends inter1,inter2{
    public abstract void methods3();
}
// 实现类
public class Person implements inter3{
    // inter3继承了inter1和2， 实现类必须重写所有接口的抽象方法
    @Override
    public void methods1(){};
    @Override
    public void methods2(){};
    @Override
    public void methods3(){};
}
```



### 适配器设计模式

当一个接口有非常多抽象方法时，实现类就需要重写所有方法，很不方便

添加一个中间类，重写所有抽象方法为空方法，再用实现类继承这个中间类，重写要用到的方法即可

```java
public interface inter{
    public abstract void methods1();
    public abstract void methods2();
    public abstract void methods3();
    public abstract void methods4();
}
// 中间类，空实现接口所有抽象方法 这个类不需要被创建，定义成抽象类
public abstract class adapter implements inter{
    @Override
    public void methods1(){};
     @Override
    public void methods2(){};
     @Override
    public void methods3(){};
     @Override
    public void methods4(){};
}
// 真正的实现类 继承这个中间类
public class interImpl extends adapter{
    // 要用哪个方法就重写哪个即可
    @Override
    public void methods3(){};
}
```



## 内部类

定义：在类里面定义的类，就叫内部类

**内部类可以访问外部类成员，包括私有**

**外部类不能访问内部类，必须创建对象**

```java
// 外部类
public class Car{
    private String carName;
    String carColor;
    public void show(){
        Engine e = new Engine();
        System.out.print(e.engineName); // 必须创建对象才能获取
    }
    // 内部类
    class Engine{
        String engineName;
        int engineAge;
        public void show(){
            System.out.print(carName); // 可以直接获取外部类
        }
    }
}
```



### 内部类分类

### **1.成员内部类 **

写在成员位置，属于外部类的成员，地位和成员变量方法一样

```java
public class Car{
    String carName;
    int carAge;
    class Engine{ // 成员内部类
        String engineName;
    }
    // 私有内部类
    private class Inner{
        
    }
    // 定义一个返回私有内部类对象的方法供外部调用
    public Inner getInstance(){
        return new Inner();
    }
}
// 直接创建成员内部类语法:Outer.Inner oi = new Outer().new Inner()
// 外部类和内部类变量重名时，内部类调用Outer.this.变量名
```



### **2.静态内部类**

```java
public class Outer{
    String carName;
    int carAge;
    static Stirng carBrand;
    
    
    // 静态内部类 静态内部类只能访问静态的变量或方法
    static class Engine{
        String engineName;
        int engineAge;
        public void show1(){
            System.out.print(carBrand); // 只能访问静态变量
        }
        public void show2(){
    		Outer o = new Outer();
            System.out.print(o.carName); // 创建外部类对象后可以访问非静态变量
        }
    }
}
// 创建静态内部类语法:Outer.Inner oi = new Outer.Inner()
// 调用静态内部类方法 也可以通过oi.show1()调用, 但不推荐
Outer.Inner.show1()
```



### **3.局部内部类**

定义在方法内的类, 类似方法内定义的变量

```java
public class Outer{
    
    
    public void methods1(){
    	
        // 方法内的类
        class Inner{
            
        }    
    }
}
```



### **4.匿名内部类（最常用）**

隐藏了名字的内部类, 可以现在成员位置,也可以写在局部位置

格式: new 类名or接口名() {

​		重写方法;

};

```java
// 定义接口
public interface swim(){
    public abstract void swim();
}

// 正常的实现类
public class Person implements swim{
    @Override // 重写接口的抽象方法
    public void swim(){}
}

// 匿名内部类实现接口 {}整体就是匿名内部类
// 关键理解: swim是被实现的接口, new关键词创建的是{}的匿名类,  {}内的部分才是真正的匿名内部类 
new swim(){
    @Override
    public void swim(){}
};



// 定义父类
public abstract class Father{
    public abstract void methods();
}
// 匿名内部类继承父类, 代码理解: new创建了{}内的匿名类, 并且继承了Father这个父类
new Father(){
    // 重新方法
    @Override;
    public void methods(){}
};
```

**应用场景**

```java
// 前提:有一个Animal的抽象类
public class Demo{
    public static void main(){
        // 常规调用
        Dog d = new Dog();
        method(d);
        
        // 使用匿名内部类, 直接创建一个匿名内部类,作为参数传给method, 本质就是创建了一个继承Animal的子类给了method, 形成了多态
        method(
        	new Animal(){
                @Override
                public void eat(){}
            };
        )
    }
    
    
    // 正常要调用这个方法,只能先创建animal子类如dog,再继承animal,重新eat方法, 然后new Dog();再通过Dog对象调用eat
    public static void method(Animal a){
        a.eat();
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





## GUI(图形化界面)

**JAVA一般做服务器开发,比较少用图形化界面**

指采用图形化的方式显示操作界面

定义在AWT包和Swing包中

java的图形组件:

**JFrame** 最外层窗体主窗体

**JMenuBar** 最外层菜单 --- **JMenu** 二层菜单 -- **JMenuItem** 最底层菜单

**JLabel** 管理文字和图片的容器, 图片必须添加到JLabel中

**ImageIcon**  添加图片

**JButton** 按钮

**JTextField** 文本输入框(明文) 

**JPasswordField** 密文输入框

**JDialog** 弹窗窗体



## 监听

KeyListener 键盘监听

MouseListener 鼠标监听

ActionListener 动作监听(只能监听鼠标左键和空格)

### 实现监听1 

implements实现监听接口

```java

package com.david.test;

import javax.swing.*;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;

public class test03 {
    public static void main(String[] args) {
        testFrame3 f3 = new testFrame3();
    }
}


// 方法1: 继承对象, 同时实现鼠标监听的接口
class testFrame3 extends JFrame implements MouseListener {
    public testFrame3() {
        this.setSize(600, 800);
        this.setTitle("test"); // 设置标题
        this.setLocationRelativeTo(null); // 窗体居中显示
        this.setDefaultCloseOperation(3); // 点击关闭按钮时直接关闭jvm虚拟机

        JButton btn = new JButton("我是jbutton");
        btn.setBounds(0,0,100,50);
        btn.addMouseListener(this); // 重点: 因为实现类, 在这里直接传入当前对象即可 事件触发时执行本类中的代码,即下面重写的方法

        // 取消隐藏容器的居中布局, 传空
        this.setLayout(null);
        this.getContentPane().add(btn);

        this.setVisible(true);
    }

    // 重写接口所有方法
    @Override
    public void mouseClicked(MouseEvent e) {
        System.out.println("mouseclick!");
    }

    @Override
    public void mousePressed(MouseEvent e) {

    }

    @Override
    public void mouseReleased(MouseEvent e) {

    }

    @Override
    public void mouseEntered(MouseEvent e) {
        System.out.println("mouseEnter");
    }

    @Override
    public void mouseExited(MouseEvent e) {

    }
}

```

### 实现监听2

传匿名内部类

```java
package com.david.test;

import javax.swing.*;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;

public class test02 {
    public static void main(String[] args) {
        TestFrame frame = new TestFrame();

    }
}

class TestFrame extends JFrame{
    public TestFrame() {
        // 初始化窗体
        this.setSize(600, 800);
        this.setTitle("test"); // 设置标题
        this.setLocationRelativeTo(null); // 窗体居中显示
        this.setDefaultCloseOperation(3); // 点击关闭按钮时直接关闭jvm虚拟机

        JButton btn = new JButton("我是jbutton");
        btn.setBounds(0,0,100,50);

        // 重点: 给对象添加监听方法, 该方法接收一个MouseListener接口类型, 直接传匿名内部类实现接口
        btn.addMouseListener(new MouseListener() {
            @Override
            public void mouseClicked(MouseEvent e) {
                System.out.println("mouseClick!");
            }

            @Override
            public void mousePressed(MouseEvent e) {
                System.out.println("mousePressed");
            }

            @Override
            public void mouseReleased(MouseEvent e) {
                System.out.println("mouseReleased!");
            }

            @Override
            public void mouseEntered(MouseEvent e) {
                System.out.println("mouseEntered");
            }

            @Override
            public void mouseExited(MouseEvent e) {
                System.out.println("mouseExited");
            }
        });
        // 取消隐藏容器的居中布局, 传空
        this.setLayout(null);
        this.getContentPane().add(btn);

        this.setVisible(true);
    }
}

```

## java游戏打包

前提: 必须包含图形化界面,  代码, 图片, jdk都要打包

1.把所有代码打包成.jar后缀的压缩包(jar就是java格式的压缩包)

2.把jar包转成exe安装包(只有代码)

3.把2的exe再和图片和jdk整合在一起, 变成最终的exe安装包





## 常用API

### Math 

常用方法和js中基本一样





### System

 入参 0: 正常停止   非0: 异常停止

System.exit(int status) // 退出虚拟机 

System.currentTimeMillis() // 获取时间戳

System.arraycopy(arr1, srcPos, arr2, destPos, length) // 把数组1中的数据拷贝到数组2中





### Runtime

Runtime.getRuntime()   //Runtime的静态方法,获取当前系统的运行环境对象

exit(int status) // 停止虚拟机, System.exit底层调用的就是这个方法

availableProcessors() // 获得cpu线程数

maxMemory //jvm能从系统中获取总内存的大小(单位byte)

exec() // 运行cmd命令

```java
Runtime.getRuntime().exec("notepad") // 用命令行打开记事本
Runtime.getRuntime().exec("shutdown -s -t 3600") // 3600秒后关机
```



### Object

object类是java顶级父类,所有类都直接或间接继承于Object

#### toString()  

返回对象的字符串表示形式

```java
// **System.out.println() 底层也会调用toString方法**
Student stu = new Student("张三", 25);

System.out.println(stu) // 默认会打印stu的地址值
    
// 重写Object toString方法, 打印具体的属性值
// class Student类内部,重写toString
@Override
public String toString(){
    return name + "," + age
}

```



#### equals(Object obj) 

比较两个对象是否相等**(Object的equals方法使用==比较两个对象的地址值, 可以重写方法比较属性值)**

#### clone(int a) 

对象克隆(浅拷贝)

```java
// 实现cloneable接口, 这是一个标记性接口,表示当前对象可被克隆
class User implements cloneable {
    
    // Object中的clone方法是protected修饰, 需要重写才能调用
    @Override
    protected Object clone() throws CloneNotSupportedException{
        // 调用父类中的clone方法
        return super.clone()
    }
    
    // 重写2: 深拷贝
    @Override
    protected Object clone() throws CloneNotSupportedExceoption{
        // 获取被克隆对象数组
        int[] data = this.data;
        // 创建新数组
        int[] newData = new int[data.length];
        for(int i = 0; i< data.length; i++){
            newData[i] = data[i];
        }
        // 调用父类方法克隆对象
        User u = (User) super.clone();
        // 因为父类中的克隆方法是浅拷贝,替换克隆出来对象中的数组地址值
        u.data = newData;
        return u;
    }
}


// 调用clone
User u1 = new User();
// 克隆u1赋值给u2
User u2 = (User) u1.clone(); // 返回的是object, 需要转成User
```



### Objects(工具类)

Objects是一个工具类, 提供一些操作对象的方法

equals(Object a, Object b) // 先做非空判断,比较两个对象, 返回boolean

isNull(Object obj) // 判断对象是否为null, 为null返回ture 

nonNull(Object obj) // 判断对象是否为Null, 跟isNull的结果相反



### BigInteger大整数

1.如果biginteger没有超过Long取值范围, 可以用静态方法BigInteger.valueOf()创建

2.如果超出了, 则用构造方法new BigInteger() 创建

3.对象一旦创建,内部值不会改变

```java
// 所有构造方法
public BigInteger(int num, Random rnd) // 获取随机大整数 范围:[0~2的num次方-1]
public BigInteger(String val) // (常用)获取指定的大整数
public BigInteger(String val, int radix) // 获取指定进制的大整数
// 静态方法
public static BigInteger valueOf(long val) // (常用)静态方法获取BigInteger对象, 内部有优化(提前创建-16~16对象)
    
```



**BigInteger是对象， 不能直接对两个对象使用算术运算符**

```java
// BigInteger部分方法 
public BigInteger add(BigInteger val) // 加法 
public BigInteger substract(BigInteger val) //减法
public BigInteger multiply(BigInteger val) // c乘法
public BigInteger divide(BigInteger val) // 除法 获取商
public BigInteger[] divideAndRemainder(BigInteger val) //除法 获取商和余数
public boolean equals(Object x) // 比较是否相同
    
    // 用法
    BigInteger bd1 = BigInteger.valueOf(10)
    BigInteger bd2 = BigInteger.valueOf(5)
    BigInteger sum = bd1.add(bd2) 
```



### BigDecimal

表示较大的小数，  解决精度丢失问题

```java
// 0.09+0.01 0.899999999
// 通过字符串形式创建对象，结果一定精确
BigDecimal bd1 = new BigDecimal("0.09")
BigDecimal bd2 = new BigDecimal("0.01")
BigDecimal result = bd1.add(bd2) // 0.1
    
 // 部分方法
public static BigDecimal valueOf(double val) //获取对象
public BigDecimal add(BigDecimal val) // 加法 
public BigDecimal substract(BigDecimal val) //减法
public BigDecimal multiply(BigDecimal val) // c乘法
public BigDecimal divide(BigDecimal val) // 除法
public BigDecimal divide(BigDecimal val, 精确几位，舍入模式) // 除法（除不尽使用）
```


### Date

```java
// format格式化 返回String（日期对象->字符串）
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss")
Date d1 = new Date(0L);
String str = sdf.format(d1) // 根据上面格式进行格式化 str输出1970年01月01日 08:00:00

// parse解析 返回Date对象（字符串->日期对象）
String str2 = "2023-11-11 11:11:11"
    // 这个格式必须和字符串格式完全一致
SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss")
Date d2 = sdf2.parse(str2) // 返回一个Date对象
```

### Calendar

 操作年月日的对象

```java
// calendar是抽象类，通过调用静态方法创建实例
Calendar c = Calendar.getInstance()
int year = c.get(Calendar.YEAR)
int month = c.get(Calendar.MONTH) + 1
    
c.set(Calendar.YEAR, 2008)

```



### LocalDate&LocalTime

```java
    // LocalDate获取年月
    LocalDate lcDate = LocalDate.of(2023, 1, 1)
    int year = lcDate.getYear()
    // 获取月对象
    Month m = lcDate.getMonth()
    // 月对象获取值
    int monthVal = m.getValue()
    
    
    // 判断今天是不是你生日
    LocalDate birDate = LocalDate.of(2000, 1, 1) // 获取指定日期对象
    LocalDate nowDate = LocalDate.now()
    MonthDay birMd = MonthDay.of(birDate.getMonthValue(), birDate.getDayOfMonth())
    MonthDay nowMd = MonthDay.from(nowDate)
    
    System.out.print("今天是你生日吗" + birMd.equals(nowMd)) 
        
        
        // 获取本地时间的日历对象（包含时分秒）
        LocalTime nowTime = LocalTime.now()
        int hour = nowTime.getHour()
        int min = nowTime.getMinute()
        
        // with系列方法 只能修改时分秒
        nowTime.withHour(10)
```



## 包装类

基本数据类型对应的对象

int ->Integer

char -> Character

long -> Long

float -> Float

...

### 自动装箱和自动拆箱

```java
// JDK5之前
// 构造方法创建Integer对象
Integer i1 = new Integer(100);
Integer i2 = new Integer("100");
// 静态方法获取
Integer i3 = Integer.valueOf(123)
Integer i4 = Integer.valueOf("123")
    

	// JDK5之后，自动装箱和自动拆箱概念
    // 自动装箱：把基本数据类型自动变成其对应的包装类
    // 自动拆箱：把包装类自动变成其对象的基本数据类型
    // 在底层会自动调用静态方法valueOf得到一个Integer对象，不需要手动调用
    // 自动装箱动作
    Integer i5 = 10;

	// 自动拆箱动作 jdk5之后 int和Integer可以看作同一个东西，因为内部会自动转化
	int i6 = i5;

	// 自动拆箱+自动装箱 java底层自动操作
	Integer i1 = 10;
	Integer i2 = 20;
	Integer i3 = i1 + i2; // 自动拆箱后相加，再自动装箱后变成对象赋值给i3
```

### 包装类成员方法

```java
//parseInt 最常用的方法， 字符串转成整数， 只能传数字类型字符，有其他字符就会报错 
int num = Integer.parseInt("123")

// boolean包装类 字符串转布尔 除了Character 其他包装类都有parseXX转化方法
String str = "true"
boolean b = Boolean.parseBoolean(str)
```



## 算法

### 查找算法

#### 基本查找（顺序查找）

```java
package com.demo03;

import java.util.ArrayList;

public class suanfa01 {
    public static void main(String[] args) {
        // 顺序查找
        // 需求：根据给定的数组和数字，返回数字在数组中的索引，数字存在重复要返回所有索引
        int[] arr1 = {131,127,148,81,103,81,23,7,79,92,81};
        ArrayList<Integer> list = getAllIndex(arr1, 81);
        for (int i = 0; i < list.size(); i++) {
            System.out.print(list.get(i) + " ");
        }
    }
    public static ArrayList<Integer> getAllIndex(int[] arr, int num) {
        // 定义一个集合，用来存放int型索引
        ArrayList<Integer> list = new ArrayList<>();
        for (int i = 0; i < arr.length; i++) {
            if(arr[i] == num) {
                list.add(i);
            }
        }
        return list;
    }
}
```



**前提：数组中数据必须是有序的**

**二分、插值、斐波那契都是通过不断缩小范围来查找数据， 不同点就是对mid计算方式的不同**



#### 二分查找

核心逻辑：每次排除一半的查找范围

```java
package com.demo03;

public class suanfa02 {
    public static void main(String[] args) {
        // 二分查找（前提是数据必须有序）
        int[] arr = {23,42,55,65,88,99,323,754,1042};
        int idx = binarySearch(arr, 55);
        System.out.println(idx);
    }
    public static int binarySearch(int[] arr, int num) {
        // 定义头尾索引
        int begin = 0;
        int end = arr.length - 1;
        while(true) {
            // 查找数据不存在的情况，begin会跑到end右边
            if (begin > end) {
                return -1;
            }
            int mid = (begin + end) / 2;
            // 查找数据大于中间值,在mid右侧
            if(arr[mid] < num) {
                begin = mid + 1; // 排除左边所有数据，begin从mid+1开始
            }
            // 查找的数据小于中间值， 在mid左侧
            if(arr[mid] > num) {
                end = mid - 1;
            }
            if(arr[mid] == num) { // 表示找到 返回该索引
                return mid;
            }
        }
    }
}
```



#### 插值查找





#### 斐波那契查找

根据黄金分割点计算Mid位置

mid = min + 黄金分割点左半边长度 - 1



#### 分块查找

前提1：每个块之间没有交集（块内无序，块间有序）

前提2：块数量一般等于数字的个数开根号，比如16个数字一般分为4块

**核心逻辑：确定要查找的元素在哪一块，然后在块内挨个查找**

```java
package com.demo03;

public class suanfa03 {
    public static void main(String[] args) {
        // 分块查找
        // 无序数组arr, 把无序数组分成4块，4块之间的数值没有交集
        int[] arr = {
                27,22,30,40,36,
                13,19,16,20,
                7,10,
                43,50,48
        };
        // 分成4个块对象
        Block b1 = new Block(40, 22, 0, 4);
        Block b2 = new Block(20, 13, 5,8);
        Block b3 = new Block(10,7,9,10);
        Block b4 = new Block(50,43,11,13);
        // 确定要查找的数字在哪一个块中 查找48
        int num = 48;
        Block[] blockArr = new Block[]{b1, b2, b3, b4};
        // 获取目标块
        Block block = getBlock(blockArr, num);
        // 获取要查找元素的索引
        int index = getIndex(block, arr, num);
        System.out.println(index);
    }
    public static Block getBlock(Block[] blockArr, int num) {
        for (int i = 0; i < blockArr.length; i++) {
            // 表示在这个块内
            if(blockArr[i].getMax() >= num && blockArr[i].getMin() <= num) {
                return blockArr[i];
            }
        }
        return null;
    }
    public static int getIndex(Block block, int[] arr, int num) {
        // 用目标块的索引遍历原数组
        for (int i = block.getStartIndex(); i <= block.getEndIndex(); i++) {
            if(arr[i] == num) {
                return i;
            }
        }
        return -1;
    }
}
// 定义一个块对象，保存每一个块数组，块数组内记录最大值，最小值，原始数组的开始索引和结束索引
class Block{
    private int max;
    private int min;
    private int startIndex;
    private int endIndex;

    public Block() {
    }

    public Block(int max, int min, int startIndex, int endIndex) {
        this.max = max;
        this.min = min;
        this.startIndex = startIndex;
        this.endIndex = endIndex;
    }

    public int getMax() {
        return max;
    }

    public void setMax(int max) {
        this.max = max;
    }

    public int getMin() {
        return min;
    }

    public void setMin(int min) {
        this.min = min;
    }

    public int getStartIndex() {
        return startIndex;
    }

    public void setStartIndex(int startIndex) {
        this.startIndex = startIndex;
    }

    public int getEndIndex() {
        return endIndex;
    }

    public void setEndIndex(int endIndex) {
        this.endIndex = endIndex;
    }
}
```



### 排序算法

#### 冒泡排序

```java
// 相邻元素两两比较，大的放右边，每轮循环结束，最大的就会在最右边，下一轮只循环剩余元素 （n个元素，总共执行n-1轮）
package com.david.demo05suanfa;

public class suanfa01 {
    public static void main(String[] args) {
        // 冒泡排序
        int[] arr = {3,2,4,5,1};

        // 核心逻辑:相邻元素两两比较，大的放右边，每轮循环结束，最大的就会在最右边，下一轮只循环剩余元素 （n个元素，总共执行n-1轮）
        for (int i = 0; i < arr.length - 1; i++) { // arr.length-1 表示最后一个元素无需和自己比较,少比较一轮
            // 内循环:每次结束,最大元素到最右侧 注意-1 防止最后一个元素下标越界
            for (int j = 0; j < arr.length -1 - i; j++) {
                // 比相邻元素, 交换位置
                if(arr[j] > arr[j + 1]) {
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
            for (int i1 = 0; i1 < arr.length; i1++) {
                System.out.print(arr[i1] + " ");
            }
            System.out.println(); // 打印4轮, 每一轮比较剩余元素, 并把最大元素放到右侧
        }
    }
}

```



#### 选择排序

逻辑和冒泡类似，也是双重循环

从索引0开始，和后面的每个元素比较，小就交换，第一轮结束，最小的就会在最左边，下一轮从索引1开始

```java
package com.david.demo05suanfa;

public class suanfa01 {
    public static void main(String[] args) {
        int[] arr = {3,2,4,5,1,6,7};

        // 2.选择排序 逻辑和冒泡类似，也是双重循环
        //从索引0开始，和后面的每个元素比较，小就交换，第一轮结束，最小的就会在最左边，下一轮从索引1开始
        for (int i = 0; i < arr.length; i++) {
            // 内循环第一个元素是i, 从1+i开始
            for (int j = 1 + i; j < arr.length; j++) {
                // 用第一个跟后面每一个元素比较, 小的放到第一个
                if(arr[i] > arr[j]) {
                    int temp = arr[i];
                    arr[i] = arr[j];
                    arr[j] = temp;
                }
            }
        }
        for (int i1 = 0; i1 < arr.length; i1++) {
            System.out.print(arr[i1] + " ");
        }
    }
}
```



#### 递归

```java
// 递归 必须有一个退出条件
package com.david.demo05suanfa;

public class digui01 {
    public static void main(String[] args) {
        // 递归求100+99+98...+1之和
        int total = getSum(100);
        System.out.println(total);
    }
    private static int getSum(int num) {
        if(num > 0) {
            return num + getSum(num - 1);
        }
        // num == 0 时退出递归
        return 0;
    }
}
```



#### 快速排序（效率最高的排序）

第一轮：把索引0的数字作为基准数，确定基准数在数组中的位置(基准数归位)，比基准数小的全部在左，比基准数大的全部在右

第二轮：对基准数左侧和右侧进行递归调用

```java
package com.david.demo05suanfa;

public class kuaipai {
    public static void main(String[] args) {
        // 快速排序
        int[] arr = {2,7,3,5,1,4,0,9,10,12};
        quickSort(arr,0,arr.length - 1);
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " "); // 第一轮结束，2是基准数，左侧比2小，右侧比2大： 1 0 | 2 | 5 3 4 7 9 10 12
        }
        // 2,0,3,5,1,4,7,9,10,12  --> 7,0互换
        // 2,0,1,5,3,4,7,9,10,12 -->  3,1互换
        // 1,0,2,5,3,4,7,9,10,12 --> 基准数归位 2，1互换
    }


    // 入参排序数组，排序开始索引，排序结束索引
    private static void quickSort(int[] arr, int i, int j) {
        // 定义两个变量确定查找范围
        int start = i;
        int end = j;
        // 记录基准数
        int baseNum = arr[i];

        // 递归退出条件
        if(start >= end) {
            return;
        }

        // 指向同一个位置时退出循环
        while (start != end) {
   			// 注意：必须先从后往前循环，否则基准数归位若刚好那个交换的数比基准数大就会把大数移动到左边
            // 从后往前循环 数组末尾往前比较，遇到比基准数小的，停止
            while (true) {
                if(arr[end] < baseNum || start >= end) {
                    break;
                }
                end--;
            }
            // 从前往后循环 数组开头往后比较, 遇到比基准数大的，停止
            while (true) {
                if(arr[start] > baseNum || start >= end) {
                    break;
                }
                start++;
            }
            // 交换索引指向的数据
            int temp = arr[start];
            arr[start] = arr[end];
            arr[end] = temp;
        }
        // start，end指向同一个元素时退出循环， 表示已找到基准数要存放的位置
        // 基准数归位
        int temp = arr[i];
        arr[i] = arr[end];
        arr[end] = temp;

        // 用递归继续比较基准数左侧和右侧的数据
        quickSort(arr, i, end - 1);
        quickSort(arr, end + 1, j);
    }
}
```



### Arrays工具类

```java
// Arrays静态方法
// public static String toString(数组)  //把数组拼接陈一个字符串
// public static int binarySearch(数组, 查找的元素)  // 二分法查找元素
// public static int[] copyOf(原数组， 新数组长度) // 拷贝数组
// public static int[] copyOfRange(原数组, from, to) // 拷贝指定范围数组
// public static void fill(数组， 元素) //填充数组
// public static void sort(数组)
// public static void sort(数组，排序规则)


// 排序写法
// 根据函数返回值判断 元素应该放在比较元素的左侧还是右侧
// 简单理解 o1-o2：升序排列  o2-o1:降序排列
Integer[] arr = {2,5,1,0,13,43,33,44,5,8};
Arrays.sort(arr, new Comparator<Integer>() { // 第二个参数接收一个接口， 用匿名类实现该接口
    @Override
    public int compare(Integer o1, Integer o2) { // 重写接口compare方法
        return o1 - o2;
    }
});
```



### Lambda表达式

JDK8后新语法

**作用：简化匿名内部类书写，只能简化函数式接口的匿名内部类写法**，体现函数式编程思想

函数式接口：有且只有一个抽象方法的接口就叫做函数式接口，接口上方加@FunctionalInterface注解

```java
Integer[] arr = {2,5,1,0,13,43,33,44,5,8};
// Lambda简写匿名内部类，类似js箭头函数
Arrays.sort(arr, (Integer o1, Integer o2) -> {
        return o1 - o2;
    }
);
```



###  经典算法

#### 斐波那契数列

定义：数列前2项之和等于后一项

1,1,2,3,5,8,13....

求数列第12个元素的值

```java
public class suanfa04 {
    public static void main(String[] args) {
        //斐波那契数列
        //1,1,2,3,5,8... 求第12位数字
        int num = getNum(12);
        System.out.println(num); //144
    }
    private static int getNum(int num){
        // 递归思路：第12位是前两位之和，fn(12) = fn(11)+fn(10)
        // 退出递归条件 n <= 2
        if(num <= 2) {
            return 1;
        } else {
            return getNum(num - 1) + getNum(num - 2);
        }
    }
}
```



#### 猴子吃桃

一堆桃子，猴子第一天吃其中的一半再多吃一个，之后每天吃剩下的一半再多吃一个，第10天还没吃只剩1个桃子，问最开始一共几个桃子

```java
package com.demo03;

public class suanfa04 {
    public static void main(String[] args) {
        // 猴子吃桃 每天吃一半加一个 count/2 - 1
        // 第10天剩1个
        // 反向推导第9天: (1+1) * 2 = 4
        // 第8天（4+1）*2 = 10
        // 第7天 (10+1)*2 = 22
        int count = getCount(1); // 入参是第一天
        System.out.println(count);
    }
    private static int getCount(int day) {
        if(day == 10) {
            return 1;
        } else {
            return (getCount(day + 1) + 1) * 2;
        }
    }
}

```





## 数据结构

数据结构是计算机底层存储、组织数据的方式，是指数据相互之间以什么样的方式排列在一起的。

常见数据结构：栈、队列、数组、链表、二叉树、二叉查找树、平衡二叉树、红黑树

### 栈

特点：先进后出、后进先出 (栈顶开口，栈底封口，从栈顶进，从栈顶出)



### 队列

特点：先进先出、后进后出（两端都开口，从后端进入队列，从前端出队列）



### 数组

**数组是一种查询快，增删慢的模型**

查询快：查询是通过数组的地址值和索引查找，(**数组元素在内存中是连续存储的，通过地址值就能直接获取到，所以查询快**)

删除和添加效率低：删除或增加新元素后，其他元素都需要移动



### 链表

**查询慢，增删快的模型，和数组相反**



**单向链表**

链表由结点组成，由前一个结点记录后一个结点的地址值，连接而成

**链表中每个结点都是独立的对象，在内存中不连续，每个结点包含数据值和下一个结点的地址值**

单向链表查询数据只能从头结点往后查，所以慢

插入（添加）： 把前一个元素的地址值指向新元素，再把新元素的地址指向后一个元素

删除：把前一个元素的地址值指向新元素，再把新元素的地址指向后一个元素，再把要删除的元素直接删除即可

**双向链表**

记录前一个结点和后一个结点两个地址值，支持从两个方向查找，查询效率比单向链表高



### 二叉树

二叉树中每个节点保存自己的值，还保存父节点地址，左子节点地址，右子节点地址

度：每个节点的子节点数量

二叉树中，任意节点的子节点的度<=2

高度：树的总层数

#### 二叉查找树(有2,3规则)

1、每个节点上最多2个子节点

2、任意节点左子树上的值都小于当前节点

3、任意节点右子树上的值都大于当前节点

**添加节点时，当节点大于当前节点时，存右边，小于当前节点，存左边，若已有节点，再继续比较。**



**二叉树遍历**

前序遍历：从根节点开始，按当前节点-->左子节点-->右子节点顺序遍历

中序遍历：从最左边的子节点开始，按左子节点-->当前节点-->右子节点顺序遍历 (左中右，获取出的数据是从小到大排列)

后序遍历：左右中，最后根节点

层序遍历：从根节点开始一层一层遍历



#### 平衡二叉树

优化二叉查找树的缺点，若添加的数据一直很大或很小，那二叉查找树就会一直往一侧添加，导致左右子树高度差很大

规则：**任意节点左右子树高度差不超过1， 注意是任意节点，不能只看根节点**





## 泛型

在集合中：

优点：统一数据类型，在编译阶段确定数据类型，避免强制转换可能出现的异常。

泛型不能写基本数据类型，必须写包装类

集合若不写泛型，类型默认是Object类型

```java
ArrayList list = new Arraylist(); // 不传泛型，默认为Object类型，接收任意数据类型
list.add(123);
list.add("david");
list.add(new Person(name: "kashin", age:35));

// 不传泛型最大缺陷：多态的Object类型不能使用子类的特有方法
Iterator it = list.iterator();
while(it.hasNext()) {
    Object obj = it.next(); // 多态定义的list元素都是Object类型
    obj.length() // 报错， object类没有length方法，强制转换也不行，因为不知道转成什么类型
}
```

### 泛型类

```java
// 创建对象时才确定类型
public class MyArrayList<E> {
    Object[] obj = new Object[10]; // 长度为10的object类型数组
    int size; // 保存长度
    
    
    // 定义该类的add方法，接收一个E类型的元素
    public boolean add(E e) {
        obj[size] = e;
        size++;
        return true;
    }
    
    // 获取指定索引元素
    public E get(int index) {
        return (E)obj[index] // 注意：因为不确定类型，要强转为E类型
    }
    
    @Override
    public String toString() {
        return Arrays.toString(obj) // 重写toStirng, 打印数组
    }
}

// 创建一个接收字符串类型的集合对象
MyArrayList<String> list = new MyArrayList<>();
list.add("abc");
MyArrayList<Integer> list2 = new MyArrayList<>();
list.add(123);
```

### 泛型方法

1.泛型加在类上，类中的方法全部可用

2.泛型单独加在方法上，本方法可用

```java
import java.util.ArrayList;
// 工具类
public class ListUtil {
    // 私有化构造方法
    private ListUtil(){

    }
    // 定义一个添加N个元素的泛型方法，接收被添加的集合，要添加的元素
    public static <E>void addAll(ArrayList<E> list, E...e) {// 不定参数
        // 不定参数e会变成数组，遍历数组进行添加即可
        for (E e1 : e) {
            list.add(e1);
        }
    }
}

// 调用
import java.util.ArrayList;

public class ListUtilTest {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("david");
        list.add("billy");
        // 泛型方法在接收第一个list参数时类型就被确定
        ListUtil.addAll(list, "DAVID","KING");
        System.out.println(list); // [david, billy, DAVID, KING]
    }
}
```

### 泛型接口

```java
// 创建语法
public interface List<E> {}

// 调用方式1：实现类传入类型
public class MyArrayList implements List<String> {
    @Override
    //... 重写List所有方法，此时所有方法接收参数都是String类型
}
// 此时创建对象不用传入类型，已默认String类型
MyArrayList list = new MyArrayList();
list.add("123") // 必须传String
    

    
    // 调用方式2： 实现类不能确定类型，延续泛型
public class MyArrayList2<E> implements List<E> {
    // 此时重写方法的参数仍是E，不确定类型
}

// 调用时和调用泛型类一样，需传入类型
MyArrayList2<String> list2 = new MyArrayList2<>();
```



### 泛型通配符

**泛型没有继承性，泛型只接受确定的类型**

**泛型通配符用来限定泛型的范围**

?： 表示不确定类型，和直接写泛型没区别

**? extends E**：表示可以传E或E的子类类型

**? super E**：表示可以传递E或者E的父类类型

```java
import java.util.ArrayList;

public class tongpeifu {
    public static void main(String[] args) {
        ArrayList<Ye> list1 = new ArrayList<>();
        ArrayList<Fu> list2 = new ArrayList<>();
        ArrayList<Zi> list3 = new ArrayList<>();
//        methods(list1); //这个会报错，因为通配符限制只能传Fu或Fu的子类
        methods(list2);
        methods(list3);
    }
    
    // 泛型通配符，限定传入的类型，单独写泛型其实什么类型都可以传
    public static void methods(ArrayList<? extends Fu> list) {}
}


class Ye {}
class Fu extends Ye {}
class Zi extends Fu {}
```



