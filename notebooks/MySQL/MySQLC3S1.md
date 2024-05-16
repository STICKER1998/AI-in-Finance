Chapter 3 经典问题
=============
## Section 3.1 行列转换问题
我们考虑如下一个问题，将如图所示的表1转换成表2

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
