Section 1.2 SQL基本语句
============================
## 1. SQL通用语法
- **以分号结尾**： SQL语句可以单行或者多行书写，以`;`结尾;
- **使用空格**：SQL语句可以使用空格或者缩进来增强语句的可读性，在执行SQL语句时，所有的空格都会被忽略；
- **SQL语句的大小写**：MySQL数据库的SQL语句不区分大小写。但是，习惯上**关键字使用大写，而对于所有列和表名使用小写**，这样做使得代码更易于阅读和调试；
- **SQL语句注释**：(1) 单行注释时使用`--`注释内容或者`#`注释内容（MySQL特有）； (2) 多行注释时使用`/*注释内容*/`；

## 2. SQL语句的分类
- DDL(Data Definition Language)： 数据定义语言，用来定义数据库对象（数据库，表，字段）；
- DML(Data Manipulation Language)： 数据操作语言， 用来对数据表中的数据进行增删改操作；
- DQL(Data Query Language)：数据查询语言，用来查询数据库中表的记录；
- DCL(Data Control Language)：数据控制语言，用来创建数据库用户，控制数据库的访问权限；

----------------
## 3. DDL语句
### 3.1 数据库操作
#### 3.1.1 查询数据库
**查询所有数据库**
```
SHOW DATABASES;
```
**查询当前数据库**
```
SHOW DATABASE();
```
    
#### 3.1.2 创建数据库
```
CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集名] [COLLATE 排序规则];
```
上面的语句中所有的方括号中的内容都是可选的， 第一个括号中的`[IF NOT EXISTS]`如果没有并且你创建的Database已经存在则会报错。
    
#### 3.1.3 删除数据库
```
DROP DATABASE [IF EXISTS] 数据库名;
```

#### 3.1.4 使用数据库
```
USE 数据库名;
```

### 3.2 表操作-查询
- **查询当前数据库所有表**：`SHOW TABLES;`

- **查询表结构**: `DESC 表名;`

- **查询指定表的建表语句**： `SHOW CREATE TABLE 表名;`

 ### 3.3 表操作-创建
  ```sql
    CREATE TABLE 表名(
      字段1 字段类型[COMMENT 字段1注释]
      字段2 字段类型[COMMENT 字段2注释]
      字段3 字段类型[COMMENT 字段3注释]
   )[COMMENT 标注释]
  ```

### 3.4 表操作-数据类型
#### 3.4.1 数值类型 
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

#### 3.4.2 字符串类型
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

