## Section 3.2 窗口函数
### 1.什么是窗口
#### 窗口
窗口限定一个范围，他可以理解为满足某些条件的记录集合，窗口函数也就是在窗口范围内执行的函数。

#### 基本语法
```sql
<函数名> OVER (PARTITION BY <分组的列> ORDER BY <排序的列> ROWS BETWEEN <起始行> AND <终止行>)
```

### 2.窗口的确定
#### 样本：`SQL_5`表记录了不同学生的id，名字和分数。

| cid | sname | score |
| :--- | :--- | :--- |
| 001 | 张三 | 78 |
| 001 | 李四 | 82 |
| 002 | 小明 | 90 |
| 001 | 王五 | 67 |
| 002 | 小红 | 85 |
| 002 | 小刚 | 90 |


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
> - 前者不会压缩行数，但是后者会；
> - 后者只能选取分组的列和聚合的列：如下两段代码中使用窗口函数时既可以返回所有的列，还可以返回使用聚合函数之后的结果，而对于group by而言只能返回分组列和聚合函数作用后的结果；
> - GROUP BY 后生成的结果集与原表的行数和列数都不同；

```sql
select *, sum(score) over (partition by cid) as "班级总分" FROM sql_5;
```

| cid | sname | score | 班级总分 |
| :--- | :--- | :--- | :--- |
| 001 | 张三 | 78 | 227 |
| 001 | 李四 | 82 | 227 |
| 001 | 王五 | 67 | 227 |
| 002 | 小明 | 90 | 265 |
| 002 | 小红 | 85 | 265 |
| 002 | 小刚 | 90 | 265 |


```sql
select cid, sum(score) as "班级总分" FROM sql_5 GROUP BY cid;
```

| cid | 班级总分 |
| :--- | :--- |
| 001 | 227 |
| 002 | 265 |


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
          dense_rank() over (partition by cid order by score desc) as '连续可并列排名'
FROM
    sql_5;
