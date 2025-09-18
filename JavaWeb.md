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
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=E:\\MySQL\\mysql-5.7.40-winx64
# 设置mysql数据库的数据的存放目录	
datadir=E:\\MySQL\\mysql-data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```



3.初始化data文件：管理员权限打开cmd，执行mysqld --initialize-insecure   (-insecure表示非可靠模式，可以不输入密码登录)

4.注册mysql服务：mysqld -install

5.启动服务：net start mysql

6. 登录mysql： mysql -u root -p （这个mysql指的是mysql.exe, 即mysql客户端，用来和服务通信，因为未设置密码，可以直接回车登录）
7. 查询用户名和密码：select host,user,authentication_string from mysql.user;

修改默认账户名密码：mysqladmin -u root password 123456  (修改管理员root账户的密码，非可靠模式使用)

**8.0以上版本mysql修改密码：ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';**  

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

**CHAR(10)在UTF-8字符集下**

| 存储内容      | 字符数 | 每字符字节数 | 总占用字节数                        |
| ------------- | ------ | ------------ | ----------------------------------- |
| `'Hello'`     | 5      | 3            | 5 × 3 = 15 字节                     |
| `'你好'`      | 2      | 3            | 2 × 3 = 6 字节                      |
| `'Hello你好'` | 8      | 3            | 8 × 3 = 24 字节                     |
| 空字符串      | 0      | -            | 仍占用 10 × 3 = 30 字节（填充空格） |

> 💡 因为 `CHAR` 是定长类型，即使只存 1 个字符，也会用空格补足 10 个字符的空间。

**CHAR最大255字符，CHAR内定义的是字符数，字符集影响的是字节数，不影响字符数**

**VARCHAR内定义的也是字符数，因为mysql每行最大65535字节，字符集对VARCHAR定义有影响**

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
select count(id) from stu; -- 统计有多少个学生，传进去的列名不能为空，不会统计列为null的数据，最好传主键或*(*表示一行只要有一个不为空就会统计)
select count(*) AS total_20 from stu where age>= 20 --给count(*)起一个别名，统计大于20岁的学生个数

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



![](D:\typora-img\image-20230815103729007.png)



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

### IDEA配置JDBC

1. **下载JDBC驱动**：
   - 访问MySQL官网下载对应的JDBC驱动（Connector/J），选择适合您操作系统的版本进行下载，通常是一个`.zip`文件。
2. **安装JDBC驱动到IDEA**：
   - 将下载的JDBC驱动解压，得到`.jar`文件，例如`mysql-connector-java-8.0.26.jar`。
   - 打开IntelliJ IDEA，找到项目设置中的`Libraries`部分，将`.jar`文件添加到项目中。可以通过`File` -> `Project Structure` -> `Libraries`，然后点击`+`号添加JARs或目录。
3. **配置数据库连接**：
   - 在IDEA的主界面右侧边栏找到“Database”选项卡，点击`+`号添加新的数据源。
   - 选择您要连接的数据库类型（例如MySQL），并填写连接配置信息，包括主机名、端口号、数据库名称、用户名和密码。
   - 确保填写的URL格式正确，例如：`jdbc:mysql://localhost:3306/your_database_name?useSSL=false&serverTimezone=UTC`。
4. **测试连接**：
   - 配置完成后，点击`Test Connection`按钮以确保IDEA能够成功连接到数据库。



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

3.配置本地仓库，修改本地maven配置文件：conf/settings.xml中<localRepository>为一个指定目录（可选，默认会配在C盘用户目录下）

4.配置阿里云私服，修改本地maven的配置文件：conf/settings.xml中<mirrors>标签，添加子标签（可选，下载依赖更快）

![image-20230817172951490](D:\typora-img\image-20230817172951490.png)

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

![image-20230821150240468](D:\typora-img\image-20230821150240468.png)





**常用命令**

compile：编译

clean：清理

test：测试 ，使用合适的单元测试框架运行测试（Junit是其中之一）

package：打包，将编译后的代码打包成jar文件

install：安装，安装项目包到本地仓库，这样项目包可以用作其他本地项目的依赖





## MyBatis

简介：MyBatis是一个持久层框架，用于简化JDBC开发。 官网：https://mybatis.org/mybatis-3/zh/index.html

JAVA经典三层结构：表现层、业务层、持久层，**持久层：将数据保存到数据库的那一层代码。**

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
        //sqlSessionFactory工厂只需要创建一次，可以抽取到工具类中的静态代码块
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
        UserMapper mapper = sqlSession.getMapper(UserMapper.class); // 传字节码对象 动态代理技术
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
// 封装对象 对象中的成员变量名和占位符名称一致
public List<Student> selectByCondition(Student student);
// 封装map集合 map集合的键名和占位符名称一致
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

​	2.Map集合，直接使用，键名和参数占位符名一致

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





## JavaWeb核心

技术栈：B/S架构，浏览器/服务器模式

​	静态资源：html,css,js,图片等负责页面展现

​	动态资源：Servlet, JSP等负责逻辑处理

​	数据库：数据存储

​	http协议：定义通信规则

​	web服务器(apache tomcat等)：负责解析http协议，解析请求数据，并发送响应数据



### HTTP

1.http协议基于TCP协议：面向连接，安全

2.基于请求-响应模型：一次请求对应一次响应

3.http协议是无状态的协议：对于事务处理没有记忆能力，每次请求-响应都是独立的。



请求数据分为3部分：

1.请求行 ，指第一行 包括了请求方式，请求资源路径，协议版本   GET /xx/xx HTTP/1.1  get的请求参数在请求行中

2.请求头，第二行开始，键值对形式

3.请求体，POST的请求参数

响应数据也分为3部分：

1.响应行，第一行，HTTP版本， 响应状态码(200, 304等等)

2.响应头：第二行开始，键值对形式， Content-type:响应内容类型，如text/html, image/jpeg

3.响应体：最后一部分，存放响应数据



### 原生服务端响应

通过new ServerSocket监听客户端请求，再通过输出流写出http响应数据给客户端返回。非常麻烦



### Tomcat服务器

web服务器就是一个应用程序，作用：

1.封装http协议操作，简化开发

2.将web项目部署到服务器中，对外提供网页浏览服务

tomcat是一个轻量级web服务器，支持servlet/jsp和少量javaEE规范，javaEE规范就是一整套java企业级开发规范，包含了jdbc,xml, spring,servlet,JTS,JTA等等

tomcat也被称为web容器，servlet容器。servlet需要依赖tomcat才能运行



### tomcat使用

启动：下载tomcat压缩包，解压后选择bin/startup.bat启动服务，默认8080端口

关闭：bin/shutdown.bat 或者 ctrl+C 

若控制台中文乱码则修改conf/logging.properties配置文件，java.util.logging.ConsoleHandler.encoding = GBK

启动后浏览器输入localhost:8080即可访问Tomcat服务器

修改启动端口：修改配置文件conf/server.xml中 <Connector port = "80" ...>

http协议默认端口为80，tomcat改成80，则浏览器访问时可以省略端口号



项目部署：如项目是名为myDemo的文件夹，内部放index.html和其他静态资源，直接把myDemo放在webapps文件夹下即可，浏览器输入**localhost/myDemo/index.html**访问



**war包自动解压：javaweb项目一般打成war的压缩包，放到webapps下tomcat会自动解压**



#### tomcat-web项目结构

开发时项目结构：main文件夹下多一个webapp文件夹存放web相关html文件和WEB-INF/web.xml配置文件