#### 3.4.3 日期类型
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
```sql
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

### 3.5 表操作-修改
**添加字段**: `ALTER TABLE 表名 ADD 字段名 类型（长度） [COMMENT 注释] [约束]`

例子： 在上面的表格`emp`中增加一个新的字段`nickname`（不超过20个字符）。

```sql
alter table emp add nickname varchar(20) comment '昵称';
```


**修改数据类型**： `ALTER TABLE 表名 MODIFY 字段名 新数据类型（长度）`


**修改字段名和字段类型**：`ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型（长度） [COMMENT 注释] [约束]`


例子： 将`emp`表中的`nickname`字段修改为`username`，类型位`VARCHAR(30)`。

```sql
alter table emp change nickname username varchar(30) comment '用户名' ;
```


**删除字段**: `ALTER TABLE 表名 DROP 字段名`

例子： 将`emp`表中的`username`字段删除。

```sql
alter table emp drop username;
```

<img src="../pictures/SQLS12P7.png" alt="实际例子" width="600"/>

**修改表名** `ALTER TABLE 表名 RENAME TO 新表名`


例子： 将`emp`表名修改位`employee`。

```sql 
alter table emp rename to employee;
```

<img src="../pictures/SQLS12P8.png" alt="实际例子" width="600"/>

### 3.6 表操作-删除
**删除表**： `DROP TABLE[IF EXISTS] 表名`；

**删除指定表，并重新创建该表**：`TRUNCATED TABLE 表名`;

----------------
## 4. MySQL图形化界面工具： DataGrip
[DataGrip](https://www.jetbrains.com/datagrip/)

----------------
## 5. DML语句（增加，修改以及删除）
### 5.1 添加数据 `INSERT`
#### 给指定字段添加数据
```sql
INSERT INTO 表名(字段名1， 字段名2,...) VALUES(值1，值2，...);
```


#### 给全部字段添加数据
```sql 
INSERT INTO 表名 VALUES(值1，值2，...);
```

#### 批量添加数据

```sql 
INSERT INTO 表名(字段名1，字段名2,...) VALUES(值1，值2，...),(值1，值2，...),(值1，值2，...);
```

```sql 
INSERT INTO 表名 VALUES(值1，值2，...),(值1，值2，...),(值1，值2，...);
```
> [!NOTE]
>  - 插入数据时，指定的字段顺序需要与值的顺序时一一对应的；
>  - 字符串和日期类型数据应该包含在引号之中；
>  - 插入的数据大小，应该在字段的规定范围之内；

**例子**：给employee表段添加一条数据。因为时添加完整的一条数据，所以有两种写法： 

```sql
insert into employee(id, workno, name,gender,age,idcard,entrydate) values(1,'1','Itcast','男',10,'123456789012345678','2000-01-01');
```

```sql
insert into employee values(2, '2','Itcast2','男',18,'123456789012345678','2005-01-01');
```

如果要一次性插入两条数据：
```sql
insert into employee values(3, '3','Itcast3','男',38,'123456789012345678','2006-01-01'),(4, '4','Itcast4','男',48,'123456789012345678','2007-01-01');
```

### 5.2 修改数据 `UPDATE'
#### 修改数据
```sql
UPDATE 表名 SET 字段名1 = 值1, 字段名2 = 值2,...[WHERE 条件];
```

**例子1：更新id=1的数据**

```sql 
update employee set name = 'itcast1' where id = 1;
```

```sql
update employee set name = 'itcast1', gender = '女' where id = 1;
```

例子2： 更新所有人的数据。

```sql
update employee set entrydate = '2008-01-01';
```


### 5.3 删除数据 `DELETE`
#### 删除数据
```sql 
DELETE FROM 表名 [WHERE 条件]
```

例子1： 删除女性数据。

```sql
delete from employee where gender = '女';
```

例子2： 删除所有数据。
```sql 
delete from employee;
```
> [!WARNING]
> 使用`UPDATE`和`DELETE`语句时，将对所有数据进行修改和删除，这是一个非常危险的操作。

*********************************************************************
## 6.DQL语句
### 6.1 基本检索语句
#### 检索多个（一个）字段
`SELECT`关键字之后给出多个字段名(字段1, 字段2,...)，并以逗号分隔可以用于检索多个指定字段，
```sql
SELECT 字段1, 字段2,... FROM 表名;
```
除了指定所需的字段外，SELECT语句还可以检索所有的字段而不必逐个字段出他们的字段名，此时可以使用通配符(`*`)来达到，
```sql
SELECT * FROM 表名;
```
> [!WARNING]
> - **减少通配符`*`的使用**：虽然使用通配符会使得你自己省事，因为不需要明确列出所有字段名，但检索不需要的字段通常会降低检索和应用程序的性能；
> - **NULL与不匹配**：再通过过滤条件选择出不具有特定值的行时，你可能希望返回具有NULL值的行，但是这是不行的。因为未知具有特殊含义，数据库不知道它们是否匹配，所以在匹配过滤或不匹配过滤时不返回它们；


#### 设置别名
```sql 
SELECT 字段1 [as 别名1], 字段2[as 别名2],... FROM 表名;
```

#### 去除重复记录
SELECT会返回所有匹配的行，但如果你不需要一些行重复出现，那么可以使用关键字`DISTINCT`,
```sql
SELECT DISTINCT 字段列表 FROM 表名;
```
> [!WARNING]
> **不可以部分使用DISTINCT**：DISTINCT关键字应用于所有列，而不仅是前置于它的列。对于如下的SQL语句
> ```sql
> SELECT DISTINCT 字段1, 字段2;
> ```
> 只有字段1和字段2均相同的行才不会重复出现在检索结果里；

