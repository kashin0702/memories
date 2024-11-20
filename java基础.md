

# **java基础**

## **微服务架构**

**![image-20230725121557135](D:\typora-img\image-20230725121557135.png)**

## **java版本**

**javaSE: java标准版，用于桌面应用开发**

**javaME: 嵌入式设备，小型移动设备应用开发(早期塞班系统的应用，已凉)**

**javaEE(重点): java企业版 web方向网站开发(浏览器+服务器) 服务器开发领域no1**

## **cmd常用命令**

**盘符+冒号  盘符切换** 

**dir   查看当前路径下内容**

**cd 目录名  进入单级目录**

**cd..    作用：回退到上级目录**

**cd 目录1\目录2\...    进入多级目录**

**cd \   回退到盘符目录**

**cls   清屏**

**exit  退出命令提示符窗口**

**环境变量作用：在任意目录下打开指定的软件**

**配置环境变量: 高级系统设置==》环境变量==》系统变量==》选择path==》点击编辑==》点击新建保存程序路径**



## **安装jdk**

**下载官网：https://www.oracle.com/java/technologies/downloads/**

**1.路径不要包含中文**

**2.开发工具放统一目录**

**java17安装时会自动配置4个环境变量(java,javac,javaw,jshell)**

**一些重要文件：**

**jdk/bin/javac 用来编译文件的工具**

**jdk/bin/java 用来执行文件的工具**

```java
public class HelloWorld{
    // public static void main是程序主入口
    public static void main(String[] args) {
        System.out.println("helloworld");
    }
}
```

**如何运行java程序**

**1、在要运行的文件目录下打开cmd  执行javac  xxx.java  编译源文件，会生成一个class文件**

**2、同目录下执行java xxx (不加后缀名) 即可看到运行结果**

**jdk和jre**

**jdk包含了jvm,核心类库，开发工具(javac,java,jhatjdb)等调试工具**

**jre:只需要运行，不需要开发时使用,就是java运行环境，包含了jvm和核心类库这些运行必备环境，去除了jdk中的开发工具的环境包**



## **java环境变量配置**

**1、新建系统变量JAVA_HOME，变量值设置为jdk安装路径如E:\develop\jdk**

**2、系统变量Path内增加一条引用：%JAVA_HOME%\bin (%%表示引用)** 



## **编译型与解释型语言区别**

**1、编译型语言整体编译后产生一个新文件，交给机器执行（c++/c）**

**2、解释型语言利用解释器按行解释，交给机器执行，不会产生新文件(python,  js)**

**java属于混合型语言，java编译后产生的class文件是运行在java虚拟机环境上的(实现了java跨平台的能力)**





## **class**

**用于定义/创建一个类， 类是java最基本的组成单元**





## **字面量类型**

**字面量：数据在程序中的书写格式**

**java中有6种字面量类型：整数，小数，字符串，字符，布尔，空**

**![image-20230628153909801](D:\typora-img\image-20230628153909801.png)**



## **制表符 \t**

**打印时把前面字符串长度补齐到8，或8的整数倍。最少补1个空格，最多补8个空格**

**![image-20230628154307751](D:\typora-img\image-20230628154307751.png)**



## **输出语句占位符**

```java
public class Demo {
    public static void main() {
        System.out.printf("%s,你好啊", "张三"); // %s就是后面张三的占位符
    }
}
```





## **变量定义格式**

**数据类型 变量名= 数据值**

```java
int a = 10;
double b = 10.2;
```

## **不同进制的表现形式**

**二进制：代码中以0b开头**

**十进制：不加任何前缀**

**八进制：代码中以0开头**

**16进制：代码中以0x开头**



## **进制转换**

**![image-20230628163008744](D:\typora-img\image-20230628163008744.png)**

**![image-20230628163351983](D:\typora-img\image-20230628163351983.png)**



## **计算机存储过程**

**任意数据都以二进制存储**

**计算机存储数据大致分为3类，文本，图片，音频， （视频其实就是许多图片+音频的集合）**

**1、文本text(数字，字母，汉字等)**

**数字==》直接转成二进制**

**'a'英文字符==》查找ASCII码表对应的数字==》转成二进制**

**汉字 ==》查找unicode码表对应的数字==》转成二进制**

**2、图片image**

**图片由像素点组成，像素点由3原色组成，3原色可以用(255,255,255)数字表示**

**3、声音sound**

**用数字记录采样点，音质越高，采样点越多**



## **数据类型**

### **JAVA内存结构**

**栈内存：方法执行的地方(main方法等)，基本数据类型都保存在栈内存**

**堆内存：new关键词创建的引用数据类型保存的地方**

**方法区：各种字节码文件保存的地方, 一个类想要被使用，这个类的字节码文件就要加载到方法区中临时存储**



### **1、基本数据类型**

```java
//定义long,float类型要加后缀标识L,F
long b = 9999999L
float z = 10.33F

```



**基本类型(整数/浮点/布尔/字符)**

**数据值存储在变量自己的内存空间,在栈内存中开辟一个独立空间**

 **变量保存的是真实值(栈内存)**

| **数据类型** | **关键字**  |
| ------------ | ----------- |
| **整数**     | **byte**    |
| **整数**     | **short**   |
| **整数**     | **int**     |
| **整数**     | **long**    |
| **浮点数**   | **float**   |
| **浮点数**   | **double**  |
| **字符**     | **char**    |
| **布尔**     | **boolean** |

**![image-20230628170513944](D:\typora-img\image-20230628170513944.png)**



### **2、引用数据类型**

**除了基本4个类型的,都是引用数据类型**

1. **new关键词创建的都是引用数据类型(变量保存的是内存地址,数据存在堆内存中)**




## **开发工具IDEA**

**目前java市场占有率最高的集成开发工具**

**http://www.jetbrains.com/idea/**

**idea项目结构**

**idea开发目录按以下结构创建**

**项目-模块-包(目录)-类**

**project-->modules-->package-->class**



## **IDEA快捷键**

**定义还没创建的方法后, 选中方法alt+回车, 快速创建**

**alt+insert 可以快速创建构造方法和get/set方法**

**ctrl+p  选中方法的括号按查看方法接收的参数**

**ctrl+b 选中方法, 跟进方法**

**ctrl+alt+M 把选中的内容抽取成一个方法**

**ctrl+n  全局搜索, 可以搜索查看类的源码**

​	**ctrl+f12  查看类的构造方法(一个类可能有很多个不同参数的构造方法)**

**alt+回车 重写接口方法**

**ctrl+alt+v 自动生成方法的接收类型**

**右键方法-->Go to --> implemetation 查看方法的实现类**

**ctrl+alt+t  把选中的代码包裹到想要的控制语句中**

**alt+回车 选中红色异常处，抛出异常**

**shift+f6 选中类名，快捷修改类名**



## **第三方包引入**

**引入的第三方jar包一般放在lib文件夹中, 右键 jar包选择: add as libary 即可展开**



## **工具类Scanner**

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



## **算术运算符**

**注意点:**

### **算术运算符+号**

**1.char字符型会转换为ascii码对应数字,再进行计算**

**2.byte,short,char类型参与计算,会统一提升为Int类型再参与计算**

**3.任意类型与string字符串型相加都会变成字符串型(字符串拼接, +号理解为拼接符, 字符串只有加操作,没有减乘除)**

**取值范围从小到大: byte < short < int < long <float < double** 

**隐式转换: 不同数据类型相加, 取值范围小的会自动变大(自动类型提升)**

**强制转换: 把大的范围类型赋值给小的范围,需要强制转换 (可能导致数据错误)**



**隐式转换底层: 如int提升为long 底层是在int(4字节32位)前补32个0, 变成long(8个字节64位)**

**强制转换底层: 如int降低为byte 底层是把int(32位)前的24位直接去掉, 变成byte(1个字节8位)**

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





### **赋值运算符**

**=, +=, -=, *=, /=, %=**

```java
public class Arithmetic {
    int a = 10;
    double b = 20.0;
    a += b; // 等同于a = (int) (a + b)  +=,-=,*=,/=,%=底层都隐藏了一个强制类型转换 
    System.out.println(b) // 30 b是double型,会强制转为a的int类型
}
```

### **关系运算符**

**关系运算符的结果都为Boolean型**

**<  >= <=**

**== 判断两边值是否相等**

**!= 判断两边值是否不等**



### **位运算符（也可参与逻辑运算但不常用）**

**&| : 不管左侧true false 右侧都会执行**

**&： 二进制数按位与，两个位都为1，结果为1；逻辑运算时：两边都为真,结果才为真**

**| ：二进制数按位或，两个位都为0，结果为0； 逻辑运算时：两边都为假,结果才为假**

**^ ：两个位相同为0，不同为1；逻辑运算时： 相同为false,不同为true**

**! 逻辑非  取反 (java中只用一个!  不要用!!)**



### **短路逻辑运算符**

**含义:左侧表达式能确定最终结果,右侧就不会执行**

**&&  短路与  结果和&相同,但具有短路效果, 左侧为false时,右侧不执行**

**||   短路或  结果和|相同, 但具有短路效果, 左侧为true时,右侧不执行** 



### **运算符优先级**

**![image-20230630164354777](D:\typora-img\image-20230630164354777.png)**

## **原码 补码 反码**

**计算机的最小储存单位是byte, 即8位 所以1个字节的储存范围是-128~127**

**正整数 :**
	**原反补码相同，都是正整数的二进制表示**
**负整数 :**
 **原码 :**
  **数值的二进制**
 **反码:**
 	**原码各个位取反**
 **补码:**
  **反码的基础上加1**

**原码: 十进制数据的二进制表现,最左边是符号位,0为正, 1为负**

**原码对正数计算没问题,但计算负数时,实际运算结果与预期是相反的**

**反码:解决原码不能计算负数的问题**

**反码计算规则: 正数的反码不变,负数的反码在原码的基础上,符号位不变,数值取反,0变1,1变0**

**反码中0有两种表现形式, 负数跨0计算时会产生1的误差(正数计算不会产生误差)**

**补码: 在反码的基础上+1 错开一位, 屏蔽误差**

**补码产生一个特殊值-128, 该数据在一个字节下,没有原码和反码**

**数字的存储和计算都是以补码的形式来操作的**

**![image-20230630172727088](D:\typora-img\image-20230630172727088.png)**



### **循环for和while使用场景**

**for循环：知道循环次数或者循环的范围**

**循环体语句执行后会执行条件控制语句，再进入下一次条件判断**

**for (初始化语句；条件判断语句；条件控制语句) ｛**

​	**循环体语句；**

**｝**

**while循环：不知道循环次数或者范围，只知道循环结束的条件**

**初始化语句；**

**while (条件判断语句) ｛**

​	**循环体语句；**

​	**条件控制语句；**

**｝**

### **回文数判断**

**从左往右和从右往左读是一样的整数，121是回文数**

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



### **生成随机数**

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

### **猜数字1~100**

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



## **数组**

**数组是一种容器，在存储数据时要考虑存在隐式转换，所以容器的类型和存储类型最好保持一致**

**数组定义的2种格式**

**格式1：数据类型[] 数组名**

**格式2：数据类型 数组名[]**

**静态初始化完整格式：数据类型[] 数组名 = new 数据类型[]{元素1，元素2，...};**

**动态初始化：数据类型[] 数组名 = new 数据类型[数组长度];**

**静态初始化指定数组元素，系统根据元素个数计算数组长度**

**动态初始知道数组长度，系统给出默认初始化值**

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

### **初始化值**

 **int默认值是0**

**double默认0.0**

**char默认'/u0000' (空格)**

**布尔默认false**

**引用类型默认null（String是引用类型）** 

### **取最值**

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

### **数组元素交换**

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

### **数组打乱**

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



### **二维数组**

**定义方式**

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





## **方法**

**形参: 方法定义中的参数**

**实参: 方法调用中的参数**

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

### **方法重载**

**重载的含义: 同一个类中,方法名相同,参数不同的方法, 与返回值无关**

**不用定义太多名字不同的方法, 方便调用**

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

### **数组拷贝**

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



### **质数判断**

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

### **随机验证码生成**

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

### **数字加密**

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



## **面向对象**

**javabean类：用来面熟一类事物的类叫javabean类； javabean类中不写main方法**

**测试类：编写main方法的类叫测试类，在测试类中可以创建javabean类的对象并进行赋值调用**

**一个java文件内可以定义多个类，但只能有一个public类， pubic修饰的类名必须和文件名一样。**

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



### **main方法**

**main方法是java程序的主入口**

**因为main方法是静态的, 所以测试类中其他方法也必须静态(静态方法只能访问静态方法和变量)**

```java
public class xxSystem { 
    // main方法其实也是属于xxSystem类中的一个静态方法, 所以main调用其他方法也必须是静态的
    public static void main(String[] args) {
        // xxx...
    }
}
```





### **封装**

**对象代表什么,就要封装对应的数据,并提供数据对应的行为**



### **成员变量和局部变量**

**成员变量和局部变量重名,不使用this就会触发就近原则, 谁离得近就用谁**

**形参也是局部变量**

**使用this使用成员变量**

**若没有局部变量, 不使用this也会查找成员变量(成员变量作用域在整个类中有效)**

**this作用: 区分成员变量和局部变量**

**this本质: 方法调用者的内存地址**

**Friend f = new Friend();  此时f获得一个Friend对象的内存地址**

**f.xxFn()  xxFn中的this就是f的内存地址**

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

**![image-20230704115804563](D:\typora-img\image-20230704115804563.png)**



### **权限修饰符范围**

**权限范围由小到大:  private < 空着不写 < protected < public**

**实际开发中常用private和public**

**![image-20230707162000404](D:\typora-img\image-20230707162000404.png)**



### **public修饰符**

**被jvm调用, 权限修饰符 访问权限足够大**



### **private修饰符**

**权限修饰符, 可以修饰成员变量和方法,被修饰的成员只能在本类中访问,需提供get/set方法让外部访问**

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







### **构造方法**

**也叫构造函数**

**作用: 创建对象时给成员变量进行赋值**

**规则: 方法名和类名相同,大小写也要一致, 没有返回值, 没有void**

**构造方法在创建对象时由虚拟机自动调用,每次创建都会调用一次**

**若没写构造方法,程序也会自动创建一个空参构造方法**

**建议: 创建类时有参构造和无参构造都要写, 方便使用 ,  若只写一个有参构造,不传参时就会报错**

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



### **标准javabean类**

**1.成员变量使用private修饰**

**2.提供空参构造和全参构造2个构造方法**

**3.提供每一个成员变量的get/set方法**

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



### **键盘接收方法区别**

**nextInt(), nextDouble(), next() 在遇到空格/制表符/回车就会停止接收, 这些符号后面的数据会被自动放到第二个next接收**

**nextLine() 也接收字符串, 遇到空格也能接收, 不建议和上面混用**

```java
Scanner sc = new Scanner(System.in)

int num1 = sc.nextInt(); // 接收整数 如果输入123 456  num1=123 num2=456
int num2 = sc.nextInt(); 
double num3 = sc.nextDouble(); // 接收小数
String str = sc.next(); // 接收字符串



```



### **复杂对象数组处理**

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



## **面向对象进阶**

### **static修饰符**

**静态属性修饰符static**  

**静态属性**

1. **对象可共享该属性,**
2. **不属于对象, 属于类**
3. **随类加载而加载,优先于对象存在, 对象在new以后才会存在**

**静态方法**

**1.多用在测试类或工具类中, javabean类中很少用**

**2.静态方法只能访问静态变量和静态方法, 静态方法中没有this关键字**

**非静态方法可以访问静态方法和变量, 也可以访问非静态的成员方法和变量(访问所有内容)**

**调用: 属性和方法都推荐用类名.调用** 

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





### **final修饰符**

**final可以修饰方法, 类, 变量**

**修饰方法: 最终方法,不能被重写**

**修饰类: 最终类, 不能被继承**

**修饰量: 变成常量, 只能被赋值一次(常用)**

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







### **工具类**

**帮助我们做一些事情,但不描述任何事物的类**

```java
public class ArrUtil{
    private ArrUtil(){} // 关键: 私有化构造函数,不让外部调用new创建对象, 工具类没有必要创建对象
   
    // 提供各种静态方法
    public static int getMax(){}
    public static int getMin(){}
}
```







## **String字符串**

**String类是java定义好的一个类,定义在java.lang包中,是java的核心包, 使用时不需要导包**

**java中所有的字符串都被实例为此类对象**

**字符串创建后,它的值不会改变**

**String name = "david";**

**name = "kashin";  // 创建了一个新的字符串类赋给了name,之前的字符串没有变**

### **字符串创建**

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



### **String中内存关系**

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



### **字符串比较**

**基本类型使用==号比较的是值**

**引用类型使用==号比较的是内存地址**

```java
// 要比较字符串值, 使用equals()方法
String name = "abc";
String name2 = "abc";
System.out.print(name.equals(name2)) // true 方法返回一个布尔值, 比较值是否相等
```

### **登录字符串校验**

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



### **字符串获取长度**

**字符串通过length()方法获取长度, 和数组的length属性直接获取有区别**

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

### **字符串反转**

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



### **数字转大写汉字**

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



### **substring**

**substring(int beginIndex, int endIndex) 包头不包尾, 返回一个截取后的字符串,不修改原字符串, 和js一样**

### **身份证截取**

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



### **replace**

**replace(旧值, 新值)**



### **startsWith**

**startsWith("xx")   以某个字符串开头, 返回布尔值**



### **StringBuilder**

**字符串创建容器, 提升字符串各种操作效率(拼接,反转)**

**实例化后打印sb对象,打印的是属性值,不是地址值(java底层处理过)**

**![image-20230705092724087](D:\typora-img\image-20230705092724087.png)**

```java
StringBuilder sb = new StringBuilder("abc");
// 添加内容
sb.append(123);
sb.append(true); //abc123true

sb.reverse(); // 反转内容
int len = sb.length() // 获取长度
    
String str = sb.toString() // sb对象转换为String
```



### **StringJoiner**

**jdk8后出现, 实际作用和Stringbuilder一样, 代码编写比StringBuilder更简洁**

**![image-20230705095054242](D:\typora-img\image-20230705095054242.png)**

```java
StringJoiner sj = new StringJoiner(", ", "[", "]"); // 指定间隔符,开始符(可选), 结束符(可选)
sj.add("aaa").add("ccc").add("bbb") // [aaa, ccc, bbb]
```



### **字符串数字变罗马数字**

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

### **字符串比较**

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

### **toCharArray**

**字符串转成字符数组**

```java
String str = "abcd";
char[] chs = str.toCharArray(); // 返回一个字符数组, 可以用new String(chs) 再变回字符串
```



### **字符串数字相乘**

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



## **集合**

**和数组区别:**

1. **数组长度固定, 集合长度可变**
2. **数组可以存基本数据类型和引用数据类型, 集合只能存引用数据类型,要存基本类型必须是包装类**



### **包装类**

**集合添加基本类型,要使用包装类, 泛型中传入包装类**

```java
ArrayList<Integer> list = new ArrayList<>();
ArrayList<Short> list2 = new ArrayList<>();
```



**![image-20230705145708622](D:\typora-img\image-20230705145708622.png)**

### **ArrayList**

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

### **ArrayList底层原理**

**集合的底层也是数组**

**1.用空参构造创建ArrayList时，底层默认创建一个长度为0的数组**

**2.当添加第一个元素时，底层创建一个新的长度为10的数组**

**3.集合存满时，自动扩容为1.5倍(每次存满都会自动扩容)**

**4.若一次性存多个元素存满，自动扩容为当前实际长度**

**集合每添加一个元素，size++**

**集合的size方法有2层含义：1.集合的元素个数  2.下次存入的位置**