部署的项目结构：web/WEB-INF文件夹下多了classes和lib文件，保存java字节码文件和依赖jar包

![image-20230828105611839](D:\typora-img\image-20230828105611839.png)

#### maven创建web项目

两种方式

1. 通过maven骨架创建(maven-archetype-webapp)

2. 手动创建maven项目，补全webapp文件结构（pom.xml中添加打包格式<packaging>war</packaging>）

   建议通过project structure--》facets, 窗口中选择当前web项目，创建webapp文件夹，再点击+号根据提示创建web.xml配置文件



#### idea集成tomcat

直接在idea中启动tomcat，简化修改代码后-打包-部署一系列操作

两种方式：

一、本地已下载tomcat, 选择run-edit configurations, 弹窗内点击+号，下拉列表找到tomcat server-local 选中，新窗口中选择application server 选择本地安装的路径，完成tomcat集成

部署项目：选择deployment(部署)选项卡，点击+号，选择artifact，会弹出当前的web项目，选择要部署的项目war包，点击apply即完成部署，deployment中可以修改项目的访问路径

启动tomcat: 右上角绿色标志点击即启动服务，此时通过浏览器即可访问相应前端页面



二、.使用maven插件配置tomcat

在pom.xml中添加tomcat坐标，并添加<configuration>配置<port>80</port>和<path>/</path>，修改端口号和访问时的路径；

最后右键项目使用maven-helper插件选择tomcat run快速启动服务





### Servlet

Servlet是java提供的动态web资源开发技术，JavaEE的规范之一，本质是一个接口

需要定义Servlet类实现Servlet接口，并由web服务器运行Servlet

实现流程

1、创建web项目，导入servlet依赖坐标

```xml
<dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
    <!--关键参数：依赖范围设置为编译环境和测试环境有效，运行环境无效，即打包后没有，因为tomcat自带servlet的jar包，防止冲突-->
      <scope>provided</scope>
    </dependency>
```

2、创建：定义一个类，实现servlet接口，并重写接口中所有方法

3、配置：在类上使用注解： @WebServlet，配置servlet访问路径

```java
package com.david.servlet;

import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;

@WebServlet("/demo1") // 配置一个demo1的servlet类 
@WebServlet(urlPatterns = "/demo1", loadOnStartup = 1) // 多参数配置：配置创建时机，启动时就创建
public class ServletDemo1 implements Servlet {

    public void init(ServletConfig servletConfig) throws ServletException {

    }

    public ServletConfig getServletConfig() {
        return null;
    }

    // 处理请求时，会调用该方法
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("servlet启动啦！");
        
        // 强转为http类型,才能调用getMehod方法
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        // 获取请求的类型
        String method = httpServletRequest.getMethod();
        
        // get和post请求参数的位置不同，要分别写处理逻辑
        if ("GET".equals(method)) {
           // get方法的处理逻辑 
        } else if ("POST".equals(method)) {
            // post方法的处理逻辑
        }
    }

    public String getServletInfo() {
        return null;
    }

    public void destroy() {

    }
}

```

4、访问：启动Tomcat，浏览器输入url，访问该servlet，访问后控制台输出“servlet启动啦”表示访问成功

http://localhost/web-demo/demo1



**tomcat启动报错：java不支持发行版本解决方案**

```xml
<!--修改pom.xml配置信息-->
<build>
    <finalName>tomcat-demo1</finalName>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <configuration>
          <source>17</source>
          <target>17</target>
          <encoding>UTF-8</encoding>
        </configuration>
      </plugin>
    </plugins>
</build>
```



#### Servlet执行流程

Servlet实体类由web服务器(tomcat)创建，内部的方法也是由tomcat在运行时调用



#### Servlet生命周期

servlet运行在Servlet容器（web服务器）中，其生命周期也由容器管理，分为4个阶段：

1、加载和实例化：默认情况下，当Servlet第一次被访问时，由容器创建Servlet对象

​		注解参数：loadOnStartup = 1 || -1    1：服务器启动时创建类  -1：Servlet被访问时创建(默认)

2、初始化：实例化后，容器调用servlet对象的**init()**方法初始化对象，完成加载配置文件、创建连接等初始化工作，**该方法只调用一次**

3、请求处理：**每次请求Servlet时**，Servlet容器都会调用servlet对象的**service()**方法对请求进行处理

4、服务终止：当需要释放内存或容器关闭时，调用**destroy()**方法完成资源释放，在该方法调用后，容器释放这个Servlet实例，被java回收



#### HttpServlet实现类

是一个对http协议封装的Servlet实现类，分发了post/get不同类型请求的方法

Http开发中，我们自定的Servlet都直接继承这个类，覆写其中的核心方法

```java
// HttpServlet类分发方法的源码逻辑
public class HttpServlet implements Servlet{
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        // 强转为http类型,才能调用getMehod方法
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        // 获取请求的类型
        String method = httpServletRequest.getMethod();
        // get和post请求参数的位置不同，要分别写处理逻辑
        if ("GET".equals(method)) {
           // get方法的处理逻辑 
            doGet(servletRequest, servletResponse)
        } else if ("POST".equals(method)) {
            // post方法的处理逻辑
            doPost(servletRequest, servletResponse)
        }
    }
    // 提供这两个方法给继承类，继承类重写即可
    protected void doGet(ServletRequest servletRequest, ServletResponse servletResponse) {}
    protected void doPost(ServletRequest servletRequest, ServletResponse servletResponse) {}
}
```

继承HttpServlet书写方式

```java
// 继承一个httpServlet, 重写get,post方法
@WebServlet(urlPatterns = "/demo2") // /demo2这种方法方式就是get
public class Servletdemo2 extends HttpServlet {
    // 通过get方式访问servlet，就会调用doGet
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("get...");
    }

    // 通过post方式访问servlet，就会调用doPost 前端模拟一个表单提交就会调用该方法
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("post...");
    }
}
```



#### UrlPatterns配置

```java
//一个Servlet可以配置多个urlPatterns
@WebServlet(urlPatterns={"/demo1", "/demo2"}) // 访问/demo1或/demo2都能访问到这个sevlet类

// 匹配规则
// 1.精确匹配
// 配置路径
@WebServlet(urlPatterns="/user/select")  // 访问路径：localhost/user/select

// 2.目录匹配
@WebServlet(urlPatterns="/user/*") // 访问路径：localhost/user/aaa  localhost/user/bbb

// 3.扩展名匹配
@WebServlet(urlPatterns="/user/*.do")  // 访问路径:localhost/user/a.do localhost/user/b.do

// 4.任意匹配
@WebServlet(urlPatterns="/")  // 会覆盖tomcat默认Servlet路径，导致静态资源无法访问，当其他都匹配不上，就会访问这个Servlet路径
@WebServlet(urlPatterns="/*")  // 匹配任意访问路径
```



#### XML方式配置Servlet（了解）

3.0版本前不支持注解方式配置，需要通过XML配置方式配置Servlet

步骤：

1、编写Servlet类

2、在web.xml中配置该Servlet

```xml
<servlet>
	<servlet-name>demo13</servlet-name>
    <!--必须写全类名-->
    <servlet-class>com.david.servlet.servletDemo13</servlet-class>
</servlet>

<!--配置映射关系-->
<servlet-mapping>
	<servlet-name>demo13</servlet-name>
    <url-pattern>/demo13</url-pattern>
</servlet-mapping>
```



### Request&Response

service(request, response) 

