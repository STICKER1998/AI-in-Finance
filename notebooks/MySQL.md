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


## Section 1.2 SQL基本语句
### 1. SQL通用语法
（1） SQL语句可以单行或者多行书写，以分号结尾。

（2） SQL语句可以使用空格/缩进来增强语句的可读性。

（3） MySQL数据库的SQL语句不区分大小写， 关键字建议使用大写

（4） 注释：
   - 单行注释： `--`注释内容或者`#`注释内容（MySQL特有）
   - 多行注释： `/*注释内容*/`
   - 
----------------
### 2. SQL语句的分类
（1） DDL(Data Definition Language)： 数据定义语言，用来定义数据库对象（数据库，表，字段）；

（2） DML(Data Manipulation Language)： 数据操作语言， 用来对数据表中的数据进行增删改操作；

（3） DQL(Data Query Language)：数据查询语言，用来查询数据库中表的记录；

（4） DCL(Data Control Language)：数据控制语言，用来创建数据库用户，控制数据库的访问权限；

----------------
### 3. DDL语句
#### （1）数据库操作
  - **查询**： 查询所有数据库 `SHOW DATABASES`， 查询当前数据库  `SHOW DATABASE()`
<img src="../pictures/SQLS12P1.png" alt="数据库查询" width="600"/>
    
  - **创建**： `CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集名] [COLLATE 排序规则] `； 上面的语句中所有的方括号中的内容都是可选的， 第一个括号中的`[IF NOT EXISTS]`如果没有并且你创建的Database已经存在则会报错。
<img src="../pictures/SQLS12P2.png" alt="数据库创建" width="600"/>
    
  - **删除**： `DROP DATABASE [IF EXISTS] 数据库名`
<img src="../pictures/SQLS12P3.png" alt="数据库删除" width="600"/>

  - **使用**： `USE 数据库名`
<img src="../pictures/SQLS12P4.png" alt="数据库使用以及查询在哪个数据库" width="600"/>

#### （2）表操作-查询
  - **查询当前数据库所有表**：`SHOW TABLES`;
    
  - **查询表结构**: `DESC 表名`；
   
  - **查询指定表的建表语句**： `SHOW CREATE TABLE 表名`

 #### （3）表操作-创建
  ```
    CREATE TABLE 表名(
      字段1 字段类型[COMMENT 字段1注释]
      字段2 字段类型[COMMENT 字段2注释]
      字段3 字段类型[COMMENT 字段3注释]
   )[COMMENT 标注释]
  ```
 <img src="../pictures/SQLS12P5.png" alt="例子" width="600"/>

#### （4）表操作-数据类型
   **数值类型** 
| 类型 | 大小 | 有符号(SIGNED)范围 | 无符号(UNSIGNED)范围 | 描述 |
| :---: | :---: | :---: | :---: | :---: |
| TINYINT | 1 byte | (-128, 127) | (0, 255) | 小整数值 |
| SMALLINT | 2 bytes | (-32768, 32767) | (0, 65535) | 大整数值 |
| MEDIUMINT | 3 bytts | (-8388608, 8388607) | (0, 16777215) | 大整数值 |
| INT或 INTEGER | 4 bytes | (-2147483648, 2147483647) | (0, 4294967295) | 大整数值 |
| BIGINT | 8 bytes | (-2^63, 2^63-1) | (0, 2^64-1) | 极大整数值 |
| FLOAT | 4 bytes | (-3.402823466 E+38,3.402823466351 E+38) | 0 和 (1.175494351 E-38,3.402823466 E+38) | 单精度深点数值 |
| DOUBLE | 8 bytes | (-1.7976931348623157 E+308,1.7976931348623157 E+308) | 0 和 (2.2250738585072014 E-308,1.7976931348623157 E+308) | 双精度浑点数值 |
| DECIMAL |  | 依赖于M(精度) 和D (标度) 的值 | 依赖于M (精度) 和D (标度) 的值 | 小数值 (精碥定点数) |

**字符串类型**
   | 类型 | 大小 | 描述 |
   | :---: | :---: | :---: |
   | CHAR | 0-255 bytes | 定长字符串 |
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

