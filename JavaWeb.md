## MySQL

DBMS：数据库管理系统，MySQL就是一个数据库管理系统（开源免费的中小型数据库）

SQL: Structured Query Language ，结构化查询语言，操作关系型数据库的编程语言，定义操作所有关系型数据库的统一标准。



### 关系型数据库

**由多张能互相连接的二维表组成的数据库， 通过表存数据的就是关系型数据库**

优点：

1.都使用表结构，格式一致，易维护。

2.使用通用SQL语言操作，可用于复杂查询 。

3.数据存在磁盘中，安全。



### 安装配置

下载：https://downloads.mysql.com/archives/community/

**1.配置环境变量：**

新建系统变量：MYSQL_HOME： '安装路径'

path配置：%MYSQL_HOME%\bin



**2.新建my.ini配置文件(存放在mysql根目录)**

```ini
[mysqld]
; 设置3306端口
port=3306
; 设置mysql的安装目录
basedir=E:\\MySQL\\mysql-5.7.40-winx64
; 设置mysql数据库的数据的存放目录	
datadir=E:\\MySQL\\mysql-data
; 允许最大连接数
max_connections=200
; 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10
; 服务端使用的字符集默认为UTF8
character-set-server=utf8
; 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
; 默认使用“mysql_native_password”插件认证
default_authentication_plugin=mysql_native_password
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
[mysql]
; 设置mysql客户端默认字符集
default-character-set=utf8
[client]
; 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```



3.初始化data文件：管理员权限打开cmd，执行mysqld --initialize-insecure 

4.注册mysql服务：mysqld -install

5.启动服务：net start mysql

6. 登录mysql： mysql -u root -p （这个mysql指的是mysql.exe, 即mysql客户端，用来和服务通信，因为未设置密码，可以直接回车登录）
7. 查询用户名和密码：select host,user,authentication_string from mysql.user;

修改默认账户名密码：mysqladmin -u root password 123456  (修改管理员root账户的密码)

8.再次登录：mysql -u root -p123456 

登录成功后，即可执行sql语句

### 卸载

```commonlisp
net stop mysql //停止服务
mysqld -remove mysql// 删除服务
删除环境变量和mysql文件夹
```





### SQL

1.SQL语句可以单行或多行书写，**以分号结尾**。

2.MySQL数据库的SQL语句不区分大小写，关键字建议用大写。

3.注释：

单行注释  --  注释内容 或 #注释内容(MySQL特有)

多行注释  /* 注释内容 */



#### SQL分类

DDL (Data Definition Language) 数据定义语言，用来定义数据库对象：数据库，表，列等

DML（Data Manipulation Language）数据操作语言，对表中的数据进行增删改

DQL (Data Query Language) 数据查询语言，对表中的数据进行查询

DCL（Data Control Language）数据控制语言，用来定义数据库的访问权限和安全级别，及创建用户



MYSQL中数据类型分为3类：

1、数值类（TINYINT, INT, BIGINT, FLOAT, DOUBLE, DECIMAL....）

2、日期时间类（DATE, TIME, YEAR, DATETIME, TIMESTAMP）

3、字符串类（CHAR, VARCHAR, TINYBLOB, BLOB, LONGBLOB, TEXT, LONGTEXT....）

**细节：**

CHAR最大255字符，字符集对CHAR没有影响，CHAR()括号内填写最大字符数255

VARCHAR最大65535字节，字符集对VARCHAR有影响

UTF8字符集，每个字符大小3字节，所以65535/3 = 21845，最大支持21845字符，因此VARCHAR()括号中最大填写21845字符

GBK字符集，每个字符大小2字节，所以65535/2 = 32767.5，最大支持32767字符，因此VARCHAR()括号中最大填写32767字符

