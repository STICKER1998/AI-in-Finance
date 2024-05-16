## Section 3.2 窗口函数
### 窗口
窗口限定一个范围，他可以理解为满足某些条件的记录集合，窗口函数也就是在窗口范围内执行的函数。

### 基本语法
```sql
<函数名> over (partition by <分组的列> order by <排序的列> rows between <起始行> and <终止行>)
```