### **集合查数据**

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



### **break跳出外层循环**

**循环内嵌套switch语句时, switch使用默认Break, 退出的是switch语句,并不能退出外层while循环**

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



## **集合进阶**

**集合体系结构**

**集合分为两类：单列集合（一次添加一个数据），双列集合（一次添加一对数据）**

**单列集合顶层接口是Collection，子类是List和Set**

**List和Set区别：**

**List：添加元素有序（指存放和获取有序），可重复，有索引**

**Set：添加元素无序，不重复，无索引**

**![image-20230720141356522](D:\typora-img\image-20230720141356522.png)**

### **单列集合Collection**

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

### **Collection遍历**

**有3种遍历方式：迭代器、增强for、Lambda表达式遍历**

**常用成员方法：add,clear,remove,contains,isEmpty,size**

**在遍历中想删除元素，使用迭代器**

**仅想遍历元素使用增强for和Lambda表达式**



#### **迭代器**

**特点：迭代器不依赖索引**

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



#### **增强for遍历**

**底层也是Iterator原理，简化迭代器书写**

**单列集合和数组才能用增强for遍历**

**格式：for(元素数据类型 变量名：数组or集合) {}**

```java
for (String s : list) { 
	System.out.println(s)  // s就是集合中的每个元素
}
```



#### **Lambda表达式遍历**

**用forEach方法遍历**

**forEach(Consumer) // 接收一个Consumer接口，要传实现类**

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

### **List集合**

**list也是接口，继承了Collection所有方法**

**List集合独有方法：**

**void add(int index, E element)  在指定位置插入指定元素**

**E remove(int index) 删除指定索引元素，返回被删除的元素**

**E set(int index, E elment) 修改指定索引元素，返回被修改的元素**

**E get(int index) 返回指定索引元素**

```java
list.add(1);
list.add(2);
// remove是一个重载方法，还有个父类方法remove(Object o), 这里优先调用实参和形参一致的方法，即remove(int index)
list.remove(1);
```



#### **List遍历**

**List集合支持Collection的所有遍历方式，还支持普通for循环遍历和列表迭代器遍历**

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



#### **LinkedList集合**

**底层数据结构是双向链表，是查询慢，增删快的模型 （操作首尾元素极快，含特有操作首尾元素的API）**

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



### **Set集合**

**无序、不重复、无索引**

**同样继承Collection的所有方法**

```java
Set<String> s = new HashSet<>()
    
s.add("david")
s.add("david") // 不会被添加，返回false
s.add("kashin")

// set除了含索引的方法，其他都支持
// 增强for遍历
for (String s1 : s) {
    System.out.println(s1);
}
// 迭代器遍历
Iterator<String> it = s.iterator();
while(it.hasNext()) {
    String str = it.next();
    System.out.println(str);
}

// forEach遍历
s.forEach(new Consumer<String>() {
    @Override
    public void accept(String s) {
        System.out.print(s + " ");
    }
});
// Lambda简写
s.forEach(s1 -> {
    System.out.println(s1 + " lambda ");
});
 
```



#### **HashSet**

**Hashset底层采用哈希表存储数据**

**jdk8之后：哈希表由数组+链表+红黑树组成**

**核心概念-->哈希值：对象的整数表现形式**

**哈希值是根据hashCode方法计算出来的int型整数，该方法定义在Object类中，所有对象都可以调用，默认采用地址值进行计算**

**一般会重写hashCode方法，利用对象内部属性值计算哈希值，不同对象只要属性值相同哈希值也一样。**

**小概率情况，不同的属性值或地址值计算出的哈希值也有可能一样（哈希碰撞）**

```java
Student s1 = new Student("david", 25);
Student s2 = new Student("david", 25);
s1.hashCode() // 一串int整数
s2.hashCode() // 若在类中重写了hashCode方法，此时两个值相等

// 哈希碰撞  字符串会根据内部属性计算哈希值
"abc".hashCode() //96354 
"acD".hashCode() //96354
```

**扩容：加载因子就是扩容时机，当16个数组存了16*0.75=12个元素时，数组自动扩容为之前的2倍**

**当链表长度大于8且数组长度大于等于64，链表自动转为红黑树**

**存储方式：**

**创建一个长度16，默认加载因子0.75的数组，根据哈希值和数组长度计算存储的位置**

**1.计算出的存储位置是null, 直接存入**

**2.计算出的存储位置已有元素，用equals方法判断：**

​	**1）属性值一样，舍弃**

​	**2）属性值不同，挂在已有元素下形成链表**



**重要结论：因为hashCode使用地址值比较，所以当要使用HashSet保存自定义对象时，必须重写hashCode和equals方法**



#### **LinkedHashSet**

**有序（数据存储和读取的顺序一致）、不重复、无索引**

**每个元素比hashSet多一个双向链表机制记录存储的位置**

**遍历时，遍历的是双向链表，所以读取的顺序和存入顺序一致**



#### **TreeSet**

**基于红黑树进行存储**

**不重复、无索引、可排序**

```java
public class treeSetTest {
    public static void main(String[] args) {
        TreeSet<Integer> ts = new TreeSet<>();
        ts.add(1);
        ts.add(10);
        ts.add(7);
        ts.add(21);
        System.out.println(ts); //[1,7,10,21] 数字默认升序排列，字符和字符串按照asc码表对应的数字升序排列
        
        // 自定义对象保存到TreeSet时，必须实现一个Comparable接口，再重写compareTo方法，才能实现排序
        TreeSet<Person> ts = new TreeSet<>();
        Person p1 = new Person("david", 35);
        Person p2 = new Person("kashin", 24);
        Person p3 = new Person("king", 33);
        ts.add(p1);
        ts.add(p2);
        ts.add(p3);
        System.out.println(ts); // 按年龄排序
    }
}
// Person自定义对象
package com.demo05.SetTest;

public class Person implements Comparable<Person> {
    private String name;
    private int age;

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
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

    public Person() {
    }

    // 重写比较方法，根据年龄排序
    @Override
    public int compareTo(Person o) {
        // this是当前对象，入参o是已存在于红黑树中的对象
        return this.getAge() - o.getAge();
    }
}

```



### **双列集合Map**

**![image-20230724152057608](D:\typora-img\image-20230724152057608.png)**

**map特点：无序，不重复、无索引，每次添加一对数据，一一对应，键不能重复，值可以重复（也叫键值对对象/Entry对象）**

**双列集合顶层接口是Map**

```java
// map要传2键和值的类型
HashMap<String, String> map = new HashMap<>();
map.put("david", "kashin"); // 添加键值对
map.put("david", "king"); // 若键已存在，put会覆盖原来的值，并返回被覆盖的值
// 删除
map.remove("david")
// 清空
map.clear();
// 是否包含,用key判断
boolean result = map.containsKey("david");
// 是否包含，用value判断
boolean result2 = map.containsValue("king");
// 长度， size()
```



#### **map遍历**

**三种遍历方式**

**1.键找值**

**2.遍历键值对对象**

**3.Lambda表达式**

```java
public class MapTest {
    public static void main(String[] args) {
        HashMap<String, String> map = new HashMap<>();
        map.put("david", "kashin");
        map.put("leon", "naduo");
        map.put("king", "billy");

        // 1.获取键再遍历, keySet方法返回一个含所有的键的Set集合
        Set<String> keys = map.keySet();
        for (String key : keys) {
            String val = map.get(key); // 利用键获取值
            System.out.println(key + " = " + val);
        }
        
        // 2.键值对遍历 entrySet返回Map对象的所有键值对
        Set<Map.Entry<String, String>> entries = map.entrySet();
        for (Map.Entry<String, String> entry : entries) {
            String key = entry.getKey();
            String val = entry.getValue();
            System.out.println(key + " : " + val);
        }
        
        // 3.lambda表达式forEach
    }
}
```



#### **HashMap**

**底层结构也是哈希表，利用键计算哈希值，计算出存放在数组中的位置**

**依赖hashCode, equals方法保证键的唯一**

**键是自定义对象时，必须重写上面两个方法**

#### **HashMap练习**

```java
package com.david.demo08Map;

import java.util.*;

public class HashMapTest {
    public static void main(String[] args) {
        // A,B,C,D 四个景点，有80个学生，每人投票一次，统计投票最多的景点

        // 创建景点
        String[] places = {"A", "B", "C", "D"};
        // 随机数生成80个学生的投票结果
        ArrayList<String> votes = new ArrayList<>();
        Random r = new Random();
        for (int i = 0; i < 80; i++) {
            int rdx = r.nextInt(places.length);
            votes.add(places[rdx]); // 随机投给一个景点
        }
        // 计算每个景点的投票数
        // 创建一个map集合，键值对是：景点名-投票数
        HashMap<String, Integer> map = new HashMap<>();
        for (String vote : votes) {
            // 存在该景点，count+1
            if (map.containsKey(vote)) {
                int count = map.get(vote);
                count += 1;
                map.put(vote, count); // 修改count值+1
            } else {
                // 不存在，添加
                map.put(vote, 1);
            }
        }
        System.out.println(map);
        // 遍历map对象，获取count最大值
        Set<Map.Entry<String, Integer>> entries = map.entrySet();
        int max = 0;
        for (Map.Entry<String, Integer> entry : entries) {
            if (entry.getValue() > max) {
                max = entry.getValue();
            }
        }
        System.out.println(max);
        String bestPlace = null;
        for (Map.Entry<String, Integer> entry : entries) {
            if (entry.getValue() == max) {
                bestPlace = entry.getKey();
            }
        }
        System.out.println("最佳景点是：" + bestPlace);
    }
}

```



#### **HashMap底层原理**

**HashMap底层是数组+链表+红黑树结构**

**大致步骤**

**1、根据键计算哈希值，和在数组中存放的位置，若该位置为null,直接存放**

**2、若该位置有数据，循环该位置链表或红黑树判断是否重复，若不重复，则挂链表或挂红黑树（链表长度超过8转红黑树）**

**3、若重复，跳出循环，并用新的值覆盖旧的值**

```java
// 内部类Node， 每个hashMap的元素都是一个Node对象, 实现了Entry接口， 所以一个元素就是一个Entry对象（键值对对象）
// 底层会调用new Node()创建键值对对象，放入数组
static class Node<K,V> implements Map.Entry<K,V> {
    
}

// 返回值：被覆盖元素的值，如果没有覆盖，返回null
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}

// 生成hash值的方法
static final int hash(Object Key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h>>>16); // 获取hash值，进行一些异或运算
}

//参数1:键的哈希值
//参数2：键
//参数3：值
//参数4：键重复时是否保留 true保留不覆盖，false不保留覆盖
final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
    
}
```





#### **LinkedHashMap**

**有序（由键决定）、无重复、无索引**

**双向链表保存存储时的位置，保证存储和取出的顺序一致**



#### **TreeMap**

**底层和TreeSet一样，都是红黑树结构，可以自定排序规则（默认按键的升序排列）**



### **不可变集合**

**场景：某个数据不能被修改，可把它防御性拷贝到不可变集合中**

```java
// 一旦创建，只能查询，不能增删
// 不可变list
List<String> list = List.of("david", "kashin", "king");

// 不可变set
Set<String> set = Set.of("david", "king", "kashin")
    
// 不可变map,难点
HashMap<String, String> hm = new HashMap<>();
hm.put("david", "hahaha");
hm.put("kashin", "hahaha");
hm.put("leon", "hahaha");

// 利用hm创建一个不可变集合
// 获取所有键值对对象
Set<Map.Entry<String, String>> entries = hm.entrySet();
// toArray方法 把集合转成数组
Map.Entry[] arr = entries.toArray(new Map.Entry[0]) // toArray接收一个数组对象，传入一个长度0的Entry数组，即可返回一个entry数组
    
// 创建不可变map
Map map = Map.ofEntries(arr) // ofEntries接收一个Entry类型不定参数...entries, 不定参数也能接收同类型数组
```





### **集合随机练习**

```java
package com.david.demo09Collection;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Random;

public class randomDianming {
    public static void main(String[] args) {
        // 需求1：有N个学生，要求每次点名70%概率点到男生，30%概率点到女生

        // 思路：创建一个长度10的集合，7个1,3个0 随机这个数组就能实现概率
        ArrayList<Integer> nums = new ArrayList<>();
        // 用Collections工具类实现addall
        Collections.addAll(nums, 1,1,1,1,1,1,1,0,0,0);
        // 打乱集合顺序
        Collections.shuffle(nums);
        // 创建男生女生集合
        ArrayList<String> boys = new ArrayList<>();
        ArrayList<String> girls = new ArrayList<>();
        Collections.addAll(boys, "张三", "李四","大卫","王五");
        Collections.addAll(girls, "韩梅梅", "刘亦菲", "黄蓉");
        // 获取nums随机结果，1就在男生里随机一个，0就在女生里随机一个
        Random r = new Random();
        int idx = r.nextInt(nums.size());
        if (nums.get(idx) == 1) {
            int idx2 = r.nextInt(boys.size());
            System.out.println(boys.get(idx2));
        } else {
            int idx3 = r.nextInt(girls.size());
            System.out.println(girls.get(idx3));
        }
        
        // 需求2：被点到名的学生不会再点到，点完所有学生后，开启下一轮点名
        // 思路：每点完一个学生，从集合中删除该学生，并用一个空集合接收，循环结束后再放回去，即可开启下一轮点名
    }
}

```

### **扑克游戏集合练习**

```java
package com.david.Gamedoudizhu;

import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.TreeSet;

// 扑克游戏构造函数
public class PokerGame {
    // 权重序号，排序核心用的就是序号
    static ArrayList<Integer> serialsNumber = new ArrayList<>();

    // 用hashMap保存{权重序号：牌面}的键值对对象
    static HashMap<Integer, String> hs = new HashMap<>();
    // 准备牌
    // 生成所有牌只需要执行一次，所以写在静态代码块中只会随类的加载执行一次，不会因为new而多次创建
    static {
        String[] color = {"♦", "♣", "♠", "♥"};
        String[] number = {"3","4","5","6","7","8","9","10","J","Q","K","A","2"};
        int count = 0;
        for (String n : number) {
            for (String c : color) {
                hs.put(count, n + c); // 保存权重：牌面键值对
                serialsNumber.add(count);// 保存所有权重序号，发牌发的就是权重序号
                count++;
            }
        }
        hs.put(count, "小王");
        serialsNumber.add(count);
        count++;
        hs.put(count, "大王");
        serialsNumber.add(count);
    }
    public PokerGame() {
        // 洗牌， 打乱集合即可
        Collections.shuffle(serialsNumber);
        System.out.println(serialsNumber);
        System.out.println(hs);

        // 发牌，斗地主有3个玩家+1个底牌池，所以定义4个集合
        // 要给牌排序，直接用TreeSet集合
        TreeSet<Integer> p1 = new TreeSet<>();
        TreeSet<Integer> p2 = new TreeSet<>();
        TreeSet<Integer> p3 = new TreeSet<>();
        TreeSet<Integer> pool = new TreeSet<>();
        // 按顺序取牌组中的牌，依次发给底池和3个玩家
        for (int i = 0; i < serialsNumber.size(); i++) {
            // 前3张给底池
            if (i <= 2) {
                pool.add(serialsNumber.get(i));
                continue;
            }
            // 依次给3个玩家发牌，利用 %3 取余的思想
            if (i % 3 == 0) {
                p1.add(serialsNumber.get(i));
            } else if (i % 3 == 1) {
                p2.add(serialsNumber.get(i));
            } else {
                p3.add(serialsNumber.get(i));
            }
        }
        // 遍历看3个玩家的牌
        lookPoker("玩家1", p1);
        lookPoker("玩家2", p2);
        lookPoker("玩家3", p3);
    }
    // 看牌
    public void lookPoker(String name, TreeSet<Integer> list) {
        System.out.print(name + ": ");
        for (Integer integer : list) {
            String number = hs.get(integer); // 根据序号获取对应的牌面
            System.out.print(number + " ");
        }
        System.out.println();
    }
}


// 界面部分：部分代码 每张牌对象的构造函数 入参1牌的名字，2.显示正面还是背面
public Poker(String name, boolean up){
    this.name = name;
    this.up = up;
    if (this.up) {
        turnFront() // 显示正面
    } else {
        turnRear() // 显示背面
    }
    // 设置牌的宽高
    this.setSize(71, 96)
    // 显示在界面上
    this.setVisible(true)
    // 监听每一张牌
    this.addMouseListener(this)
}

public void turnFront() {
    this.setIcon(new ImageIcon("path\\..."+ name + ".jpg")); // 根据名字显示每一张牌
    this.up = true // 修改成员变量
}
public void turnRear() {
    this.setIcon(new ImageIcon("path\\...rear.jpg")) // 反面就一种图
    this.up = false
}

// 初始化牌（准备牌，洗牌，发牌） 牌名用数字-数字表示
public void initCard() {
    // 生成牌池
    for(int i = 1;i<=5;i++) {
        for(j=1;j<=13;j++) {
            if (i==5 && j>2) {
                break;
            } else {
                Poker poker = new Poker(i+"-"+j, false); // 循环创建牌对象，传牌的i-j就是牌的名字
                poker.setLocation(350, 150);
                pokerList.add(poker);
                container.add(poker);
            }
        }
    }
    Collections.shuffle(pokerList) // 洗牌
        
    // 创建3个集合保存玩家的牌
    //...
}
```







## **继承**

**1.java只能单继承，一个类只能继承一个直接父类**

**2.java不能多继承，但支持多层继承， （可以继承父类的父类）**

**3.java中所有类都直接或间接继承于Object类**

**子类可以得到父类的属性和方法**

**子类可以在父类基础上增加功能**



### **继承的范围**

**继承和调用概念不要混淆， 继承是拷贝了一份父类的方法或属性，复制到了子类中**

**构造方法: 非私有/private都不能**

**成员变量: 非私有/private都能 (private會被继承，但不能直接调用，一样要用set/get方法调用)**

**成员方法: 非私有能, private不能  只有虚方法会被继承(非private/非static/非final就是虚方法)**

```java
// 子类 extends父类
pubic class Student extends Person{}
```



### **super**

**super()就是执行父类的无参构造函数**

**super可以直接调用父类的变量和方法**

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



### **空参构造附初始值**

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







### **成员变量访问特性**

**遵循就近原则，局部变量--》成员变量--》父类变量**

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



### **方法重写 Override**

**子类中出现和父类名字一样的方法，就是重写方法 重写的方法必须和父类保持一致，形参，方法名,返回值（小于等于父类）**

**不被继承的方法不能重写**

**@override 重写注解，加在重写方法后面，让虚拟机校验重写语法是否正确**

**个人理解:方法重写是重写从父类虚方法表中复制过来的方法, 不会影响父类的方法**



## **多态**

**表现形式：**

**子类对象赋值给父类类型 , 格式: 父类类型 名称 = 子类对象;**

**前提：**

**1、必须是继承关系** 

**2、必须有方法重写** 

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



### **多态创建子类**

**成员变量==> 堆内存中的一个对象中,会分别创建一个父类变量和一个子类变量, 当用父类类型调用时,查的就是父类的变量**

**成员方法==> 编译看左边, 运行看右边, 运行被重写的方法, 也就是成员方法**

**![image-20230707113119742](D:\typora-img\image-20230707113119742.png)**

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





### **多态优点**

**1,多态形式下,实现右边对象解耦,便于扩展和维护**

```java
Person p = new Teacher(); // 比如想调用 student.work 直接把new teacher改成new Student即可
p.work() // 业务逻辑改变时,后续代码无需修改
```



**2.重要:定义方法的时候,使用父类型作为形参,可以接收所有子类对象,体现多态扩展性**