```sql
/* DDL */
show databases; -- 查看所有数据库
create database if not exists db1; -- 创建一个名为db1的数据库，先判断是否已存在同名数据库
drop database db1; -- 删除数据库
use db1; -- 使用db1数据库
select database(); -- 查看当前在使用的数据库

show tables; -- 查询当前数据中的所有表， 必须先进入数据库

-- 创建user表，格式：名称(空格)类型，和java相反
create table tb1_user(
	id int,
    username varchar(20), -- varchar 不定长字符串，存储空间可变
    password varchar(32)，
    usergender char(10), -- char 定长字符串，char(10)固定用10个字符空间，存储性能高，但浪费空间
    score double(5,2), -- 分数：范围0~100，double第一位是总长度，第二位是小数位数， 保留2位小数，所以总长度是3+2=5
    telephone varchar(15), --电话 最大长度不超过15位
    birthday date -- 生日 日期类型
);
desc tb1_user; -- 查看表结构

drop table tb1_user; -- 删除表
drop table if exists tb1_user; -- 删除表 先判断表是否存在

-- 修改表
alter table tb1_user rename to tb2_user; -- 修改表名
alter table tb1_user add address varchar(50); -- 添加名为address的列
alter table tb1_user modify address char(50); -- 修改address列的数据类型
alter table tb1_user change address addr varchar(30); -- 同时修改addrees的列名和数据类型
alter table tb1_user drop addr; -- 删除addr列


/* DML */
-- 给指定列添加数据 INSERT INTO 表名(列名1，列名2...) VALUES(值1，值2...);
INSERT INTO user(id, name) VALUES(1, '张三');

-- 给全部列添加数据 可省略列名（实际开发不建议省略列名）
INSERT INTO user VALUES(1,'王五','男', '1999-11-11', 20)；

-- 批量添加数据
-- INSERT INTO 表名(列名1，列名2) VALUES(值1，值2...),(值1，值2...),(值1，值2...)...;
INSERT INTO user(id, name) VALUES(1,'张三'),(2,'王五'),(3,'赵六');
-- INSERT INTO 表名 VALUES(值1，值2...),(值1，值2...),(值1，值2...)...;
INSERT INTO user VALUES(1,'王五','男', '1999-11-11', 20),(2,'赵六','男', '1997-02-03', 20);

-- 修改数据 UPDATE表名 SET 列名1=值1，列名2=值2...[WHERE 条件]; 不加条件则所有数据都会修改
UPDATE user SET name='david' where name = '张三';
UPDATE user SET birthday = '2000-11-11' where name = 'david';

-- 删除数据
DELETE from user where name = '李四';


/* DQL */
/*
 语法：
 SELECT 字段列表
 FROM 表名列表
 WHERE 条件列表
 GROUP BY 分组字段
 HAVING 分组后条件
 ORDER BY 排序字段
 LIMIT 分页限定
*/
select name,age from stu; -- 查询name,age列数据， 项目不建议直接用*查所有列数据，不符合代码规范
select DISTINCT address from stu -- 查询学生的的地址，DISTINCT去除重复记录
select math AS 数学,english AS 英语 from  stu; -- AS 字段起别名

-- 条件查询
select * from stu where age > 20; -- 查询年龄大于20的学生
select * from stu where age >=20 and age <= 30; --写法1：查询年龄20~30之间的学生
select * from stu where age BETWEEN 20 and 30 -- 写法2：between关键词，查询年龄20~30之间的学生
select * from stu where age = 20; --年龄等于20的学生， sql不要用==, 和java有区别
select * from stu where age = 18 or age = 20 or age = 22; -- 查询年龄是18或20或22的学生
select * from stu where age in(18,20,22); --写法2：in关键词
select * from stu where age is null; --查询年龄是null的学生,null值比较要用is，不能用=

-- 模糊查询like, 通配符 _：匹配任意单个字符，%: 匹配任意多个字符
select * from stu where name like '顾%'; -- 查询姓顾的学生
select * from stu where nam like '_云%'; -- 查询第二个字是“云”的学生
select * from stu where name like '%德%'; -- 查询名字中含“德”的学生（最常用）

-- 排序查询 select 字段列表 from 表名 order by 排序字段名1 [排序方式1], 排序字段名2 [排序方式2]...;
-- ASC: 升序排列(默认，可不写)
-- DESC:降序排列
select * from stu order by math desc; -- 按数学成绩降序排列
-- 多条件排序，第一个条件值一样时，第二个条件才会启用
select * from stu order by math desc, english asc; -- 按数学降序排列，如果数学成绩一样，再按英语成绩升序排列

-- 分组查询
/* 
	聚合函数：将一列数据作为一个整体，进行纵向计算。null值不参与聚合函数运算
	聚合函数分类： count(列名)-统计数量， max(列名)-最大值，min(列名)-最小值, sum(列名)-求和, avg(列名)-平均值
	聚合函数语法：select 聚合函数名(列名) from 表； 
*/
select count(id) from stu; -- 统计有多少个学生，传进去的列名不能为空，不会统计列为null的数据，最好传主键或*(星号表示一行只要有一个不为空就会统计)
select max(math) from stu; -- 查询数学成绩最高分
select min(english) from stu; -- 查询英语成绩最低分，如果有null,不参与计算，只会统计有值的最低值

--分组查询语法：select 字段列表 from 表名 [where 分组前条件限定] group by 分组字段名 [having 分组后条件过滤]；
select sex, avg(math) from stu group by sex; -- 查询男生和女生各自的数学平均分
select sex, avg(math), count(*) from stu group by sex; -- 查询男生和女生各自数学平均分和各自人数
select sex, avg(math), count(*) from stu where math > 70 group by sex; -- 前置条件：分组前分数大于70才参与聚合函数计算和分组
select sex, avg(math), count(*) from stu where math > 70 group by sex having count(*) > 2; -- 后置条件：展示分组后数量大于2的

-- 分页查询
-- 分页查询语法：select 字段列表 from 表名 LIMIT 起始索引, 查询条目数;
-- 公式：起始索引= (当前页码 - 1) * 每页条数  （起始索引从0开始）
select * from stu limit 0, 10; -- 从0开始查询，查询10条数据
select * from stu limit 20, 10; -- 查询第3页数据
```



### 约束

概念：作用于**表中列上**的规则，用于限制加入表的数据，约束的存在保证了数据库中数据的正确性、有效性和完整性。

分类：

1.非空约束：保证列中所有数据不能用NULL值，关键字：NOT NULL

2.唯一约束：保证列中所有数据各不相同，关键字：UNIQUE

3.主键约束：主键是一行数据的唯一标识，要求非空且唯一，一张表只能有一个主键。关键字：PRIMARY KEY

4.检查约束：保证列中的值满足某一条件 关键字：CHECK （MySQL不支持该约束）

5.默认约束：保存数据时，未指定值采用默认值 关键字：DEFAULT

6.外键约束：外键用来让来个表的数据之间建立连接，保证数据的一致性和完整性 关键字：FOREIGN KEY

```mysql
-- 建表并添加约束
CREATE TABLE emp (
	id INT PRIMARY KEY auto_increment, -- 主键，且自增(必须是数字类型，且唯一约束可使用自增)
    ename VARCHAR(50) NOT NULL UNIQUE, -- 姓名，非空且唯一
    joindate DATE NOT NULL, -- 入职日期，非空
    salary DOUBLE(7,2) NOT NULL, -- 工资，非空
    bonus DOUBLE(7,2) DEFAULT 0 -- 奖金，没有默认为0
)
-- 插数据
INSERT INTO emp(id,ename,joindate,salary,bonus) VALUES(1,'大卫','2021-10-10',8800,5000);
INSERT INTO emp(ename,joindate,salary,bonus) VALUES('小王','1999-11-11',5000,5000); -- 不写主键，自增

-- 建完表后添加约束语法: alter table 表名 MODIFY 字段名 数据类型 NOT NULL;
```

#### 外键约束

创建表时外键语法：[CONSTRAINT] [外键名称] FOREIGN KEY(外键列名) REFERENCES 主表(主表列名)

建完表后添加外键语法： ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键列名)  REFERENCES 主表名称(主表列名)

删除外键约束： ALTER TABLE 表名 DROP FOREIGN KEY 外键名称

```mysql
-- dept部门表 细节：先创建外键关联的主表，再创建员工表
create table dept(
	id INT PRIMARY KEY auto_increment,	
    dep_name VARCHAR(20) NOT NULL UNIQUE,
    addr VARCHAR(50) NOT NULL
)
insert into dept(dep_name, addr) values('研发部', '杭州'),('销售部', '上海'); -- 若部门存在关联的员工，直接删除部门会报错，因为外键存在

-- emp员工表
create table emp(
	id INT PRIMARY KEY auto_increment,
    name VARCHAR(50) NOT NULL,
    age INT,
    dep_id INT NOT NULL
    -- 创建表时添加外键， 外键字段dep_id关联主表字段id
    CONSTRAINT fk_emp_dept FOREIGN KEY(dep_id) REFERENCES dept(id);
)
insert into emp(name,age,dep_id) values('张三', 20, 1),('李四', 25, 2),('大卫', 35, 1);
```



