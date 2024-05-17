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
```sql
SELECT *, row_number() over (partition by cid order by score desc) as '不可并列排名',
          rank() over (partition by cid order by score desc) as '跳跃可并列排名',
          dense_rank() over (partition by cid order by score desc) as '不可并列排名'
FROM
    sql_5;
```


#### 聚合函数
sum avg count max min

#### 跨行行数
lag, lead

### 4.TOPN问题以及分类聚合问题
**数据库**
```sql
CREATE TABLE SQL_6(
    cid varchar(4),
    sname varchar(4),
    course varchar(10),
    score int
);

insert into SQL_6(cid, sname, course, score) values('001', '张三','地理', 70);
insert into SQL_6(cid, sname, course, score) values('001', '李四','地理', 52);
insert into SQL_6(cid, sname, course, score) values('001', '王五','地理', 88);

insert into SQL_6(cid, sname, course, score) values('001', '张三','数学', 77);
insert into SQL_6(cid, sname, course, score) values('001', '李四','数学', 56);
insert into SQL_6(cid, sname, course, score) values('001', '王五','数学', 97);

insert into SQL_6(cid, sname, course, score) values('001', '张三','英语', 66);
insert into SQL_6(cid, sname, course, score) values('001', '李四','英语', 61);
insert into SQL_6(cid, sname, course, score) values('001', '王五','英语', 81);

insert into SQL_6(cid, sname, course, score) values('001', '张三','语文', 78);
insert into SQL_6(cid, sname, course, score) values('001', '李四','语文', 60);
insert into SQL_6(cid, sname, course, score) values('001', '王五','语文', 80);

insert into SQL_6(cid, sname, course, score) values('002', '小明','地理', 45);
insert into SQL_6(cid, sname, course, score) values('002', '小红','地理', 82);
insert into SQL_6(cid, sname, course, score) values('002', '小刚','地理', 66);

insert into SQL_6(cid, sname, course, score) values('002', '小明','数学', 49);
insert into SQL_6(cid, sname, course, score) values('002', '小红','数学', 82);
insert into SQL_6(cid, sname, course, score) values('002', '小刚','数学', 67);

insert into SQL_6(cid, sname, course, score) values('002', '小明','英语', 55);
insert into SQL_6(cid, sname, course, score) values('002', '小红','英语', 87);
insert into SQL_6(cid, sname, course, score) values('002', '小刚','英语', 50);

insert into SQL_6(cid, sname, course, score) values('002', '小明','语文', 58);
insert into SQL_6(cid, sname, course, score) values('002', '小红','语文', 87);
insert into SQL_6(cid, sname, course, score) values('002', '小刚','语文', 71);
```

**问题1：返回每个同学的信息及其考试分数排名前三的科目及其信息**
```sql
SELECT *
FROM
    (SELECT *, row_number() over (partition by sname order by score desc) as rn
    FROM sql_6) as temp
WHERE rn<=3;
```

**问题2：求出每门课程都高于班级课程平均分的学生**
```sql
WITH
t1 as (SELECT *, AVG(score) OVER(PARTITION BY cid, course) as avg FROM sql_6),

t2 as (SELECT *, score - avg as del FROM t1)

SELECT
    sname
FROM
    t2
GROUP BY
    sname
HAVING
    min(del)>=0;

```

### 5.连续问题
#### 问题1：连续登陆问题
根据用户的`user_id`以及`login_date`查找出连续三天登陆的用户的`user_id`
```sql
CREATE TABLE SQL_8(
    user_id varchar(2),
    login_date date
);
```
```sql
INSERT INTO SQL_8(user_id, login_date) VALUES('A','2022-09-02');
INSERT INTO SQL_8(user_id, login_date) VALUES('A','2022-09-03');
INSERT INTO SQL_8(user_id, login_date) VALUES('A','2022-09-04');
INSERT INTO SQL_8(user_id, login_date) VALUES('B','2021-11-25');
INSERT INTO SQL_8(user_id, login_date) VALUES('B','2021-12-30');
INSERT INTO SQL_8(user_id, login_date) VALUES('C','2022-01-01');
INSERT INTO SQL_8(user_id, login_date) VALUES('C','2022-04-04');
INSERT INTO SQL_8(user_id, login_date) VALUES('C','2022-09-03');
INSERT INTO SQL_8(user_id, login_date) VALUES('C','2022-09-04');
INSERT INTO SQL_8(user_id, login_date) VALUES('C','2022-09-05');
INSERT INTO SQL_8(user_id, login_date) VALUES('A','2022-09-03');
INSERT INTO SQL_8(user_id, login_date) VALUES('D','2022-10-20');
INSERT INTO SQL_8(user_id, login_date) VALUES('D','2022-10-21');
INSERT INTO SQL_8(user_id, login_date) VALUES('A','2022-10-03');
INSERT INTO SQL_8(user_id, login_date) VALUES('D','2022-10-22');
INSERT INTO SQL_8(user_id, login_date) VALUES('D','2022-10-23');
```