request：获取请求数据，request就是tomcat获得客户端请求的信息后，放到request对象中

response：设置响应数据，通过reponse对象设置响应数据，tomcat在真正响应前会把响应数据拼接上http响应行和响应头，再返回给客户端

```java
//简单的请求和响应
@WebServlet(urlPatterns = "/demo2") // /demo2这种方法方式就是get
public class Servletdemo2 extends HttpServlet {

    // 通过get方式访问servlet，就会调用doGet
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 使用request对象获取get请求传递的数据
        String name = req.getParameter("name"); // url?name=david

        // 使用response对象设置响应数据
        resp.setHeader("content-type","text/html;charset=utf-8");
        resp.getWriter().write("<h1>" + name + ",欢迎您！</h1>");
    }
}
```



#### Request对象继承体系

ServletRequest（Java提供的请求对象根接口）

↓

HttpServletRequest（Java提供的对http协议封装的请求对象接口）

↓

RequestFacade（Tomcat定义的实现类，实现上面接口的所有方法）



#### Request获取请求参数

```java
@WebServlet("/req1")
public class ServletRequestdemo3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取请求行
        String method = req.getMethod(); // 获取请求方式 GET
        System.out.println("method:"+method);

        String contextPath = req.getContextPath(); // 获取虚拟目录（项目访问路径） 无，因为tomcat设置了/
        System.out.println("contextPath: "+contextPath);

        StringBuffer requestURL = req.getRequestURL(); // 获取URL（统一资源定位符） http://localhost/req1
        System.out.println("requestURL： "+requestURL);

        String requestURI = req.getRequestURI(); // 获取URI（统一资源标识符） 域名后面的项目访问路径+资源 /req1
        System.out.println("requestURI:"+requestURI);

        String queryString = req.getQueryString(); // 获取请求参数(GET),url?后的查询字符串，没传返回null; name=david&age35
        System.out.println("queryString: "+queryString);

        // 获取请求头
        String header = req.getHeader("user-agent"); // 根据请求头名，获取值
        System.out.println("header:"+ header);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取请求体（POST） 要写一个post提交的html页面
        // 请求体有2种获取方式，字节输入流和字符输入流，如果获取文本用字符流，获取二进制文件用字节流
        BufferedReader reader = req.getReader();
        String s = reader.readLine();
        System.out.println(s); // 输出:username=xxx&password=xxx

        // 字节流获取数据
        ServletInputStream inputStream = req.getInputStream();
    }
}
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
  <h1>hello david!</h1>
    <form action="/req1" method="post">
        <input name="username" type="text"/>
        <input name="password" type="password">
        <input type="submit">
    </form>
</body>
</html>
```



#### 通用方法获取请求参数

doGet获取请求参数用的是getQueryString()，doPost获取请求体参数用的是字节输入流和字符输入流

通用方法可以在doGet和doPost中使用同一方法获取请求参数，简化代码

```java
package com.david.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Map;

@WebServlet("/req2")
public class ServletRequestDemo4 extends HttpServlet {

    // 通用方法获取请求参数
    private void getData(HttpServletRequest req, HttpServletResponse resp) {
        String method = req.getMethod();
        System.out.println("当前方法：" + method);
		req.setCharacterEncoding("UTF-8"); // 解决post请求中文乱码
        
        // 方法1.获取所有参数Map集合
        // getParameterMap返回请求参数键值对的map集合，注意值是一个String数组，因为入参可能存在键名相同的情况
        Map<String, String[]> paramsMap = req.getParameterMap();
        for (String key : paramsMap.keySet()) {
            System.out.print(key + ": ");
            String[] values = paramsMap.get(key);
            for (String value : values) {
                System.out.print(value);
            }
            System.out.println();
        }

        // 方法2.根据名称获取参数值（数组）
        String[] hobbies = req.getParameterValues("hobby");
        for (String hobby : hobbies) {
            System.out.println(hobby);
        }
        // 方法3.根据名称获取参数值（单个值）
        String name = req.getParameter("username");
        System.out.println(name);
    }
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        getData(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        getData(req, resp);
    }
}

```



#### Request中文乱码解决

post请求是输入字符流和字节流，**调用req.setCharacterEncoding("UTF-8") 设置输入流编码即可**



get请求调用的是req.getQueryString()获取参数，设置字符集无效

原因：get请求参数通过url传送，在发送时，中文会被浏览器URL编码成类似%E5%BC这种格式，Tomcat解码URL通过ISO-8859-1解码，所以乱码

URL编码规则：

1、建字符串按编码方式转为二进制

2、每个字节转为2个16进制数并在前面加上%

java工具类进行url编码和解码

```java
URLEncoder.encode(str, "utf-8") // 把str通过utf-8进行URL编码，转成%E5%BC这种格式
URLDecoder.decode(s, "utf-8") // 把s进行URL解码
```

**解决get请求乱码**

```java
// 思路：先把tomcat解码后的乱码数据转换成字节
String encode = URLEncoder.encode(username, "utf-8") // 浏览器做的事
String decode = URLDecoder.decode(encode, "ISO-8859-1") // tomcat做的事
    
// 我们要做的事，把乱码转为字节数组
byte[] bytes = decode.getBytes("ISO-8859-1")
// 解码成字符串,传入字符集
String str = new String(bytes, "UTF-8")
```

**备注：Tomcat8之后默认采用utf-8对Url解码，不需要再解决url传参乱码问题**



#### 请求转发（forward）

逻辑：请求资源A时，资源A把请求转发给了资源B，资源B处理请求再返回响应，就是请求转发的过程

特点：

浏览器地址路径不发生变化；

只能转发当前服务器内部资源；

因为是一次请求，转发时可以通过request对象共享数据

```java
@WebServlet("/demo5")
public class ServetForwardDemo5 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("demo5执行");
        // 转发前存储共享数据
        req.setAttribute("message", "hello");
        // 转发给demo6
        req.getRequestDispatcher("/demo6").forward(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}

@WebServlet("/demo6")
public class ServletForwardDemo6 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取转发的共享数据
        Object message = req.getAttribute("message");
        System.out.println(message);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}
```



#### Response对象继承体系

ServletResponse（Java提供的请求对象根接口）

↓

HttpServletResponse（Java提供的对http协议封装的请求对象接口）

↓

ResponseFacade（Tomcat定义的实现类，实现上面接口的所有方法）



#### Response响应结构

响应数据结构：

响应行： HTTP/1.1 200  OK

​				**void setStatus(int sc)** 设置响应状态码

响应头：Content-type: text/html

​				**void setHeader(String name, String value)** 设置响应头键值对

响应体：<html><head><body></body></head></html> 

​				通过字节/字符输出流写出 



#### response重定向

特点（和转发相反）：

1、浏览器地址会变成重定向的资源地址

2、可转发服务器内部，或服务器外部资源

3、两次请求，所以不能通过request共享数据

```java
// 请求/resp1时被转发到/resp2

@WebServlet("/resp1")
public class respDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 重定向到/resp2
        resp.sendRedirect("/resp2");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        
    }
}

@WebServlet("/resp2")
public class respDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("resp2");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        
    }
}
```



#### response响应字符数据

```java
@WebServlet("/resp3")
public class respDemo3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 1.响应字符数据
        // 设置响应中文的字符编码
        resp.setCharacterEncoding("utf-8");
        // 设置响应数据类型，设置后才会被浏览器解析成html文档，浏览器默认解析成纯文本
        resp.setContentType("text/html");
        // 通过resp对象获取字符输出流，细节：不需要close，resp会随响应结束后销毁，自动close
        PrintWriter writer = resp.getWriter();
        writer.write("你好啊，我是大卫");
        writer.write("<h1>我是html</h1>");
    }
}
```