### 数据库设计

软件开发步骤：需求分析--设计--编码--测试--安装部署

设计又分为：软件结构设计，数据库设计，接口设计，过程设计

数据库设计概念：根据业务具体需求结合所选DBMS，为业务系统构造出最优的数据存储模型，**建立数据库中的表结构以及表和表之间的关联关系的过程**

数据库逻辑建模：ER图(实体-关系图)  

如论坛用户是一个实体，用户有积分、性别、等级等各种属性；

板块也是一个实体，有板块名称、发帖数等属性； 

用户和板块之间存在“管理”的关系



![image-20230815103729007](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230815103729007.png)



#### 表关系

1、一对一

用户和用户详情（多用于表拆分）

比如一个用户常用的用户字段放到A表，用户的详情字段放B表，查询时查A表提升查询效率

**实现方式：在任意一方加入外键，关联另一方主键，并且设置外键为唯一（UNIQUE）**



2、一对多（多对一）

部门和员工

**实现方式：多的一方（员工）建立外键，指向一的一方（部门）的主键**



3、多对多

商品和订单；一个商品对应多个订单，一个订单含多个商品

**实现方式：建立第3张中间表，中间表至少包含2个外键，分别关联两方的主键**

中间表order_id关联订单表主键，goods_id关联商品表主键，且一个order_id对应了多个goods_id，一个goods_id也对应了多个订单

| id   | order_id | goods_id | count |
| ---- | -------- | -------- | ----- |
| 1    | 1        | 1        | 2     |
| 2    | 1        | 3        | 1     |
| 3    | 2        | 1        | 5     |

```sql
-- 订单表
create table tb_order(
	id int PRIMARY KEY auto_increment,
	payment DOUBLE(8,2),
	payment_type varchar(20),
	payment_status int
)
-- 商品表
create table tb_goods(
	id int PRIMARY KEY auto_increment,
	title varchar(50) NOT null,
	price double(8,2) DEFAULT 0
);
-- 中间表
create table tb_order_goods(
	id int PRIMARY KEY auto_increment,
	order_id int,
	goods_id int,
	count int
);

-- 建表完成后，给中间表添加外键
ALTER TABLE tb_order_goods add CONSTRAINT fk_order_id FOREIGN KEY(order_id) REFERENCES tb_order(id);
ALTER TABLE tb_order_goods add CONSTRAINT fk_goods_id FOREIGN KEY(goods_id) REFERENCES tb_goods(id);
```





#### 多表查询

分类：1.内连接查询、2.外连接查询、3.子查询

##### 内外连接查询

```sql
-- 查询用户表和部门表，这种查询方式会产生笛卡尔积，即把两张表的所有数据排列组合，返回所有的组合情况，产生很多无效数据
select * from emp,dept

/* 
内连接查询查询：AB表的交集部分
内连接语法： 
	隐式内连接： select 字段列表 from 表1,表2...where 条件 
	显式内连接： select 字段列表 from 表1 [inner] join 表2 on 条件
*/
select * from emp,dept where emp.dept_id = dept.id; --查出员工和所属部门，不能查出没有部门的员工和没有员工的部门
select * from emp join dept on where emp.dept_id = dept.id;

/*
外连接查询
左外连接： 查询左表所有数据和交集部分的数据。语法：select 字段列表 from 表1 left [outer] join 表2 on 条件；
右外连接：查询右表所有数据和交集部分数据。语法： select 字段列表 from 表1 right [outer] join 表2 on 条件;
*/

select * from emp left join dept on emp.dept_id = dept.id; --查出员工及所属部门，以及没有部门的员工
select * from emp right join dept on emp.dept_id = dept.id;--查出员工及所属部门，以及没有员工的部门

```

##### 子查询

子查询也叫嵌套查询

```sql
--根据查询结果不同，作用不同
-- 单行单列：作为条件值，使用=,!=,>,<进行条件判断 
-- select 字段列表 from 表 where 字段名 = (子查询)
--需求：查询工资高于大卫的员工 ， 先查大卫的工资，再查高于大卫工资的员工， 把第一条查询直接放在()中嵌套查询；工资和条件都是1个，所以是单行单列
select * from emp where salary > (select salary from emp where name = '大卫');

-- 多行单列：作为条件值，使用in 等关键字极性条件判断
-- select 字段列表 from 表 where 字段名 in (子查询)
--需求：查询财务部和市场部全部员工信息
--思路：先查财务部和市场部的id, 再根据id查所有员工信息 部门有好几个，id是一个字段，所以是多行单列
select * from emp where dept_id in (select id from dept where name = '财务部' or name = '市场部');

-- 多行多列：作为虚拟表
-- select 字段列表 from (子查询) where 条件 
--需求：查询入职日期是‘2011-11-11’后的员工信息和部门信息。
--思路：子查询作为一个虚拟表，和dept表组成内连接查询。细节：给虚拟表起别名t1
select * from (select * from emp where joindate > '2011-11-11') as t1,dept where t1.dept_id = dept.id;
```





### 事务

概念：

数据库的事务（Transcation）是一种机制，**包含了一组数据库操作命令**

事务把所有命令作为一个整体一起向系统提交，即一组数据库命令**要么同时成功，要么同时失败**

事务是一个不可分割的工作逻辑单元

例子：A向B转账500，具体步骤就是A账户-500，B账户+500，若A-500后，B+500出错，那么金额总数就异常了。

此时用事务操作，若中间产生异常，整个操作都会回滚，不会被提交。

```sql
--显式的开启事务
BEGIN;
-- 大卫和比利都有1000元，大卫向比利转账500
UPDATE account set money = money - 500 where name = '大卫';
UPDATE account set money = money + 500 where name = '比利';

--提交事务 此时数据库才会持久化更改
COMMIT;
--回滚事务
ROLLBACK;
```

