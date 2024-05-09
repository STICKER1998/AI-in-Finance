## Section 1.4 约束
### 1.概述
- **概念**： 约束是作用于表中字段上的规则，用于限制存储在表中的数据；
- **目的**：保证数据库中数据的正确性，有效性和完整性；
- **分类**
  | 约束  | 描述   | 关键字  |
  | :---: | :---: | :---:   |
  |非空约束| 限制该字段的数据不能为null| NOT NULL|
  |唯一约束| 保证该字段的所有数据都是唯一的，不能重复的|UNIQUE|
  |主键约束|逐渐使一行数据的唯一标识，要求非空且唯一|PRIMARY KEY|
  |默认约束|保存数据时，如果未指定该字段的值，则采用默认值|DEFAULT|
  |检查约束|保证字段值满足某一个条件| CHECK|
  |外键约束|用来让两张表的数据之间连接，保证数据的一致性和完整性|




### 2.约束的具体使用
**案例**
| 字段名 | 字段含义 | 字段类型 | 约束条件 | 约束关键字 |
|:---: | :---: | :---: |:---: |:---:|
|id|ID唯一标识| int|主键且自动增长|PRIMARY KEY, AUTO_INCREMENT|
|name| 姓名 | varchar(10)| 不为空，且唯一| NOT NULL, UNIQUE|
|age| 年龄 | int | 大于0，并且小于等于120| CHECK |
|status| 状态| char(1)| 如果没有指定该值，则默认为1| DEFAULT|
|gender| 性别| char(1)| 无 | |

```sql
create table user(
    id int primary key auto_increment comment 'id主键',
    name varchar(10) not null unique  comment '姓名',
    age int check ( age>0 and age<=120) comment '年龄',
    status char(1) default '1' comment '状态',
    gender char(1) comment '性别'
) comment '用户表';
```

### 3.外键约束

**概念**： 外键用来将两张表的数据之间建立连接，从而保证数据的一致性和完整性。

**添加外键**

```sql
create table 表名(
   字段名 数据类型,
   ...
   [CINSTRAINT] [外键名称] foreign key (外键字段名) references 主表(主表列名)
);
```

```sql
 alter table 表名 add constraint 外键名称 foreign key(外键字段名) references 主表(主表列名);
```

**删除外键**

```sql
alter table 表名 drop foreign key 外键名称;
```

**删除/更新行为**
| 行为 | 说明 |
|:---:|:---:|
|No action|当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新。|
|restrict|当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新。|
|cascade|当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则也删除/更新外键在子表中的记录。|
|set null|当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null。|
|set default|附表有更新时，子表将外键列设置为一个默认的值。|


```sql
 alter table 表名 add constraint 外键名称 foreign key(外键字段名) references 主表(主表列名) on update cascade on delete cascade;
```