```java
ArrayList<Person> list = new ArrayList<>(); // 把父类作为对象类型传给集合, 集合可接收所有子类
```





### **多态缺点**

**多态不能访问子类的特有方法**

**当多态定义的对象调用的方法在父类中不存在时,就会报错**

**此时就要使用强制类型转换转成子类,再调用**

```java
class Animal {
    
}
class Dog extends Animal{
    public void show(){}
}

Animal a = new Dog();
Dog d = (Dog) a; // animal强制转换成dog类型, 调用show方法才不会报错
d.show();
```



### **instancof**

**类型判断**

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



### **多态应用**

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



## **抽象类**

**当使用多态时, 子类不重写父类方法,父类就无法调用, 所以定义抽象类和抽象方法可以让子类强制重写抽象方法**

**抽象类不能创建对象**

**有抽象方法的类一定是抽象类, 抽象类可以没有抽象方法**

**抽象类可以有构造方法**

**子类继承抽象类时需满足2个要求其中1个**

  **1.重写抽象类的所有抽象方法**

2. **自己也定义成一个抽象类**

**抽象类的好处：强制重写子类，也就统一了子类的方法名参数和返回，方便统一维护**

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



## **接口**

 **接口是一种规则， 是对行为的抽象，使用implements关键词表示**

**接口不能实例化**

**接口的子类（实现类）**

**1.要么重写接口中的所有抽象方法**

**2.要么自己也是抽象类**

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



### **接口中成员的特点**

**成员变量：只能是常量，用public static final修饰**

**构造方法:  没有**

**成员方法：jdk7之前：只能是抽象方法，默认用public abstract修饰。**  

 **jdk8后可以添加默认方法（有方法体）不需要强制重写，但如果重写需要去掉default关键词**

**jdk9后可以添加私有方法（普通私有，静态私有）**

**目的：默认方法中有重复代码，不想让外部调用又想抽取，定义到私有方法中**

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



### **接口的多态**

**当一个方法的参数是接口时，可以传递接口所有实现类的对象，这种方式就叫接口多态**

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





### **接口和类之间的关系**

**类和类：继承关系，只能单继承，但可以多层继承**

**类和接口：实现关系，可以单实现也可以多实现，还可以在继承一个类同时实现多个接口**

**接口和接口：继承关系，可以单继承，也可以多继承**

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



### **适配器设计模式**

**当一个接口有非常多抽象方法时，实现类就需要重写所有方法，很不方便**

**添加一个中间类，重写所有抽象方法为空方法，再用实现类继承这个中间类，重写要用到的方法即可**

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



## **内部类**

**定义：在类里面定义的类，就叫内部类**

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



### **内部类分类**

### **1.成员内部类** 

**写在成员位置，属于外部类的成员，地位和成员变量方法一样**

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

**定义在方法内的类, 类似方法内定义的变量**

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

**隐藏了名字的内部类, 可以现在成员位置,也可以写在局部位置**

**格式: new 类名or接口名() {**

​		**重写方法;**

**};**

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





## **导包**

**包就是文件夹,用来管理不同功能的java类**

**写法: 域名反写+包的作用, 全部小写**

**不需要导包的情况**

**1.使用同一个包中的类**

**2.使用java.lang包中的类**

**同时使用两个包中相同名字的类时, 要使用全类名**

```java
package com.david.demoDuotai04; // 导包

// 全类名
com.david.demoDuotai04.Person p1 = new com.david.demoDuotai04.Person();
```



## **代码块**

**局部代码块(已淘汰, 没必要)**

**构造代码块(不灵活, 建议把构造代码块内容抽取到独立方法中, 在构造方法中再调用)**

**静态代码块(重点, static修饰, 随类加载而加载, 并且自动触发, 只执行一次, new创建不会再执行)**

**静态代码块作用: 在类加载中, 做一些数据初始化**

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





## **GUI(图形化界面)**

**JAVA一般做服务器开发,比较少用图形化界面**

**指采用图形化的方式显示操作界面**

**定义在AWT包和Swing包中**

**java的图形组件:**

**JFrame 最外层窗体主窗体**

**JMenuBar 最外层菜单 --- JMenu 二层菜单 -- JMenuItem 最底层菜单**

**JLabel 管理文字和图片的容器, 图片必须添加到JLabel中**

**ImageIcon  添加图片**

**JButton 按钮**

**JTextField 文本输入框(明文)** 

**JPasswordField 密文输入框**

**JDialog 弹窗窗体**



## **监听**

**KeyListener 键盘监听**

**MouseListener 鼠标监听**

**ActionListener 动作监听(只能监听鼠标左键和空格)**

### **实现监听1** 

**implements实现监听接口**

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

### **实现监听2**

**传匿名内部类**

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

## **java游戏打包**

**前提: 必须包含图形化界面,  代码, 图片, jdk都要打包**

**1.把所有代码打包成.jar后缀的压缩包(jar就是java格式的压缩包)**

**2.把jar包转成exe安装包(只有代码)**

**3.把2的exe再和图片和jdk整合在一起, 变成最终的exe安装包**





## **常用API**

### **Math** 

**常用方法和js中基本一样**





### **System**

 **入参 0: 正常停止   非0: 异常停止**

**System.exit(int status) // 退出虚拟机** 

**System.currentTimeMillis() // 获取时间戳**

**System.arraycopy(arr1, srcPos, arr2, destPos, length) // 把数组1中的数据拷贝到数组2中**





### **Runtime**

**Runtime.getRuntime()   //Runtime的静态方法,获取当前系统的运行环境对象**

**exit(int status) // 停止虚拟机, System.exit底层调用的就是这个方法**

**availableProcessors() // 获得cpu线程数**

**maxMemory //jvm能从系统中获取总内存的大小(单位byte)**

**exec() // 运行cmd命令**

```java
Runtime.getRuntime().exec("notepad") // 用命令行打开记事本
Runtime.getRuntime().exec("shutdown -s -t 3600") // 3600秒后关机
```



### **Object**

**object类是java顶级父类,所有类都直接或间接继承于Object**

#### **toString()**  

**返回对象的字符串表示形式**

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



#### **equals(Object obj)** 

**比较两个对象是否相等(Object的equals方法使用==比较两个对象的地址值, 可以重写方法比较属性值)**

#### **clone(int a)** 

**对象克隆(浅拷贝)**

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



### **Objects（工具类）**

**Objects是一个工具类, 提供一些操作对象的方法**

**equals(Object a, Object b) // 先做非空判断,比较两个对象, 返回boolean**

**isNull(Object obj) // 判断对象是否为null, 为null返回ture** 

**nonNull(Object obj) // 判断对象是否为Null, 跟isNull的结果相反**



### **Collections（工具类）**

**常用API**

```java
public static <T> boolean addAll(Collection<T> c, T...elements) //批量添加元素，只能添加单列集合
public static void shuffle(List<?> list) // 打乱list集合元素的顺序


ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "abc","cba","ddd"); // 批量添加
```



### **Arrays（工具类）**

```java
// Arrays静态方法
public static String toString(数组)  //把数组拼接陈一个字符串
public static int binarySearch(数组, 查找的元素)  // 二分法查找元素
public static int[] copyOf(原数组， 新数组长度) // 拷贝数组
public static int[] copyOfRange(原数组, from, to) // 拷贝指定范围数组
public static void fill(数组， 元素) //填充数组
public static void sort(数组)
public static void sort(数组，排序规则)


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

// 二分法查找相关技巧
/*如果key在数组中，则返回搜索值的索引；否则返回-1或“-”（插入点）。插入点是索引键将要插入数组的那一点，即第一个大于该键的元素的索引。
[1] 搜索值不是数组元素，且在数组范围内，从1开始计数，得“ - 插入点索引值”；
[2] 搜索值是数组元素，从0开始计数，得搜索值的索引值；
[3] 搜索值不是数组元素，且小于数组内元素，索引值为 – 1；
[4] 搜索值不是数组元素，且大于数组内元素，索引值为 – (length + 1);
*/
Arrays.binarySearch()
```





### **BigInteger大整数**

**1.如果biginteger没有超过Long取值范围, 可以用静态方法BigInteger.valueOf()创建**

**2.如果超出了, 则用构造方法new BigInteger() 创建**

**3.对象一旦创建,内部值不会改变**

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



### **BigDecimal**

**表示较大的小数，  解决精度丢失问题**

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


### **Date**

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

### **Calendar**

 **操作年月日的对象**

```java
// calendar是抽象类，通过调用静态方法创建实例
Calendar c = Calendar.getInstance()
int year = c.get(Calendar.YEAR)
int month = c.get(Calendar.MONTH) + 1
    
c.set(Calendar.YEAR, 2008)

```



### **LocalDate&LocalTime**

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



## **包装类**

**基本数据类型对应的对象**

**int ->Integer**

**char -> Character**

**long -> Long**

**float -> Float**

**...**

### **自动装箱和自动拆箱**

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

### **包装类成员方法**

```java
//parseInt 最常用的方法， 字符串转成整数， 只能传数字类型字符，有其他字符就会报错 
int num = Integer.parseInt("123")

// boolean包装类 字符串转布尔 除了Character 其他包装类都有parseXX转化方法
String str = "true"
boolean b = Boolean.parseBoolean(str)
```



## **算法**

### **查找算法**

#### **基本查找（顺序查找）**

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



#### **二分查找**

**核心逻辑：每次排除一半的查找范围**

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



#### **插值查找**





#### **斐波那契查找**

**根据黄金分割点计算Mid位置**

**mid = min + 黄金分割点左半边长度 - 1**



#### **分块查找**

**前提1：每个块之间没有交集（块内无序，块间有序）**

**前提2：块数量一般等于数字的个数开根号，比如16个数字一般分为4块**

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



### **排序算法**

#### **冒泡排序**

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



#### **选择排序**

**逻辑和冒泡类似，也是双重循环**

**从索引0开始，和后面的每个元素比较，小就交换，第一轮结束，最小的就会在最左边，下一轮从索引1开始**

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



#### **递归**

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



#### **快速排序（效率最高的排序）**

**第一轮：把索引0的数字作为基准数，确定基准数在数组中的位置(基准数归位)，比基准数小的全部在左，比基准数大的全部在右**

**第二轮：对基准数左侧和右侧进行递归调用**

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





### **Lambda表达式**

**JDK8后新语法**

**作用：简化匿名内部类书写，只能简化函数式接口的匿名内部类写法，体现函数式编程思想**

**函数式接口：有且只有一个抽象方法的接口就叫做函数式接口，接口上方加@FunctionalInterface注解**

```java
Integer[] arr = {2,5,1,0,13,43,33,44,5,8};
// Lambda简写匿名内部类，类似js箭头函数
Arrays.sort(arr, (Integer o1, Integer o2) -> {
        return o1 - o2;
    }
);
```



###  **经典算法**

#### **斐波那契数列**

**定义：数列前2项之和等于后一项**

**1,1,2,3,5,8,13....**

**求数列第12个元素的值**

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



#### **猴子吃桃**

**一堆桃子，猴子第一天吃其中的一半再多吃一个，之后每天吃剩下的一半再多吃一个，第10天还没吃只剩1个桃子，问最开始一共几个桃子**

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





## **数据结构**

**数据结构是计算机底层存储、组织数据的方式，是指数据相互之间以什么样的方式排列在一起的。**

**常见数据结构：栈、队列、数组、链表、二叉树、二叉查找树、平衡二叉树、红黑树**

### **栈**

**特点：先进后出、后进先出 (栈顶开口，栈底封口，从栈顶进，从栈顶出)**



### **队列**

**特点：先进先出、后进后出（两端都开口，从后端进入队列，从前端出队列）**



### **数组**

**数组是一种查询快，增删慢的模型**

**查询快：查询是通过数组的地址值和索引查找，(数组元素在内存中是连续存储的，通过地址值就能直接获取到，所以查询快)**

**删除和添加效率低：删除或增加新元素后，其他元素都需要移动**



### **链表**

**查询慢，增删快的模型，和数组相反**

**单向链表**

**链表由结点组成，由前一个结点记录后一个结点的地址值，连接而成**

**链表中每个结点都是独立的对象，在内存中不连续，每个结点包含数据值和下一个结点的地址值**

**单向链表查询数据只能从头结点往后查，所以慢**

**插入（添加）： 把前一个元素的地址值指向新元素，再把新元素的地址指向后一个元素**

**删除：把前一个元素的地址值指向新元素，再把新元素的地址指向后一个元素，再把要删除的元素直接删除即可**

**双向链表**

**记录前一个结点和后一个结点两个地址值，支持从两个方向查找，查询效率比单向链表高**



### **二叉树**

**二叉树中每个节点保存自己的值，还保存父节点地址，左子节点地址，右子节点地址**

**度：每个节点的子节点数量**

**二叉树中，任意节点的子节点的度<=2**

**高度：树的总层数**

#### **二叉查找树(有2,3规则)**

**查找效率比普通二叉树高**

**1、每个节点上最多2个子节点**

**2、任意节点左子树上的值都小于当前节点**

**3、任意节点右子树上的值都大于当前节点**

**添加节点时，当节点大于当前节点时，存右边，小于当前节点，存左边，若已有节点，再继续比较。**



**二叉树遍历**

**前序遍历：从根节点开始，按当前节点-->左子节点-->右子节点顺序遍历**

**中序遍历：从最左边的子节点开始，按左子节点-->当前节点-->右子节点顺序遍历 (左中右，获取出的数据是从小到大排列)**

**后序遍历：左右中，最后根节点**

**层序遍历：从根节点开始一层一层遍历**



#### **平衡二叉树**

**优化二叉查找树的缺点，若添加的数据一直很大或很小，那二叉查找树就会一直往一侧添加，导致左右子树高度差很大**

**规则：任意节点左右子树高度差不超过1， 注意是任意节点的子树**

**（难点）平衡旋转机制：旋转的前提：当添加结点导致树不平衡，就需要旋转， 有左旋、右旋两种方式**



#### **红黑树**

**是一种自平衡的二叉查找树，每个结点上都有存储位表示结点的颜色**

**每个结点可以是红或黑，红黑树不是高度平衡，而是通过“红黑规则”实现平衡**

**红黑规则：**

**1、每个节点是红色或黑色，根节点必须是黑色**

**2、如果一个节点没有子节点或父节点，则该节点相应的指针属性值为Nil,这些Nil视为叶节点（叶节点是空节点），叶节点是黑色**

**简单理解：一个节点若没有子节点，会挂2个Nil节点，Nil的值是空的**

**3、不能出现两个红色结点相连**

**4、每个节点到其后代叶节点的路径上，包含相同数目的黑色节点**



## **不定参数细节**

```java
// 形参是不定参数时，也可传这个类型的数组
public static void testFn(String...args) {}

testFn("david", "kashin")  // 调用1：内部转为String[]处理
    
String[] strs = new String[]{"david", "kashin"};
testFn(strs) // 调用2：直接传一个String[] 内部直接作为数组处理
```



## **泛型**

**在集合中：**

**优点：统一数据类型，在编译阶段确定数据类型，避免强制转换可能出现的异常。**

**泛型不能写基本数据类型，必须写包装类**

**集合若不写泛型，类型默认是Object类型**

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

### **泛型类**

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



### **泛型方法**

**1.泛型加在类上，类中的方法全部可用**

**2.泛型单独加在方法上，本方法可用**

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

### **泛型接口**

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



### **泛型通配符**

**泛型没有继承性，泛型只接受确定的类型**

**泛型通配符用来限定泛型的范围**

**?： 表示不确定类型，和直接写泛型没区别**

**? extends E：表示可以传E或E的子类类型**

**? super E：表示可以传递E或者E的父类类型**

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



## **Stream流**

**结合lambda表达式，简化数组、集合的操作**

**利用Stream流的API进行各种操作，返回新数据，不改变原数据**

**链式编程中出现终结方法后就不能继续链式编程，如打印语句，forEach方法，因为没有返回值**

**stream流每次只能使用一次，原来的流不能再次使用，所以建议用链式编程**

```java
Stream<String> s1 = list.stream().filter(s -> s.startsWith("d"));
Stream<String> s2 = s1.filter(s -> s.length() == 3); // s1在这里已被使用
Stream<String> s3 = s1.filter(s -> s.length() == 3); // 报错
```



**使用：**

**单列集合：Collection默认方法stream()**

**双列集合：无法直接使用（需要通过keySet,entrySet转换后使用）**

**数组：Arrays静态方法stream(T[] array)**

**一堆零散数据：Stream接口中的静态方法 of(T...values)**

```java
package com.demo07;

import java.util.*;
import java.util.function.Consumer;
import java.util.function.Predicate;

public class StreamTest {
    public static void main(String[] args) {
        // 单列集合操作stream流
        ArrayList<String> list1 = new ArrayList<>();
        Collections.addAll(list1, "kashin", "leon", "david");
        // 把数据放到stream流中，进行链式编程
        list1.stream().filter(new Predicate<String>() {
            @Override
            public boolean test(String s) {
                return s.startsWith("d");
            }
        }).forEach(new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        });

        // 双列集合无法直接操作stream
        HashMap<String, String> hm = new HashMap<>();
        hm.put("david", "NB1");
        hm.put("kashin", "NB2");
        hm.put("LEON", "NB3");
        // 调entrySet先获取一个set集合，在调stream
        hm.entrySet().stream().forEach(new Consumer<Map.Entry<String, String>>() {
            @Override
            public void accept(Map.Entry<String, String> stringStringEntry) {
                System.out.println(stringStringEntry);
            }
        });

        // 数组操作stream
        int[] num = new int[]{1,2,3,44,5,77};
        // lambda表达式简化链式编程
        Arrays.stream(num).filter(i -> i > 3).forEach(i -> System.out.println(i));
    }
}
```



### **常用方法**

```java
Stream<T> filter(Predicate<? super T> predicate) // 过滤
Stream<T> limit(long maxSize) // 获取前几个元素
Stream<T> skip(long n) //跳过前几个元素
Stream<T> distinct() // 元素去重，必须重写hashCode和equals方法
static<T> Stream<T> concat(Stream a, Stream b) // 合并ab两个流
Stream<R> map(Function<T,R> mapper) // 转换流中的数据类型 
    
// 去重
// stream去重
ArrayList<String> list = new ArrayList<>();
Collections.addAll(list, "张三丰","张无忌","张无忌","张三丰","乔峰","令狐冲","赵云");

// distinct去重依赖hashCode和equals方法，String类已写好，若是自定义对象必须重写这2个方法
list.stream().distinct().forEach(i-> System.out.print(i + " "));

// concat合并，是Stream静态方法
ArrayList<String> list2 = new ArrayList<>();
Collections.addAll(list2, "david", "kashin");
Stream.concat(list.stream(), list2.stream()).forEach(i-> System.out.print(i + " "));

 // map转换 获取集合中的所有数字
ArrayList<String> list3 = new ArrayList<>();
Collections.addAll(list3, "david-24","kashin-35", "leon-27");
// 思路：先截取，然后string转int
// map接收一个Function接口实现类，要实现apply方法, 第一个泛型是接收的类型，第二个泛型是返回的类型
list3.stream().map(new Function<String, Integer>() {
    @Override
    public Integer apply(String s) {
        String s2 = s.split("-")[1];
        return Integer.parseInt(s2);
    }
}).forEach(i -> System.out.print(i + " "));
```



### **终结方法**

**没有返回值，就是终结方法，就无法链式编程**