**日期类型**
| 类型 | 大小 | 范围 | 格式 | 描述 |
| :---: | :---: | :---: | :---: | :---: |
| DATE | 3 | 1000-01-01 至 9999-12-31 | YYYY-MM-DD | 日期值 |
| TIME | 3 | -838: 59: 59 至 838:59:59 | HH:MM:SS | 时间值或持综时间 |
| YEAR | 1 | 1901 至 2155 | YYYY | 年份值 |
| DATETIME | 8 | 1000-01-01 00:00:00 至 9999-12-31 23:59:59 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值 |
| TIMESTAMP | 4 | 1970-01-01 00:00:01 至 2038-01-19 03:14:07 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值，时间载 |

- 例子： 如果我们想要存储用户的生日，则使用`DATE`即可，因为生日只需要记录年月日。

**实际例子**
设计一张员工信息表，要求如下:
- 1. 编号 (纯数字)
- 2. 员工工号 (字符串类型, 长度不超过10位)
- 3. 员工姓名 (字符串类型, 长度不超过10位)
- 4. 性别(男/女, 存储一个汉字)
- 5. 年龄 (正常人年龄，不可能存储负数)
- 6. 身份证号 (二代身份证号均为18位，身份证中有X这样的字符)
- 7. 入职时间 (取值年月日即可)

其SQL语句如下：
```
create table emp(
   id INT comment '编号',
   workno VARCHAR(10) comment '工号',
   name VARCHAR(10) comment '姓名',
   gender CHAR(1) comment '性别',
   age TINYINT UNSIGNED comment '年龄',
   idcard CHAR(18) comment '身份证号',
   entrydate DATE comment '入职时间'
) comment '员工信息表';
```

<img src="../pictures/SQLS12P6.png" alt="实际例子" width="600"/>

#### （5）表操作-修改
**添加字段**: `ALTER TABLE 表名 ADD 字段名 类型（长度） [COMMENT 注释] [约束]`

例子： 在上面的表格`emp`中增加一个新的字段`nickname`（不超过20个字符）。

`  alter table emp add nickname varchar(20) comment '昵称';`


**修改数据类型**： `ALTER TABLE 表名 MODIFY 字段名 新数据类型（长度）`


**修改字段名和字段类型**：`ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型（长度） [COMMENT 注释] [约束]`


例子： 将`emp`表中的`nickname`字段修改为`username`，类型位`VARCHAR(30)`。

` alter table emp change nickname username varchar(30) comment '用户名' ;`


**删除字段**: `ALTER TABLE 表名 DROP 字段名`

例子： 将`emp`表中的`username`字段删除。

` alter table emp drop username;`

<img src="../pictures/SQLS12P7.png" alt="实际例子" width="600"/>

**修改表名** `ALTER TABLE 表名 RENAME TO 新表名`


例子： 将`emp`表名修改位`employee`。

`alter table emp rename to employee;`

<img src="../pictures/SQLS12P8.png" alt="实际例子" width="600"/>

#### （6）表操作-删除
**删除表**： `DROP TABLE[IF EXISTS] 表名`；

**删除指定表，并重新创建该表**：`TRUNCATED TABLE 表名`;