**分析**
- useid要相同，表示同一用户；
- 每一用户每行记录以登陆时间从小到大排序；
- 后一行记录比前一行登陆时间多一天；
- 数据行数据大于N；

```sql
WITH 
t0 as (SELECT DISTINCT * FROM SQL_8 ),
t1 as (SELECT *, ROW_NUMBER() over (PARTITION BY user_id ORDER BY SQL_8.login_date) as rn FROM t0),
t2 as(SELECT *, date_sub(login_date, INTERVAL rn DAY) as sub_date FROM t1)
SELECT DISTINCT user_id FROM t2 GROUP BY user_id,  sub_date having count(*)>=3;
```

这个解法的精髓在于可以生成**自增伪列**，如果使用login_date减掉伪列后的结果中出现连续出现三个一样的结果时，那么说明该用户连续三天登录。

下面这个解法先是创造两个新的列用于分别表示login_date滞后一天和滞后两天的序列。当login_date和两个同行中滞后值分别相差1和2时，则表示该用户连续三天登录。

```sql
WITH
t0 as (SELECT DISTINCT * FROM SQL_8 ),
t1 as (SELECT *,
              lag(login_date, 1) OVER(PARTITION BY user_id ORDER BY login_date) as l1_login_date,
              lag(login_date, 2) OVER(PARTITION BY user_id ORDER BY login_date) as l2_login_date FROM t0),
t2 as (SELECT *, datediff(login_date, l1_login_date) as diff1, datediff(login_date, l2_login_date) as diff2 from t1)
SELECT DISTINCT user_id FROM t2 WHERE diff1=1 AND diff2=2 GROUP BY user_id;
```

> [!WARNING]
> 下面的代码看似正确，但实际上是错误的。
>
> **反例**：用户A先连续两天登录，然后再连续2天登录，中间间隔的时间段中其他用户（B,C,D）无连续登陆时，用户A也会被判定为连续3天登录。
```sql
WITH
t0 as (SELECT DISTINCT * FROM SQL_8 ),
t1 as (SELECT *, lag(login_date, 1) OVER(PARTITION BY user_id ORDER BY login_date) as last_login_date FROM t0),
t2 as (SELECT *, datediff(login_date,last_login_date) as diff from t1)
SELECT DISTINCT user_id FROM t2 WHERE diff=1 GROUP BY user_id HAVING count(*)>=2;
```

#### 问题2：连续进球问题
编写SQL语句，检索出连续进了3球的队员编号`player_id`。
```sql
CREATE TABLE SQL_9(
    player_id varchar(2),
    score int,
    score_time datetime
);
```
```sql
INSERT INTO SQL_9 (player_id, score, score_time)
VALUES ('A3', 1,'2022-09-20 19:00:14') ,('A3', 1,'2022-09-20 19:01:04'),
       ('A3', 3,'2022-09-20 19:01:16') ,('B1', 3,'2022-09-20 19:02:05'),
       ('A2', 2,'2022-09-20 19:02:25') ,('B3', 2,'2022-09-20 19:02:54'),
       ('A1', 3,'2022-09-20 19:03:10') ,('A1', 2,'2022-09-20 19:03:34'),
       ('B2', 2,'2022-09-20 19:03:58') ,('B1', 3,'2022-09-20 19:04:07'),
       ('A1', 1,'2022-09-20 19:04:19') ,('A1', 2,'2022-09-20 19:04:31');
```

**解法**
```sql
WITH t1 as (SELECT *, lag(player_id,1) OVER(ORDER BY score_time) as l1_player_id,
    lag(player_id,2) OVER(ORDER BY score_time) as l2_player_id FROM SQL_9)
SELECT DISTINCT player_id FROM t1 WHERE player_id = l1_player_id AND player_id = l2_player_id GROUP BY player_id;
```

**错误解法**
```sql
WITH t1 as (SELECT *, lag(player_id,1) OVER(ORDER BY score_time) as layer_player_id FROM SQL_9)
SELECT DISTINCT player_id FROM t1 WHERE player_id = layer_player_id GROUP BY player_id HAVING count(*)>=2;
```

#### 问题3：连续区间起止点id查找
查找出`log_id`中的连续区间，并返回其起点`start_id`和终止点`end_id`。譬如下面的`log_id`中（1，2，3）为一个连续区间，其`start_id=1`，`end_id=3`》
```sql
CREATE TABLE SQL_10(
    log_id int
);
INSERT INTO SQL_10(log_id) VALUES(1),(2),(3),(7),(8),(10);
```