### 6.2 条件查询(`WHERE`)
#### `WHERE`子句
数据库表中一般包含大量的数据，我们很少需要检索所有的行，通常只需要根据**过滤条件**来提取表的子集，其语法如下：
```sql
SELECT 字段列表 FROM 表名 WHERE 条件列表;
```
#### `WHERE`子句操作符
|比较运算符|功能|逻辑运算符|功能|
|:---: | :---: | :---: | :---: |
|>/>=|大于/大于等于|AND 或 &&| 并且|
|</<=|小于/小于等于|OR 或 \|\| |或者|
|=/!=|等于/不等于  |NOT 或 \|| 非|
|BETWEEN...AND...  |在某个范围内（含最大最小值）|||
|IN (...)|在IN之后的列表中的值|||
|LIKE|模糊匹配(`_ `匹配单个字符，`%`匹配任意字符)|||
|IS NULL|是否无值 （NULL与仅仅包含空字符串或者空格不同）|||

#### 模糊匹配
**通配符**是来匹配值的一部分的特殊字符，在模糊匹配中我们经常使用`%`通配符和`_`通配符。
- 百分号通配符`%`表示**任何字符**出现**任意次数**；
- 下划线通配符`_`只匹配**单个字符**而不是多个字符；

> [!WARNING]
>  **使用通配符的注意事项**
> - 注意尾部空格，有可能会干扰匹配；
> - 虽然通配符可以匹配任何东西，但是`NULL`是例外；
>   
>  **使用通配符的技巧**： 通配符搜索的处理比其他搜索要花的时间更长，这里给出一些技巧
> - 不要过度使用通配符；
> - 在确实需要通配符时，除非绝对有必要，否则不要把它们用在搜索模式的开始处；
> - 仔细注意通配符的位置；
>   
>  **逻辑运算符优先级问题**
> - 建议使用圆括号： 在同时包含AND和OR的子句中建议使用圆括号，不要依赖逻辑运算符的计算次序。`AND`的优先级比`OR`更高；
> - `IN`与`OR`具有相同的功能，但一般更推荐使用`IN`，因为其运行效率更高，而且条件更加清晰；
> - 在MySQL中，NOT可以对IN/BETWEEN/EXISTS取反；


#### 例子:寻找用户推荐人（leetcode 584）
在表`Customer`中，`id`是该表的主键列，该表的每一行表示一个客户的id、姓名以及推荐他们的客户的id。找出那些没有被 id = 2 的客户推荐的客户的姓名，以任意顺序返回结果表。结果格式如下所示：

| Column Name | Type    |
|    :---:    |  :---:  |
| id          | int     |
| name        | varchar |
| referee_id  | int     |

**输入**

|id | name | referee_id |
|:---:|:---:|:---:|
| 1  | Will | null       |
| 2  | Jane | null       |
| 3  | Alex | 2          |
| 4  | Bill | null       |
| 5  | Zack | 1          |
| 6  | Mark | 2          |

**输出**
| name |
|:---: |
| Will |
| Jane |
| Bill |
| Zack |

**代码**
```sql
SELECT name FROM Customer WHERE referee_id NOT IN (2) OR referee_id IS NULL;
```

> [!NOTE] 
> 本题最大的坑在于NULL与不匹配的问题。MySQL使用三值逻辑: TRUE, FALSE和UNKNOWN。任何与`NULL`值进行的比较都会与第三种值UNKNOWN做比较。这个“任何值”包括 NULL本身！这就是为什么 MySQL 提供`IS NULL`和`IS NOT NULL`两种操作来对`NULL`特殊判断。因此，在`WHERE`语句中我们需要做一个额外的条件判断`referee_id IS NULL`。
> 
> 下面的答案同样是错的
```sql
SELECT name FROM customer WHERE referee_id = NULL OR referee_id <> 2;
```

