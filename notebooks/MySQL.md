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
  - **查询**： 查询所有数据库 `SHOW DATABASES`， 查询当前数据库  `SHOW DATABASE()`
<img src="../pictures/SQLS12P1.png" alt="数据库查询" width="600"/>
    
  - **创建**： `CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集名] [COLLATE 排序规则] `； 上面的语句中所有的方括号中的内容都是可选的， 第一个括号中的`[IF NOT EXISTS]`如果没有并且你创建的Database已经存在则会报错。
<img src="../pictures/SQLS12P2.png" alt="数据库创建" width="600"/>
    
  - **删除**： `DROP DATABASE [IF EXISTS] 数据库名`
<img src="../pictures/SQLS12P3.png" alt="数据库删除" width="600"/>

  - **使用**： `USE 数据库名`
<img src="../pictures/SQLS12P4.png" alt="数据库使用以及查询在哪个数据库" width="600"/>

（2）表操作-查询
  - **查询当前数据库所有表**：`SHOW TABLES`;
    
  - **查询表结构**: `DESC 表名`；
   
  - **查询指定表的建表语句**： `SHOW CREATE TABLE 表名`

 （3）表操作-创建
  ```
    CREATE TABLE 表名(
      字段1 字段类型[COMMENT 字段1注释]
      字段2 字段类型[COMMENT 字段2注释]
      字段3 字段类型[COMMENT 字段3注释]
   )[COMMENT 标注释]
  ```

（4）表操作-数据类型
   **数值类型** 
   |类型             |大小    |有符号范围|
   |---              |---     |---|
   |TINYINT     |1 byte       | (-128, 127)|
   |SMALLINT    |2 bytes      | (-32768, 32767)|
   |MEDIUMINT   |3 bytes   | (-8388608, 8388607)|
   |INT / INTEGER |4 bytes| (-2147483648, 2147483647)|
   |BIGINT      |8 bytes|(-2^63, 2^63-1)|
   |FLOAT       |4 bytes|(-3.402823466 E+38, 3.402823466351 E+38)|
   |DOUBLE      |8 bytes| (-1.797 E308～1.797 E+308) |
   |DECIMAL     |        |依赖于精度M和标度D的值|         

**字符串类型**
   | 类型 | 大小 | 描述 |
   | :---: | :---: | :---: |
   | CHAR | $0-255$ bytes | 定长字符串 |
   | VARCHAR | 0 - 65535 bytes | 变长字符串 |
   | TINYBLOB | 0 - 255 bytes | 不超过 255 个字符的二进制数据 |
   | TINYTEXT | 0 - 255 bytes | 短文本字符串 |
   | BLOB | 0 - 65535 bytes | 二进制形式的长文本数据 |
   | TEXT | 0 - 65535 bytes | 长文本数据 |
   | MEDIUMBLOB | 0-16 777 215 bytes | 二进制形式的中等长度文本数据 |
   | MEDIUMTEXT | 0-16 777 215 bytes | 中等长度文本数据 |
   | LONGBLOB | 0- 4 294 967 295 bytes | 二进制形式的极大文本数据 |
   | LONGTEXT | 0- 4 294 967 295 bytes | 极大文本数据 |

   一般我们只是用`CHAR` 和`VARCHAR`，其余类型一般不使用，我们虽然最开始都要指定定长，但是CHAR对于多于位置会用0补充（性能好，不需要判断占用空间），但是对于VARCHAR来说它会根据输入来判断占用空间（性能相对较差）。
   - 例子1： 如果我们想要存储用户名，规定其最长长度不得大于50位，此时使用`VARCHAR(50)`更好，因为用户输入的用户名不一样长。
   - 例子2： 如果想要储存用户性别， 此时推荐使用`CHAR(1)`，因为性别的输入固定长度的。