事务的4大特征：

原子性（Atomicity）:事务是不可分割的最小操作单位，要么同时成功要么同时失败

一致性（Consistency）: 事务完成时，必须使所有的数据都保持一致状态

隔离性（Isolation）: 多个事务之间，操作的可见性(事务提交或回滚后，其他人的查询才能看到状态的修改)

持久性（Duration）: 事务一旦提交或回滚，它对数据库中数据的改变都是永久的

PS: mysql 的事务默认是自动提交，在执行sql后会自动执行commit（显式开启事务后需要手动提交），oracle则都是手动提交





## JDBC

JDBC(Java DataBase Connectivity)：java数据库连接，使用JAVA操作关系型数据库的一套API

mysql, oracle等数据库厂商分别定义自己的实现类，实现JDBC接口，并提供数据库jar包（也叫驱动）

核心流程：

```java
package com.david.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class demo01 {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        // 0. 先把数据库驱动Jar包导入到同一个模块中
        
        // 1.注册驱动  利用反射：将驱动类加载到内存中
        Class.forName("com.mysql.jdbc.Driver");
        // 2.获取连接对象
        String url = "jdbc:mysql://127.0.0.1:3306/david_db1?useSSL=false";
        String username = "root";
        String password = "123456";
        // 创建连接
        Connection conn = DriverManager.getConnection(url, username, password);

        // 3.定义要执行的SQL
        String sql = "INSERT INTO stu(name,gender) VALUES('里昂', '男')";

        // 4.获取执行sql的对象
        Statement statement = conn.createStatement();
        // 5.执行sql 返回值是执行成功的行数
        int count = statement.executeUpdate(sql);
        // 6.处理结果
        System.out.println(count);
        // 7.释放资源 后开的先释放
        statement.close();;
        conn.close();
    }
}
```

### DriverManager（工具类）

作用：

1. 注册驱动 

   DriverManager.reigisterDriver，当调用Class.forName("com.mysql.jdbc.Driver")加载类时，源码中的静态代码块内会执行DriverManager.reigisterDriver注册驱动

   提示：MySQL 5之后的驱动包，可以省略注册驱动这一步，自动加载jar包中META-INF/services/java.sql.Driver文件中的驱动类

2. 获取数据库连接（静态方法getConnection）



### Connection(接口)

作用：

1.获取执行SQL的对象

​	普通执行SQL对象：createStatement()

​	预编译SQL的执行SQL对象：防止SQL注入 prepareStatement(sql)

​	执行存储过程的对象：prepareCall(sql)



2.管理事务

​	jdbc事务管理，Connection接口中定了3个对应的方法

​	开启事务：setAutoCommit(boolean autoCommit)  true:自动提交 false:手动提交，此处传false,即开启了事务

​	提交事务：commit()

​	回滚事务：rollback()

```java
package com.david.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class demo02shiwu {
    public static void main(String[] args) throws SQLException, ClassNotFoundException {
        // 1.注册驱动  利用反射：将驱动类加载到内存中
        Class.forName("com.mysql.jdbc.Driver");
        // 2.获取连接对象
        String url = "jdbc:mysql://127.0.0.1:3306/david_db1?useSSL=false";
        String username = "root";
        String password = "123456";
        // 创建连接
        Connection conn = DriverManager.getConnection(url, username, password);

        // 3.定义要执行的SQL
        String sql1 = "INSERT INTO stu(name,gender) VALUES('本阿弗莱克', '男')";
        String sql2 = "INSERT INTO stu(name, gender) VALUES('莱昂纳多', '男')";
        // 4.获取执行sql的对象
        // 用事务提交sql代码， 把要执行的代码放到try/catch中
        try {
            conn.setAutoCommit(false); // false, 手动提交，即开启了事务
            Statement statement = conn.createStatement();
            // 执行sql
            int count = statement.executeUpdate(sql1);
            System.out.println("sql1成功行数：" + count);
            // 制造一个异常，sql2就不会执行
//            int num = 100 / 0;
            int count2 = statement.executeUpdate(sql2);
            System.out.println("sql2成功行数：" + count2);
            conn.commit(); // 提交事务
            // 释放资源 后开的先释放
            statement.close();
            conn.close();
        } catch (SQLException e) {
            // 发生错误， 回滚事务
            conn.rollback();
            throw new RuntimeException(e);
        }
    }
}

```



### Statement

作用：执行SQL语句

​	执行DML,DDL： **int executeUpdate(sql)**    DML返回值是影响的行数，DDL执行成功也可能返回0 (删除表、删除库返回0)

​	执行DQL：**ResultSet executeQuery(sql) **   返回值ResultSet结果集对象



**ResultSet: 返回了查询结果的集合**

结果集默认有一个指针，指向当前结果集的表头行

方法：

​	boolean next()： 1）将指针向下移动一行。2）判断当前和是否有数据  返回true|false

​	xxx getXxx(参数)：获取数据

​	xxx: 数据类型，如 int getInt(参数)，String getString(参数)

​	参数有2种重载方式： 

​		int: 列的编号，**从1开始**

​		String：列的名称，如列名是name 则传"name"



### DQL查询数据

```java
package com.david.jdbc;

import java.sql.*;
import java.util.ArrayList;

public class demo03ResultSet {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        /**
         * 需求：查询学生表数据，封装到stu对象，并存入ArrayList集合
         * */
        Class.forName("com.mysql.jdbc.Driver");
        String url = "jdbc:mysql://127.0.0.1:3306/david_db1?useSSL=false";
        String username = "root";
        String password = "123456";
        // 创建连接
        Connection conn = DriverManager.getConnection(url, username, password);

        // 定义DQL语句
        String sql = "select * from stu";

        // 获取执行sql对象
        Statement statement = conn.createStatement();
        // executeQuery方法获取DQL的返回值，返回对象有next()和getXxx()方法
        ResultSet resultSet = statement.executeQuery(sql);

        ArrayList<Student> stuList = new ArrayList<>(); // 保存stu的集合
        // 循环获取返回结果
        while (resultSet.next()) {
            String name = resultSet.getString("name");
            String gender = resultSet.getString("gender");
            int age = resultSet.getInt("age");
            Student stu = new Student();
            stu.setName(name);
            stu.setAge(age);
            stu.setGender(gender);
            // stu对象放到集合中
            stuList.add(stu);
        }
        System.out.println(stuList);
        // 记得释放资源！
        resultSet.close();
        statement.close();
        conn.close();
    }
}
// 创建一个student类存放获取到的表数据
class Student {
    private String name;
    private String gender;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", gender='" + gender + '\'' +
                ", age=" + age +
                '}';
    }
}
```