#### response响应字节数据

```java
@WebServlet("/resp4")
public class respDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 响应字节数据
        // 通过resp对象获取字节输出流
        ServletOutputStream os = resp.getOutputStream();

        // 读取文件
        FileInputStream fis = new FileInputStream("C:\\Users\\yoki\\Desktop\\javaTest\\a.jpg");

        // 流的对拷，写到输出流 进行响应
        byte[] bytes = new byte[1024];
        int len;
        while ((len = fis.read(bytes)) != -1) {
            os.write(bytes,0, len);
        }
        fis.close();
    }
}
```





## JSP概述

java server page(JAVA服务端页面，是一种动态网页技术，在html页面中书写java代码)，JSP = HTML + JAVA

作用：简化了在Servlet中写HTML页面的麻烦

**本质：JSP本质就是一个Servlet类，在执行时JSP被Tomcat自动转换为Servlet, 这个Servlet内部会调用write()输出html页面**

**备注：若访问jsp资源报错，修改jdk版本为1.8即可**

JSP创建步骤：

1、导入依赖坐标

```xml
<dependency>
  <groupId>javax.servlet.jsp</groupId>
  <artifactId>jsp-api</artifactId>
  <version>2.2</version>
  <scope>provided</scope>
</dependency>
```

2、创建JSP文件

3、编写HTML标签和Java代码

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>
  hello jsp
</h1>
    <!--嵌入java代码-->
<%
  System.out.println("im jsp");
%>
</body>
</html>
```



### JSP脚本分类

1、<%....%> ：内容会直接放到_jspService()方法中

2、<%=.....%>：内容会放到out.print()中，作为out,print()的参数

3、<%!.....%>：内容会放到_jspService()方法外，被类直接包含，（定义成员方法，成员变量等）

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>
  hello jsp
</h1>
<%
  System.out.println("im jsp");
  int num = 100;
%>
    <!--输出到页面上-->
<%= "hello" + num %>
<%= "david is king"%>
<%= "david is king is forever" %>
    
    <!--定义成员变量和成员方法-->
<%!
    void show(){};
    String name = "david";
%>
</body>
</html>
```

### JSP页面展示数据

```jsp
<%@ page import="java.util.ArrayList" %>
<%@ page import="com.david.Pojo.Brand" %><%--
  Created by IntelliJ IDEA.
  User: yoki
  Date: 2023-08-30
  Time: 11:37
  To change this template use File | Settings | File Templates.
--%>
<%
    // 创建要插入的数据
    ArrayList<Brand> list = new ArrayList<>();
    list.add(new Brand(1, "三只松鼠", "三只松鼠", 8, "三只松鼠，好吃不上火", 1));
    list.add(new Brand(2, "优衣库", "优衣库", 4, "优衣库，舒适人生", 1));
    list.add(new Brand(3, "小米", "小米科技有限公司", 5, "为发烧而生", 0));
%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>JSP嵌入数据</title>
</head>
<style>
    table {
        border-collapse: collapse;
    }
</style>
<body>
    <table style="width: 800px;" border="1">
        <!--插入jsp遍历集合-->
        <%
            for (Brand brand : list) {
        %>
        <!-- jsp截断1 -->
        <tr align="center">
            <td>
                <!-- %= 输出数据到页面 -->
                <%=brand.getId()%>
            </td>
            <td>
                <%=brand.getBrandName()%>
            </td>
            <td>
                <%=brand.getCompanyName()%>
            </td>
            <td>
                <%=brand.getSort()%>
            </td>
            <td>
                <%=brand.getDescription()%>
            </td>
            <td>
                <%=brand.getStatus()%>
            </td>
        </tr>
        <!-- jsp截断2 -->
        <%
            }
        %>
    </table>
</body>
</html>
```



### JSP转换后的Servlet源代码

编译后的jsp文件根据控制台**org.apache.catalina.startup.VersionLoggerListener.log CATALINA_BASE:** 的输出的路径信息查看

```java
// 核心响应代码
// 这部分数据在_jspService方法中执行
ArrayList<Brand> list = new ArrayList<>();
list.add(new Brand(1, "三只松鼠", "三只松鼠", 8, "三只松鼠，好吃不上火", 1));
list.add(new Brand(2, "优衣库", "优衣库", 4, "优衣库，舒适人生", 1));
list.add(new Brand(3, "小米", "小米科技有限公司", 5, "为发烧而生", 0));

  out.write("\r\n");
  out.write("\r\n");
  out.write("<html>\r\n");
  out.write("<head>\r\n");
  out.write("    <title>JSP嵌入数据</title>\r\n");
  out.write("</head>\r\n");
  out.write("<style>\r\n");
  out.write("    table {\r\n");
  out.write("        border-collapse: collapse;\r\n");
  out.write("    }\r\n");
  out.write("</style>\r\n");
  out.write("<body>\r\n");
  out.write("    <!-- jsp截断 -->\r\n");
  out.write("    <table style=\"width: 800px;\" border=\"1\">\r\n");
  out.write("        ");

        for (Brand brand : list) {

          out.write("\r\n");
          out.write("        <tr align=\"center\">\r\n");
          out.write("            <td>\r\n");
          out.write("                <!-- 输出数据到页面 -->\r\n");
          out.write("                ");
          out.print(brand.getId());
          out.write("\r\n");
          out.write("            </td>\r\n");
          out.write("            <td>\r\n");
          out.write("                ");
          out.print(brand.getBrandName());
          out.write("\r\n");
          out.write("            </td>\r\n");
          out.write("            <td>\r\n");
          out.write("                ");
          out.print(brand.getCompanyName());
          out.write("\r\n");
          out.write("            </td>\r\n");
          out.write("            <td>\r\n");
          out.write("                ");
          out.print(brand.getSort());
          out.write("\r\n");
          out.write("            </td>\r\n");
          out.write("            <td>\r\n");
          out.write("                ");
          out.print(brand.getDescription());
          out.write("\r\n");
          out.write("            </td>\r\n");
          out.write("            <td>\r\n");
          out.write("                ");
          out.print(brand.getStatus());
          out.write("\r\n");
          out.write("            </td>\r\n");
          out.write("        </tr>\r\n");
          out.write("        ");

        }

  out.write("\r\n");
  out.write("    </table>\r\n");
  out.write("</body>\r\n");
  out.write("</html>\r\n");
}
```



### JSP缺点

1、书写麻烦，各种嵌套和截断

2、阅读困难

3、运行麻烦，依赖JRE, Tomcat，JAVAEE环境

4、调试困难，需要找到转换后的.java文件进行调试

**JSP已基本退出历史舞台，目前主流是前端html+ajax动态请求服务端数据进行页面响应**



### EL表达式

作用：简化JSP内的JAVA代码

语法：${xxx}  获取域中存储的key=xxx的数据

javaWeb中四大域

1、Page 当前页面有效

2、request 当前请求有效

3、session 当前会话有效

4、application 当前应用有效

