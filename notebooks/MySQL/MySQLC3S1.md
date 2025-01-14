Chapter 3 经典问题
=============
## Section 3.1 行列转换问题
### 例子1：成绩汇总表I - 行转列
我们考虑如下一个问题，将如图所示的表1转换成表2

`sql_1`表

| name | subject |  score |
| :---: | :---: | :---: |
| 张三| 语文 |   78 |
| 张三 | 数学 |   88 |
| 张三 | 英语 |   98 |
| 李四 | 语文 |   89 |
| 李四 | 数学 |   76 |
| 李四 | 英语 |   90 |
| 王五 | 语文 |   99 |
| 王五 | 数学 |   66 |
| 王五 | 英语 |   91 |

```sql
create table SQL_1
(
    name varchar(20),
    subject varchar(20),
    score float
);
```

```sql
insert into SQL_1(name, subject, score) values('张三', '语文', 78);
insert into SQL_1(name, subject, score) values('张三', '数学', 88);
insert into SQL_1(name, subject, score) values('张三', '英语', 98);
insert into SQL_1(name, subject, score) values('李四', '语文', 89);
insert into SQL_1(name, subject, score) values('李四', '数学', 76);
insert into SQL_1(name, subject, score) values('李四', '英语', 90);
insert into SQL_1(name, subject, score) values('王五', '语文', 99);
insert into SQL_1(name, subject, score) values('王五', '数学', 66);
insert into SQL_1(name, subject, score) values('王五', '英语', 91);
```

`sql_2`表

| name| 语文 | 数学 | 英语 |
| :---: | :---: | :---: | :---: |
| 张三| 78 | 88 | 98 |
| 李四 | 89 | 76 | 90 |
| 王五 | 99 | 66 | 91 |

**解答**

```sql
SELECT
    name, SUM(语文), SUM(数学), SUM(英语)
FROM
    (SELECT *,
           CASE subject WHEN '语文' THEN SCORE ELSE null END as '语文',
           CASE subject WHEN '数学' THEN SCORE ELSE null END as '数学',
           CASE subject WHEN '英语' THEN SCORE ELSE null END as '英语'
    FROM
        sql_1) as Result
GROUP BY
    name;
```

**分析**
我们首先创建一张临时表增加三个“伪列”
```sql
SELECT *,
           CASE subject WHEN '语文' THEN SCORE ELSE null END as '语文',
           CASE subject WHEN '数学' THEN SCORE ELSE null END as '数学',
           CASE subject WHEN '英语' THEN SCORE ELSE null END as '英语'
    FROM
        sql_1
```
其返回结果为

|name|subject|score|语文|数学|英语|
| :---: | :---: | :---: | :---: |:---: | :---: | 
|张三| 语文 | 78 | 78 | <null> | <null> | 
| 张三 | 数学 | 88 | <null> | 88 | <null> |  
| 张三 | 英语 | 98 | <null> | <null> | 98 |  
| 李四 | 语文 | 89 | 89 | <null> | <null> |  
| 李四 | 数学 | 76 | <null> | 76 | <null> |  
| 李四 | 英语 | 90 | <null> | <null> | 90 |  
| 王五 | 语文 | 99 | 99 | <null> | <null> |  
| 王五 | 数学 | 66 | <null> | 66 | <null> 
| 王五 | 英语 | 91 | <null> | <null> | 91 |  

再使用分组查询与聚合函数来生成最终表。

另外我们还可以使用如下一步法的解答
```sql
SELECT
    name,
    SUM(CASE subject WHEN '语文' THEN SCORE ELSE null END) as '语文' ,
    SUM(CASE subject WHEN '数学' THEN SCORE ELSE null END) as '数学',
    SUM(CASE subject WHEN '英语' THEN SCORE ELSE null END) as '英语'
FROM
    sql_1
GROUP BY
    name;
```

**解题步骤**
- (1) 确定分组列，转换列，数据列
- (2) 生成伪列
- (3) 做分组查询
- (4) 选择合适的聚合函数


注意我们可以使用如下语法保存SELECT查询的表
```sql
CREATE TABLE new_table AS
SELECT column1, column2, ...
FROM existing_table
WHERE condition;
```


### 例子2：成绩汇总表 II — 列转行
我们如果想要将表2反向转换为表1该怎么做？——此时应该使用`UNION ALL`连接

```sql
SELECT name, '语文' as subject, 语文 FROM sql_2
UNION ALL
SELECT name, '数学' as subject, 数学 FROM sql_2
UNION ALL
SELECT name, '英语' as subject, 英语 FROM sql_2
ORDER BY
    name;
```

**解题步骤**
- (1) 确定转换列，非转换列
- (2) 生成新列
- (3) 使用`union all`或者`union`进行合并
- (4) 根据需要进行排序`order by`