### SQL注入

sql注入本质上就是拼接sql字符串，改变sql语意

```java
String name = "asdasd";
String gender = "男";
String gender2 = "'or'1' = '1"; // SQL注入
String sql = "select * from stu where name = '"+name+"' and gender = '"+gender+"'";
String sql2 = "select * from stu where name = '"+name+"' and gender = '"+gender2+"'";
System.out.println(sql); // select * from stu where name = 'asdasd' and gender = '男'
System.out.println(sql2);// select * from stu where name = 'asdasd' and gender = ''or'1' = '1' 这条sql会查出所有数据
```



### PrepareStatement

是一个接口，继承自statement

作用：

1.预编译SQL语句并执行，性能更高（**预编译默认关闭，在url后拼接开启：useServerPrepStmts=true**）

底层：

​	java把sql传给mySql服务器，数据库执行的流程分为1.检查SQL语法--》2.编译SQL--》3.执行SQL，前两步是最耗性能的

​	java在执行conn.prepareStatement(sql) 这段代码时，已经把sql传给了mySql服务器，

​	设置变量的值时，数据库只需执行第3步，不需要再执行前面2步，性能更快

2.解决SQL注入的问题，本质是转义了传入的变量，不会把单引号拼接到查询语句，而是作为字符串处理



**MySql添加日志：**

在my.ini配置文件中添加配置（

log-output=FILE 

general-log=1 

general_log_file="D:\mysql.log"

slow-query-log=1

slow_query_log_file="D:\mysql_slow_log"

long_query_time=2

），重启服务后生成2个日志文件

执行sql操作后日志文件会自动打印

```java
package com.david.jdbc;

import java.sql.*;

public class demo05PrepareStatement {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        Class.forName("com.mysql.jdbc.Driver");
        String url = "jdbc:mysql://127.0.0.1:3306/david_db1?useSSL=false";
        String username = "root";
        String password = "123456";
        // 创建连接
        Connection conn = DriverManager.getConnection(url, username, password);

        // 创建prepareStatement对象，预编译sql,需要传入sql
        String name = "kashin";
        String gender = "男";
        // sql内的变量用？作为占位符代替
        String sql = "select * from stu where name = ? and gender = ?";
        PreparedStatement pstmt = conn.prepareStatement(sql);
        // 设置变量的值 参数1：第几个? 参数2：要设置的值
        pstmt.setString(1, name);
        pstmt.setString(2, gender);
        // 执行sql, 不用再传sql
        ResultSet rs = pstmt.executeQuery();
        while (rs.next()) {
            System.out.println(rs.getString("name"));
            System.out.println(rs.getString("gender"));
        }
        rs.close();
        pstmt.close();
        conn.close();
    }
}
```



### 数据库连接池

数据库连接池是一个容器，负责分配、管理数据库连接（Connection）； 逻辑和线程池类似

运行应用程序重复使用一个现有的数据库连接，而不是再新建一个；

释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏

好处：资源重用、提升响应速度、避免数据库连接遗漏



标准接口：**DataSource**

官方提供的数据库连接池标准接口，由第三方实现此接口。

功能：获取连接

```java
Connection getConnection()
```

常见的数据库连接池：DBCP，C3P0，Druid

Druid是阿里巴巴开源的数据库连接池项目，是java最好的数据库连接池之一。



#### Druid使用

1.导入jar包druid-1.1.12.jar

2.定义配置文件

```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/david_db1?useSSL=false&useServerPrepStmts=true
username=root
password=123456
# 初始化连接数
initialSize=5
# 最大连接数
maxActive=10
# 最大等待时间
maxWait=3000
```

```java
public class demo06Druid {
    public static void main(String[] args) throws Exception {
        // 1.加载druid.jar包
        // 2.定义properties配置文件 包含了数据库地址，用户名密码，线程池连接数等配置

        // 3.加载配置文件 用properties对象的load方法加载
        Properties prop = new Properties();
        prop.load(new FileInputStream("jdbc-demo\\druid.properties"));
        // 4.获取连接池对象
        DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

        // 5.获取数据库连接，这里开始和普通的数据库连接一样
        Connection conn = dataSource.getConnection();
        System.out.println(conn);
        System.out.println(System.getProperty("user.dir")); // C:\Users\yoki\IdeaProjects\base-practise 获取当前项目完整路径
    }
}
```



#### 增删改查练习

```java
package com.david.jdbc;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.Properties;

public class demo07CRUD {
    public static void main(String[] args) throws Exception {
        // 3.加载配置文件 用properties对象的load方法加载
        Properties prop = new Properties();
        prop.load(new FileInputStream("jdbc-demo\\druid.properties"));
        // 4.获取数据库连接池对象
        DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

        // 5.获取数据库连接，这里开始和普通的数据库连接一样
        Connection conn = dataSource.getConnection();

        // 查
		String sql = "select * from stu";
        PreparedStatement pstmt = conn.prepareStatement(sql);
        ResultSet rs = pstmt.executeQuery();
        ArrayList<Student2> list = new ArrayList<>();
        while (rs.next()) {
            String name = rs.getString("name");
            String gender = rs.getString("gender");
            int age = rs.getInt("age");
            Student2 student2 = new Student2();
            student2.setAge(age);
            student2.setName(name);
            student2.setGender(gender);
            list.add(student2);
        }
        rs.close();
        pstmt.close();
        
        // 增
        String sql2 = "insert into stu(name,gender,age) values(?,?,?)"; // 预编译占位符
        PreparedStatement pstmt2 = conn.prepareStatement(sql2);
        pstmt2.setString(1, "顾凡");
        pstmt2.setString(2, "男");
        pstmt2.setInt(3, 42);
        int count = pstmt2.executeUpdate();
        System.out.println(count);
        if (count > 0) {
            System.out.println("增加成功");
        } else {
            System.out.println("增加失败");
        }
        pstmt2.close();
        conn.close();
    }
}
class Student2 {
    private String name;
    private String gender;
    private Integer age; // 实体类最好用包装类，因为int类有默认值0，在数据库中会产生歧义

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", gender='" + gender + '\'' +
                ", age=" + age +
                '}';
    }
}

```