```java
void forEach(Consumer action) //遍历
long count() //统计
toArray() // 收集流中的数据，放到数组中（有空参和返回指定类型的两种传参方式）
collect(Collector colllector) // 收集流中的数据，放到集合中


// toArray收集到数组中 空参默认返回一个Object[]数组
Object[] array1 = list.stream().toArray();
System.out.println(Arrays.toString(array1)); // 返回Object[]数组
// toArray带参，返回指定类型数组
String[] str1 = list.stream().toArray(new IntFunction<String[]>() {
    @Override
    // 形参value表示流中数据的个数，要和数组个数一致
    public String[] apply(int value) {
        return new String[value];
    }
});
System.out.println(Arrays.toString(str1)); // 返回String[]数组


// 终结方法：collect:把数据收集到map集合中 toMap接收2个方法作为形参，一个是键的生成方法，一个是值的生成方法
// Function的两个泛型：第一个是流中每个数据的类型，第二个是map集合中键的数据类型
Map<String, Integer> map = list3.stream().collect(Collectors.toMap(new Function<String, String>() {
    @Override
    // 返回类型要和上面的第二个泛型对应
    public String apply(String s) {
        String ns = s.split("-")[0];
        return ns;
    }
    // 泛型1：流的每个数据的数据类型，泛型2：map集合中值的数据类型
}, new Function<String, Integer>() {
    @Override
    // 返回类型要和上面的第二个泛型对应
    public Integer apply(String s) {
        int num = Integer.parseInt(s.split("-")[1]);
        return num;
    }
}));
map.entrySet().forEach(s -> System.out.println(s));

// collect转list集合 直接在collect方法中调Collectors.toList()
List<String> stringList = list3.stream().collect(Collectors.toList());
```







## **方法引用**

**把已经有的方法拿过来用，当做函数式接口中抽象方法的方法体**

**个人理解：类似JS中直接把一个定义好的函数作为参数使用**

**满足条件:**

**1.引用处必须是函数式接口**

**2.被引用的方法已经存在**

**3.被引用方法形参和返回值必须和抽象方法一致**

**4.被引用方法功能要满足当前需求**

```java
import java.util.Arrays;
import java.util.Comparator;

public class methodsTest {
    public static void main(String[] args) {
        Integer[] arr = {1,2,3,4,5};
        // 普通写法，传匿名内部类，实现compare方法
        Arrays.sort(arr, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2-o1;
            }

            @Override
            public boolean equals(Object obj) {
                return false;
            }
        });
        for (Integer integer : arr) {
            System.out.println(integer);
        }
        Integer[] arr2 = {3,5,2,3,6,7};
        // 静态方法引用 类名::引用方法名
        Arrays.sort(arr2, methodsTest::substractionMethods);
    }
    static int substractionMethods(int num1, int num2){
        return num1 - num2;
    }
}
```

### **引用成员方法**

**格式：对象::成员方法**

**1.其他类： 其他类对象::方法名**

**2.本类： this::方法名(若在static模块中引用，不能使用this)**

**3.父类： super::方法名(若在static模块中引用，不能使用super)**

```java
import java.util.ArrayList;
import java.util.Collections;

public class methodsTest {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        Collections.addAll(list, "张三丰", "张强", "张无忌", "乔峰", "周芷若", "任盈盈");
        // 引用其他类的方法，需要new创建实例后引用
        list.stream().filter(new OtherMethod()::StringJudge).forEach(i -> System.out.println(i));
        // 本类成员方法引用，static中没有this, 也需要创建实例后引用 
        list.stream().filter(new methodsTest()::myFilter).forEach(i -> System.out.println(i));
    }
    // 本类成员方法
   	public boolean myFilter(String s) {
        return s.startsWith("张") && s.length() == 3;
    }
}

// 其他类
public class OtherMethod {
    // 成员方法，定义一个字符串过滤的方法，接收字符串，返回布尔值
    public boolean StringJudge(String s) {
        return s.startsWith("张") && s.length() == 3;
    }
}
```



### **引用构造方法**

**格式：类名::new**

**目的：创建这个类的对象**

```java
// 构造方法引用
// 需求：根据给定的string集合，把其中的姓名和年龄封装到Student对象中，并添加到一个list集合中
ArrayList<String> str1 = new ArrayList<>();
Collections.addAll(str1, "张三-23", "李四-25", "王五-32");
// 1.正常stream流 map传匿名类
List<Student> studentList = str1.stream().map(new Function<String, Student>() {
    @Override
    public Student apply(String s) {
        String name = s.split("-")[0];
        int age = Integer.parseInt(s.split("-")[1]);
        return new Student(name, age);
    }
}).collect(Collectors.toList());

// 2.引用构造方法 引用方法要满足规则：形参和返回值必须和接口方法一致，所以要重写一个构造方法
List<Student> studentList1 = str1.stream().map(Student::new).collect(Collectors.toList());



// Student类
package com.david.demo12Fangfayinyong;

public class Student {
    private String name;
    private int age;

    // 给引用方法使用的构造方法
    public Student(String s) {
        String name = s.split("-")[0];
        int age = Integer.parseInt(s.split("-")[1]);
        this.name = name;
        this.age = age;
    }
    
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public Student() {
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



### **类名引用成员方法**

**格式：类名::成员方法**

**例子：String::subString**

**独有规则：**

**1.返回值一致，被引用方法的形参要跟接口方法第二个形参到最后一个形参保持一致，如果没第二个参数，说明被引用方法需要是无参的方法**

**2.第一个形参的类型决定了引用方法只能是这个类的方法**

```java
// 类名引用方法
// 需求：把字符串集合的每个字母转成大写
ArrayList<String> str4 = new ArrayList<>();
Collections.addAll(str4,"abc","cba","nba");
str4.stream().map(new Function<String, String>() {
    @Override
    public String apply(String s) {
        return s.toUpperCase();
    }
}).forEach(i -> System.out.print(i + " "));

// 类名引用成员方法 toUpperCase没有参数也可以用，因为符合独有规则1
str4.stream().map(String::toUpperCase).forEach(i -> System.out.print(i));


// 类名引用2
// 需求：把原集合中每个对象的姓名提取出来，放到一个数组内
ArrayList<Student> students = new ArrayList<>();
students.add(new Student("david", 25));
students.add(new Student("kashin", 35));
// 直接引用Stuedent类的方法getName，就能获取每个对象的名字
String[] strings = students.stream().map(Student::getName).toArray(new IntFunction<String[]>() {
    @Override
    public String[] apply(int value) {
        return new String[value];
    }
});
```



### **引用数组的构造方法**

**格式：数据类型[]::new**

**例子：int[]::new**

```java
// 引用数组构造方法
ArrayList<Integer> nums = new ArrayList<>();
Collections.addAll(nums, 3,4,6,4,7,81,100);
Integer[] integers = nums.stream().toArray(Integer[]::new); // 流转数组
```



## **异常**

**异常体系：**

**顶层：Java.lang.Throwable**

**子类：Exception（异常，程序相关的问题；Error（错误，硬件相关错误，无需关注）**

**异常分为：编译时异常、运行时异常（RuntimeException）**

**jvm虚拟机默认处理异常的方式：直接把异常类型打印到控制台,且会停止代码继续执行**

### **捕获异常**

**try/catch的核心目的就是不让程序停止**

 ```java
int[] arr = {1,2,3};
try {
    System.out.print(arr[5]); // 可能出现异常的代码
    System.out.print(2/0);
    // othercode... 当执行到上面出现异常后，就直接进入catch，other code不会被执行
}catch (ArrayIndexOutOfBoundsException e) { // 捕获索引越界异常
    System.out.print("索引越界")
}catch (ArithmeticException e) { // 若try中可能存在多个异常，就要写多个ctach进行捕获
    System.out.print("算术异常")
}catch (Exception e) { // 父类异常一定要写在最下面
    
}
System.out.print("继续执行")
 ```



### **异常的方法**

```java
public String getMessage() // 返回此throwable的详细消息字符串
public String toString() // 返回此可抛出的简短描述
public void printStackTrace() // 把异常的错误信息输出到控制台，包含了上面2个方法的信息（常用）

int[] arr = {1,2,3};
try {
    System.out.print(arr[5]);
}catch (ArrayIndexOutOfBoundsException e) {
    e.printStackTrace(); // 输出到控制台，不会停止虚拟机
}
System.out.print("继续执行") // 这里还会执行
```

### **抛出处理**

**throws**

**写在方法定义处，表示声明一个异常，告诉调用者使用本方法时可能会出现哪些异常(运行时异常可以省略，编译时异常必须写)**

**规则：public void 方法 () throws 异常类名1，异常类名2... {}**



**throw**

**写在方法内，结束方法，手动抛出异常对象，交给调用者，方法中下面的代码不再执行**

**规则：public void 方法（）{  throw new NullPointerException();  }**



**抛出后需要在调用处添加try/catch进行捕获**



### **自定义异常**

```java
// 1.必须继承一个异常父类
public class NameFormatException extends RuntimeException{
    // 2.必须定义2个构造方法
    public NameFormatException() {
    }

    public NameFormatException(String message) {
        super(message);
    }
}

// 使用自定义异常
package com.david.demo13;

import java.util.Scanner;

public class ExceptionTest {
    public static void main(String[] args) {
        try {
            printName();
        } catch (NameFormatException e) {
            e.printStackTrace(); // 输出异常信息
        }
    }
    static void printName() {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入姓名:");
        String name = sc.nextLine();
        if (name.length() > 5) {
            throw new NameFormatException("名字太长啦"); // 自定义异常，传入自定义错误信息
        }
        System.out.println("名字格式正确");
    }
}

```

### **finally**

```java
try{
    
}catch() {
    
}finally{
  // 不管上面是否遇到了异常，finally一定执行
  //类似io流释放资源的操作，可以放到finally中
  try{
      fos.close() // 因为close也有编译异常，也要放到try/catch中，嵌套一个try/catch
  } catch(IOException e) {
      e.printStaceTrace()
  }  
}
```







## **File**

**File对象表示一个路径，可以是文件路径，也可以是文件夹路径，还可以是不存在的路径**

```java
// 构造方法
public File(String pathname) // 根据文件路径创建文件对象
public File(String parent, String child) //根据父路径名字符串和子路径名字符串创建文件对象
public File(File parent, String child) //根据父路径对应文件对象和子路径名字符串创建文件对象
    
// 父路径: C:\\users  除了文件自己就是父路径
// 子路径: a.txt
String str = "C:\\users\\a.txt";
File f1 = new File(str); // 根据路径字符串创建File对象
f1.delete() // 删除文件或空文件夹
```

**![image-20230728161700713](D:\typora-img\image-20230728161700713.png)**

**![image-20230728163835103](D:\typora-img\image-20230728163835103.png)**



```java
// 创建单级文件夹bbb
File f1 = new File("D:\\aaa\\bbb"); // window中路径必须唯一 ,若bbb已存在则创建失败
f1.mkdir();  // 创建成功则返回true

// 创建多级文件夹,也能创建单级，直接用这个方法即可
File f2 = new File("D:\\aaa\\vvv\\zzz\xxx")
f2.mkdirs()
```



### **File遍历**

**核心方法：listFiles()  含重载过滤的方法**

```java
public File[] listFiles() // 获取当前该路径下的所有内容，获取路径下所有的文件和文件夹，放在一个File数组中返回

File f1 = new File("D:\\aaa");
File[] files = f1.listFiles(); // 空参返回所有， 重载方法 传过滤器方法还能进行过滤
for(File fs : files) {
    System.out.print(fs) // 遍历file数组
}

// 需求：删除一个多层级有内容的文件夹
// 递归文件夹
// 1.删除文件夹下所有内容
// 2.删除自己
psvm {
    File f1 = new File("D:\\aaa\\src");
    removeFile(f1);
}

// 定义删除方法，入参是要删除的文件夹对象
static void removeFile(File src) {
    File[] fs = src.listFiles(); // 遍历到空文件夹返回长度为0的数组
    if (fs != null) { // 没有访问权限的文件夹会返回null，防止返回null后报错
        for(File file : fs) {
            if (file.isFile()) {
                file.delete()
            } else {
                removeFile(file); // 如果是文件夹，执行递归继续判断
            }
        }
        src.delete() // 循环结束后，删除自己
    }
}
```



### **计算文件夹大小**

**核心思想：递归计算**

```java
import java.io.File;

public class FileTest {
    public static void main(String[] args) {
        File f1 = new File("C:\\Users\\Administrator\\Desktop\\前端笔记");
        System.out.println(getSize(f1));
    }
    
    // 递归计算文件夹内所有txt文件的大小，包括子文件夹
    static long getSize(File src) {
        long size = 0; // 这个size是局部变量，每次递归调用都是一个新值，所以递归时要累加
        File[] fs = src.listFiles();
        if (fs != null) {
            for (File f : fs) {
                if (f.isFile() && f.getName().endsWith("txt")) {
                    size += f.length();
                } else {
                    size += getSize(f); // 重点：递归的返回值要累加，否则只能计算一层文件夹的大小
                }
            }
        }
        return size;
    }
}
```



### **统计各类文件数量（难点）**

```java
// 需求：统计一个文件夹中每种文件的个数，如txt:3个 md:2个
// 思想：用map集合进行统计

public static void main(String[] args) {
    File f1 = new File("C:\\Users\\Administrator\\Desktop\\前端笔记");
    System.out.println(getCount(f1)); // {txt=36, md=21}
}
static HashMap getCount(File src) {
    HashMap<String, Integer> hm = new HashMap<>(); //键值对 文件类型:文件个数
    File[] fs = src.listFiles();
    if (fs != null) {
        for (File f : fs) {
            // 是文件，进行统计
            if (f.isFile() && (f.getName().endsWith("txt") || f.getName().endsWith("md"))) {
                String[] nameArr = f.getName().split("\\.");
                String name = nameArr[nameArr.length - 1];
                System.out.print(name + " ");
                // 判断集合中是否已有该类型
                if (hm.containsKey(name)) { // 已存在，count+1
                    int count = hm.get(name);
                    count += 1;
                    hm.put(name, count); // count+1后更新数据
                } else {
                    hm.put(name, 1); // 没有，添加数据
                }
            } else { // 不是文件，递归操作
                HashMap<String, Integer> sonHm = getCount(f); //重点：递归要对返回值进行保存
                // 把sonHm数据合并到hm中
                Set<Map.Entry<String, Integer>> entries = sonHm.entrySet();
                for (Map.Entry<String, Integer> entry : entries) {
                    String key = entry.getKey();
                    int value = entry.getValue();
                    if (hm.containsKey(key)) {
                        // 获取Hm的count数
                        int count = hm.get(key);
                        count += value; // 合并递归返回的count数
                        hm.put(key, count);
                    } else {
                        hm.put(key, value);
                    }
                }
            }
        }
    }
    return hm;
}
```





## **IO流**

**![image-20230731143019501](D:\typora-img\image-20230731143019501.png)**

**作用：读、写文件数据的解决方案**

**按操作文件的类型分类：**

**1、字节流：操作所有类型的文件**

**2、字符流：只能操作纯文本文件**

**按流向分类：**

**1、输出流：程序-->文件   程序中的数据写出到文件**

**2、输入流：文件-->程序  文件中的数据读取到程序中**

**IO流 的所有创建输出流，都会清空原数据，保持随用随创建的原则进行创建对象**



### **字节流**

```java
public class IOtest {
    public static void main(String[] args) throws IOException {
        // 输出流
        // 创建输出对象 每次创建默认会清空文件，续写开关默认为关
        FileOutputStream fos = new FileOutputStream("C:\\Users\\Administrator\\Desktop\\IOTEST\\david.txt");
        // 写入数据 转成对应asc码的字符
        fos.write(100);
        // 关闭连接
        fos.close();

        // 使用f对象创建输出流对象, 文件不存在时会直接创建新的，前提是父路径存在
        File f = new File("C:\\Users\\Administrator\\IdeaProjects\\practise01\\module01\\KASHIN.txt");
        FileOutputStream fos2 = new FileOutputStream(f);
        fos2.write(101);
        fos2.write(102);
        fos.close();

        // 用字节数组写出数据 第二参数是续写开关，表示下次写入数据不会清空文件
        FileOutputStream fos3 = new FileOutputStream("C:\\Users\\Administrator\\Desktop\\IOTEST\\david.txt", true);
        String s = "hello david";
        byte[] bytes = s.getBytes(); // getBytes方法转成字节数组

        // fos3.write(bytes, 1, 3); // 写入部分数据 从索引1开始，写入3个数据
        // 实现换行
//        String wrap = "\r\n";
//        byte[] wrapBytes = wrap.getBytes();
//        fos3.write(wrapBytes);
        fos3.write(bytes); // 写入全部字节数组
        fos3.close();

        // 输入流
        FileInputStream fis = new FileInputStream("C:\\Users\\Administrator\\Desktop\\IOTEST\\david.txt");
        // 读取数据 read方法每次读取一个字节，读取的是数据在asc码上对应的数字，读到末尾时返回-1
//        int read = fis.read();
//        System.out.println((char) read);
//        // 释放资源
//        fis.close();

        // 循环读取
        int read;
        // read()在判断条件中调用，并赋值给变量，防止在循环体再次调用，读取就会有有问题
        while((read = fis.read()) != -1) {
            System.out.print((char) read);
        }
        fis.close();
    }
}

```



#### **文件拷贝**

**单次循环读一个字节**

```java
public class copyTest {
    public static void main(String[] args) throws IOException {
        // 文件拷贝
        // 创建输入流和输出流
        FileInputStream fis = new FileInputStream("C:\\Users\\Administrator\\Desktop\\IOTEST\\david.txt");
        FileOutputStream fos = new FileOutputStream("C:\\Users\\Administrator\\Desktop\\IOTEST\\copy.txt");
        // 读取要拷贝的文件
        int read;
        // 循环读取
        while ((read = fis.read()) != -1) {
            fos.write(read);
        }
        // 释放资源，先打开的后关闭
        fos.close();
        fis.close();
    }
}
```

#### **大文件拷贝**

**一次循环读N个字节（自定义）**

```java
public int read()  //空参：一次读一个字节，返回值是读取的字节对应asc码
public int read(byte[] Buffer) //传参： 一次读一个字节数组，返回值是本次读取到了多少个字节

FileInputStream fis2 = new FileInputStream("C:\\Users\\Administrator\\Desktop\\IOTEST\\david.txt");
FileOutputStream fos2 = new FileOutputStream("C:\\Users\\Administrator\\Desktop\\IOTEST\\copy2.txt");

// 创建长度为2的字节数组，一次循环读2个字节
byte[] bytes = new byte[2];
int len; // read()方法在传入字节数组后，返回的是读取到的字节个数，要注意
while ((len = fis2.read(bytes)) != -1) {
    fos2.write(bytes, 0, len); // 写入字符数组,开始的索引值,读取到的个数(不能只写一个bytes，数组中可能存在残留数据)
}
fos2.close();
fis2.close();
```



#### **AutoCloseable**

```java
//jdk7之后实现了AutoCloseable的类，可以在try,catch后自动释放资源
// 流对象定义在外面
FileInputStream fis = new FileInputStream("xxx/a.txt");
FileOutputStream fos = new FileOutputStream("xxx/a.txt");
try(fis; fos) {
    // 拷贝文件的逻辑代码...
}catch(IOException e) {
    e.printStackTrace;
}
// 自动释放资源，不用写finally
```



#### **文件加密**

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class fileCopy {
    public static void main(String[] args) throws IOException {
        // 文件加密
        // 核心思路：制定一个加密规则，对每个字节进行加密操作（解密反向操作即可）
        FileInputStream fis = new FileInputStream("C:\\Users\\yoki\\Desktop\\原始文件.txt"); // 源文件
        FileOutputStream fos = new FileOutputStream("C:\\Users\\yoki\\Desktop\\encpt.txt"); // 加密后文件

        int data;
        while ((data = fis.read()) != -1) {
            int encryData = data ^ 10; // 使用异或操作对每个字节加密
            fos.write(encryData);
        }
        fos.close();
        fis.close();

        // 文件解密
        FileInputStream fis2 = new FileInputStream("C:\\Users\\yoki\\Desktop\\encpt.txt");
        FileOutputStream fos2 = new FileOutputStream("C:\\Users\\yoki\\Desktop\\openEncpt.txt");
        int data2;
        while ((data2 = fis2.read()) != -1) {
            int decodingData = data2 ^ 10; // 再次异或操作 会变回原数据
            fos2.write(decodingData);
        }
        fos2.close();
        fis2.close();
    }
}
```