**el表达式会依次从这个4个域中寻找数据**

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!--解决el表达式不生效问题-->
<%@ page isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <!-- 使用EL表达式直接获取request域中的数据 -->
${brands}
</body>
</html>
```

```java
@WebServlet("/eldemo")
public class ElServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 虚拟数据
        ArrayList<Brand> list = new ArrayList<>();
        list.add(new Brand(1, "三只松鼠", "三只松鼠", 8, "三只松鼠，好吃不上火", 1));
        list.add(new Brand(2, "优衣库", "优衣库", 4, "优衣库，舒适人生", 1));
        list.add(new Brand(3, "小米", "小米科技有限公司", 5, "为发烧而生", 0));
        
        // 保存数据到req域
        req.setAttribute("brands", list);
        // 转发给jsp
        req.getRequestDispatcher("/el-demo1.jsp").forward(req, resp);
    }
}
```



### JSTL标签

使用标签取代JSP页面上的Java代码，免去了<%...%>截断代码的操作

步骤：

1、pom.xml导入依赖坐标

2、JSP页面上引入JSTL标签库

3、使用<c: if> <c: forEach> 标签语法

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page isELIgnored="false" %>
<%--引入JSTL标签库--%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<html>
<head>
    <title>JSTL-demo</title>
</head>
<body>
<%--JSTL forEach循环--%>
<table border="1" cellspacing="0" width="800">
    <tr align="center">
        <th>序号</th>
        <th>品牌</th>
        <th>公司名称</th>
        <th>排序</th>
        <th>品牌描述</th>
        <th>状态</th>
    </tr>
<%--    varStatus属性代表索引或序号 主要属性有index和count --%>
    <c:forEach items="${brands}" var="brand" varStatus="status">
        <tr align="center">
            <td>${status.count}</td>
                <%--        <td>${brand.id}</td>--%>
            <td>${brand.brandName}</td> <!-- 注意：这里不是获取成员变量，而是通过属性名拼接成getBrandName()调成员方法 -->
            <td>${brand.companyName}</td>
            <td>${brand.sort}</td>
            <td>${brand.description}</td>
            <!--c if 没有else写法，只能用两个if-->
            <c:if test="${brand.status == 1}">
                <td>启用</td>
            </c:if>
            <c:if test="${brand.status != 1}">
                <td>禁用</td>
            </c:if>
        </tr>
    </c:forEach>
</table>
<!-- forEach普通遍历，类似for 可设置循环次数 实现类似分页器效果 -->
<c:forEach begin="1" end="10" step="1" var="i">
    ${i}
</c:forEach>
</body>
</html>
```





## MVC模式

M：Model:业务模型，处理业务，模型是一个广义概念，javabean类就是一个模型

V：View，视图，界面展示

C：Controller 控制器，处理请求，调用模型和视图

![image-20230830162546014](D:\typora-img\image-20230830162546014.png)



**三层架构**

表现层：接收请求、封装数据，调用业务逻辑层，最后通过jsp响应数据 （包名一般叫web或controller）

业务逻辑层：对业务逻辑封装，组合数据访问层中基本功能，形成复杂的业务逻辑功能（包名一般叫service）

数据访问层：对数据库的CRUD操作（包名一般叫dao或mapper）



表现层框架：SpringMVC/struts2

业务层框架：Spring（主要框架，包含了SpringMVC）

数据层框架：Mybatis



### 三层架构创建

1、创建javaWeb项目，pom引入所有的依赖坐标

2、创建3层架构的包结构（web/service/mapper/pojo/util(存放sqlFactory工厂对象)）

3、数据库建表

4、实体类创建

5、Mybatis基础环境（Mybatis-config.xml / Mapper.xml / Mapper接口）

逻辑：

![image-20230830170401082](D:\typora-img\image-20230830170401082.png)





### 三层架构CRUD开发

![image-20230901090635113](D:\typora-img\image-20230901090635113.png)

![image-20230901091134578](D:\typora-img\image-20230901091134578.png)

```java
// -----brandService服务层
package com.davidmvc.service;

import com.davidmvc.mapper.BrandMapper;
import com.davidmvc.pojo.Brand;
import com.davidmvc.util.SqlSessionFactoryUtils;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;

import java.io.IOException;
import java.util.List;

// 创建数据库连接，执行sql操作并把数据给servlet
public class BrandService {

    // 封装查询方法
    public List<Brand> selectAll() throws IOException {
        // 用工具类获取sql工厂
        SqlSession sqlSession = SqlSessionFactoryUtils.getSqlSessionFactory().openSession();
        // 获取映射
        BrandMapper mapper = sqlSession.getMapper(BrandMapper.class);
        // 执行sql
        List<Brand> brands = mapper.selectAll();
        sqlSession.close();
        return brands;
    }
    // 封装新增方法
    public void add (Brand brand) {
        // 用工具类获取sql工厂
        SqlSession sqlSession = SqlSessionFactoryUtils.getSqlSessionFactory().openSession();
        BrandMapper mapper = sqlSession.getMapper(BrandMapper.class);
        // 执行sql
        mapper.add(brand);
        // 增删改操作，记得提交事务
        sqlSession.commit();
        sqlSession.close();
    }
    // 根据id查询单个数据
    public Brand selectById(Integer id) {
        SqlSession sqlSession = SqlSessionFactoryUtils.getSqlSessionFactory().openSession();
        BrandMapper mapper = sqlSession.getMapper(BrandMapper.class);
        // 执行sql
        Brand brand = mapper.selectById(id);
        sqlSession.close();
        return brand;
    }

    // 修改数据
    public void updateById (Brand brand) {
        SqlSession sqlSession = SqlSessionFactoryUtils.getSqlSessionFactory().openSession();
        BrandMapper mapper = sqlSession.getMapper(BrandMapper.class);
        // 执行sql
        mapper.updateById(brand);
        // 增删改操作，记得提交事务
        sqlSession.commit();
        sqlSession.close();
    }
}


//-------------------------------- sqlSessionFactory工厂类
package com.davidmvc.util;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

// sql工厂函数工具类 避免重复创建浪费资源
public class SqlSessionFactoryUtils {
    private static SqlSessionFactory sqlSessionFactory;

    // 创建静态代码块，在class类加载时执行，且只执行一次
    static {
        try {
            String resource = "Mybatis-config.xml";
            InputStream resourceAsStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    // 静态方法：返回这个工厂对象
    public static SqlSessionFactory getSqlSessionFactory () {
        return sqlSessionFactory;
    }
}

```

```java
// --------brandMapper 接口
package com.davidmvc.mapper;

import com.davidmvc.pojo.Brand;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.annotations.Update;

import java.util.List;

// 数据访问层
public interface BrandMapper {
    // 查询所有数据
    @Select("select * from tb_brand")
    List<Brand> selectAll();

    // 新增数据 用sql注解方式
    @Insert("INSERT INTO tb_brand VALUES(null, #{brandName}, #{companyName}, #{sort}, #{description}, #{status})")
    void add(Brand brand);

    // 根据id查询单条数据
    @Select("select * from tb_brand where id=#{id}")
    Brand selectById(Integer id);

    // 根据id修改数据
    @Update("update tb_brand set brand_name=#{brandName},company_name=#{companyName},sort=#{sort},description=#{description},state=#{status} where id=#{id}")
    void updateById(Brand brand);
}

```