## Maven

专门用来管理和构建JAVA项目的工具，基于项目对象模型（POM：project,object,module）

标准化项目结构，例如ecplise和idea创建的项目结构是不同的，通过Maven创建统一的项目结构，可以通用

提供了标准化构建流程（编译，测试，打包，发布）

提供了依赖管理机制（jar包，插件） 通过Pom.xml导入

maven的jar包和插件有3个仓库分为：1.本地仓库，2.中央仓库（maven维护），3.远程仓库(私服，公司团队搭建的私有仓库)

jar包依赖流程：先找本地仓库有没有，如果搭建了私服，就去私服找，没有再去中央仓库找，找到后下载到私服，再传给本地。



### maven配置

1.解压包

2.配置环境变量MAVEN_HOME

3.配置本地仓库，修改conf/settings.xml中<localRepository>为一个指定目录（可选，默认会配在C盘用户目录下）

4.配置阿里云私服，修改conf/settings.xml中<mirrors>标签，添加子标签（可选，下载依赖更快）

![image-20230817172951490](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230817172951490.png)

**IDEA配置MAVEN**

1.选择file--->settings

2.搜索maven

3.设置IDEA使用本地安装maven，并修改文件配置路径，即上面的settings.xml文件（IDEA默认集成了maven）

**创建MAVEN项目**

创建模块，选择Maven,点击next

填写模块名称，坐标信息，点击finish，完成



**MAVEN坐标**

坐标是资源的唯一标识，使用坐标来定义和引入依赖，由3部分组成：

groupId：定义当前maven项目隶属组织的名称（通常域名反写，com.xxx）

atrifactId：定义当前maven项目名称（通常为模块名称）

version：定义当前项目版本号

备注：

1.可以在mvn repository官网内查看配置写法

2.快捷键：alt+insert  选择dependency快速写配置导jar包

```xml
<!--在POM.mxl文件内填写：导入mysql jar包， 配置后记得点击右上角load changes下载依赖-->
<dependencies>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.34</version>
        <!-- 依赖的有效范围, 3个环境 编译，测试，运行 -->
        <scope>test</scope>
    </dependency>
</dependencies>
```

**依赖有效范围： 默认值compile，3个环境都有效**

![image-20230821150240468](C:\Users\yoki\AppData\Roaming\Typora\typora-user-images\image-20230821150240468.png)





**常用命令**

compile：编译

clean：清理

test：测试 ，使用合适的单元测试框架运行测试（Junit是其中之一）

package：打包，将编译后的代码打包成jar文件

install：安装，安装项目包到本地仓库，这样项目包可以用作其他本地项目的依赖





## MyBatis

简介：MyBatis是一个持久层框架，用于简化JDBC开发。 官网：https://mybatis.org/mybatis-3/zh/index.html

JAVA经典三层结构：表现层、业务层、持久层，持久层：将数据保存到数据库的那一层代码。

简化了什么： 

1、将JDBC一些硬编码的代码抽取到配置文件中

2、手动设置sql参数和获得结果集封装到一个方法中

```java
// 一行代码获取sql查询结果，并封装到students集合中
List<Student> students = sqlSession.selectList("test.selectByGender", "男");
```



### mybatis流程

**两个XML配置文件，都放在resource文件夹下**

```xml
<!-- userMapper sql映射语句配置文件 -->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace: 命名空间，可以理解成一个模块, 外部调用使用test.selectAll-->
<mapper namespace="test">
    <select id="selectAll" resultType="com.david.Pojo.User">
        select * from tb_mybatis_users
    </select>
</mapper>


<!-- mybatis-config.xml 核心配置文件 -->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
			<!--数据库连接配置-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://127.0.0.1:3306/david_db1?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="UserMapper.xml"/>
    </mappers>
</configuration>
```

**执行mybatis核心代码**

```java
package com.david;

import com.david.Pojo.User; // 自定义User类
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class demo1 {
    public static void main(String[] args) throws IOException {
        // 1.加载mybatis核心配置文件 放在resources根目录下，直接写文件名即可
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        // 2.获取sqlSession对象，用它来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();
        // 3.执行sql, 传入要执行的映射语句，获取查询返回的结果集，封装到list集合中
        List<User> list = sqlSession.selectList("test.selectAll");
        System.out.println(list);

        // 释放资源
        sqlSession.close();
    }
}

```



### mapper代理方式

1.定义和sql映射文件同名的Mapper接口，并把接口和映射文件放在同一目录下

​	java文件夹下新建com.david.mapper包，新建UserMapper接口

​	resources文件夹下新建com/david/mapper/UserMapper.xml 配置文件，编译后就在同一个文件夹下了



2.设置sql映射文件的nameSpace书写为mapper接口全限定名

3.在mapper接口中定义方法，方法名就是sql映射文件中sql语句的id，并保持参数类型和返回值一致

4.代码部分：通过sqlSession.getMapper方法获取mapper接口的代理对象，调用对应方法完成sql执行

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace: mapper代理方式，命名空间必须是接口的全限定名-->
<mapper namespace="com.david.mapper.UserMapper">
    <select id="selectAll" resultType="com.david.Pojo.User">
        select * from tb_mybatis_users
    </select>
</mapper>
```

```java
package com.david.mapper;

import com.david.Pojo.User;
import java.util.List;

public interface UserMapper {
    List<User> selectAll(); // 方法名和mapper配置文件中的id名一致
}

```

```java
package com.david;

import com.david.Pojo.User;
import com.david.mapper.UserMapper;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class demo1 {
    public static void main(String[] args) throws IOException {
        // 1.加载mybatis核心配置文件
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        // 2.获取sqlSession对象，用它来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();

        /**
        	用mapper代理方式执行mybatis
        	优化了这种传字符串的硬编码：List<User> list = sqlSession.selectList("test.selectAll")
        */ 
        // 获取mapper代理对象
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> userList = mapper.selectAll();// 执行接口同名方法，会自动去执行mapper配置文件中id对应的sql语句
        System.out.println(userList);
// 释放资源
        sqlSession.close();
    }
}