#### **修改内容**

```java
package com.david.demo14;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Arrays;
import java.util.function.Function;
import java.util.function.IntFunction;

public class fileMod {
    public static void main(String[] args) throws IOException {
        // 需求：修改文件内容，把文件内数字按升序排列
        FileInputStream fis = new FileInputStream("C:\\Users\\yoki\\Desktop\\javaTest\\a.txt");
        StringBuilder sb = new StringBuilder(); // 创建sb对象接收字符串
        int data;
        while ((data = fis.read()) != -1) {
            sb.append((char) data);
        }
        fis.close();
        // 字符串转数组
        String[] stringArr = sb.toString().split("-");
        // 用stream流处理 先转为数字，再排序，最后返回Int数组
        Integer[] intArr = Arrays.stream(stringArr).map(new Function<String, Integer>() {
            @Override
            public Integer apply(String s) {
                return Integer.parseInt(s);
            }
        }).sorted().toArray(new IntFunction<Integer[]>() {
            @Override
            public Integer[] apply(int value) {
                return new Integer[value];
            }
        });
        System.out.println(Arrays.toString(intArr));
        // 改写文件
        FileWriter fr = new FileWriter("C:\\Users\\yoki\\Desktop\\javaTest\\a.txt");
        String s = Arrays.toString(intArr).replaceAll(", ", "-");
        String res = s.substring(1, s.length() - 1); // 截掉[]括号
        fr.write(res);

        fr.close();
    }
}
```





### **字符流**

**字符流 = 字节流 + 字符集**

**使用场景：用来操作含有中文的纯文本文件**

**输入流：一次读一个字节，遇到中文时，一次读多个字节(UTF-8读3个字节，GBK读2个字节)**

**输出流：底层按照指定编码方式进行编码，变成字节再写到文件中**

#### **read/write底层原理**

**read会把文件先放到一个8192长度的数组缓冲区，获取文件直接时会先从缓冲区获取**

**若超过这个长度，则下次读取时会把超出的数据放到缓冲区头部**



**write同理，先创造一个8192缓冲区，写到缓冲区，缓冲区装满后自动刷新到文件中，也可以调用flush手动操作**

**flush 方法：手动将缓冲区文件刷新到文件中，调用后还可以继续写入数据**

**close方法：释放资源后自动刷新到文件中，调研后不能继续写入数据**



#### **FileReader**

```java
//构造方法
public FileReader(File f) // 创建字符输入流关联本地文件
public FileReader(String pathName)
    
//读取方法
public int read() // 读取数据，读到末尾返回-1
public int read(char[] buffer) // 读取多个数据， 读到末尾返回-1，返回值表示一次读了多少数据
//释放资源
public int close()
    
    
import java.io.FileReader;
import java.io.IOException;

public class FilereaderTest {
    public static void main(String[] args) throws IOException {
        // 注意记事本也要用UTF-8编码
        FileReader fr = new FileReader("C:\\Users\\yoki\\Desktop\\UTF-8测试.txt");
        // 读取文件
        int data;
        // 空参read每次读一个字节，碰到中文时读3个字节(UTF-8)
        while ((data = fr.read()) != -1) {
            // 把读到的2进制字节，解码转成10进制，10进制就是UniCode字符集对应的字符
            System.out.print((char) data);
        }
        fr.close();

        // 一次读N个字符，有参read
        FileReader fr2 = new FileReader("C:\\Users\\yoki\\Desktop\\UTF-8测试.txt");
        // 创建一个字符数组, 一次读4个数据
        char[] chars = new char[4];
        int len;
        // 有参read返回的是读取到的个数，内部会把读到的数据自动放到字符数组中
        while ((len = fr2.read(chars)) != -1) {
            String s = new String(chars, 0, len); // 获取每次读到的数据
            System.out.print(s);
        }
        fr2.close();
    }
}
```





#### **FileWriter**

```java
// 构造方法
public FileWriter(File f) // 创建输出流关联本地文件
public FileWriter(String pathname)
public FileWriter(File f, boolean append) // 续写开关
public FileWriter(String pathname, boolean append)
// 写入方法
void write(int c) // 写出1个字符
void write(String str) // 写出一个字符串
void write(String str, int off, int len) // 写出字符串一部分
void write(char[] cbuf) // 写出字符数组
void write(char[] cbuf, int off, int len) // 写出字符数组一部分
    
FileWriter fw = new FileWriter("C:\\Users\\yoki\\Desktop\\UTF-8测试.txt", true);
fw.write("你好啊大卫王！");
fw.close();
```



### **高级流**

#### **字节缓冲流**

**性能比字节基本流更高，自带一个缓冲区**

```java
// 创建字节缓冲流 入参是一个字节流对象
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("C:\\Users\\Administrator\\Desktop\\IOTEST\\david.txt"));
BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("C:\\Users\\Administrator\\Desktop\\IOTEST\\copy3.txt"));
// 1.读写1个字节
int b;
while ((b = bis.read()) != -1) {
    bos.write(b);
}
// 2.读写字节数组
byte[] bytes = new byte[1024];
int len;
while ((len = bis.read(bytes)) != -1) {
    bos.write(bytes, 0, len);
}
// 释放资源 只需关闭外层缓冲流，入参的字节流会在内部关闭
bos.close();
bis.close();
```

#### **字符缓冲流**

**字符基本流和字符缓冲流底层都有缓冲区，字符缓冲流多2个方法**

```java
// 字符缓冲流特有的2个方法
public String readLine() // 读取一行数据，如果没有数据返回null
public void newLine() // 跨平台换行
    
BufferedReader br = new BufferedReader(new FileReader("c.txt"));
String line = br.readLine(); // 读一行，不会读换行符
br.close()

BufferedWriter bw = new BufferedWriter(new FileWriter("a.txt"));
bw.write("hello")；
bw.newLine(); // 通用换行方法， 每个操作系统的换行符不同
bw.close();

// 需求：记录软件执行次数，3次内免费试用，超过3次后提示不可用
public class buffer {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader("C:\\Users\\yoki\\Desktop\\count.txt"));
        // 把读到的数据转成Int,一次读一行
        int count = Integer.parseInt(br.readLine());
        br.close();
        count += 1;
        if (count <= 3) {
            System.out.println("当前使用第"+count+"次");
        } else {
            System.out.println("已使用超过3次, 请付费！");
        }
        // 把count再写入文件保存, 用的时候创建，创建writer会清空文件内原数据
        BufferedWriter bw = new BufferedWriter(new FileWriter("C:\\Users\\yoki\\Desktop\\count.txt"));
        bw.write(new String(String.valueOf(count))); // 注意：写入的int值会转成asc码表中对应的值，需要转成string存进去
        bw.close();
    }
}
```



#### **转换流**

**作用：**

**1、指定字符集读写数据（JDK11之后淘汰）**

**2、字节流想要使用字符流的方法**

**InputStreamReader: 把字节流转换成字符流进行读取**

**OutputStreamWriter: 把字符流转换成字节流进行存储**

```java
// jdk11之前读取GBK文件 已淘汰
InputStreamReader isr = new InputStreamReader(new FileInputStream("xxx.txt"), "GBK"); 
int ch;
while((ch = isr.read()) != -1) {
    System.out.print((char) ch);
}
isr.close()
// jdk11开始（掌握）
FileReader fr = new FileReader("xxx.txt", Charset.forName("GBK"));// 指定字符编码
int ch;
while((ch = fr.read()) != -1) {
    System.out.print((char) ch);
}
fr.close()
    
// 写出数据用FileWriter 指定GBK编码
OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("xxx.txt"), "GBK")
osw.write("你好啊");
osw.close();
// JDK11写法
FileWriter fw = new FileWriter("xx.txt", Charset.forName("GBK"))
fw.write("你好");
fw.close()

    
// 需求：把GBK转成UTF-8保存到新文件
//思路：先用FileReader指定字符编码读取，再把读到的数据用FileWriter指定UTF-8保存
    

// 需求2：用字节流读取文件中的数据，每次读一行，不能出现乱码
// 思路：字节流读到中文是会出现乱码的，但字符流不会
FileInputStream fis = new FileInputStream("xxx.txt");
InputStreamReader isr = new InputStreamReader(fis);
// 用bufferedReader一次读一行
BufferedReader br = new BufferedReader(isr);
String str;
while((str = br.readLine()) != null) {
    // sout str
}
br.close()
```



#### **序列化/反序列化流**

**序列化：把java中的对象写入本地文件**

**反序列化：读取本地文件中的java对象**

```java
// 序列化流
// 构造方法
public ObjectOutputStream(OutputStream out) // 把基本流包装成高级流
// 成员方法
public final void writeObject(Object obj) // 把对象序列化到文件中
    
// 创建一个要序列化的javabean类, 注意必须实现Serializable接口，这是一个标记性接口，表示该类可以被序列化
public class Student implements Serializable {
    // 生成版本号，每个序列化类都会生成一个版本号，当修改类时，这个版本号会更新，在反序列化会报版本号异常，需要手动指定一个版本号
    private static final long serialVersionUID = 1L;
    private transient String address; // transient关键词表示不会被序列化到本地文件中，反序列化读到的是初始值
    // 类的代码。。
}

// 序列化操作
// 1.创建对象
Student stu = new Studeng("张三", 35);
// 2.创建序列化的对象/对象操作输出流
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("a.txt")); // 关联一个基本流
// 3.写出数据
oos.writeObject(stu);
// 4.释放资源
oos.close();


// 反序列化流
public ObjectInputStream(InputStraam in) // 把基本流变成高级流
// 成员方法
public Object readObject() // 把文件中的对象读到程序中
    
// 反序列化操作
ObjectInputStream ois = new ObjectInputStream(new FileInputStream("a.txt"))；
Object o = ois.readObject(); // 读取数据 一次读取一个对象
System.out.print(o);
ois.close();
```

##### **序列化多个对象**

```java
// 序列化
public class serializable {
    public static void main(String[] args) throws IOException {
        Student stu1 = new Student("david", 36);
        Student stu2 = new Student("kashin", 27);
        // 序列化添加多个对象时，建议放到一个集合中，方便反序列化读取
        ArrayList<Student> list = new ArrayList<>();
        list.add(stu1);
        list.add(stu2);
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("C:\\Users\\yoki\\Desktop\\javaTest\\aa.txt"));
        oos.writeObject(list);
        oos.close();
    }
}
public class antiSerializable {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        // 反序列化读取一次，读一个集合即可
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("C:\\Users\\yoki\\Desktop\\javaTest\\aa.txt"));
        ArrayList<Student> list = (ArrayList<Student>) ois.readObject();
        for (Student stu : list) {
            System.out.println(stu);
        }
    }
}
```



#### **打印流**

**打印流指PrintStream,PrintWriter 两个类**

**打印流只能写出，不能读取**

**System.out.print 其实获取的就是打印流， 默认指向控制台**



**字节打印流**

**字节打印流底层没有缓冲区，开不开自动刷新都会直接写到文件中**

```java
//构造方法
public PrintStream(OutputStream/file/String) //关联字节输出流
public PrintStream(String filename, Charset charset) // 指定字符编码
public PrintStream(OutputStream out, boolean autoFlush) // 自动刷新
public PrintStream(OutputStream out, boolean autoFlush, String encoding) // 指定编码且自动刷新
// 成员方法
public void write(int b) // 常规方法，和其他输出流一样
public void println(Xxx xx) // 打印任意数据，自动刷新和换行
public void print(Xxx xx) // 打印任意数据，不换行
public void printf(String format, Object...args) // 带有占位符的打印语句，不换行
    
PrintStream ps = new PrintStream(new FileOutputStream("aa.txt"), true);
ps.println(97); // 数据原样打印， 文件中保存的就是97，不是对应的asc码字符
ps.println(true); // 保存的就是true
ps.printf("%s 哈哈哈 %s", "david", "kashin") // %s就是后面2个参数的占位符 输出：david 哈哈哈 kashin
ps.close();
```

**字符打印流**

**底层有缓冲区，默认不自动刷新，想要自动刷新出缓冲区需要传true**

```java
// 构造方法
public PrintWriter(Write/File/String) // 管理输出流/文件/文件路径
public PrintWriter(String filename,Charset charset) // 指定字符编码
public PrintWriter(OutputStream out, boolean autoFlush) // 自动刷新
public PrintWriter(OutputStream out, boolean autoFlush, String encoding) // 指定编码且自动刷新
// 成员方法和字节打印流一样
```



#### **压缩/解压缩流**

**解压缩流：ZipInputStream**

**压缩流：ZipoutputStream**

**java中解压缩必须是zip格式，解压的流程就是把压缩包中的文件或文件夹读取出来，按照层级拷贝到指定目录下**

**ZipEntry：压缩包中的每个文件或文件夹都是一个ZipEntry对象**

**解压缩**

```java
public class ziptest {
    public static void main(String[] args) throws IOException {
        // 要解压的文件
        File f1 = new File("C:\\Users\\yoki\\Desktop\\nhdogjmejiglipccpnnnanhbledajbpd.zip");
        // 解压到的文件夹位置
        File f2 = new File("C:\\Users\\yoki\\Desktop\\javaTest");
        unZip(f1, f2);
    }
    // 解压方法，传要解压的文件和解压到哪里
    static void unZip(File src, File to) throws IOException {
        ZipInputStream zip = new ZipInputStream(new FileInputStream(src));
        // 核心：利用zipEntry对象进行文件夹和文件遍历，拷贝
        ZipEntry zipEntry; // zipEntry是压缩文件中的每一个文件夹或文件
        while((zipEntry = zip.getNextEntry()) != null) {
            System.out.println(zipEntry);
            // 判断是文件还是文件夹
            // 文件夹
            if (zipEntry.isDirectory()) {
                File file = new File(to, zipEntry.toString());// 用父路径+entry自己的name建立一个file对象
                file.mkdirs(); // 创建文件夹
            } else {
                // 文件
                FileOutputStream fos = new FileOutputStream(new File(to, zipEntry.toString())); // 创建输出流
                int b;
                while ((b = zip.read()) != -1) {
                    fos.write(b);
                }
                fos.close();
                zip.closeEntry(); // 表示压缩包中的一个文件处理完毕
            }
        }
        zip.close();
    }
}
```

**压缩单个文件**

```java
public class singeZip {
    public static void main(String[] args) throws IOException {
        // 要压缩的文件
        File src = new File("C:\\Users\\yoki\\Desktop\\亚信账号.txt");
        // 压缩到哪里
        File to = new File("C:\\Users\\yoki\\Desktop\\javaTest");
        toZip(src, to);
    }
    static void toZip(File src, File to) throws IOException {
        // 创建压缩流关联压缩包
        ZipOutputStream zos = new ZipOutputStream(new FileOutputStream(new File(to, "亚信账号.zip")));
        // 创建zipEntry对象
        ZipEntry entry = new ZipEntry("亚信账号aaa.txt"); // 表示压缩包里的每个文件或文件夹
        // 把entry放到压缩包中
        zos.putNextEntry(entry);
        // 把src文件的数据写到压缩包中
        FileInputStream fis = new FileInputStream(src);
        int b;
        while ((b = fis.read()) != -1) {
            zos.write(b);
        }
        fis.close();
        zos.closeEntry();
        zos.close();
    }
}
```

**压缩文件夹（难点）**

```java
public class multiZip {
    public static void main(String[] args) throws IOException {
        // 需求：把test文件夹压缩成一个压缩包
        // 创建File对象表示要压缩的文件夹
        File src = new File("C:\\Users\\yoki\\Desktop\\javaTest\\test");
        // 创建file表示压缩包放在哪(压缩包父级路径)
        File destParent = src.getParentFile();
        // 创建file表示压缩包的路径
        File dest = new File(destParent, src.getName() + ".zip"); // 根据父路径和原文件名动态创建file
        // 创建压缩流关联压缩包
        ZipOutputStream zos = new ZipOutputStream(new FileOutputStream(dest));
        // 获取Src里的每个文件，变成zipEntry对象，放入压缩包
        toZip(src, zos, src.getName());
        // 释放资源
        zos.close();
    }
    // 压缩方法，关键参数第三个name表示压缩包里面的路径
    static void toZip(File src, ZipOutputStream zos, String name) throws IOException {
        // 遍历src文件夹
        File[] files = src.listFiles();
        for (File file : files) {
            // 是文件
            if (file.isFile()) {
                ZipEntry entry = new ZipEntry(name + "\\" + file.getName()); // 难点：拼接文件路径
                zos.putNextEntry(entry);
                FileInputStream fis = new FileInputStream(file);
                int b;
                while((b = fis.read()) != -1) {
                    zos.write(b);
                }
                fis.close();
                zos.closeEntry();
            } else { // 文件夹, 递归 注意name路径要传上层路径
                toZip(file, zos, name + "\\" + file.getName());
            }
        }
    }
}
```



### commons-io

io操作的工具包

是一个jar包，需要放到lib文件夹，右键add as libaray导入后即可使用

```java
// 两个大类
// FileUtils IOUtils
FileUtils.copyFile(src, dest) // 拷贝文件
FileUtils.copyDirectory(src, dest) // 拷贝文件夹
```

### Hutool

第三方工具包，包含时间类，数字类，IO类等等各种工具







## **乱码概念**

### **字符集**

**常用字符集：**

**asc：记录128个字符（0~127），所有英文字符只需一个字节，二进制的第一位是0**

**GBK：完全兼容asc，包含英文、简体、繁体、中日韩汉字（window默认使用GBK，公共显示为ANSI），汉字使用2个字节存储（上限65535个汉字）**

**Unicode：国际标准字符集(也完全兼容asc)，对世界各种语言每个字符定义一个唯一编码**



**ASC规则：**

**存储规则：**

**字母a -->查询asc得到97-->转成二进制是7位数字：1100001--> 用asc编码规则：前面补0，补齐8位：01100001->存储到硬盘**

**读取规则:**

**01100001解码-->前面补0对进制转换无影响，直接转成10进制得到97-->查码表对应得到字母a**



**GBK规则**

**汉字使用2个字节存储，二进制高位进制的第一位一定是1，转为二进制后为负数，10111010 10111010 --> -70 -70 和asc区分**

**存储规则：**

**汉字“汉”-->查询GBK得到47802-->转成二进制为：10111010 10111010 ---> 编码 10111010 10111010 （汉字编码不需要变动，英文编码和acs规则一样）**

**解码：**

**10111010 10111010 --> 解码得到47802 --->查询GBK得到汉字“汉”**



**Unicode规则**

**编码：**

**字母a--> 查询unicode得到数字97 --> 二进制1100001 -->UTF-8编码：前面补0得到01100001** 

**汉字“汉” --> 查询unicode得到数字27721 -->二进制01101100 01001001 -->UTF-8编码：11100110 10110001 10001001**

**解码：**

**11100110 10110001 10001001 --> UTF-8解码得到 01101100 01001001 --> 转10进制得27721--> 查unicode得到汉字“汉”**



**Unicode编码规则：**

**UTF-8: 用1~4个字节存储（可变长度编码，常用）**

**属于acs表的用1个字节，中文用3个字节， 其他国家用2或4个字节**

**1个字节时第一位补0，兼容asc**

