## Section 3.2 窗口函数
### 1.什么是窗口
#### 窗口
窗口限定一个范围，他可以理解为满足某些条件的记录集合，窗口函数也就是在窗口范围内执行的函数。

#### 基本语法
```sql
<函数名> OVER (PARTITION BY <分组的列> ORDER BY <排序的列> ROWS BETWEEN <起始行> AND <终止行>)
```

### 2.窗口的确定
#### 样本
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

#### 分组子句(`PARTITION BY`)

> [!NOTE]
> **注意partition by 和group by的区别**
> - 前者不会压缩行数，但是后者会
> - 后者只能选取分组的列和聚合的列：如下两段代码中使用窗口函数时既可以返回所有的列，还可以返回使用聚合函数之后的结果，而对于group by而言只能返回分组列和聚合函数作用后的结果。

```sql
select *, sum(score) over (partition by cid) as "班级总分" FROM sql_5;
```

```sql
select cid, sum(score) as "班级总分" FROM sql_5 GROUP BY cid;
```

#### 排序子句（`ORDER BY`)
其使用方式和一般的排序检索一致，如果不要排序可以不写或者`ORDER BY NULL`。

#### 窗口子句 (`ROWS`)
窗口子句的描述：`ROWS BETWEEN <起始行> AND <终止行>`
- 起始行：N preceding/ unbounded preceding
- 当前行：current row
- 终止行：N following/unbounded following

**例子**

- `ROWS BETWEEN unbounded preceding AND current row`：从当前分区的起始行到当前行
- `ROWS BETWEEN 2 preceding AND current row`：从前面2行到当前行
- `ROWS BETWEEN current row AND unbounded following`：从当前行到当前分区的最后一行
- `ROWS BETWEEN current row AND 1 following`：从当前行到下一行

> [!NOTE]
> - 如果不使用排序子句`ORDER BY`和窗口子句`ROWS`，则默认使用`ROWS BETWEEN unbounded preceding AND unbounded following`;
> - 如果只使用排序子句`ORDER BY`，但不使用窗口子句`ROWS`，则默认使用`ROWS BETWEEN unbounded preceding AND current row`;


#### 总体流程
- 通过`PARTITION BY`和`ORDER BY`子句确定大窗口；（即每个分区的起始处unbounded preceding和终止处unbounded following）
- 通过`ROWS`子句针对每一行数据确定小窗口；
- 对每行的小窗口内的数据执行行数并生成新的列；


### 3.函数分类
#### 排序函数
rank, dense_rank, row_number



#### 聚合函数

#### 跨行行数
lag, lead