```



### 核心配置文件解释

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    
    <!-- 别名配置 配置包扫描的路径，在sqlMapper文件内的resultType就不用再写包名+类名的格式，直接写类名就行，并且不区分大小写 -->
    <typeAliases>
    	<package name="com.david.Pojo"/>
    </typeAliases>
    
    <!--默认环境配置，可以配置多个environment 方便切换测试库/生产库 -->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
			<!--数据库连接配置-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://127.0.0.1:3306/david_db1?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="com/david/mapper/UserMapper.xml"/>
        
        <!--如果接口文件和映射文件名称一致，可以用包扫描方式加载所有映射文件，省去了路径书写-->
        <package name="com.david.mapper" />
    </mappers>
</configuration>
```



### mybatis细节

1.查询的sql列名和实体类的属性名不一致时，sql语句可以用as 关键词修改别名保持一致

2.定义sql片段，执行相同的sql查询条件

3.**resultMap**映射列名和类的属性名（最常用）

```xml
<!-- 映射文件  数据库表字段名和实体类字段名不一致的处理方式，两种：sql片段起别名，map映射 -->
<mapper namespace="com.david.mapper.StuMapper">
    <!--方式1：sql片段-->
    <sql id="user_column">
    	id, xx_name as name, xx_gender as gender 
    </sql>
    <select id="selectAll" resultType="com.david.Pojo.Student">
        <!--通过sql片段查询-->
        select <include refid="user_column" /> 
        from tb_user
    </select>
    
    <!-- 方式2：通过map映射字段 -->
    <resultMap id="userResultMap" type="user">
        <!--column就是数据库字段名，property就是实体类属性名-->
    	<result column="xx_name" property="name"/>
        <result column="xx_gender" property="gender"/>
    </resultMap>
    
    <!--注意这里要改成resultMap-->
    <select id="selectAll" resultMap="userResultMap">
    	select * from tb_user
    </select>
</mapper>
```



### mybatis基本查询

传参查询，使用参数占位符：#{}   会被替换成 ?

动态查询表名，拼接sql：使用${}  

注意点：sql 查询使用 < 号这种和xml符号冲突的

1.使用转义字符\&lt;

2.使用CDATA区 格式：<![CDATA[内容]]>

```xml
<select id="selectByName">
    <!--转义字符-->
	select * from tb_user where id &lt; #{id};
    
    <!--CDATA区  符号写在CDATA区中 字符很多时可以用这种-->
    select * from tb_user where id
    <![CDATA[
		<
	]]>
</select>
```

```java
public class batisTest {
	// test注解需要Junit依赖
    @Test
    public void testSelectAll() throws IOException {
        // 读取配置文件
        InputStream resourceAsStream = Resources.getResourceAsStream("Mybatis-config.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        // 创建sql连接
        SqlSession sqlSession = sqlSessionFactory.openSession();
        // 获取映射
        StuMapper stuMapper = sqlSession.getMapper(StuMapper.class);

        // 执行sql
        List<Student> studentList = stuMapper.selectAll();
        System.out.println(studentList);

        // 释放资源
        sqlSession.close();
    }
    @Test
    public void testSelectByName () throws IOException {
        // 读取配置文件
        InputStream resourceAsStream = Resources.getResourceAsStream("Mybatis-config.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        // 创建sql连接
        SqlSession sqlSession = sqlSessionFactory.openSession();
        // 获取映射
        StuMapper stuMapper = sqlSession.getMapper(StuMapper.class);

        // 执行sql
        Student stu = stuMapper.selectByName("里昂");
        System.out.println(stu);

        // 释放资源
        sqlSession.close();
    }
}

// 接口文件
package com.david.mapper;
import com.david.Pojo.Student;
import java.util.List;

public interface StuMapper {
    public List<Student> selectAll();
    public Student selectByName(String name);
}
```

```xml
<!--映射文件-->
<mapper namespace="com.david.mapper.StuMapper">
    <select id="selectAll" resultType="com.david.Pojo.Student">
        select * from stu
    </select>
    <select id="selectByName" resultType="com.david.Pojo.Student">
            <!-- 根据姓名查询对象， 传参数占位符    -->
        select * from stu where name = #{name};
    </select>
</mapper>
```



### mybatis多条件查询

#### 接口传递多个参数 

目的：让接口参数和sql中查询的字段一一对应

1、通过接口注解的方式

2、把参数封装到一个对象中，对象的属性名和sql查询字段一致

3、把参数封装到map集合中， map的key和sql查询字段一致

```java
// 多个参数时，接口3种写法
// 参数注解，参数名必须和占位符字段名一致
public List<Student> selectByCondition(@Param("name")String name, @Param("gender")String gender);
// 封装对象
public List<Student> selectByCondition(Student student);
// 封装map集合
public List<Student> selectByCondition(Map map);
```

```xml
<!--sql映射文件-->
<!-- 多条件查询 -->
<select id="selectByCondition" resultType="com.david.Pojo.Student">
    select * from stu
    where name like #{name} <!--接口的字段名和占位符字段名必须一致-->
    and gender = #{gender};
</select>
```

```java
@Test
public void testSelectByCondition() throws IOException {
    InputStream rs = Resources.getResourceAsStream("Mybatis-config.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(rs);
    // 创建sql连接
    SqlSession sqlSession = sqlSessionFactory.openSession();
    // 获取映射
    StuMapper stuMapper = sqlSession.getMapper(StuMapper.class);

    String name = "昂";
    String gender = "男";
    // 因为name是like查询，要对字符串做模糊处理
    String fname = "%" + name + "%";
    // 执行sql
    // 1.参数注解方式
//        List<Student> studentList = stuMapper.selectByCondition(fname, gender);

    // 2.封装对象方式
//        Student student = new Student();
//        student.setName(fname);
//        student.setGender(gender);
//        List<Student> studentList = stuMapper.selectByCondition(student);

    // 3.封装map集合方式
    HashMap<String, String> hm = new HashMap<>();
    hm.put("name", fname);
    hm.put("gender", gender);
    List<Student> studentList = stuMapper.selectByCondition(hm);
    System.out.println(studentList);

    // 释放资源
    sqlSession.close();
}
```