**3个字节时规定格式： 1110xxxx 10xxxxxx 10xxxxxx**

**UTF-16: 用2~4个字节存储** 

**UTF-32: 固定用4个字节存储**	



### **乱码产生的原因**

**1.读取数据时未读完整个汉字（由3个字节组成的汉字只读了1个字节）**

​	**使用字节流一次读一个字节时就会出现这种乱码**

**2.编码和解码的规则不统一（主要原因）**

**例子：**

**编码汉字“汉”使用utf-8得到 ：11100110 10110001 10001001**

**解码使用GBK：把11100110 10110001当成一个汉字， 10001001当成单独的，解码得到59057, -119 -->查询GBK后得到汉字“姹”和一个未匹配的?**



### **java中编码/解码方式**

**String类中的方法**

```java
//编码
public byte[] getBytes() // 使用默认方式编码 IDEA中默认是UTF-8方式编码
public byte[] getBytes(String charsetName) // 使用指定方式编码
//解码 就是String的构造方法
String(byte[] bytes) // 默认方式解码
String(byte[] bytes, String charsetName) //使用指定方式解码
```



### char为什么可以存汉字

问题：一个char类型占2个字节，但一个utf-8编码汉字占3个字节，为什么char可以存？

char字符存储的是Unicode编码的代码点，也就是存储的是U+FF00这样的数值，然而我们在调试或者输出到输出流的时候，是JVM或者开发工具按照代码点对应的编码字符输出的。

所以虽然UTF-8编码的中文字符是占用3个或者4个字节，但是对应的代码点仍然集中在[0x4E00, 0x9FBB]，所以char是能够存下在这个范围内的中文字符的。



## 正则

**Pattern 类：** 

pattern 对象是一个正则表达式的编译表示。Pattern 类没有公共构造方法。要创建一个 Pattern 对象，你必须首先调用其公共静态编译方法，它返回一个 Pattern 对象。该方法接受一个正则表达式作为它的第一个参数。

```java
Pattern p = Pattern.compile(reg);
```

**Matcher 类：**

 Matcher 对象是对输入字符串进行解释和匹配操作的引擎。与Pattern 类一样，Matcher 也没有公共构造方法。你需要调用 Pattern 对象的 matcher 方法来获得一个 Matcher 对象。

matcher.find方法：是否存在该匹配模式的下一个子序列，存在返回true,再次调用时匹配下一个

```java
Matcher matcher = p.matcher(str);
```





## 综练

### 爬虫+生成随机数据

```java
package com.david.demo18practise;

import java.io.IOException;
import java.io.InputStreamReader;
import java.net.MalformedURLException;
import java.net.URL;
import java.net.URLConnection;
import java.util.ArrayList;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class crew {
    public static void main(String[] args) throws IOException {
        String nameNet = "http://www.haoming8.cn/baobao/10881.html";
        // 1.爬取数据
        String str = webCrawler(nameNet);
        /*
        * 2.筛选要用的数据
        * @params 1-原始数据 2-正则规则 3-要获取哪个子匹配，传1获取第一个括号中的表达式
        * */
        ArrayList<String> arrayList = getData(str, "([\\u4e00-\\u9FA5]{2})(、|。)", 1);
        // 3.名字去重
        HashSet<String> hs = new HashSet<>();
        for (String s : arrayList) {
            hs.add(s);
        }
        // 4.生成最终随机数据：姓名-年龄
        HashSet<String> infoList = getInfos(hs, 30);// 参数：原数据，生成数据个数
        // 5.把最终数据写入本地文件
        BufferedWriter bw = new BufferedWriter(new FileWriter("C:\\Users\\yoki\\Desktop\\javaTest\\namesData.txt"));
        for (String infoStr : infoList) {
            bw.write(infoStr);
            bw.newLine();
        }
        bw.close();
    }

    private static ArrayList<String> getData(String str, String reg, int index) {
        // 用集合存放数据
        ArrayList<String> list = new ArrayList<>();
        // 创建正则表达式对象，Pattern类没有构造方法，调用compile方法返回一个pattern对象
        Pattern p = Pattern.compile(reg);
        // Matcher类也没有构造方法，用Pattern.matcher()方法创建Matcher类实例
        Matcher matcher = p.matcher(str);
        while(matcher.find()) { // find方法：是否存在该匹配模式的下一个子序列，存在返回true,再次调用时匹配下一个
            String group = matcher.group(index); // group：返回满足条件的字符串，传参可以返回指定的子匹配，即()中的子表达式
            list.add(group);
        }
        return list;
    }

    public static String webCrawler(String net) throws IOException {
        // 定义一个Sb接收爬到的数据, 必须是字符串类型，方便正则处理
        StringBuilder sb = new StringBuilder();
        // 创建URL对象
        URL url = new URL(net);
        // 链接上这个网址
        URLConnection conn = url.openConnection();
        // 读取数据, conn.getInputStream()返回一个字节流，因为网站有中文，所以要用转换流读取中文
        InputStreamReader isr = new InputStreamReader(conn.getInputStream());
        int b;
        while ((b = isr.read()) != -1) {
            sb.append((char) b); // 转成字符保存到sb中
        }
        isr.close();
        return sb.toString();
    }
    // 生成最终随机数据
    private static HashSet<String> getInfos(HashSet<String> hs, int count) {
        ArrayList<String> list = new ArrayList<>(); // set转list
        for (String h : hs) {
            list.add(h);
        }
        HashSet<String> res = new HashSet<>();// 用set保存，去重
        Random r = new Random();
        // 生成指定个数的循环
        while (res.size() <= count) {
            // 先打乱，再获取第0个索引，实现随机
            Collections.shuffle(list);
            // 年龄范围18~27
            int age = r.nextInt(10) + 18;
            res.add(list.get(0) + "-" + age);
        }
        return res;
    }
}
```



### 带权重随机算法

概念：

假如随机10个人，每个人的初始权重为1，总权重就是所有人的权重之和， 那么每个人的权重占比就是 1/10=0.1

权重占比范围：把每个人的权重占比分成10等分, 比如随机到0.345 就是第4位，随机到0.56就是第6位

1=》(0.0~0.1]

2=》(0.1~0.2]

3=>(0.2~0.3]

...

10=》(0.9~1.0]

```java
package com.david.demo18practise;

import java.io.*;
import java.util.ArrayList;
import java.util.Arrays;

public class weightRandom {
    public static void main(String[] args) throws IOException {
        // 带权重随机
        // 缓冲流 读取文件数据
        BufferedReader br = new BufferedReader(new FileReader("C:\\Users\\yoki\\Desktop\\javaTest\\weight.txt"));
        ArrayList<Student> list = new ArrayList<>();
        String str;
        while ((str = br.readLine()) != null) {
            String[] strings = str.split("-");
            // 1.集合保存student数据 {姓名，年龄，权重}
            Student stu = new Student(strings[0], Integer.parseInt(strings[1]), Double.parseDouble(strings[2]));
            list.add(stu);
        }
        br.close();
        // 2.计算总权重
        double weight = 0;
        for (Student student : list) {
            weight += student.getWeight();
        }
        // 3.计算每个人权重占比
        double[] arr = new double[list.size()];
        int index = 0;
        for (Student student : list) {
            arr[index] = student.getWeight() / weight;
            index++;
        }
        // 4.计算每个人的权重占比范围 权重范围就是前两个权重占比相加
        for (int i = 1; i < arr.length; i++) {
            arr[i] = arr[i] + arr[i - 1];
        }
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
        // 5.随机抽取
        // 获取0.0~1.0随机数
        double num = Math.random();
        // 二分法查找随机元素对应的数据，也能查找不在数组中，但在该数组范围内的元素，返回的是-插入点的索引, 插入点就是第一个大于该元素的索引
        System.out.println(num);
        int idx = -Arrays.binarySearch(arr, num) - 1; // 从1开始计数，返回的是-插入点， 取反-1就是要获取的那个元素
        System.out.println(idx);
        // 被选中的元素，权重减半
        double newWeight = list.get(idx).getWeight() / 2;
        list.get(idx).setWeight(newWeight);
        System.out.println(list.get(idx));
        System.out.println(list);
        // 把更新后的权重重新写入本地文件，下次即会根据最新权重文件进行随机
        BufferedWriter bw = new BufferedWriter(new FileWriter("C:\\Users\\yoki\\Desktop\\javaTest\\weight.txt"));
        for (Student student : list) {
            bw.write(student.toString());
            bw.newLine();
        }
        bw.close();
    }


    public static class Student {
        private String name;
        private int age;
        private double weight;

        @Override
        public String toString() {
            return name + "-" + age + "-" + weight;
        }
        public Student() {
        }

        public Student(String name, int age, double weight) {
            this.name = name;
            this.age = age;
            this.weight = weight;
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

        public double getWeight() {
            return weight;
        }

        public void setWeight(double weight) {
            this.weight = weight;
        }
    }
}

```



### 配置文件

1、可以把软件的设置永久化存储

2、若要修改参数，不需要改动代码，直接修改配置文件即可

常见的配置文件：XML / ini  / properties / YAML

**properties配置文件**

properties: 后缀名.properties   用键值对方式存储的配置文件

MAP类有一个properties子类，有一些特有方法，可以把集合中的数据按照键值对形式写到配置文件中，也可以把配置文件的数据读到集合中。

```java
// properties类没有泛型，接收任意类型，就是Object类型
Properties prop = new Properties();

// 添加数据 注意map集合key是唯一的，值可以重复 
prop.put("aaa", "bbb");
prop.put("aaa", "ccc"); // key唯一，不会被添加

// 特殊方法store, 把集合的数据以键值对形式保存到.properties文件中
FileOutputStream fos = new FileOutputStream("xxx\\xxx.properties");
prop.store(fos, "test"); // 第二个参数是注释
fos.close();

// 特殊方法load, 读取.properties中的数据，接收字符或字节输入流
FileInputStream fis = new FileInputStream("xxx\\xxx.properties");
prop.load(fis);
fis.close();
System.out.println(prop) // 返回键值对形式的map集合
String val = (String) prop.get("aaa") // 获取键对应的值，propertie默认是obj类型, 强转为String

```



## 多线程

进程：程序运行的基本实体

线程：操作系统能够进行运算调度的最小单位。**被包含在进程中，是进程的实际运作单位**

多线程简单理解：一个应用软件中互相独立，又可以同时运行的功能。

多线程应用场景：想让多个事情同时运行就要用到多线程

**并发：**同一个时刻，多个指令在单个CPU上交替执行。

**并行：**同一时刻，多个指令在多个CPU上同时执行。



### 多线程实现方式

```java
// 方式1
public class threadcase01 {
    public static void main(String[] args) {
        // 创建多线程
        /*
            1.创建一个继承Thread的类，并重写run()方法
            2.创建实例
            3.调用start()开启线程
        */
        MyThread t1 = new MyThread();
        t1.setName("线程1");
        MyThread t2 = new MyThread();
        t2.setName("线程2");
        t1.start(); // 开启线程， 执行结果是两个线程会交替执行
        t2.start();
    }
}
public class MyThread extends Thread{
    @Override
    public void run() {
        // 线程要执行的代码
        for (int i = 0; i < 100; i++) {
            System.out.println(getName() +" "+ i);
        }
    }
}
//=========================================
// 方式2
 /*
* 1.创建一个实现Runable接口的类，重写run()方法
* 2.创建该实例
* 3. 把实例传给Thread对象
* 4. 调Start()开启线程
* */
Myrun run = new Myrun();
Thread thread = new Thread(run);
thread.setName("线程3");
thread.start();

public class Myrun implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i < 50; i++) {
            // 获取当前线程, 因为是实现Runable接口，没有继承Thread, 不能直接调用getName
            Thread t = Thread.currentThread();
            System.out.println(t.getName() + " "+i);
        }
    }
}
// ============================================
// 方式3 特点，可以获取多线程执行的返回结果
/*
	1.创建一个类实现Callable接口，重写call方法（有返回值，表示多线程执行的结果）
	2.创建myCalld对象（表示多线程要执行的任务）
	3.创建futureTask对象（管理多线程运行的结果）
	4.创建Thread类对象，并启动线程
*/
Mycall mycall = new Mycall();
FutureTask<Integer> futureTask = new FutureTask<>(mycall); // futureTask用来管理多线程的运行结果
Thread t4 = new Thread(futureTask); // 线程传入futureTask
t4.start();
Integer result = futureTask.get(); // 获取线程中的代码返回结果
System.out.println(result);

import java.util.concurrent.Callable;

public class Mycall implements Callable<Integer> {

    @Override
    // 多线程代码执行结果的返回值类型和Callable泛型一样
    public Integer call() throws Exception {
        int sum = 0;
        for (int i = 0; i < 100; i++) {
            sum += i;
        }
        return sum;
    }
}
```



### 成员方法

![image-20230804173034164](D:\typora-img\image-20230804173034164.png)

```java
// 线程休眠，执行到的地方会停止指定时间，再执行后面的代码
System.out.println(1111111111)
Thread.sleep(5000);
System.out.println(222222222) // 5秒后打印
    

```

### 线程调度

JAVA采用抢占式调度，即CPU随机执行多线程

**随机到的概率和线程优先级有关，JAVA线程的优先级范围是1~10，默认优先级为5**

```java
MyRunable mr = new MyRunable();
Thread t1 = new Thread(mr, "飞机");
Thread t2 = new Thread(mr, "坦克");
// 设置线程优先级
t1.setPriority(1);
t2.setPriority(10); // t2先执行完毕的概率远大于t1 (t1也有小概率先执行完毕)
t1.start();
t2.start();

public class MyRunable implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 50; i++) {
            // 获取当前线程, 因为是实现Runable接口，没有继承Thread, 不能直接调用getName
            Thread t = Thread.currentThread();
            System.out.println(t.getName() + " "+i);
        }
    }
}

//守护线程=================== 会等到非守护线程执行完毕后，陆续结束（不是马上结束）
public class threadcase02 {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        MyThread2 t2 = new MyThread2();
        t1.setName("传输文件");
        t2.setName("聊天窗口");

        // t1设置为守护线程
        t1.setDaemon(true);
        // 当非守护线程t2执行完毕后，t1会陆续结束 应用场景：当聊天窗口结束后，传输文件也没有存在必要就会结束
        t1.start();
        t2.start();
    }
}

// 出让线程=============》尽可能让线程能均匀的交替执行
public class MyThread extends Thread {
    @Override
    public void run() {
        for(int i=0;i<=100;i++) {
            System.out.println(getName() + " " + i);
            Thread.yield(); // 表示让出当前CPU执行权
        }
    }
}

// 插入线程======================》 把想执行的线程插入到当前线程之前， 当前线程就是代码运行的线程
public class test {
    public static void main() {
        MyThread t = new Thread();
        t.setName("我的线程");
        t.start();
        t.join(); // 优先执行 我的线程会优先执行完毕
        
        // 主线程后执行
        for (int i = 0; i < 10; i++) {
            System.out.println("主线程:" + i);
        }
    } 
}
```



### 线程生命周期

1.创建对象==> 2.start() ==>3.有执行资格，没有执行权（抢夺CPU执行权中） ==>4 抢到后，有执行资格和执行权 ==> 5.run() 执行完毕线程死亡

PS: 其他线程抢夺或sleep()等阻塞方法会再次让线程没有执行权， sleep方法结束后重新进入抢夺状态， 3，4步会不断重复

### 线程6大状态

新建

就绪(start执行完毕，notify，获得锁) ------ 有执行资格，没有执行权，此时可以抢夺CPU执行权

计时等待(sleep) ------ 没有执行资格，没有执行权

等待(wait) ------ 没有执行资格，没有执行权

阻塞(无法获得锁) ------ 没有执行资格，没有执行权

死亡(run执行完毕)



### sleep理解

sleep属于Thread类，线程暂停执行指定时间，让出CPU给其他线程，当指定时间到了自动恢复运行状态

**在同步代码块中调用sleep，不会释放锁**



### wait/notify理解

wait/notify/notifyAll 都是Object类的方法，调用wait()，线程会自动放弃锁对象，进入等待此对象的等待锁定池，只有针对此对象调用notify()方法后，本线程才进入对象锁定池准备获取对象锁进入运行状态



### 多线程操作共享数据

#### synchronized同步代码块

**一个线程访问一个对象中的synchronized(this)同步代码块时，其他试图访问该对象的线程将被阻塞**

**重点：锁对象必须是唯一的，如果线程的锁对象不是同一个，同步代码不会有效果，线程不会被阻塞**

run方法4步套路：

1、循环

2、同步代码块

3、判断共享数据是否到末尾（到末尾的代码逻辑，退出）

4、判断共享数据是否到末尾（没到末尾，核心逻辑）



特点：锁默认打开，当一个线程进入，锁自动关闭；里面代码全部执行完毕，线程出来后锁自动打开。

```java
package com.david.demo19ThreadCase;

public class MyThread03 extends Thread{
    static int ticket = 0; // 定义静态变量，多个实例共享该数据
    @Override
    public void run() {
        while(true) {
            // 同步代码块 线程进入后会在执行完毕时其他线程才能进入执行
            // 必须传入一个唯一的锁对象
            synchronized (MyThread03.class) {
                if (ticket < 100) {
                    try {
                         // 休眠100毫秒 -- 同步代码块内执行sleep不会让出线程
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                    // 不加锁时，多线程执行是完全随机的，可能这段代码执行后执行权就被其他线程抢夺，打印代码还没执行，ticktet又+1了，出现超出100的情况
                    ticket++; 
                    System.out.println(Thread.currentThread().getName() + " " + ticket);
                } else {
                    break;
                }
            }
        }
    }
}
public class threadcase03 {
    public static void main(String[] args) {
        // 需求：3个窗口卖100张电影票
        // 3个线程内的同步代码块是同一个锁对象，当一个线程访问时，其他线程将被阻塞，只有代码块执行完才会释放锁
        MyThread03 t1 = new MyThread03();
        MyThread03 t2 = new MyThread03();
        MyThread03 t3 = new MyThread03();
        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");
        t1.start();
        t2.start();
        t3.start();
    }
}
```



#### 同步方法

**必须用接口实现类的方式创建同步方法，这样才能保证线程调用时，同步方法中的锁对象是唯一的**

语法：修饰符 synchronized 返回值类型 方法名(参数) {}

特点：同步方法锁住方法里的所有代码；锁对象不能自己指定，非静态：this，静态：当前类的字节码文件对象

```java
public class Myrun2 implements Runnable{
    int ticket = 0; // 用Runnable接口实现，只创建一个实例，所以不需要定义成静态变量

    @Override
    public void run() {
        while (true) {
            if (method()) break;
        }
    }

    // 同步方法 锁对象自动指定为this
    private synchronized boolean method() {
        if (ticket == 100) {
            return true;
        } else {
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            ticket++;
            System.out.println(Thread.currentThread().getName() + "正在卖第" + ticket + "张票");
        }
        return false;
    }
}


public class threadcase03 {
    public static void main(String[] args) {
        // 需求：3个窗口卖100张电影票
        // 用同步方法实现
        Myrun2 mr = new Myrun2(); // 只创建一个mr对象，所有线程的锁对象是同一个
        Thread t1 = new Thread(mr);
        Thread t2 = new Thread(mr);
        Thread t3 = new Thread(mr);
        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");
        t1.start();
        t2.start();
        t3.start();
    }
}

```



#### StringBuilder和StringBuffer

所有方法都一样，但是StringBuffer所有方法都加了Synconized

结论：多线程代码建议用StringBuffer，单线程用StringBuilder



#### lock锁