```java
// --------servlet层
// --------增改查 逻辑
package com.davidmvc.controller;

import com.davidmvc.pojo.Brand;
import com.davidmvc.service.BrandService;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

//------------ 增加， 注意：实际开发时每个Servlet都是独立文件
@WebServlet("/add")
public class AddServlet extends HttpServlet {
    private BrandService brandService = new BrandService();
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 设置字符编码格式，防止显示时乱码
        req.setCharacterEncoding("UTF-8");
        // 解析请求数据
        String brandName = req.getParameter("brandName");
        String companyName = req.getParameter("companyName");
        String sort = req.getParameter("sort");
        String description = req.getParameter("description");
        String status = req.getParameter("status");
        // 封装实体类
        Brand brand = new Brand(null, brandName, companyName, Integer.parseInt(sort), description, Integer.parseInt(status));

        // 调用Service中的方法，进行sql层操作
        brandService.add(brand);
        System.out.println(req.getContextPath() + "add");
        // 重定向到getBrandServlet 该类会再次转发到jsp页面展示数据
        resp.sendRedirect("/mvc/getBrand");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}

// -----修改
package com.davidmvc.controller;

@WebServlet("/updateById")
public class UpdateByIdServlet extends HttpServlet {
    private BrandService brandService = new BrandService();
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("utf-8");
        String id = req.getParameter("id");
        String brandName = req.getParameter("brandName");
        String companyName = req.getParameter("companyName");
        String sort = req.getParameter("sort");
        String description = req.getParameter("description");
        String status = req.getParameter("status");
        Brand brand = new Brand(Integer.parseInt(id), brandName, companyName, Integer.parseInt(sort), description, Integer.parseInt(status));
        // 执行更新数据sql
        brandService.updateById(brand);
        resp.sendRedirect("/mvc/getBrand");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}



// ----- 查所有
// 此处路径不用加/mvc
@WebServlet("/getBrand")
public class SelectAllServlet extends HttpServlet {
    private BrandService brandService = new BrandService();
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        List<Brand> brands = brandService.selectAll();
        // 保存数据
        req.setAttribute("brands", brands);
        System.out.println(req.getContextPath());
        // 转发给Jsp, 展示数据
        req.getRequestDispatcher("/brand.jsp").forward(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}

// ------------ 查单个
@WebServlet("/selectById")
public class SelectByIdServlet extends HttpServlet {
    private BrandService brandService = new BrandService();

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String id = req.getParameter("id");
        Brand brand = brandService.selectById(Integer.parseInt(id));
        // 数据存到req域
        req.setAttribute("brand", brand);
        // 转发给jsp，进行数据展现
        req.getRequestDispatcher("/update.jsp").forward(req, resp);
    }
}
```





## 会话跟踪

会话：用户打开浏览器访问web服务器资源，会话建立，当一方断开连接（关闭浏览器，关闭服务器），会话结束，一次会话中可以包含多次请求和响应。

http是一种无状态协议，服务器会被每次请求当做一个新的请求。所以需要会话跟踪技术

**客户端会话跟踪：Cookie（数据保存在客户端）**

**服务端会话跟踪：Session（数据保存在服务端）**

实现功能：一次会话中多次请求间的数据共享

一般来说：记住用户、购物车等功能用Cookie，验证码校验用Session



### Cookie

底层：基于http协议

响应头：response设置了set-cookie， 浏览器接收到set-cookie响应头，会自动保存该cookie

请求头：cookie，下次请求会在header中携带该cookie



基本使用：

```java
String username = req.getParameter("username");
// 创建Cookie对象，设置数据
Cookie cookie = new Cookie("username", username);
// 发送Cookie到客户端，使用response对象
resp.addCookie(cookie)
    
    
// 获取客户端发送过来的Cookie,返回的是浏览器cookie数组，使用request对象
Cookie[] cookies = req.getCookies();
for(cookie : cookies)
```



Cookie细节1：默认情况下，浏览器关闭内存释放，Cookie自动销毁

使用**setMaxAge(int seconds)**方法可以设置Cookie保存时间

````java
// 参数:int seconds
cookie.setMaxAge(60*60*24*7) // 保存7天； 参数为负数（默认值，关闭自动销毁）； 参数为0删除对应Cookie
````

Cookie细节2：cookie存中文要使用URL编码 URLEncoder.encode(value, "UTF-8")





### Session

底层原理：基于Cookie实现，服务端响应response会发送set-cookie: JSessionid=xxxxx  

基本使用：

```java
HttpSession session = request.getSession(); // 获取session对象
// session对象功能
void setAttribute(String name, Object o) // 存储数据到session域中
Object getAttribute(String name) // 根据key获取值
void removeAttribute(String name) // 根据key删除该键值对
```

session细节：服务器重启后，session中的数据是否存在？

​	钝化：服务器正常关闭后，tomcat会自动把session数据写入硬盘的文件中

​	活化：再次启动服务器后，从文件中加载数据到session中

​	自动销毁：默认无操作时，30分钟后自动销毁，可在web.xml中配置

```xml
<session-config>
    <!--单位：分钟-->
	<session-timeout>100</session-timeout>
</session-config>
```





## Filter

过滤器，是JavaWeb三大组件之一（Servlet、Filter、Listener）

过滤器语法和Sevrlet类似，需要实现一个Filter接口，重写内部的方法

```java
package com.davidmvc.filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

// 拦截特定资源
@WebFilter("/selectById")
public class FilterDemo implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("filter请求资源前");
        // 放行
        filterChain.doFilter(servletRequest, servletResponse);
        System.out.println("filter请求资源后");
    }

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void destroy() {

    }
}
```



过滤器可以把对资源的请求拦截下来，从而实现一些特殊功能

![image-20230901223954729](D:\typora-img\image-20230901223954729.png)



**细节：**

放行前，可以对request进行一些数据的处理

放行前response中是没有数据的，放行后的response才有数据



### 过滤器拦截配置

拦截具体资源：/index.jsp：只有访问index.jsp时才会被拦截