### 6.3 聚合函数
我们经常需要汇总数据而不需要把他们实际检索出来，为此SQL提供了专门的函数。**聚合函数**是指运行在行组上，计算和返回单个值的函数，其常见的例子有以下作用
- 确定表中行数；
- 获得表中行组的和；
- 找出表列的最大值，最小值和平均值；

#### 常见聚合函数

|聚合函数|作用|
|:---:|:---:|
|AVG()|返回某列平均值|
|COUNT()|返回某列的行数|
|MAX()|返回某列的最大值|
|MIN()|返回某列的最小值|
|SUM()|返回某列的和|


> [!WARNING]
> - **忽略NULL**： 与前面介绍的各类SQL语句情况一样，`AVG()/MAX()/MIN()/SUM()`忽略列值为`NULL`的行；
> 
> - **COUNT()函数两种形式**
>    - `COUNT(*)`对表中行的数目进行计数，不管表列中包含的是空值还是非空值；
>    - `COUNT(column)`对特定列中具有值的行进行计数，忽略`NULL`的行；
> 
> - **MAX()/MIN()函数可以作用于字符串数据**;


#### 聚合不同值
我们可以使用关键字`DISTINCT`来聚合包含不同值的行。若不指定则默认使用关键字`ALL`，则对所有的行执行计算。

下面给出一个例子：
```sql
SELECT AVG(DISTINCT prod_price) FROM products;
```
这计算了products表中产品价格prod_price不同的所有产品的产品价格prod_price平均值。如果不加`DISTINCT`关键字，则计算所有产品的产品价格平均值，无论是否重复或者相同。

> [!WARNING]
> - 在`COUNT`函数中若指定字段名，则可以使用`DISTINCT`关键字，比如`SELECT COUNT(DISTINCT prod_price) FROM products;`
> - 在不指定字段名时不可以使用`COUNT(DISTINCT)`命令；

### 6.4 排序检索(`ORDER BY`)
通过SELECT语句检索出来的结果中，检索结果的顺序并没有意义。虽然数据一般以它在底层表中的顺序出现，但是如果数据后来进行过更新或删除，则此顺序将会受到MySQL重用回收存储空间的影响。**在关系数据库中，如果不明确规定排序顺序，则不应该假定检索出的数据的顺序有意义。** 排序检索数据的语法如下所示
```sql
SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方向1, 字段2 排序方向2;
```
#### 指定排序方向
- `ASC`是升序关键字，如果不指定排序方向，那么将默认升序；
- `DESC`是降序关键字，如果想要在多个字段上进行降序排序，则需要对每个字段均指定`DESC`关键字；


### 6.5 分组查询(`GROUP BY`)
#### 创建分组
在SQL中分组允许把数据分为多个逻辑组，以便能对每个组进行聚合计算，使用`GROUP BY`子句进行分组的语法如下：
```sql
SELECT 字段列表 FROM 表名[WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件]
```

> [!NOTE]
> **使用`GROUP BY` 分组的重要规定**
> - GROUP BY 子句可以包含任意数目的列，这使得对分组进行嵌套，为数据分组提供更细致的控制；
> - 如果在GROUP BY 子句中嵌套了分组，数据将在最后规定的分组上进行汇总；
> - GROUP BY 子句中列出的每个列都必须是检索列或有效表达式（但不可以是聚集函数）；
> - 如果在SELECT中使用表达式，则必须在GROUP BY子句中指定相同的表达式，不可以用别名；
> - 如果分组列中具有NULL值，则NULL将会作为一个分组返回。如果列中有多行NULL值，则将它们分为一组；
> - GROUP BY 子句必须要在WHERE子句之后，在ORDER BY子句之前；

#### 过滤分组
除了能使用`GROUP BY`创建分组之外，MySQL还允许过滤分组，规定包括哪些分组，排除哪些分组。 