```
| cid | sname | score | 不可并列排名 | 跳跃可并列排名 | 连续可并列排名 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 001 | 李四 | 82 | 1 | 1 | 1 |
| 001 | 张三 | 78 | 2 | 2 | 2 |
| 001 | 王五 | 67 | 3 | 3 | 3 |
| 002 | 小明 | 90 | 1 | 1 | 1 |
| 002 | 小刚 | 90 | 2 | 1 | 1 |
| 002 | 小红 | 85 | 3 | 3 | 2 |


#### 聚合函数
sum avg count max min
这些函数的使用方法与之前一样

#### 跨行行数
lag, lead

```sql
-- 同一班级内，成绩比自己低一名的分数是多少
SELECT *, LAG(score, 1) OVER (PARTITION BY cid ORDER BY score) as '低一名的分数' FROM SQL_5;
```
| cid | sname | score | 低一名的分数 |
| :--- | :--- | :--- | :--- |
| 001 | 王五 | 67 | null |
| 001 | 张三 | 78 | 67 |
| 001 | 李四 | 82 | 78 |
| 002 | 小红 | 85 | null |
| 002 | 小明 | 90 | 85 |
| 002 | 小刚 | 90 | 90 |

```sql
-- 同一班级内，成绩比自己高两名的分数是多少
SELECT *, LEAD(score, 2) OVER (PARTITION BY cid ORDER BY score) as '高两名的分数' FROM SQL_5;
```
| cid | sname | score | 高两名的分数 |
| :--- | :--- | :--- | :--- |
| 001 | 王五 | 67 | 82 |
| 001 | 张三 | 78 | null |
| 001 | 李四 | 82 | null |
| 002 | 小红 | 85 | 90 |
| 002 | 小明 | 90 | null |
| 002 | 小刚 | 90 | null |

### 4.TOPN问题以及分类聚合问题
#### 样本：：`SQL_6`表记录了不同学生的id，名字，课程和对应的课程分数。
| cid | sname | course | score |
| :--- | :--- | :--- | :--- |
| 001 | 张三 | 地理 | 70 |
| 001 | 李四 | 地理 | 52 |
| 001 | 王五 | 地理 | 88 |
| 001 | 张三 | 数学 | 77 |
| 001 | 李四 | 数学 | 56 |
| 001 | 王五 | 数学 | 97 |
| 001 | 张三 | 英语 | 66 |
| 001 | 李四 | 英语 | 61 |
| 001 | 王五 | 英语 | 81 |
| 001 | 张三 | 语文 | 78 |
| 001 | 李四 | 语文 | 60 |
| 001 | 王五 | 语文 | 80 |
| 002 | 小明 | 地理 | 45 |
| 002 | 小红 | 地理 | 82 |
| 002 | 小刚 | 地理 | 66 |
| 002 | 小明 | 数学 | 49 |
| 002 | 小红 | 数学 | 82 |
| 002 | 小刚 | 数学 | 67 |
| 002 | 小明 | 英语 | 55 |
| 002 | 小红 | 英语 | 87 |
| 002 | 小刚 | 英语 | 50 |
| 002 | 小明 | 语文 | 58 |
| 002 | 小红 | 语文 | 87 |
| 002 | 小刚 | 语文 | 71 |

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

| cid | sname | course | score | rn |
| :--- | :--- | :--- | :--- | :--- |
| 002 | 小刚 | 语文 | 71 | 1 |
| 002 | 小刚 | 数学 | 67 | 2 |
| 002 | 小刚 | 地理 | 66 | 3 |
| 002 | 小明 | 语文 | 58 | 1 |
| 002 | 小明 | 英语 | 55 | 2 |
| 002 | 小明 | 数学 | 49 | 3 |
| 002 | 小红 | 英语 | 87 | 1 |
| 002 | 小红 | 语文 | 87 | 2 |
| 002 | 小红 | 地理 | 82 | 3 |
| 001 | 张三 | 语文 | 78 | 1 |
| 001 | 张三 | 数学 | 77 | 2 |
| 001 | 张三 | 地理 | 70 | 3 |
| 001 | 李四 | 英语 | 61 | 1 |
| 001 | 李四 | 语文 | 60 | 2 |
| 001 | 李四 | 数学 | 56 | 3 |
| 001 | 王五 | 数学 | 97 | 1 |
| 001 | 王五 | 地理 | 88 | 2 |
| 001 | 王五 | 英语 | 81 | 3 |


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

| sname |
| :--- |
| 王五 |
| 小红 |


### 5.连续问题
#### 问题1：连续登陆问题
根据用户的`user_id`以及`login_date`查找出连续三天登陆的用户的`user_id`

| user\_id | login\_date |
| :--- | :--- |
| A | 2022-09-02 |
| A | 2022-09-03 |
| A | 2022-09-04 |
| B | 2021-11-25 |
| B | 2021-12-30 |
| C | 2022-01-01 |
| C | 2022-04-04 |
| C | 2022-09-03 |
| C | 2022-09-04 |
| C | 2022-09-05 |
| A | 2022-09-03 |
| D | 2022-10-20 |
| D | 2022-10-21 |
| A | 2022-10-03 |
| D | 2022-10-22 |
| D | 2022-10-23 |

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

**解题思路（行号过滤法）**
- user_id要相同，表示同一用户；
- 每一用户每行记录以登陆时间从小到大排序；
- 后一行记录比前一行登陆时间多一天；
- 数据行数据大于N；

行号过滤法的思路在于首先根据`user_id`分组且在组内生成**自增伪列**（连续区间列），如果使用`login_date`减掉伪列后的结果中`sub_date`出现连续出现三个一样的结果时，说明该用户连续三天登录。

```sql
WITH 
t0 as (SELECT DISTINCT * FROM SQL_8 ),
t1 as (SELECT *, ROW_NUMBER() over (PARTITION BY user_id ORDER BY SQL_8.login_date) as rn FROM t0),
t2 as(SELECT *, date_sub(login_date, INTERVAL rn DAY) as sub_date FROM t1)
SELECT DISTINCT user_id FROM t2 GROUP BY user_id,  sub_date having count(*)>=3;
```

**结果**
| user\_id |
| :--- |
| A |
| C |
| D |



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
**要求**： 编写SQL语句，检索出连续进三球的队员编号`player_id`。

**样本**：表`SQL_9`分别记录了球员的id： `player_id`，进球得分：`score`和进球时间：`score_time`。
| player\_id | score | score\_time |
| :--- | :--- | :--- |
| A3 | 1 | 2022-09-20 19:00:14 |
| A3 | 1 | 2022-09-20 19:01:04 |
| A3 | 3 | 2022-09-20 19:01:16 |
| B1 | 3 | 2022-09-20 19:02:05 |
| A2 | 2 | 2022-09-20 19:02:25 |
| B3 | 2 | 2022-09-20 19:02:54 |
| A1 | 3 | 2022-09-20 19:03:10 |
| A1 | 2 | 2022-09-20 19:03:34 |
| B2 | 2 | 2022-09-20 19:03:58 |
| B1 | 3 | 2022-09-20 19:04:07 |
| A1 | 1 | 2022-09-20 19:04:19 |
| A1 | 2 | 2022-09-20 19:04:31 |

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

**解题思路**
连续N次以上为球队得分，要求数据满足如下条件
- player_id要相同表示同一球员；
- 每行记录以得分时间从小到大排序；
- 数据行数大于等于N；


我们先利用窗口函数生成两个新列用于记录前一进球的球员了l1_player_id和前二进球的球员l2_player_id，只有当前进球球员的id满足`player_id=l1_player_id=l2_player_id`时才表示该球员连进三球。

| player\_id | score | score\_time | l1\_player\_id | l2\_player\_id |
| :--- | :--- | :--- | :--- | :--- |
| A3 | 1 | 2022-09-20 19:00:14 | null | null |
| A3 | 1 | 2022-09-20 19:01:04 | A3 | null |
| A3 | 3 | 2022-09-20 19:01:16 | A3 | A3 |
| B1 | 3 | 2022-09-20 19:02:05 | A3 | A3 |
| A2 | 2 | 2022-09-20 19:02:25 | B1 | A3 |
| B3 | 2 | 2022-09-20 19:02:54 | A2 | B1 |
| A1 | 3 | 2022-09-20 19:03:10 | B3 | A2 |
| A1 | 2 | 2022-09-20 19:03:34 | A1 | B3 |
| B2 | 2 | 2022-09-20 19:03:58 | A1 | A1 |
| B1 | 3 | 2022-09-20 19:04:07 | B2 | A1 |
| A1 | 1 | 2022-09-20 19:04:19 | B1 | B2 |
| A1 | 2 | 2022-09-20 19:04:31 | A1 | B1 |


**解答**
```sql
WITH t1 as (SELECT *, lag(player_id,1) OVER(ORDER BY score_time) as l1_player_id,
    lag(player_id,2) OVER(ORDER BY score_time) as l2_player_id FROM SQL_9)
