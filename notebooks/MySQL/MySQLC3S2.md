## Section 3.2 窗口函数
### 窗口
窗口限定一个范围，他可以理解为满足某些条件的记录集合，窗口函数也就是在窗口范围内执行的函数。

### 基本语法
```sql
<函数名> over (partition by <分组的列> order by <排序的列> rows between <起始行> and <终止行>)
```


```sql
CREATE TABLE SQL_5(
    cid varchar(4),
    sname varchar(4),
    score int
);


insert into SQL_5 (cid, sname, score) values('001', '张三', 78);
insert into SQL_5 (cid, sname, score) values('001', '李四', 82);
insert into SQL_5 (cid, sname, score) values('002', '小明', 90);
insert into SQL_5 (cid, sname, score) values('001', '王五', 67);
insert into SQL_5 (cid, sname, score) values('002', '小红', 85);
insert into SQL_5 (cid, sname, score) values('002', '小刚', 62);
```

> [!NOTE]
> 注意partition by 和group by的区别
> - 前者不会压缩行数，但是后者会
> - 后者只能选取分组的列和聚合的列：如下两段代码中使用窗口函数时既可以返回所有的列，还可以返回使用聚合函数之后的结果，而对于group by而言只能返回分组列和聚合函数作用后的结果。

```sql
select *, sum(score) over (partition by cid) as "班级总分" FROM sql_5;
```
```sql
select cid, sum(score) as "班级总分" FROM sql_5 GROUP BY cid;
```