> [!NOTE]
> **`WHERE`与`HAVING`的区别**
> - 执行时机不同：`WHERE`是分组前进行过滤，不满足`WHERE`条件不参与分组，而`HAVING`是分组之后对结果进行过滤；
> - 过滤动机不同：`WHERE`过滤指定的是行，而`HAVING`过滤的是分组；
> - 判断条件不同： `WHERE`不能对聚合函数进行判断，而`HAVING`可以；
> - 在有聚合函数时，它们的执行顺序是执行顺序是 `WHERE` -> 聚合函数 -> `HAVING`。 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无意义；

**下面给出一个例子**
其中`vent_id`为供货商id，`prod_price`为商品价格，下面给出了供应了2个及以上且价格在10元及以上的产品的供应商id：
```sql
SELECT vent_id, COUNT(*) AS num_prods
FROM products
WHERE prod_price >=10
GROUP BY vend_id
HAVING COUNT(*)>=2;
```
**步骤**
- 首先，通过`WHERE prod_price >=10`子句过滤行，得到所有产品价格在10元及以上的产品数据。
- 接着，通过`GROUP BY vend_id`子句根据供应商id创建分组；
- 最后，通过`HAVING COUNT(*)>=2`过滤分组，从而得到分组中数据个数大于等于2的分组；
  
#### 分组与排序
我们首先给出ORDER BY和GROUP BY的区别
||ORDER BY| GROUP BY|
|:---:|:---:|:---:|
|输出顺序|排序产生的输出|根据分组输出，但是输出顺序可能不是分组顺序|
|使用方式|任何字段均可使用（甚至非选择的列也可以使用）|只可能使用选择列或者表达式列，而且必须使用每个选择列表达式|
|使用场景|不一定需要|需要按组计算聚集函数时则必须使用|

> [!WARNING]
> **不要忘记`ORDER BY`**： 一般在使用GROUP BY子句时，也应该给出ORDER BY子句，这是保证数据正确排序的唯一方法！

### 6.6 分页检索(`LIMIT`)
SELECT语句返回所有匹配的行，如果想要返回第一行或者某几行我们可以通过`LIMIT`关键字来进行控制：
```sql 
SELECT 字段列表 FROM 表名 LIMIT 起始索引,查询记录数;
```
> [!NOTE]
> - 起始索引从0开始, `起始索引 = （查询页码-1）*每页显示记录`;
> - 分页查询是数据库的方言，不同的数据库有不同的实现； MySQL是LIMIT；
> - 如果查询的是第一页数据，起始索引可以省略，直接简写为LIMIT 10.


### 6.7 DQL语句执行顺序
#### DQL语句的编写顺序
```sql
SELECT 字段列表 FROM 表名列表 WHERE 条件列表 GROUP BY 分组字段列表 HAVING 分组后条件列表 ORDER BY 排序字段列表 LIMIT 分页参数;
```
#### DQL语句的执行顺序
执行顺序和编写顺序不同，首先执行`FROM`，接着执行`WHERE`以及`GROUP BY`，再执行`SELECT`， 最后执行`ORDER BY`和`LIMIT`。 
```sql
FROM -> WHERE -> FROUP BY -> HAVING -> SELECT ->LIMIT
```


------------------------------------------------------------------------------------------------
## 7.DCL语句

DCL英文全称是Data Control Language，用来管理数据库用户，控制数据库的访问权限。
#### （1）用户管理
**查询用户**

用户信息都存在了mysql文件夹下的user表中； 一般根据用户名和主机地址同时定位：当前用户只能在哪一个主机上访问SQL。
```sql
use mysql
select * from user;
```

**创建用户**
```sql
create user '用户名'@'主机名' identified by '密码';
```

**修改用户密码**
```sql
alter user '用户名'@'主机名' identified with mysql_native_passport '新密码';
```

**删除用户**
```sql
drop user '用户名'@'主机名';
```

#### （2）权限控制
Mysql 定义了很多权限，我们讲如下几种

**查询权限** 

```sql
show grants for '用户名'@'主机名'
```

**授予权限**

```sql
grant 权限列表 on 数据库名.表名 to '用户名'@'主机名'
```

**撤销权限** 

```sql
revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名'
```


**注意**
1. 授权列表有多个时需要用逗号分隔；
2. 可以使用*代表通配；

