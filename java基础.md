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



## java所有环境变量配置

1、新建系统变量JAVA_HOME，变量值设置为jdk安装路径如E:\develop\jdk

2、系统变量Path内增加一条引用：%JAVA_HOME%\bin (%%表示引用) 



## 编译型与解释型语言区别

1、编译型语言整体编译后产生一个新文件，交给机器执行（c++/c）

2、解释型语言利用解释器按行解释，交给机器执行，不会产生新文件(python,  js)

java属于混合型语言，java编译后产生的class文件是运行在java虚拟机环境上的(实现了java跨平台的能力)



## 关键字

java中关键字都是小写

### class

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

1、基本数据类型

```java
//定义long,float类型要加后缀标识L,F
long b = 9999999L
float z = 10.33F

```



2、引用数据类型

**基本类型**

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



## 开发工具IDEA

目前java市场占有率最高的集成开发工具

http://www.jetbrains.com/idea/



## 导入工具类

```java
import java.util.Scanner; // 导入Scanner类,该类的作用是记录键盘选择的数字

public class myImport{
	public static void main(String[] agrs){
		Scanner sc = new Scanner(System.in);
		System.out.println("请输入第一个数字");
		int number1 = sc.nextInt(); // 保存键盘选择的数字
		
		System.out.println("请输入第二个数字");
		int number2 = sc.nextInt();
		System.out.println(number1+number2); // 打印之和
	}
}
```