### 动态SQL

不固定查询条件，根据用户输入的查询条件动态变化

mybatis支持的动态SQL标签

**if**

**choose(when, otherwise)**

**trim(where, set)**

**foreach**

```xml
<!--改写上面的SQL，变成动态SQL-->
<!--多条件动态查询-->
<select id="selectByCondition" resultType="com.david.Pojo.Student">
    select * from stu
    <!--where包裹条件，这样当第一个条件为空时，sql就不会出现where and xxx这种语法错误，where内自动去掉and-->
    <where>
        <!--test内写逻辑判断-->
        <if test="name != null and name != ''">
            name like #{name}
        </if>
        <if test="gender != null and gender != ''">
            and gender = #{gender};
        </if>
    </where>
</select>


<!--单条件动态查询 前端通过单选一个条件后进行查询的场景-->
<select id="selectByCondition" resultType="com.david.Pojo.Student">
    select * from stu
    <where>
        <!--类似switch-->
        <choose>  
            <!--类似case-->
            <when test="name != null and name != ''">  
                name like #{name}
            </when>
            <when test="gender != null and gender != ''">
                gender = #{gender}
            </when>
             <!--类似default，用户什么都没传的情况； 用where包裹的话可以不写-->
            <otherwise> 
                1 = 1
            </otherwise>
        </choose>
    </where>
</select>
```



### mybatis添加

关键点：useGeneratedKeys="true" keyProperty="id"  获取数据生成的主键

```java
@Test
public void testAdd() throws IOException {
    InputStream rs = Resources.getResourceAsStream("Mybatis-config.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(rs);
    // 创建sql连接
    SqlSession sqlSession = sqlSessionFactory.openSession();
    // 获取映射
    StuMapper stuMapper = sqlSession.getMapper(StuMapper.class);

    String name = "梅艳芳";
    String gender = "女";
    Integer age = 39;
    Student stu = new Student();
    stu.setName(name);
    stu.setGender(gender);
    stu.setAge(age);
    stuMapper.add(stu); // 调用add添加对象到数据库

    // 注意：要手动提交事务，否则不会真正提交到数据库
    sqlSession.commit();

    System.out.println(stu.getId()); // 获得数据库自动创建的主键
    // 释放资源
    sqlSession.close();
}
```

```xml
<!--sql映射-->
<!--添加数据 useGeneratedKeys表示主键返回，创建对象后可以获取到对象的主键，用于其他关联表使用-->
<insert id="add" useGeneratedKeys="true" keyProperty="id">
    insert into stu(name,gender,age)
    values(#{name},#{gender}, #{age})
</insert>
```



### mybatis动态修改

```java
@Test
public void testUpdate() throws IOException {
    InputStream rs = Resources.getResourceAsStream("Mybatis-config.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(rs);
    // 创建sql连接
    SqlSession sqlSession = sqlSessionFactory.openSession();
    // 获取映射
    StuMapper stuMapper = sqlSession.getMapper(StuMapper.class);

    String name = "kashin Forever";
    Integer id = 2;
    Student student = new Student();
    student.setName(name);
    student.setId(id);
    // 修改name字段，传入id,name
    stuMapper.update(student);

    sqlSession.commit();
    sqlSession.close();
}
```

```xml
<!--修改数据使用动态SQL，只修改用户传入的数据，没传的不变-->
<update id="update">
    update stu
    <!--set标签作用和where一样，如果字段不存在时，可保证内部语法正确-->
    <set>
        <if test="name != null and name != ''">
            name = #{name},
        </if>
        <if test="gender != null and gender != ''">
            gender = #{gender},
        </if>
        <if test="age != null">
            age = #{age}
        </if>
    </set>
    where id = #{id};
</update>
```



### mybatis批量删除

```xml
<!--重点：批量删除 使用动态SQL和foreach标签-->
<delete id="deleteByIds">
    <!--
        mybatis会将数组参数封装为一个map集合
       * 默认：array = 数组
       * 使用@Param注解可改变map集合默认key的名称
    -->
    delete from stu
    where id in
    (
        <!--collection=array集合的默认名称； separator: 集合内每个元素的分隔符 id1,id2,id3-->
        <foreach collection="array" item="id" separator=",">
            #{id}
        </foreach>
    )
</delete>
```

```java
@Test
public void testDeleteByIds() throws IOException {
    InputStream rs = Resources.getResourceAsStream("Mybatis-config.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(rs);
    // 创建sql连接
    SqlSession sqlSession = sqlSessionFactory.openSession();
    // 获取映射
    StuMapper stuMapper = sqlSession.getMapper(StuMapper.class);

    int[] ids = {3, 10};
    stuMapper.deleteByIds(ids); // 执行批量删除，传入id数组

    sqlSession.commit();
    sqlSession.close();
}

// 接口参数
public interface StuMapper{
    public void deleteByIds(int[] ids); // 这里可以使用@Param注解修改 foreach标签collection集合的默认名称
}
```



### mybatis传参细节

mybatis底层在传参时的处理逻辑：

一、传单个参数时：

​	1.POJO类：直接使用，属性名和参数占位符名一致

​	2.Map集合，直接使用，减免和参数占位符名一致

​	3.Collection: 封装为Map集合，可使用@Param注解，替换Map集合中默认的arg键名

​	底层操作：

​		map.put("arg0", collection集合)

​		map.put("collection", collection集合)

​	4.List: 封装为map集合，可使用@Param注解，替换Map集合中默认的arg键名

​		map.put("arg0", list集合)

​		map.put("collection", list集合)

​		map.put("list", list集合)

​	5.Array: 封装为map集合，可使用@Param注解，替换Map集合中默认的arg键名

​		map.put("arg0", 数组)

​		map.put("array", 数组)

​	6.其他类型：直接使用



二、传多个参数：封装为Map集合，可使用@Param注解，替换Map集合中默认的arg键名

​	map.put("arg0", 参数值1)	

​	map.put("param1", 参数值2)

​	....



### 注解方式完成增删改查

建议简单的增删改查可以用，复杂查询或动态SQL还是用xml映射的方式

```java
// 写在接口中方法的上方
public interface StuMapper {
    
    @Select("select * from tb_user where id = #{id}")
    public User selectById(int id);
}
```



