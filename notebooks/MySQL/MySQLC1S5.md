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
```sql
select 字段列表 from 表1,表2 where 条件...;
```

- 显式内连接
```sql
select 字段列表 from 表1 [inner] join 表2 on 连接条件...;
```
**例子: 产品销售分析 I(leetcode 1068)**

销售表`Sales`：
- `(sale_id, year)`是销售表`Sales`的主键（具有唯一值的列的组合）。
- `product_id`是关联到产品表 Product 的外键（reference 列）。
- 该表的每一行显示`product_id`在某一年的销售情况

| Column Name | Type  |
|:---:|:---:|
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |

产品表`Product`：`product_id` 是表的主键（具有唯一值的列）,该表的每一行表示每种产品的产品名称。

| Column Name  | Type    |
|:---:|:---:|
| product_id   | int     |
| product_name | varchar |

编写解决方案，以获取 Sales 表中所有 sale_id 对应的 product_name 以及该产品的所有 year 和 price 。


**输入**

`Sales`表

| sale_id | product_id | year | quantity | price |
|:---:|:---:|:---:|:---:|:---:| 
| 1       | 100        | 2008 | 10       | 5000  |
| 2       | 100        | 2009 | 12       | 5000  |
| 7       | 200        | 2011 | 15       | 9000  |

`Product`表：

| product_id | product_name |
|:---:|:---:|
| 100        | Nokia        |
| 200        | Apple        |
| 300        | Samsung      |


**输出**

| product_name | year  | price |
|:---:|:---:|:---:|
| Nokia        | 2008  | 5000  |
| Nokia        | 2009  | 5000  |
| Apple        | 2011  | 9000  |

**解答**
```sql
SELECT
Product.product_name, Sales.year, Sales.price
FROM
Product INNER JOIN Sales
ON
Product.product_id = Sales.product_id;
```

### 4.外连接
**左外连接**：查询表1（左表）的所有数据，包含表1和表2交集部分的数据
```sql
select 字段列表 from 表1 left [outer] join 表2 on 条件...;
```

**右外连接**：查询表2（右表）的所有数据，包含表1和表2交集部分的数据
```sql
select 字段列表 from 表1 right [outer] join 表2 on 条件...;
```

**例子: 使用唯一标识码替换员工ID (leetcode 1378)**

`Employees`表： 在SQL中，`id`是这张表的主键。这张表的每一行分别代表了某公司其中一位员工的名字和ID 。
| Column Name   | Type    |
|:---:|:---:|
| id            | int     |
| name          | varchar |

`EmployeeUNI`表：在 SQL中，`(id, unique_id)`是这张表的主键。这张表的每一行包含了该公司某位员工的ID和他的唯一标识码(unique ID)。

| Column Name   | Type    |
|:---:|:---:|
| id            | int     |
| unique_id     | int     |

展示每位用户的唯一标识码，如果某位员工没有唯一标识码，使用 null 填充即可。你可以以任意顺序返回结果表。
示例 1：

**输入**

`Employees`表:

| id | name     |
|:---:|:---:|
| 1  | Alice    |
| 7  | Bob      |
| 11 | Meir     |
| 90 | Winston  |
| 3  | Jonathan |

`EmployeeUNI`表:

| id | unique_id |
|:---:|:---:|
| 3  | 1         |
| 11 | 2         |
| 90 | 3         |

**输出**

| unique_id | name     |
|:---:|:---:|
| null      | Alice    |
| null      | Bob      |
| 2         | Meir     |
| 3         | Winston  |
| 1         | Jonathan |

**解答**
```sql
SELECT 
    EmployeeUNI.unique_id, Employees.name
FROM 
    Employees
LEFT JOIN 
    EmployeeUNI 
ON 
    Employees.id = EmployeeUNI.id;
```


```sql
SELECT 
name, bonus
FROM
Employee LEFT JOIN Bonus
ON 
Employee.empId = Bonus.empId
HAVING 
(bonus <1000 OR bonus IS NULL);
```

### 5.自连接
**自连接查询语法**
```sql
select 字段列表 from 表A 别名A join 表A 别名B on 条件...;
```
自连接可以是内连接也可以是外连接。

自连接时要给表其两个别名。

### 6.联合查询
对于union 查询，就是要把多次查询的结果合并起来，形成一个新的查询结果集；

```sql
select 字段列表 from 表A...
union [all]
select 字段列表 from 表B...
```

注意：这里union all会直接合并所有查询结果，union会删除重复项。

### 7.子查询
#### 7.1 概念
SQL语句中嵌套使用`SELECT`语句被称为嵌套查询，又称为**子查询**，其语法如下
```sql
SELECT 字段列表 FROM 表1 WHERE 字段1 操作符 (SELECT 字段2 FROM 表2 WHERE...);
```
> [!WARNING]
> **列必须匹配**
> 在WHERE子句中使用子查询，应该保证**嵌套语句中的SELECT语句具有与WHERE子句中相同数目的列**。
> 通常子查询将会返回单个列并且与单个列匹配，但如果需要也可以使用多个列。

**例子** 给出`customers`,`orders`以及`orderitems`三张表，并给出部分字段名描述
|表名|列名|表述|
|:---:|:---:|:---:|
|customers|cust_name|顾客名|
|customers|cust_contact|顾客联系方式|
|customers/orders|cust_id|顾客id|
|orders/orderitems|order_num|订单号|
|orderitems|prod_id|产品id|

请给出够买产品id为`TNT2`的顾客名字和联系方式。

```sql
SELECT cust_name, cust_contact
FROM customers
WHERE cust_id IN (SELECT cust_id
                  FROM orders
                  WHERE order_num IN (SELECT order_num
                                      FROM orderitems
                                      WHERE prod_id = 'TNT2'));
```

#### 7.2 子查询分类
根据子查询返回的结果不同我们可以将其分为：
- **标量子查询**：子查询返回的结果是单个值（数字，字符串，日期等等），最简单的形式，这种子查询称为标量子查询。
- **列子查询**：子查询返回的结果是一列，常见操作有：in，not in，any，some，all；
- **行子查询**：子查询返回的结果是一行（可以是多列）， 常见操作有：=，！=，in，not in；
- **表子查询**：子查询返回的结果是多行多列，常见操作为in；

根据子查询位置可以将其分为：**where之后，from之后，select之后**。

