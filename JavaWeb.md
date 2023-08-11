## MySQL

DBMS：数据库管理系统，MySQL就是一个数据库管理系统（开源免费的中小型数据库）

SQL: Structured Query Language ，结构化查询语言，操作关系型数据库的编程语言，定义操作所有关系型数据库的统一标准。



### 下载和配置

下载：https://downloads.mysql.com/archives/community/

配置环境变量：

新增变量：MYSQL_HOME： '安装路径'

path配置：%MYSQL_HOME%\bin



### 关系型数据库

**由多张能互相连接的二维表组成的数据库， 通过表存数据的就是关系型数据库**

优点：

1.都使用表结构，格式一致，易维护。

2.使用通用SQL语言操作，可用于复杂查询 。

3.数据存在磁盘中，安全。



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

3、字符串类（CHAR, VARCHAR, TINYBLOB, BLOB, LONGBLOB, TEXT, LONGTEXT....）al

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
    usergender char(10), -- char 定长字符串，固定用10个字符空间，存储性能高，但浪费空间
    score double(5,2), -- 分数：范围0~100，double第一位是总长度，第二位是小数位数， 保留2位小数，所以总长度是3+2=5
    telephone varchar(15) --电话 最大长度不超过15位
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
```