----------------
### 4. MySQL图形化界面工具： DataGrip
[DataGrip](https://www.jetbrains.com/datagrip/)

----------------
### 5. DML语句（增加，修改以及删除）
#### （1）添加数据 `INSERT`
**给指定字段添加数据**： `INSERT INTO 表名(字段名1， 字段名2,...) VALUES(值1，值2，...);`

**给全部字段添加数据**: `INSERT INTO 表名 VALUES(值1，值2，...);`

**批量添加数据**

`INSERT INTO 表名(字段名1，字段名2,...) VALUES(值1，值2，...),(值1，值2，...),(值1，值2，...);`

`INSERT INTO 表名 VALUES(值1，值2，...),(值1，值2，...),(值1，值2，...);`

注意事项
   - 插入数据时，指定的字段顺序需要与值的顺序时一一对应的；
   - 字符串和日期类型数据应该包含在引号之中；
   - 插入的数据大小，应该在字段的规定范围之内；

例子：给employee表段添加一条数据。因为时添加完整的一条数据，所以有两种写法： 

`insert into employee(id, workno, name,gender,age,idcard,entrydate) values(1,'1','Itcast','男',10,'123456789012345678','2000-01-01');`

`insert into employee values(2, '2','Itcast2','男',18,'123456789012345678','2005-01-01');`

如果要一次性插入两条数据：

`insert into employee values(3, '3','Itcast3','男',38,'123456789012345678','2006-01-01'),(4, '4','Itcast4','男',48,'123456789012345678','2007-01-01');`

#### （2）修改数据 `UPDATE'

**修改数据**：`UPDATE 表名 SET 字段名1 = 值1, 字段名2 = 值2,...[WHERE 条件];`

例子1：更新id=1的数据。

`update employee set name = 'itcast1' where id = 1;`

`update employee set name = 'itcast1', gender = '女' where id = 1;`

例子2： 更新所有人的数据。

`update employee set entrydate = '2008-01-01';`


#### （3）删除数据 `DELETE`
**删除数据**： `DELETE FROM 表名 [WHERE 条件]`

例子1： 删除女性数据。

`delete from employee where gender = '女';`

例子2： 删除所有数据。

`delete from employee;`

注意：使用`UPDATE`和`DELETE`语句时，将对所有数据进行修改和删除，这是一个非常危险的操作。

----------------
### 6.DQL语句
#### （1）基本查询语句
**查询多个字段** 

`select 字段1, 字段2,... from 表名;`

`select * from 表名;`

**设置别名**

`select 字段1 [as 别名1], 字段2[as 别名2],... from 表名;`

**去除重复记录**

`select distinct 字段列表 from 表名;`

#### (2) 条件查询（where)

`select 字段列表 from 表名 where 条件列表;`

#### (3) 聚合函数

1.将一列数据作为整体，进行纵向计算

2.常见聚合函数

`count max min avg sum`

3.null值不参与计算

#### (4) 分组查询

`select 字段列表 from 表名[where 条件] group by 分组字段名 [having 分组后过滤条件]`

1.where 与 having区别
   
   执行时机不同： where是分组前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果进行过滤；
   
   判断条件不同：where不能对聚合函数进行判断，而having可以；

2.注意
   执行顺序： where>聚合函数>having；

   分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无意义；


#### (5) 排序查询

`select 字段列表 from 表名 order by 字段1 排序方式1, 字段2 排序方式2;`

ASC: 升序    DESC：降序

#### （6） 分页查询
`select 字段列表 from 表名 limit 起始索引,查询记录数;`

1. 起始索引从0开始, 起始索引 = （查询页码-1）*每页显示记录;
2. 分页查询是数据库的方言，不同的数据库有不同的实现； mySQL是LIMIT；
3. 如果查询的是第一页数据，起始索引可以省略，直接简写为limit 10.


#### (7) DQL语句执行顺序
**DQL语句的编写顺序**

```
select 字段列表 from 表名列表 where 条件列表 group by 分组字段列表 having 分组后条件列表 order by 排序字段列表 limit 分页参数
```

**DQL语句的执行顺序**

执行顺序和编写顺序不同，首先执行`from`，接着执行`where`以及`group by`，再执行`select`， 最后执行`order by`和`limit`。 

```
from -> where -> group by -> having -> select ->limit
```

### 7.DCL语句

DCL英文全称是Data Control Language，用来管理数据库用户，控制数据库的访问权限。
#### （1）用户管理
**查询用户**

用户信息都存在了mysql文件夹下的user表中； 一般根据用户名和主机地址同时定位：当前用户只能在哪一个主机上访问SQL。
```
use mysql
select * from user;
```

**创建用户**
```
create user '用户名'@'主机名' identified by '密码';
```

**修改用户密码**
```
alter user '用户名'@'主机名' identified with mysql_native_passport '新密码';
```

**删除用户**
```
drop user '用户名'@'主机名';
```

#### （2）权限控制
Mysql 定义了很多权限，我们讲如下几种

**查询权限** 

```show grants for '用户名'@'主机名'```

**授予权限**

```grant 权限列表 on 数据库名.表名 to '用户名'@'主机名'```

**撤销权限** 

``` revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名'```


**注意**
1. 授权列表有多个时需要用逗号分隔；
2. 可以使用*代表通配；

## Section 1.3 函数
**函数**是指一段可以直接被另一段程序调用的程序或者代码。

### 1.字符串函数
MySQL提供了很多字符串函数，常用的如下：

- `concat(s1,s2,...sn)`: 字符串拼接函数，将s1,s2,...sn拼接成一个字符串
- `lower(str)/upper(str)`：将字符串str全部转换成小写/大写
- `lpad(str,n,pad)/rpad(str,n,pad)`:左/右填充，用字符串pad对str的左边/右边进行填充，达到n个字符串的长度
- `trim(str)`:去掉字符串头部和尾部的空格
- `substring(str,start,len)`:返回字符串str从start位置起的len个长度的字符串

### 2.数值函数
- `ceil(x)\floor(x)`: 向上\向下取整；
- `mod(x,y)`：返回x/y的模；
- `rand()`：生成(0,1)之间的随机数
- `round(x)`：四舍五入；

### 3.日期函数
- `curdate()`：返回当前日期;
- `curtime()`:返回当前时间；
- `now()`：返回当前日期和时间；
- `year(date)/month(date)/day(date)`：获取指定date的年/月/；
- `date_add(date, interval expr type)`：返回一个日期/时间值加上一个时间间隔expr后的时间值；
- `datediff(date1, date2)`：返回起始时间date1和结束时间date2之间的天数;

### 4.流程函数
- `if(value, t, f)`： 如果value的值为true，则返回值t，否则返回值f；
- `ifnull(value1, value2)`: 如果value1不是null，则返回value1，否则返回value2；
- `case when [con1] then [result1] [con2] then [result2] ...  else [default] end` : 如果条件1满足则返回结果1，...，否则返回default；
- `case [expr] when [value1] then [result1] when [value2] then [result2]... else [default] end`：如果expr等于值1，则返回结果1，...，否则返回default；

## Section 1.4 约束
### 1.概述
- **概念**： 约束是作用于表中字段上的规则，用于限制存储在表中的数据；
- **目的**：保证数据库中数据的正确性，有效性和完整性；
- **分类**
  | 约束  | 描述   | 关键字  |
  | :---: | :---: | :---:   |
  |非空约束| 限制该字段的数据不能为null| NOT NULL|
  |唯一约束| 保证该字段的所有数据都是唯一的，不能重复的|UNIQUE|
  |主键约束|逐渐使一行数据的唯一标识，要求非空且唯一|PRIMARY KEY|
  |默认约束|保存数据时，如果未指定该字段的值，则采用默认值|DEFAULT|
  |检查约束|保证字段值满足某一个条件| CHECK|
  |外键约束|用来让两张表的数据之间连接，保证数据的一致性和完整性|




### 2.约束的具体使用
**案例**
| 字段名 | 字段含义 | 字段类型 | 约束条件 | 约束关键字 |
|:---: | :---: | :---: |:---: |:---:|
|id|ID唯一标识| int|主键且自动增长|PRIMARY KEY, AUTO_INCREMENT|
|name| 姓名 | varchar(10)| 不为空，且唯一| NOT NULL, UNIQUE|
|age| 年龄 | int | 大于0，并且小于等于120| CHECK |
|status| 状态| char(1)| 如果没有指定该值，则默认为1| DEFAULT|
|gender| 性别| char(1)| 无 | |

```
create table user(
    id int primary key auto_increment comment 'id主键',
    name varchar(10) not null unique  comment '姓名',
    age int check ( age>0 and age<=120) comment '年龄',
    status char(1) default '1' comment '状态',
    gender char(1) comment '性别'
) comment '用户表';
```

### 3.外键约束

**概念**： 外键用来将两张表的数据之间建立连接，从而保证数据的一致性和完整性。

**添加外键**

```
create table 表名(
   字段名 数据类型,
   ...
   [CINSTRAINT] [外键名称] foreign key (外键字段名) references 主表(主表列名)
);
```

```
 alter table 表名 add constraint 外键名称 foreign key(外键字段名) references 主表(主表列名);
```

**删除外键**

```
alter table 表名 drop foreign key 外键名称;
```

**删除/更新行为**
| 行为 | 说明 |
|:---:|:---:|
|No action|当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新。|
|restrict|当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新。|
|cascade|当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则也删除/更新外键在子表中的记录。|
|set null|当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null。|
|set default|附表有更新时，子表将外键列设置为一个默认的值。|


```
 alter table 表名 add constraint 外键名称 foreign key(外键字段名) references 主表(主表列名) on update cascade on delete cascade;
```

## Section 1.5 多表关系
### 1. 多表关系
项目开发中，在进行数据库表结构设计时，会根据业务需求以及业务模块之间的关系，分析并设计表结构，由于业务之间相互关联，所以各个表结构之间也存在着各种联系，主要分为：一对一，多对一（一对多）和多对多几种多表关系。

- 多对一（一对多）： 在考虑部门和员工关系时，一个部门可以对应多个员工，一个员工只对应一个部门。此时，**选择在多的一方建立外键，指向一的一方的主键**。
- 多对多： 在考虑学生与课程的关系时，一个学生可以选修多门课程，一门课程也可供多个学生选择。此时，**建立第三张中间表，中间表至少包含两个外键，分别关联两方主键**。
- 一对一： 一对一的关系主要用于单表拆分，将一张表的基础字段放在一张表中，将其他详情字段放在另一张表中，以提高操作效率。此时，**在任意一方加入主键，关联另外一方的主键，并设置外键为唯一的（unique）**。

### 2.多表查询
**概念**
**分类**
- 连接查询
     - 内连接：相当于查询左表和右表交集部分数据；
     - 外连接：
         - 左外连接：查询左表的所有信息以及左表和右表交集部分数据；
         - 右外连接：查询右表的所有信息以及左表和右表交集部分数据；
     - 自连接：查询一张表的所有数据；
- 子查询


###  3.内连接
- 隐式内连接
```
select 字段列表 from 表1,表2 where 条件...;
```

- 显式内连接
```
select 字段列表 from 表1 [inner] join 表2 on 连接条件...;
```

思考：为什么需要两种内连接的方式？

### 4.外连接
**左外连接**：查询表1（左表）的所有数据，包含表1和表2交集部分的数据
```
select 字段列表 from 表1 left [outer] join 表2 on 条件...;
```

**右外连接**：查询表2（右表）的所有数据，包含表1和表2交集部分的数据
```
select 字段列表 from 表1 right [outer] join 表2 on 条件...;
```

### 5.自连接
**自连接查询语法**
```
select 字段列表 from 表A 别名A join 表A 别名B on 条件...;
```
自连接可以是内连接也可以是外连接。

自连接时要给表其两个别名。

### 6.联合查询
对于union 查询，就是要把多次查询的结果合并起来，形成一个新的查询结果集；

```
select 字段列表 from 表A...
union [all]
select 字段列表 from 表B...
```

注意：这里union all会直接合并所有查询结果，union会删除重复项。

### 7.子查询
**概念**：SQL语句中嵌套select语句，称为嵌套查询，又称为子查询。
```
select * from t1 where column1 = (Select column1 from t2);
```
**分类**
- 根据子查询结果不同我们可以将其又分为：
   - **标量子查询**：子查询返回的结果是单个值（数字，字符串，日期等等），最简单的形式，这种子查询称为标量子查询。
   - **列子查询**：子查询返回的结果是一列，常见操作有：in，not in，any，some，all；
        | 操作符 | 描述 |
        | :---:  | :---: |

   - **行子查询**：子查询返回的结果是一行（可以是多列）， 常见操作有：=，！=，in，not in；
   - **表子查询**：子查询返回的结果是多行多列，常见操作为in；
- 根据子查询位置可以将其分为：**where之后，from之后，select之后**。


## Section 1.6 事务
### 1.事物简介
事物是一组操作的集合，是一个不可分割的工作单位，事物会把所有的操作作为一个整体一起向系统提交或撤销操作请求，这些操作**要么同时成功，要么同时失败**。

### 2.事务操作
**查看/设置事务提交方式**

```
select @@autocommit;
-- 设置为手动提交
set @@autocommit = 0; 
```

**提交事务**

```
commit;
```

**回滚事务**

```
rollback;
```

**开启事务**

```
start transaction 或者 begin
```

### 3.事务的四大特性（ACID)
- 原子性：事务是不可分割的最小操作单元，要么全部执行成功，要么全部失败；
- 一致性：事务完成时，必须使所有的数据都保持一致状态；
- 隔离性： 数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行；
- 持久性：事务一旦提交或者回滚，它对数据库中的数据的改变就是永久的；

### 4.并发事务问题
| 问题 | 描述 |
| :---: | :---: |
|脏读| 一个事务读到另外一个事务还没有提交的数据|
|不可重复读| 一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读|
|幻读|一个事务按照条件查询数据时，没有对应的数据行，但在插入数据时，有发现这行数据已经存在，好像出现了“幻影”|
