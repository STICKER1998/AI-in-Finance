MySQL
=================================

# Chapter 1 MySQL 基础知识
## Section 1.1 MySQL概述
### 1. 数据库的相关概念
**数据库(DataBase, DB)**： 存储数据的仓库， 数据是有组织的进行储存

**数据管理系统(Database Management System, DBMS)**： 操纵和管理数据的大型软件

**SQL(Structed Query Language)**：操作关系型数据库的编程语言，定义了一套操作关系型数据库统一标准

### 2. MySQL下载
Windows: [MySQL](https://dev.mysql.com/downloads/installer/)
在安装时需要选择Full Install， 需要耗时几分钟
<img src="../pictures/SQLS11P1.png" alt="MySQL 安装" width="600"/>

### 3. MySQL启动与停止
方法1： win+R -> services.msc -> 找到MySQL80 右键可以控制启动以及停止

方法2： cmd 以管理员身份运行 -> 输入命令行 
`net start mysql80` / `net stop mysql80`
<img src="../pictures/SQLS11P2.png" alt="MySQL 启动与停止" width="600"/>

### 4. MySQL客户端连接
方法1： 利用MySQL提供的客户端命令行工具 `MySQL 8.0 Command Line Client`
<img src="../pictures/SQLS11P3.png" alt="MySQL 连接" width="600"/>

方法2： 利用windows命令行连接`mysql -u root -p`

### 5. 数据模型
1. 数据模型： DBMS用来操作数据库， 而每个数据库包含多个表结构，数据就是存储在表结构中， 这个表被称为**二维表**。

2. 关系型数据库(RDBMS)：
   - 概念： 建立在关系模型基础上，由多张互相连接的二维表组成的数据库。
   - 特点： （1）使用表存储数据， 格式统一， 便于维护； （2） 使用SQL语言操作，标准统一，使用方便；


## Section 1.2 SQL
### 1. SQL通用语法
（1） SQL语句可以单行或者多行书写，以分号结尾。

（2） SQL语句可以使用空格/缩进来增强语句的可读性。

（3） MySQL数据库的SQL语句不区分大小写， 关键字建议使用大写

（4） 注释：
   - 单行注释： `--`注释内容或者`#`注释内容（MySQL特有）
   - 多行注释： `/*注释内容*/`
  
### 2. SQL语句的分类
（1） DDL(Data Definition Language)： 数据定义语言，用来定义数据库对象（数据库，表，字段）；

（2） DML(Data Manipulation Language)： 数据操作语言， 用来对数据表中的数据进行增删改操作；

（3） DQL(Data Query Language)：数据查询语言，用来查询数据库中表的记录；

（4） DCL(Data Control Language)：数据控制语言，用来创建数据库用户，控制数据库的访问权限；

### 3. DDL语句
（1） 数据库操作
  - 查询： 查询所有数据库 `SHOW DATABASES`， 查询当前数据库  `SHOW DATABASE()`
<img src="../pictures/SQLS12P1.png" alt="数据库查询" width="600"/>
    
  - 创建： `CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集名] [COLLATE 排序规则] `； 上面的语句中所有的方括号中的内容都是可选的， 第一个括号中的`[IF NOT EXISTS]`如果没有并且你创建的Database已经存在则会报错。
<img src="../pictures/SQLS12P2.png" alt="数据库创建" width="600"/>
    
  - 删除： `DROP DATABASE [IF EXISTS] 数据库名`
  - 使用： `USE 数据库名`




