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