```java
// 作用和Syncronized一样，但可以手动在任意位置加锁，释放锁
import java.util.concurrent.locks.ReentrantLock;

public class ThreadLock extends Thread{
    static int tickets = 0;
    static ReentrantLock lock = new ReentrantLock();

    @Override
    public void run() {
        while (true) {
            lock.lock(); // 加锁
            try {
                if (tickets < 100) {
                    sleep(10);
                    tickets++;
                    System.out.println(getName() + "售卖第" + tickets + "张票");
                } else {
                    break; // break会直接跳出循环，导致unlock执行不到，虚拟机无法结束
                }
            } catch (RuntimeException e) {
                throw new RuntimeException(e);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            } finally {
                lock.unlock(); // 关键点：释放锁放在finally中，保证break执行后，unlock依然会执行释放锁
            }
        }
    }
}

public class threadcase03 {
    public static void main(String[] args) {
        // lock锁实现
        ThreadLock t1 = new ThreadLock();
        ThreadLock t2 = new ThreadLock();
        ThreadLock t3 = new ThreadLock();
        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");
        t1.start();
        t2.start();
        t3.start();
       
}

```



#### 死锁

一种错误代码方式，同步代码块中，线程1：A锁套B锁， 线程2：B锁套A锁，导致互相等待对方释放锁，代码会卡死 

**防止思路：不要嵌套锁**



### 等待唤醒机制

一种多线程协作的模式



#### 消费者/生产者模式

两个线程交替执行

```java
// 常用方法
void wait() // 当前线程等待，直到被其他线程唤醒
void notify() // 随机唤醒单个线程
void notifyAll() // 唤醒所有线程
  

// 第三者状态，维护公共数据，用来给生产者、消费者进行线程切换的判断
public class Desk extends Thread{
    public static int count = 10; // 消费者一共可以吃10碗面，吃到后退出线程
    public static Object lock = new Object(); // 定义一个唯一的锁对象
    public static int flag = 1; // 状态值：桌上是否有食物 1：有食物 2：无食物
}


// 生产者
public class Cook extends Thread{
    @Override
    public void run() {
        while (true) {
            // 传入锁对象Desk.lock
            synchronized (Desk.lock) {
                if (Desk.count == 0) {
                    break;
                } else {
                    // 桌上有食物
                    if (Desk.flag == 1) {
                        try {
                            Desk.lock.wait(); // 线程和锁对象绑定，等待消费者线程
                        } catch (InterruptedException e) {
                            throw new RuntimeException(e);
                        }
                    } else {
                        System.out.println("生产者正在制作食物");
                        Desk.lock.notifyAll(); // 唤醒消费者线程
                        Desk.flag = 1;
                    }
                }
            }
        }
    }
}

// 消费者
public class Eater extends Thread{
    @Override
    public void run() {
        while (true) {
            // 锁对象用Desk.lock静态变量，保证唯一
            synchronized (Desk.lock) {
                // 先判断是否吃完10碗, 即循环体的退出条件
                if (Desk.count == 0) {
                    break;
                } else {
                    // 执行代码
                    // 判断桌上是否有食物
                    if (Desk.flag == 1) {
                        Desk.count--;
                        System.out.println("消费者正在吃面，还能吃" + Desk.count + "碗");
                        Desk.lock.notifyAll(); // 唤醒生产者线程执行代码
                        Desk.flag = 0; // 把状态设置为0
                    } else {
                        try {
                            Desk.lock.wait();// 没有食物，等待， 必须用锁对象调用这个方法，和当前线程进行绑定
                        } catch (InterruptedException e) {
                            throw new RuntimeException(e);
                        }
                    }
                }
            }
        }
    }
}

public class waitNotifyTest {
    public static void main(String[] args) {
        // 生产者消费者模式
        Eater eater = new Eater();
        Cook cook = new Cook();
        eater.start();
        cook.start();
        /*最终执行结果，生产者消费者交替执行*/
    }
}
```



#### 阻塞队列

消费者生产者通过阻塞队列交替执行

阻塞队列继承接口：Iterable、Collection、Queue、BlockingQueue

实现类：ArrayBlockingQueue（底层是数组，有界，指定数组长度）、LinkedBlockingQueue（底层是链表，无界，但不是真正的无界，最大值是int的最大值）

```java
public class waitNotifyTest {
    public static void main(String[] args) {
        // 阻塞队列方式实现消费/生产者模式
        /*
        * 1.创建队列对象
        * 2.传给线程，保证2个线程用的是同一个队列
        *
        * */
        // 泛型String就是队列中的数据类型，参数是队列中元素的个数
        ArrayBlockingQueue<String> queue = new ArrayBlockingQueue<>(1);

        Cook cook = new Cook(queue);
        Eater eater = new Eater(queue);
        cook.start();
        eater.start();
    }
}

public class Eater extends Thread{
    // 定义成员变量，在创建对象时接收阻塞队列
    ArrayBlockingQueue<String> queue;
    // 构造函数接收阻塞队列参数
    public Eater(ArrayBlockingQueue<String> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        while (true) {
            try {
                /*阻塞队列不用添加Syncronized同步代码块，底层在调用put,take方法时已经添加了lock锁
                * */
                String food = queue.take(); // 用take方法不断从阻塞队列中获取元素, 队列为空时会等待
                System.out.println(food);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}

public class Cook extends Thread{
    ArrayBlockingQueue<String> queue;
    public Cook(ArrayBlockingQueue<String> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        while (true) {
            try {
                queue.put("海鲜面"); // 不断往阻塞队列中添加元素，队列满了会等待
                System.out.println("厨师烧了一碗面");
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}

```



### 线程栈

概念：JAVA的内存中，堆内存是唯一的，栈内存不是，每个线程都有自己独立的栈空间，比如main线程，main方法中的代码就执行在main的线程栈中

**当执行start()后，就会开启独立的线程栈，而每个线程都会执行自己的run方法，run方法中创建的变量都是互相独立的**

![image-20230808222516836](D:\typora-img\image-20230808222516836.png)



### 线程综练

```java
public class practise03 {
    public static void main(String[] args) {
        // 需求： 抢红包，100元，分3个红包，5个人抢
        // 打印结果：xxx抢到xxx元*3  xxx没抢到*2
        Thread03 t1 = new Thread03();
        Thread03 t2 = new Thread03();
        Thread03 t3 = new Thread03();
        Thread03 t4 = new Thread03();
        Thread03 t5 = new Thread03();

        t1.setName("线程1");
        t2.setName("线程2");
        t3.setName("线程3");
        t4.setName("线程4");
        t5.setName("线程5");

        t1.start();
        t2.start();
        t3.start();
        t4.start();
        t5.start();
    }
}
class Thread03 extends Thread {
    static double money = 100;
    static int count = 3; // 红包个数
    static final double MIN = 0.01; // 不能抽到0，定义能抽到的最小金额
    @Override
    public void run() {
        synchronized (Thread03.class) {
            double prize;
            if (count == 0) {
                System.out.println(getName() + "没抢到");
            } else if (count == 1) {
                prize = money; // 只剩一个红包，全部给出
                count--;
                System.out.println(getName() + "抢到了" + prize + "元");
            } else {
                Random r = new Random();
                // 随机金额范围：第一次最大99.98 因为要剩2个红包,以此类推
                // 随机范围 = 总金额 - (count - 1) * MIN
                double bounds = money - (count - 1) * MIN;
                prize = r.nextDouble(bounds);
                if (prize < MIN) {
                    prize = MIN;
                }
                count--;
                money = money - prize; // 每次金额=上次剩余-红包
                System.out.println(getName() + "抢到了" + prize + "元");
            }
        }
    }
}

```



### 线程池

前面的多线程代码，在执行完任务后线程就会销毁，下次再执行需要再创建线程，浪费资源，线程池就是用来保存线程的容器

概念：

1.存放线程的容器，创建线程池时初始是空的，提交任务时，线程池创建线程对象，任务执行完毕，线程归还给线程池，下次再提交时，不需要创建新的线程，复用已有线程

2.若提交任务时没有空闲线程，也无法创建新线程（达到最大线程数），则任务会等待

```java
// 工具类Executors创建线程池
public static ExecutorService newCachedThreadPool() // 创建一个没有上限的线程池
public static ExecutorService newFixedThreadPool(int nThreads) // 创建有上限的线程池

    
public class p1 {
    public static void main(String[] args) throws InterruptedException {
        // 创建无上限线程池对象
        ExecutorService pool1 = Executors.newCachedThreadPool();

        // 创建有上限的线程池对象 参数表示线程数，最多3条线程
        ExecutorService pool2 = Executors.newFixedThreadPool(3);

        // 提交任务给线程池, 可接受一个Runnable对象
        pool1.submit(new MyRun());

        // main线程休眠，确保线程空闲
        Thread.sleep(1000);

        // 再提交一个任务，依然是线程池的线程1执行，线程会被复用
        pool1.submit(new MyRun());

        // 关闭线程池，实际开发中线程池一般不关闭
        // pool1.shutdown();
    }
}

```



#### 自定义线程池

概念：

核心线程：一直存在

临时线程：空闲时会被销毁

线程池执行逻辑：

例：核心线程3，临时线程3，队伍长度3，任务数10

核心线程执行任务123，任务456放到队伍中等待，创建3个临时线程执行任务789，此时多出来的任务10就会被执行拒绝策略(默认会拒绝，并抛出异常)

```java
public class p2 {
    public static void main(String[] args) {
        // 自定义线程池 接收7个参数
        /*
        * 1.核心线程数 不能小于0
        * 2.最大线程数 不能小于0，不能小于核心线程数
        * 3.临时线程最大存在时间
        * 4.时间的单位
        * 5.任务队列
        * 6.创建线程工厂
        * 7.任务的拒绝策略
        * */
        ThreadPoolExecutor pool = new ThreadPoolExecutor(
                3,
                6,
                60,
                TimeUnit.SECONDS, // 用TimeUnit指定时间单位
                new ArrayBlockingQueue<>(3), // 任务队列
                Executors.defaultThreadFactory(), // 线程工厂对象
                new ThreadPoolExecutor.AbortPolicy() // AbortPolicy是一个静态内部类，语法是先调用外部类再创建内部类
        );

        // 提交任务 线程池会自动分配核心线程给任务，若满了则会排队，若队列也满了则会创建临时线程
        pool.submit(new MyRun());
        pool.submit(new MyRun());
        pool.submit(new MyRun());
        pool.submit(new MyRun());
        pool.submit(new MyRun());
    }
}
```



#### 最大并行数

假如电脑CPU是4核8线程，那么该电脑的最大线程数就是8，能并行处理8个任务，最大并行数就是8



根据项目特点，一般分为：

CPU密集型运算（计算多）=====> 线程池设置：最大并行数+1，1是后备线程，防止前面线程有问题

IO密集型运算（读写文件，数据库多） ====> 线程池设置：最大并行数*期望CPU利用率 * （ 总时间(CPU计算时间+等待时间) / CPU计算时间 ）

例如一个任务需要读取2个文件，并进行相加，读取文件硬盘耗时1秒，相加CPU耗时1秒，总时间就是2秒，可以用Thread.dump算出时间

核心数 8 * CPU利用率100% * (总时间2秒/CPU时间1秒) = 16 



## 网络编程

### 三要素

IP：设备在网络中的地址，唯一标识

端口号：应用程序在设备中的唯一标识

协议：数据在网络中进行传输的规则，常见协议有UDP,TCP,HTTP,HTTPS,FTP

**InetAddress**：表示IP对象的类



本机IP和路由分配IP的区别：

本机IP永远是127.0.0.1，假设路由器分配的本机IP是192.168.1.100

往127.0.0.1发送数据时，不走路由器，网卡判断是本机地址直接返回

往路由分配的本机IP,往192.168.1.100发送数据时，会先通过路由器再返回给本机



**端口：**

端口号由2个字节表示的整数，取值范围0~65535，0~1023用于一些网络服务或应用，1024以上可以自己用

**一个端口号只能被一个应用使用**

端口就是应用程序对外的出口，一个应用必须绑定一个端口，否则无法使用网络

比如使用微信聊天，微信绑定了65533端口，那么两台电脑的微信就能通过65533端口收发数据



**协议**

OSI 7层参考模型，世界互联网协议标准，模型过于理想化，未广泛推广

7层：应用层，表示层，会话层，传输层，网络层，数据链路层，物理层

**TCP/IP 4层参考模型（TCP/IP协议）：事实上的国际标准**

4层：应用层(应用程序相关协议http,ftp,dns,telnet..)，传输层(传输协议TCP,UDP)，网络层(IP,ICMP,ARP封装)，物理链路层(转010101物理设备传输)



#### UDP协议

user datagram protocal 用户数据报协议， 使用场景：网络会议，视频通话等，丢失一点数据影响不大的场景

UDP是面向**无连接**通信协议，速度快，有大小限制，一次最大发送64K，数据不安全，易丢失

无连接含义：不会检查接收方连接是否畅通，直接就发送了

```java
public class UDPtest {
    public static void main(String[] args) throws IOException {
        // UDP发送数据
        // 1.创建发送数据的承载对象 可以接受一个参数，表示绑定本机哪个端口
        DatagramSocket ds = new DatagramSocket();

        // 2.打包要发送的数据
        String str = "hello david!"; // 要发送的数据
        byte[] bytes = str.getBytes(); // 转字节数组
        InetAddress address = InetAddress.getByName("David-0702"); // 接收的主机
        int port = 10086; // 接收主机的端口

        DatagramPacket dp = new DatagramPacket(bytes, bytes.length, address, port);

        // 3.发送数据
        ds.send(dp);

        // 4.释放资源
        ds.close();
    }
}

public class UDPrecieve {
    public static void main(String[] args) throws IOException {
        // UDP接收数据
        // 1.接收数据的对象，必须绑定端口，要和发送数据指定的端口一致
        DatagramSocket ds = new DatagramSocket(10086);

        // 2.创建接收数据用的数据包
        byte[] bytes = new byte[1024];
        DatagramPacket dp = new DatagramPacket(bytes, bytes.length);

        // 这是一个阻塞方法，若没有接收到数据会一直等待
        ds.receive(dp);

        // 3.解析数据
        byte[] data = dp.getData();
        int length = dp.getLength();
        InetAddress address = dp.getAddress();
        int port = dp.getPort();
        System.out.println("接收到的数据" + new String(data, 0, length)); // 字节数组要解析为字符串输出
        System.out.println("该数据从" + address + "这台电脑中" + port +"这个端口发出");

        // 4.释放资源
        ds.close();
    }
}
```

**基于UDP的聊天室原型**

```java
public class UDPtest {
    public static void main(String[] args) throws IOException {
        // UDP发送数据
        /*
        * 需求：
        * UDP发送数据，数据来自键盘录入，直到输入数据是886，发送数据结束
        * UDP接收数据，因为接收端不知道发送端什么时候结束，采用死循环接收
        * */
        // 1.创建发送数据的承载对象，可以传一个参数绑定本机哪个端口，不传就随机绑定
        DatagramSocket ds = new DatagramSocket();

        // 2.打包要发送的数据
        // 因为是不断输入的，用循环处理这段代码
        while (true) {
            Scanner sc = new Scanner(System.in);
            System.out.println("请输入要发送的内容:");
            String str = sc.nextLine();
            if (str.equals("886")) { // 当输出886时，发送端结束
                break;
            }
            byte[] bytes = str.getBytes();
            InetAddress address = InetAddress.getByName("David-0702"); // 接收的主机
            int port = 10086; // 接收主机的端口

            DatagramPacket dp = new DatagramPacket(bytes, bytes.length, address, port);
            // 3.发送数据
            ds.send(dp);
        }

        // 4.释放资源
        ds.close();
    }
}

public class UDPrecieve {
    public static void main(String[] args) throws IOException {
        // UDP接收数据
        // 1.接收数据的对象，必须绑定端口，要和发送数据的端口一致
        DatagramSocket ds = new DatagramSocket(10086);

        // 2.创建接收数据用的数据包
        byte[] bytes = new byte[1024];
        DatagramPacket dp = new DatagramPacket(bytes, bytes.length);

        // 因为接收端不知道发送端什么时候会停止发送，所以需要一直循环接收
        while (true) {
            ds.receive(dp); // 这是一个阻塞方法，若没有接收到数据会一直等待
            // 3.解析数据
            byte[] data = dp.getData();
            int length = dp.getLength();
            InetAddress address = dp.getAddress();
            int port = dp.getPort();
            System.out.println("接收到的数据" + new String(data, 0, length)); // 字节数组要解析为字符串输出
            System.out.println("该数据从" + address + "这台电脑中" + port +"这个端口发出");
        }
    }
}
```

#### UDP三种发送方式

单播：一对一发送 (上面的代码就是单播)

组播：一对多发送 组播地址224.0.0.0~239.255.255.255，其中224.0.0.0~224.0.0.255为预留的组播地址

广播：对局域网内所有电脑发送 广播地址255.255.255.255

```java
// 组播部分代码
// 发送端
MulticastSocket ms = new MulticastSocket(); // 创建MulticastSocket对象
String s = "你好啊";
byte[] bytes = s.getBytes[];
InetAddress address = InetAddress.getByName("224.0.0.1") // 发送地址设置为组播地址
int port = 10000;
DatagramPacket dp = new DatagramPacket(bytes, bytes.length, address, port);
ms.send(dp);
ms.close();
   
// 接收端 可以创建多个接收端都添加到224.0.0.1组中，就实现了组播效果
MulticastSocket ms = new MulticastSocket(10000);    
InetAddress address = InetAddress.getByName("224.0.0.1")
ms.joinGroup(address) // 将本机添加到224.0.0.1这一组中

byte[] bytes = new byte[1024];
DatagramPacket dp = new DatagramPacket(bytes, bytes.length);    
ms.receive(dp)
    
// 解析数据
...
    
// 广播 发送端改这个地址就行，接收端和单播一样
InetAddress address = InetAddress.getByName("255.255.255.255")
```





#### TCP协议

transmission control protocal 传输控制协议，使用场景：文件下载，邮件发送，不能丢失数据的场景

TCP**面向连接**的通信协议，传输较慢，无大小限制，传输安全，通过Socket产生IO流来进行网络通信

**在服务端和客户端各建立一个ServerSocket/Socket对象，在通信前保证连接已经建立（三次握手）**

四次挥手：客户端向服务端发出取消连接请求，服务端返回一个响应信息表示收到请求，将数据处理完毕后再向客户端发出确认取消信息，客户端收到后再次发送取消连接请求真正断开连接

客户端是输出流，发送数据；服务端是输入流，接收数据

```java
public class TCPClient {
    public static void main(String[] args) throws IOException {
        // tcp客户端
        // 1.创建socket对象并连接服务端， 如果服务端端口没有启用会报错
        Socket socket = new Socket("127.0.0.1", 10001);

        // 2.从连接通道获取输出流，不是自己创建
        OutputStream os = socket.getOutputStream();

        os.write("你好啊，david".getBytes());

        // 3.释放资源
        os.close(); // 可以不写，关闭连接时自动释放
        socket.close(); //关闭链接 内部会等到服务端获取到数据后再真正关闭（四次挥手协议，确保连接断开，且通道中数据处理完毕）
    }
}

public class TCPServer {
    public static void main(String[] args) throws IOException {
        // TCP接收端 服务器端
        // 创建ServerSocket 绑定端口
        ServerSocket serverSocket = new ServerSocket(10001);

        // 监听客户端的链接，阻塞方法 会一直等待，有客户端连接就会返回客户端的socket对象，连接真正建立
        Socket socket = serverSocket.accept();

        // 从连接通道获取输入流，读取数据
        InputStream is = socket.getInputStream();
        // 因为数据包含中文，用转换流转成字符流读取
        InputStreamReader isr = new InputStreamReader(is);
        // 再把字符流转成缓冲流，提高读取效率
        BufferedReader br = new BufferedReader(isr);
        String str;
        while ((str = br.readLine()) != null) {
            System.out.println(str);
        }
        socket.close();
        serverSocket.close();
    }
}
```



