## Section 1.6 事务
### 1.事物简介
事物是一组操作的集合，是一个不可分割的工作单位，事物会把所有的操作作为一个整体一起向系统提交或撤销操作请求，这些操作**要么同时成功，要么同时失败**。

### 2.事务操作
**查看/设置事务提交方式**

```sql
select @@autocommit;
-- 设置为手动提交
set @@autocommit = 0; 
```

**提交事务**

```sql
commit;
```

**回滚事务**

```sql
rollback;
```

**开启事务**

```sql
start transaction 或者 begin
```

### 3.事务的四大特性（ACID)
- 原子性(Atomicity)：事务是不可分割的最小操作单元，要么全部执行成功，要么全部失败；
- 一致性(Consistency)：事务完成时，必须使所有的数据都保持一致状态；
- 隔离性(isolation)： 数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行；
- 持久性(Durability)：事务一旦提交或者回滚，它对数据库中的数据的改变就是永久的；

### 4.并发事务问题
| 问题 | 描述 |
| :---: | :---: |
|脏读| 一个事务读到另外一个事务还没有提交的数据|
|不可重复读| 一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读|
|幻读|一个事务按照条件查询数据时，没有对应的数据行，但在插入数据时，有发现这行数据已经存在，好像出现了“幻影”|

### 5.事务的隔离级别
**事务隔离级别**
|隔离级别        |脏读   |不可重复读|幻读|
| :---:          | :---: | :---: | :---: |
|read uncommitted|有|有|有|
|read committed  |无|有|有|
|repeatable read |无|无|有|
|serializable    |无|无|无|

从上往下，隔离级别越来越高，数据安全性越来越高，但是性能越来越差。其中serialization隔离级别最高，此时对于一个数据库只能有一个事务在操作，不能多个事务并发。

**查看事务隔离级别**
```sql
select @@transaction_isolation;
```

**设置事务的隔离级别**
```sql
set [session|global] transaction isolation level [read uncommitted|read committed|repeatable read|serializable]
```