SELECT DISTINCT player_id FROM t1 WHERE player_id = l1_player_id AND player_id = l2_player_id GROUP BY player_id;
```

**结果**
| player\_id |
| :--- |
| A3 |


**错误解法**
```sql
WITH t1 as (SELECT *, lag(player_id,1) OVER(ORDER BY score_time) as layer_player_id FROM SQL_9)
SELECT DISTINCT player_id FROM t1 WHERE player_id = layer_player_id GROUP BY player_id HAVING count(*)>=2;
```

**错误结果**
| player\_id |
| :--- |
| A3 |
| A1 |

**错因分析**
因为在表中如下部分第一条，第三条和第四条数据会因为过滤条件`WHERE player_id = layer_player_id`而被删除，
| player\_id | score | score\_time |
| :--- | :--- | :--- |
| A1 | 3 | 2022-09-20 19:03:10 |
| A1 | 2 | 2022-09-20 19:03:34 |
| B2 | 2 | 2022-09-20 19:03:58 |
| B1 | 3 | 2022-09-20 19:04:07 |
| A1 | 1 | 2022-09-20 19:04:19 |
| A1 | 2 | 2022-09-20 19:04:31 |

从而`GROUP BY player_id HAVING count(*)>=2`分组检索会在如下子表上进行，故`A1`将会被保留。
| player\_id | score | score\_time |
| :--- | :--- | :--- |
| A1 | 2 | 2022-09-20 19:03:34 |
| A1 | 1 | 2022-09-20 19:04:19 |
| A1 | 2 | 2022-09-20 19:04:31 |


#### 问题3：连续区间起止点id查找
查找出`log_id`中的连续区间，并返回其起始点id `start_id` 和终止点id `end_id`。譬如下面表`SQL_10`的`log_id`中（1，2，3）为一个连续区间，其`start_id=1, end_id=3`。

表`SQL_10`：记录了登陆账户的id。
| log\_id |
| :--- |
| 1 |
| 2 |
| 3 |
| 7 |
| 8 |
| 10 |

```sql
CREATE TABLE SQL_10(
    log_id int
);
INSERT INTO SQL_10(log_id) VALUES(1),(2),(3),(7),(8),(10);
```

**解题思路**
我们先插入一段自增伪列`row_num`，再使用`log_id`减去`row_num`得到`diff`。在`log_id`的连续区间，`diff`将会是一样的。`log_id`=1,2,3时，对应的`diff`均为0。最后根据`diff`的值进行分组查询，并在每个分组（每个连续区间）分别检索其起始点id和终止点id。
| log\_id | row\_num | diff |
| :--- | :--- | :--- |
| 1 | 1 | 0 |
| 2 | 2 | 0 |
| 3 | 3 | 0 |
| 7 | 4 | 3 |
| 8 | 5 | 3 |
| 10 | 6 | 4 |


**解答**
```sql
SELECT * FROM SQL_10;
WITH t1 as (SELECT *, ROW_NUMBER() over (PARTITION BY NULL ORDER BY log_id) as row_num FROM sql_10),
t2 as (SELECT *, log_id-row_num as diff FROM t1)
SELECT min(log_id) as start_id, max(log_id) as end_id FROM t2 GROUP BY diff HAVING count(*)>1;
```

**结果**
| start\_id | end\_id |
| :--- | :--- |
| 1 | 3 |
| 7 | 8 |


> [!NOTE]
> **技术总结**
> 如何求解连续区间？
> - 1）行号过滤法：通过row_number()生成自增伪列（连续行号），与区间列进行差值运算，得到的临时结果如果相同表示为同一连续区间。
> - 2）错位相减法：通过lead/lag函数直接生成错位列；

> [!WARNING]
> **避坑点**
> - 1）在实际问题中，原区间列行与行之间的差值可能不是1，此时可以用ROW_NUMBER()生成一个伪区间列，因为在连续问题中“事件”发生的相对顺序是我们所关注的；比如**连续进球问题**中，我们只关心进球的顺序，并不关心进球的具体时间，所以也可以先按进球顺序生成一个伪区间列（连续区间列），再使用行号过滤法来解决问题；
> - 2）使用错位相减法的时候一定要特别小心**连续进球问题**中提到的错误，不能简单地使用`COUNT`函数来判断连续问题，特别是连续个数大于2的情况；
> - 3）在最后的输出中，我们要注意使用`DISTINCT`关键字，因为在所有问题中，同一个体的连续行为可能出现很多次；

**连续进球问题的行号过滤法**

我们先按照进球顺序生成自增伪列`score_order`，再根据player_id分组在每组内生成自增伪列`row_num`，并作差得到`diff`。
| player\_id | score | score\_time | score\_order | row\_num | diff |
| :--- | :--- | :--- | :--- | :--- | :--- |
| A1 | 3 | 2022-09-20 19:03:10 | 7 | 1 | 6 |
| A1 | 2 | 2022-09-20 19:03:34 | 8 | 2 | 6 |
| A1 | 1 | 2022-09-20 19:04:19 | 11 | 3 | 8 |
| A1 | 2 | 2022-09-20 19:04:31 | 12 | 4 | 8 |
| A2 | 2 | 2022-09-20 19:02:25 | 5 | 1 | 4 |
| A3 | 1 | 2022-09-20 19:00:14 | 1 | 1 | 0 |
| A3 | 1 | 2022-09-20 19:01:04 | 2 | 2 | 0 |
| A3 | 3 | 2022-09-20 19:01:16 | 3 | 3 | 0 |
| B1 | 3 | 2022-09-20 19:02:05 | 4 | 1 | 3 |
| B1 | 3 | 2022-09-20 19:04:07 | 10 | 2 | 8 |
| B2 | 2 | 2022-09-20 19:03:58 | 9 | 1 | 8 |
| B3 | 2 | 2022-09-20 19:02:54 | 6 | 1 | 5 |


```sql
WITH t1 as (
    SELECT *, ROW_NUMBER() over (ORDER BY score_time) as score_order FROM sql_9
),
t2 as (
SELECT *, score_order - ROW_NUMBER() over(PARTITION BY player_id) as diff FROM t1
)
SELECT player_id FROM t2 GROUP BY player_id, diff HAVING count(*)>2;
```