##### 多发多收

```java
public class TCPClient {
    public static void main(String[] args) throws IOException {
        // 客户端
        // 需求：发送用户输入内容，直到输入886，结束连接

        // 1.创建socket对象并连接服务端， 如果服务端端口没有启用，会报错
        Socket socket = new Socket("127.0.0.1", 10001);

        // 2.从连接通道获取输出流，不是自己创建
        OutputStream os = socket.getOutputStream();

        Scanner sc = new Scanner(System.in);
        while (true) {
            System.out.println("请输入内容：");
            String str = sc.nextLine();
            if (str.equals("886")) {
                break;
            }
            // 写出数据
            os.write(str.getBytes());
        }
        socket.close(); //关闭链接
    }
}

public class TCPServer {
    public static void main(String[] args) throws IOException {
        // TCP接收端 服务器端
        // 创建ServerSocket 绑定端口
        ServerSocket serverSocket = new ServerSocket(10001);

        System.out.println("准备连接");
        Socket socket = serverSocket.accept(); // 监听客户端的链接，阻塞方法 会一直等待
        System.out.println("已监听到客户端连接，等待数据中"); // 当客户端开启连接，就会执行到这里
        // 从连接通道获取输入流，读取数据
        InputStream is = socket.getInputStream();

        // 因为数据包含中文，用转换流转成字符流读取
        InputStreamReader isr = new InputStreamReader(is);
        int b;
        System.out.println("已获取连接通道IO流，开始读取数据");
        while ((b = isr.read()) != -1) { // read会一直等待，直到结束标记，或客户端关闭连接
            System.out.println("已读到数据");
            System.out.print((char) b);
        }
        System.out.println("已读完数据，准备关闭服务端");
        socket.close();
        serverSocket.close();
    }
}
```



##### 服务端反馈

```java
public class TCPClient {
    public static void main(String[] args) throws IOException {
        // 客户端
        // 需求：发送用户输入内容，直到输入886，结束连接

        // 1.创建socket对象并连接服务端， 如果服务端端口没有启用，会报错
        Socket socket = new Socket("127.0.0.1", 10001);

        // 2.从连接通道获取输出流，不是自己创建
        OutputStream os = socket.getOutputStream();

        Scanner sc = new Scanner(System.in);
        while (true) {
            System.out.println("请输入内容：");
            String str = sc.nextLine();
            if (str.equals("886")) {
                break;
            }
            // 写出数据
            os.write(str.getBytes());
        }
        socket.shutdownOutput(); // 结束标识，服务端不会再等待read
        // 接收服务端反馈数据
        InputStream is = socket.getInputStream();
        InputStreamReader isr = new InputStreamReader(is);
        int z;
        while ((z = isr.read()) != -1) {
            System.out.print((char) z);
        }
        socket.close(); //关闭链接
    }
}


public class TCPServer {
    public static void main(String[] args) throws IOException {
        // TCP接收端 服务器端
        // 创建ServerSocket 绑定端口
        ServerSocket serverSocket = new ServerSocket(10001);


        System.out.println("准备连接");
        Socket socket = serverSocket.accept(); // 监听客户端的链接，阻塞方法 会一直等待
        System.out.println("已监听到客户端连接，等待数据中");
        // 从连接通道获取输入流，读取数据
        InputStream is = socket.getInputStream();

        // 因为数据包含中文，用转换流转成字符流读取
        InputStreamReader isr = new InputStreamReader(is);
        int b;
        System.out.println("已获取连接通道IO流，开始读取数据");

        // 反馈给客户端，用输出流
        OutputStream os = socket.getOutputStream();

        while ((b = isr.read()) != -1) { // read会一直等待通道中的数据，直到遇到结束标记，或客户端关闭连接
            System.out.print((char) b);
        }
        System.out.println();
        System.out.println("已读完数据，准备关闭服务端");
        os.write("服务端回显：大卫你好呀".getBytes());
        socket.close();
        serverSocket.close();
    }
}
```



##### 文件上传

```java
public class TCPClient {
    public static void main(String[] args) throws IOException {
        // 文件上传客户端
        Socket socket = new Socket("127.0.0.1", 10001);

        // 读取本地文件
        FileInputStream fs = new FileInputStream("C:\\Users\\yoki\\Desktop\\javaTest\\a.jpg");
        // 缓冲流读取文件
        BufferedInputStream bis = new BufferedInputStream(fs);

        OutputStream os = socket.getOutputStream();
        // 用缓冲流提高写出效率
        BufferedOutputStream bos = new BufferedOutputStream(os);
        byte[] bytes = new byte[1024];
        int len;
        while ((len = bis.read(bytes)) != -1) {
            bos.write(bytes, 0, len);
            bos.flush(); // 重要！清空缓存
        }

        socket.shutdownOutput(); // 传输结束标识
        socket.close(); // 关闭连接
    }
}

public class TCPServer {
    public static void main(String[] args) throws IOException {
        // 服务端接收上传文件
        ServerSocket serverSocket = new ServerSocket(10001);

        Socket socket = serverSocket.accept();

        // 把流中读到的文件保存到指定文件夹中
        InputStream is = socket.getInputStream();
        BufferedInputStream bis = new BufferedInputStream(is);
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("fileUpload\\AAA.jpg")); // 保存到项目的相对路径

        byte[] bytes = new byte[1024];
        int len;
        while ((len = bis.read(bytes)) != -1) { // 读取文件
            bos.write(bytes, 0, len); // 存入指定位置
            bos.flush(); // 重要！清空缓存，防止数据不完整
        }
        socket.close();
        serverSocket.close();
    }
}

```



##### 多用户文件上传

不关闭服务端，利用无限循环+多线程解决，只有循环就必须排队处理，多线程可以同时处理多个上传

```java
// 客户端不变，服务端代码
package com.david.demo25netUpload;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Random;

public class TCPServer {
    public static void main(String[] args) throws IOException {
        // 服务端接收上传文件
        ServerSocket serverSocket = new ServerSocket(10001);

        // 创建线程池对象
        ThreadPoolExecutor pool = new ThreadPoolExecutor(
                3, // 核心线程数
                16, //线程池总大小
                60, // 空闲时间
                TimeUnit.SECONDS, // 空闲时间的单位
                new ArrayBlockingQueue<>(2), // 队列
                Executors.defaultThreadFactory(), // 线程工厂，让线程池如何创建线程
                new ThreadPoolExecutor.AbortPolicy() // 阻塞策略
        );
        // 多线程接收文件上传
        while (true) {
            Socket socket = serverSocket.accept();

            // 1.当有客户端连接就用MyRunnable实现类创建一个线程
            //new Thread(new MyRunnable(socket)).start();
            
            // 2.用线程池创建线程
            pool.submit(new MyRunnable(socket));
        }
    }
}

public class MyRunnable implements Runnable{
    Socket socket;
    public MyRunnable (Socket socket) {
        this.socket = socket;
    }
    @Override
    public void run() {
        try {
            InputStream is = socket.getInputStream();
            BufferedInputStream bis = new BufferedInputStream(is);
            // 保存到项目的相对路径
            BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("fileUpload\\AAA.jpg"));
            byte[] bytes = new byte[1024];
            int len;
            while ((len = bis.read(bytes)) != -1) { // 读取文件
                bos.write(bytes, 0, len); // 存入指定位置
                bos.flush(); // 重要！清空缓存
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            if (socket != null) {
                try {
                    socket.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }
    }
}

```





## 反射

定义：反射允许对封装类的成员变量，方法，构造函数的信息进行编程访问。

反射必须要获取字节码文件对象，通过Class类可以获取class对象， Class也是JAVA中定义好的一个类

获取Class类的3种方式：

1.Class.forName("全类名")    // 全类名：包名+类名

2.类名.class

3.对象.getClass()    //getClass是Object的方法，所有对象都可调用



### 用反射获取类中所有元素

```java
package com.david.demo26Reflect;

import java.lang.reflect.*;

public class reflectTest {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException, NoSuchFieldException {
        // 获取class字节码对象,要用全类名
        Class clazz = Class.forName("com.david.demo26Reflect.Person");

        // 获取构造方法==================================
        // 获取该类的公共构造方法的对象的数组
        Constructor[] cons1 = clazz.getConstructors();
        for (Constructor con1 : cons1) {
            System.out.println("公共:" + con1);
        }

        // 获取所有构造方法的对象的数组,包括私有
        Constructor[] cons2 = clazz.getDeclaredConstructors();
        for (Constructor con2 : cons2) {
            System.out.println("所有：" + con2);
        }

        // 获取单个公共构造方法对象, 要传构造函数形参的class类, 参数和构造方法的参数一致
        Constructor con3 = clazz.getConstructor(String.class, int.class, String.class, Double.class);
        System.out.println("con3:" + con3);

        // 获取单个构造方法对象，可获取私有构造方法
        Constructor con4 = clazz.getDeclaredConstructor(String.class, int.class);
        System.out.println("con4:" + con4);

        // 获取构造方法的权限修饰符
        int modifiers = con4.getModifiers();
        System.out.println("权限修饰符：" +modifiers); // 返回2， 返回整数形式， JDK文档可以查对应权限
        // 用构造方法创造对象
        con4.setAccessible(true); // con4是私有构造方法，要先设置权限才能调用，这种操作叫暴力反射
        Person p = (Person) con4.newInstance("大卫", 36);
        System.out.println(p);


        // 获取成员变量==================================================
        System.out.println("成员变量================================");
        // 获取公共成员变量
        Field[] fields = clazz.getFields();
        for (Field field : fields) {
            System.out.println(field);
        }
        // 获取包括私有在内的所有成员变量
        Field[] declaredFields = clazz.getDeclaredFields();
        for (Field declaredField : declaredFields) {
            System.out.println(declaredField);
        }
        // 获取公共的单个成员变量
        Field name = clazz.getField("name");
        System.out.println(name);

        // 获取单个成员变量,可获取私有
        Field gender = clazz.getDeclaredField("gender");
        System.out.println(gender);
        // 获取权限修饰符
        int modifiers1 = name.getModifiers();
        System.out.println(modifiers1);

        // 获取成员变量名
        String n = name.getName();
        System.out.println(n);

        // 获取成员变量数据类型
        Class<?> type = name.getType();
        System.out.println(type); // class java.lang.String

        // 获取成员变量保存的值, 先创建一个对象
        Person p2 = new Person("david", 35, "男", 1.75);
        String value = (String)name.get(p2);
        System.out.println(value);

        // 修改对象中的值 参数是要修改的对象，和变更的值
        name.set(p2, "KING");
        System.out.println("修改后的对象：" + p2);

        System.out.println("成员方法========================");
        // 获取成员方法===============================================
        // 获取公共的成员方法，包括继承的父类公共方法
        Method[] methods = clazz.getMethods();
        for (Method method : methods) {
            System.out.println("本类+父类的方法：" + method);
        }
        // 获取本来方法，包括私有，没有父类
        Method[] declaredMethods = clazz.getDeclaredMethods();
        for (Method declaredMethod : declaredMethods) {
            System.out.println("本类方法：" +declaredMethod);
        }

        // 获取单个公共成员方法
        Method eat = clazz.getMethod("eat", String.class);
        Method eat2 = clazz.getMethod("eat");
        System.out.println(eat);
        System.out.println(eat2);
        // 获取单个私有成员方法
        Method sing = clazz.getDeclaredMethod("sing");
        System.out.println(sing);
        // 获取方法名
        System.out.println(sing.getName());
        // 获取方法形参的类型，返回一个数组
        Parameter[] params = eat.getParameters();
        for (Parameter param : params) {
            System.out.println(param);
        }
        // 重点：运行方法
        /*
        * Object invoke(Object obj, Object...args)： 运行方法
        * 参数1.用obj对象调用该方法
        * 参数2.调用方法传递的参数，没有就不写
        * 可以有返回值，直接接受方法运行结果即可
        * */
        Person p3 = new Person("大哥", 35, "男", 1.75);
        eat.invoke(p3, "香蕉"); // p3对象调用eat方法
    }
}

```



### 反射使用场景

1、对任意对象，都可以把对象的所有字段名和值保存到文件中

2、通过配置文件，动态创建对象并调用方法，实现不修改代码，只修改配置文件就能完成业务变更的效果

```java
// 配置文件样式 只修改配置文件就能执行不同的类和方法
classname=com.david.demo.Person
method=eat

// 读取配置文件
Properties prop = new Properties();
FileInputStream fis = new FileInputStream("xxx\\prop.properties");
prop.load(fis);
fis.close();

// 获取全类名和方法名
String className = (String)prop.get("classname");
String methodName = (String)prop.get("method");

// 反射获取构造方法并创建对象
Class clazz = Class.forName(className);
Constructor con = clazz.getDeclaredConstructor();
Object o = con.newInstance();

// 反射获取成员方法并运行
Method eat = clazz.getDeclaredMethod(methodName); //传入方法名获取方法
m.invoke(o, "苹果") // 传入创建的o对象，调用eat方法，并传入参数
```







## 动态代理

**定义：无侵入式的给代码增加额外功能**

```java
public class Test {
    public static void main(String[] args) {
        // 使用代理
        // 1.创建要被代理的对象
        SuperStar star = new SuperStar("萧敬腾");
        Star proxy = ProxyUtil.createProxy(star);

        // 2.用代理调用方法
        proxy.sing("王妃"); 
        /*
        * 执行效果：
        * 准备话筒，收钱
        * 萧敬腾正在唱王妃
        * */
    }
}

// 把想要被代理的方法定义在接口中
public interface Star {
    public abstract String sing(String name);
    public abstract void dance();
}

// 要被代理的对象，实现接口的方法就是要代理的方法
public class SuperStar implements Star {
    private String name;
    public SuperStar(String name) {
        this.name = name;
    }

    // 重写接口方法
    @Override
    public String sing(String name) {
        System.out.println(this.name+"正在唱"+name);
        return "谢谢";
    }

    @Override
    public void dance () {
        System.out.println(this.name+"正在跳舞");
    }
}


// 类的作用：创建一个代理
public class ProxyUtil {
    /*
    * 形参：被代理的对象
    *返回值：创建的代理,就是接口的类型，多态
    *
    * 实际逻辑：
    * 1. 获取代理对象
    * 代理对象 = ProxyUtil.createProxy(SuperStar)
    *
    * 2.再调用代理的唱歌方法
    *   代理对象.唱歌方法() 实际调用的就是invoke中的代码
    * */
    public static Star createProxy(SuperStar superStar) {
        /*
        * java.lang.reflect.Proxy类：提供了为对象产生代理对象的方法
        *
        * public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)
        * 参数1：用于指定哪个类加载器，去加载生成的代理类
        * 参数2：指定接口，用于指定生成的代理有哪些方法
        * 参数3：指定生成代理对象要做的事，即要执行的代码
        * */
        Star star = (Star) Proxy.newProxyInstance(
                ProxyUtil.class.getClassLoader(), // 当前类的类加载器
                new Class[]{Star.class}, // 指定Star接口
                new InvocationHandler() { // 也是一个接口，要写匿名内部类实现接口，重写invoke方法，真正要执行的代码
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        /*
                        * 参数1. 代理的对象
                        * 参数2. 要运行的方法 sing
                        * 参数3. 调sing方法时传递的实参
                        * */
                        if ("sing".equals(method.getName())) {// method类，可以用getName获取方法名
                            System.out.println("准备话筒，收钱");
                        } else if ("dance".equals(method.getName())) {
                            System.out.println("准备场地，收钱");
                        }
                        return method.invoke(superStar, args); // 传原对象和要给方法执行的参数
                    }
                }
        );
        return star;
    }
}
```



## 添加日志

第三方日志jar包：logback

1、添加3个jar包（logback-classic/logback-core/slf4j-api）

2、src下添加logback.xml配置文件

3、代码中创建logger日志对象

```java
// 创建一个logger日志对象
public static final Logger LOGGER = LoggerFactory.getLogger("logTest");// 参数是日志打印对象的名字

// 在执行的地方打日志，日志会输出到控制台，并保存到指定的日志文件中
try{
    LOGGER.info("方法执行了")
}catch(Exception e) {
    LOGGER.error("方法出错啦")
}
    
```

### 日志配置文件解析

1、设置日志的输出位置，通常是控制台和日志文件

2、设置日志的输出格式

3、有开关，开启或关闭日志

4、配置日志文件拆分规则，即每个日志文件大小，防止日志文件过大

5、设置日志级别，低于配置的级别不会被记录

```xml
<!--日志基本样式-->
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!--
        CONSOLE ：表示当前的日志信息是可以输出到控制台的。
    -->
    <appender name="Console" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>[%level] %blue(%d{HH:mm:ss.SSS}) %cyan([%thread]) %boldGreen(%logger{15}) - %msg %n</pattern>
        </encoder>
    </appender>

    <logger name="com.itheima" level="DEBUG" additivity="false">
        <appender-ref ref="Console"/>
    </logger>


    <!--

      level:用来设置打印级别，大小写无关：TRACE, DEBUG, INFO, WARN, ERROR, ALL 和 OFF
     ， 默认debug
      <root>可以包含零个或多个<appender-ref>元素，标识这个输出位置将会被本日志级别控制。
      -->
    <root level="DEBUG">
        <appender-ref ref="Console"/>
    </root>
</configuration>
```





## Junit单元测试

导入jar包（IDEA已集成）

为需要测试的业务类，定义对应的测试类，并为每个业务方法编写测试方法（必须：公共、无参、无返回值）

测试方法必须添加@Test注解，如报错则导入junit包即可

```java
// 创建测试类
public class StringUtilTest{
    // 测试方法，可以直接执行
    @Test
    public void testPrintNumber(){
        StringUtil.printNumber("david");
    }
}
```



## 注解

代码中的特殊标记，如@test @Override 让其他程序根据注解信息来决定怎么执行该程序

自定义注解：

```java
public @interface MyTest1{
	String name();
    boolean flag() default true;
    String[] ccc();
}

// 使用注解 括号中的本质是注解的实现类对象	
@MyTest1(name="david", ccc={"xx", "yy"})
public class AnnotationTest{
    
}


public @interface Mytest2{
    String value();
}

//当属性名=value，且只有1个，可以省略字段名，直接传值
@MyTest2("king")
```



**注解的真正含义：**

对java编译后产生的class文件进行反编译，可以看到注解的本质是接口，继承了Annotation接口

```java
// 反编译后的注解
public interface Mytest1 extends Annotation{
    public abstract String name();
    public abstract boolean flag();
    public abstract String[] ccc();
}
```



**元注解**

修饰注解的注解

```java
@Retention(RetentionPolicy.RUNTIME) // 注解的保留周期 runtime一直保留到运行阶段
@Target(ElementType.TYPE) // 当前注解只能用在类和接口上
public @interface Mytest1{}
```



### 解析注解

Class,Mehod等类都实现了Annotation接口，可以实现注解的解析

```java
// 业务类，其中有很多加了注解的成员方法
AnnoTationTest a = new AnnotationTest();
// 获取class对象
Class c = AnnotationTest.class
// 获取类中的所有方法 利用了反射
Method[] methods = c.getDeclearedMethods()
    
 // 遍历方法，判断方法是否有注解
    for(Method m : methods) {
        if(m.isAnnotationPresent(MyTest.class)) {
            // 获取注解实现类对象
           	Mytest mt = (MyTest) m.getDeclearedAnnotation(Mytest.class)
            mt.name(); // 取注解中的数据
            mt.ccc();
            // 有注解的，执行, 传入执行对象
            m.invoke(a);
        }
    }
```