目录拦截：/user/*：访问/user下的所有资源都会被拦截

后缀名拦截：*.jsp：访问后缀名为jsp的资源都会被拦截

拦截所有： /*： 访问所有资源都会被拦截



### 过滤器链

一个web应用可以配置多个过滤器，被称为过滤器链，**过滤器执行顺序按字符串名自然排序，内部代码执行顺序为依次执行每个过滤器放行前逻辑，访问资源后再回头执行放行后逻辑，像一根链条**





## Listener

监听器可以监听application、session、request三个对象创建、销毁或者往其中增删改属性时**自动执行代码**的功能组件

Listener分类：

ServletContext监听 （对应application，监听整个web应用）

Session监听

Request监听

```java
// ServletContext监听
public class ListenerDemo implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        // 监听web应用初始化时执行
        System.out.println("监听器执行~");
    }

    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {

    }
}
```





## 其他知识点

### @ApiOperation

`@ApiOperation`是 Swagger 框架中的一个注解，主要用于在 API 文档生成过程中描述接口（API 方法）的功能。Swagger 是一个强大的工具，用于设计、构建、记录和使用 RESTful Web 服务。它通过解析代码中的注解来自动生成 API 文档，方便前后端开发人员理解接口的用途、参数、返回值等信息

```java
       @RestController
       @RequestMapping("/api")
       public class MyController {
           @ApiOperation("获取用户信息")
           @GetMapping("/user/{id}")
           public User getUserById(@PathVariable("id") Long id) {
               // 这里是获取用户信息的具体逻辑
               return userService.getUserById(id);
           }
       }
```



### @RestController

`@RestController` 是 Spring 框架中的一个组合注解，用于标识一个控制器类，专门处理 RESTful API 请求。它相当于 `@Controller` 和 `@ResponseBody` 的组合，意味着该类中的所有方法默认都会将返回值直接写入 HTTP 响应体中，而不是通过视图解析器渲染视图。

1. **返回数据直接写入响应体**：无需在方法上额外添加 `@ResponseBody`，适合返回 JSON、XML 等数据格式（默认依赖 Jackson 库实现对象转 JSON）。
2. **专注于 API 接口开发**：与传统的 `@Controller` 不同（用于 MVC 模式的视图渲染），`@RestController` 更适合构建 RESTful API，供前端、移动端等调用。



### @Controller

`@Controller` 是 Spring MVC 中的核心注解之一，用于标识一个类作为控制器（Controller），负责处理客户端发来的 HTTP 请求，并协调模型（Model）和视图（View）的交互。

它通常与请求映射注解（如 `@RequestMapping`、`@GetMapping` 等）配合使用，将特定的请求路径映射到控制器的方法上。

### 与 `@RestController` 的区别

| 注解              | 主要用途          | 响应处理方式                                                 |
| ----------------- | ----------------- | ------------------------------------------------------------ |
| `@Controller`     | 传统 MVC 视图开发 | 方法返回视图名称，需配合 `@ResponseBody` 才返回数据          |
| `@RestController` | RESTful API 开发  | 所有方法默认返回数据（等效于 `@Controller + @ResponseBody`） |

```java
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller // 标识此类为控制器
public class UserViewController {
    
    // 处理 GET 请求，路径为 "/users"
    @GetMapping("/users")
    public String showUserList(Model model) {
        // 模拟从服务层获取用户数据
        List<String> users = Arrays.asList("张三", "李四", "王五");
        
        // 将数据存入 Model，供视图使用
        model.addAttribute("userList", users);
        
        // 返回视图名称（由视图解析器解析为具体的视图文件，如 HTML、JSP 等）
        return "userList"; 
    }
}
```





### @GetMapping

是 `@RequestMapping(method = RequestMethod.GET)` 的简化形式。

- `@GetMapping`是 Spring 框架（具体是 Spring Web MVC）中的一个注解，用于将 HTTP GET 请求映射到特定的处理方法上。它是一种方便的、基于注解的方式来定义 RESTful API 端点或者处理普通的网页请求（在 MVC 模式下）。
- 它表明被标注的方法将处理 HTTP GET 请求。在例子中，`getBooks`方法会处理对`/api/books`路径的 GET 请求。当客户端（如浏览器或者其他 HTTP 客户端）发送一个 GET 请求到`/api/books`时，Spring 框架会自动调用这个`getBooks`方法，并将方法的返回结果（在这个例子中是书籍列表）返回给客户端。

```java
       @RestController
       @RequestMapping("/api")
       public class MyController {
           @GetMapping("/books")
           public List<Book> getBooks() {
               // 从数据库或者其他数据源获取书籍列表
               return bookService.getBooks();
           }
       }
```



### @Autowired

`@Autowired` 是 Spring 框架中用于实现依赖注入（Dependency Injection）的核心注解之一，它可以自动装配（注入）所需的依赖对象，无需手动创建实例，简化了组件之间的耦合。

通常用在类的字段、构造方法或 setter 方法上，告诉 Spring 自动注入匹配的 Bean。

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {
    // 1. 字段注入（最常用）直接在需要注入的字段上使用 @Autowired：
    // 自动注入 UserRepository 类型的 Bean
    @Autowired
    private UserRepository userRepository;
    
    public User getUserById(Long id) {
        return userRepository.findById(id);
    }
}



// 2. 在构造方法上使用 @Autowired，Spring 会在创建对象时自动传入所需的依赖：
@Service
public class UserService {
    
    private final UserRepository userRepository;
    
    // 构造方法注入（Spring 4.3+ 后，单构造方法可省略 @Autowired）
    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```





## SSM(Spring+SpringMVC+Mybatis)

官网：spring.io

**spring的核心**

**1.简化开发**

IOC - Inversion of control 控制反转，对象的控制权由程序转到外部 （解耦） ，由IOC容器创建对象并管理

AOP -面向切面编程  不改变源代码的情况下进行增强

事务处理

**2.框架整合**

MyBatis

MyBatis-plus

Struts

Struts2

Hibernate



### Spring framework

spring framework是spring生态圈最基础的项目，是其他项目的根基

**目标：充分解耦**

IOC: 使用IOC容器管理bean

DI: dependency injection 依赖注入，在ioc容器内将有依赖关系的bean进行绑定



### IOC案例

使用spirng框架进行ioc操作

1.POM.xml导入spring-framework依赖  坐标信息：spring-context

2.idea右侧maven刷新依赖

3.resources文件夹右键新建，选择XML configuration File，新建一个spring config文件



### 依赖注入bean配置

**resouces/appconfig.xml文件配置**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

<!--  spring对bean的配置管理文件  -->
<!--   id bean的名字。唯一， class 定义类型  scope：单例或非单例模式 单例时容器只创建一个实例-->
<!--   init-method 和destroy-method代表bean的生命周期对应的方法，在初始化和容器销毁时执行 -->
    <bean id="bookDao" class="com.david.dao.impl.BookDaoImpl" scope="singleton" init-method="init" destroy-method="destroy"></bean>

    <bean id="bookServiceId" class="com.david.service.impl.BookServiceImpl" scope="singleton">
<!-- 依赖注入：1.set方法注入： 配置service和dao的绑定关系 因为是service中引用dao，所以在service内注入dao
    name代表BookServiceImpl中定义的变量名， ref代表引用的bean，注意两者是不一样的 -->
        <property name="bookDao" ref="bookDao"></property>
        <property name="userDao" ref="userDao"></property>
<!--     注入基本类型，使用value属性赋值   -->
        <property name="myName" value="david"></property>
        <property name="myNumber" value="38"></property>
<!--      2.构造函数注入  注意这里的name是构造函数的形参名   -->
        <constructor-arg name="orderDao" ref="orderDaoId"></constructor-arg>
    </bean>

<!--    静态工厂模式创建实例时，要传factory-method -->
    <bean id="orderDaoId" class="com.david.factory.OrderDaoFactory" factory-method="getOrderDao"></bean>

<!--  实例工厂创建对象，实现了FactoryBean接口，不需要factory-method -->
    <bean id="userDao" class="com.david.factory.UserDaoFactory"></bean>

</beans>
```



### bean属性

scope:  prototype | singleton(默认单例)  控制bean的作用范围，是创建单例对象还是非单例对象，单例时容器每次创建的都是同一个对象

适合交给容器管理的bean：表现层/业务层/数据层/工具的对象

不适合的：封装实体的域对象（有数据）

```java
public class Main {
    public static void main(String[] args) {
        // 传统写法
//        BookService bookService = new BookServiceImpl();
//        bookService.save();

        // spring IOC写法
        // 初始化IOC容器，传入的参数就是bean配置文件 得到容器对象
        ApplicationContext ctx = new ClassPathXmlApplicationContext("appconfig.xml");
        // getBean返回的是Object类，强转一下 这里的参数就是bean中设置的id或者name
        BookService bookService = (BookService) ctx.getBean("bookService");
        bookService.save(); // service层save，内部再调用dao层的save
        
                // 静态工厂模式创建bean, xml中配置factory-method
        OrderDao orderDao = (OrderDao) ctx.getBean("orderDaoId");
        orderDao.save();

        // 实例工厂创建bean, xml配置更简单 （重要，主流方式）
        UserDao userDao = (UserDao) ctx.getBean("userDao");
        userDao.save();


        // bean的生命周期 会先打印book dao init， 执行初始化方法
        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        ctx.registerShutdownHook(); // 注册一个容器关闭时的钩子，当容器关闭时，执行destroy方法 这行代码可以放在任何位置
        bookDao.save();
//        ctx.close(); // 关闭容器， 此时会执行destroy方法 暴力方法
    }
}
```

```java
package com.david.service.impl;
import com.david.dao.BookDao;
import com.david.dao.impl.BookDaoImpl;
import com.david.service.BookService; // 接口

public class BookServiceImpl implements BookService {
    // 使用DI进行依赖注入，不需要new创建实例
    // private BookDao bookDao = new BookDaoImpl();

    // 只定义，不创建
    private BookDao bookDao;
    public void save() {
        System.out.println("book service save...");
        bookDao.save(); // 调用数据层save
    }

    // DI关键：创建一个set方法，给spring容器调用，让容器进行创建，获得bookDao实例
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
}
```

```java
package com.david.dao.impl;
import com.david.dao.BookDao; // 接口

public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save...");
    }
}
```



### bean的生命周期函数配置

1. xml配置方式
2. 接口实现方式， 2个接口：  InitializingBean，DisposableBean（常用）

```xml
<!--   init-method 和destroy-method代表bean的生命周期对应的方法，在初始化和容器销毁时执行 -->
<bean id="bookDao" class="com.david.dao.impl.BookDaoImpl" scope="singleton" init-method="init" destroy-method="destroy"></bean>
```

service实现类，实现2个spirng提供的接口 

```java
package com.david.service.impl;

import com.david.dao.BookDao;
import com.david.dao.OrderDao;
import com.david.dao.UserDao;
import com.david.dao.impl.BookDaoImpl;
import com.david.service.BookService;
import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;

public class BookServiceImpl implements BookService, InitializingBean, DisposableBean {

    // 使用DI进行依赖注入，不需要new创建实例
//    private BookDao bookDao = new BookDaoImpl();

    // 只定义，不创建
    private BookDao bookDao;
    private UserDao userDao; // 再注入一个依赖

    private OrderDao orderDao;
    private int myNumber; // 注入基本类型
    private String myName; // 注入基本类型

    // spring实例化bean时，会自动调用构造函数 私有构造方法也能调用（利用反射）
    public BookServiceImpl(OrderDao orderDao) {
        this.orderDao = orderDao;
        System.out.println("BookServiceImpl constructor is running ...");
    }
    public void save() {
        System.out.println("book service save...");
        // 下面的bean都是通过依赖注入创建的
        bookDao.save();
        userDao.save();
        orderDao.save();
        System.out.println("myNumber==>"+myNumber+" myName===>"+myName);
    }

    // 引用类型  DI(依赖注入)：set注入：创建一个set方法，给spring容器调用，让容器进行创建，获得bookDao实例
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    // 依赖注入 基本类型
    public void setMyName(String myName) {
        this.myName = myName;
    }

    public void setMyNumber(int myNumber) {
        this.myNumber = myNumber;
    }

    //  两个接口需要实现的方法，分别用来实现bean销毁和创建的生命周期方法，这种方式就不需要在xml中配置init-method和destroy-method了
    @Override
    public void destroy() throws Exception {
        System.out.println("book service destory...");
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("book service init...");
    }
}

```



### autowire自动注入

用的很少，实际开发时使用注解

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--  创建bean  -->
    <bean name="bookDao" class="com.david.dao.impl.BookDaoImpl"></bean>
<!--  autowire自动装配，不用自己写property，会根据type自动去调set方法注入 byType时bean需要唯一 -->
    <bean name="bookService1" class="com.david.service.impl.BookServiceImpl" autowire="byType"/>
</beans>
```



### 集合注入

比较少用到，学下格式

```xml
<bean name="listDao" class="com.david.dao.impl.ListDaoImpl">
        <property name="array">
            <array>
                <value>1000</value>
                <value>2000</value>
            </array>
        </property>
        <property name="list">
            <list>
                <value>david1</value>
                <value>david2</value>
            </list>
        </property>
    </bean>
```



### 第三方数据源注入



### 加载properties



### 容器总结

```java
package org.example;

import org.springframework.context.support.ClassPathXmlApplicationContext;

import javax.sql.DataSource;

// Press Shift twice to open the Search Everywhere dialog and type `show whitespaces`,
// then press Enter. You can now see whitespace characters in your code.
public class Main {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("appConfig.xml");
       	 // 写法1 传入bean的id 需要强转
        // DataSource dataSource = (DataSource) ctx.getBean("dataSource");
        // 写法2 只传入类的class, 但是这个bean必须唯一
       //  DataSource dataSource = ctx.getBean(DataSource.class);
        // 写法3 传入名字和类class， 不用强转
        DataSource dataSource1 = ctx.getBean("dataSource", DataSource.class);
        System.out.println(dataSource1);
    }
}
```





### 注解开发定义bean

```java
// 使用@Component定义bean 2种写法 
@Component("bookDao")
public class BookDaoImpl implements BookDao {
    
}
// 不传名字, 这种写法需要传入class获得bean -》 getBean(BookDao.class)
@Component
public class BookDaoImple2 implements BookDao {}
```

```xml
// 配置文件通过组件扫描加载bean 不用再定义bean
<context:component-scan base-package="com.david"></context:component-scan>
```

### @Component的3个衍生注解

**这3个衍生注解和@Component的功能一样，方便在不同的层级进行区分**

1. @Controller 用于表现层bean定义
2. @Service 用于业务层bean定义
3. @Repository 用于数据层bean定义



### 纯注解开发

1. 定义一个config类，把之前在xml中定义的bean 改写到这个config类中
2. getBean读取配置文件方式，改为调用读取配置类，调不同的方法

```java
package com.david.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

// 纯注解开发的配置类，替代appConfig.xml配置文件

// @Configuration用于设定当前类为配置类
// @ComponentScan用于设置扫描文件路径，用来获取bean信息
@Configuration
@ComponentScan("com.david.dao")
public class SpringConfig {
}
```

```java
package com.david;

import com.david.config.SpringConfig;
import com.david.dao.BookDao;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;


// Press Shift twice to open the Search Everywhere dialog and type `show whitespaces`,
// then press Enter. You can now see whitespace characters in your code.
public class Main {
    public static void main(String[] args) {
        // 1. 通过读取配置文件创建容器
//        ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("appConfig.xml");

        // 2. 纯注解开发，通过读取配置类创建容器，就不需要appConfig.xml了， 传入config类
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookDao bookDao = (BookDao) ctx.getBean("bookDao111");
        bookDao.save();
        System.out.println(bookDao);
    }
}
```





### 注解方式依赖注入

```java
package com.david.service.impl;

import com.david.dao.UserDao;
import com.david.service.BookService;
import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;

@Service("bookService1")
public class BookServiceImpl implements BookService {

    // 注解方式依赖注入，自动装配 不需要再提供set方法注入  原理：暴力反射对私有属性访问，所以无需set方法
    @Autowired
//    @Qualifier("UserDao") // 指定要装配的bean名称， 当存在多个相同类型的bean时需要指定
    private UserDao userDao;

    // 基本类型注入 一般使用配置文件注入
    @Value("${name}")
    private String name;
    @Override
    public void save() {
        System.out.println("book service save "+name);
        userDao.save();
    }
}

```





### 第三方Bean管理





### 第三方Bean依赖注入





### XML配置和注解方式区别总结

![image-20250911181537993](D:\typora-img\image-20250911181537993.png)