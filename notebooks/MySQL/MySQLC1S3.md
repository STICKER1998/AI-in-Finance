## Section 1.3 函数
**函数**是指一段可以直接被另一段程序调用的程序或者代码。

### 1.字符串函数
MySQL提供了很多字符串函数，常用的如下：

- `concat(s1,s2,...sn)`: 字符串拼接函数，将s1,s2,...sn拼接成一个字符串
- `lower(str)/upper(str)`：将字符串str全部转换成小写/大写
- `lpad(str,n,pad)/rpad(str,n,pad)`:左/右填充，用字符串pad对str的左边/右边进行填充，达到n个字符串的长度
- `trim(str)`:去掉字符串头部和尾部的空格
- `substring(str,start,len)`:返回字符串str从start位置起的len个长度的字符串

### 2.数值函数
- `ceil(x)\floor(x)`: 向上\向下取整；
- `mod(x,y)`：返回x/y的模；
- `rand()`：生成(0,1)之间的随机数
- `round(x)`：四舍五入；

### 3.日期函数
- `curdate()`：返回当前日期;
- `curtime()`:返回当前时间；
- `now()`：返回当前日期和时间；
- `year(date)/month(date)/day(date)`：获取指定date的年/月/；
- `date_add(date, interval expr type)`：返回一个日期/时间值加上一个时间间隔expr后的时间值；
- `datediff(date1, date2)`：返回起始时间date1和结束时间date2之间的天数;

### 4.流程函数
```sql
if(value, t, f)
```
如果value的值为true，则返回值t，否则返回值f；

```sql
ifnull(value1, value2)
```
如果value1不是null，则返回value1，否则返回value2；
 
```sql
case when [con1] then [result1] [con2] then [result2] ...  else [default] end
```
如果条件1满足则返回结果1，...，否则返回default；

```sql
case [expr] when [value1] then [result1] when [value2] then [result2]... else [default] end
```
如果expr等于值1，则返回结果1，...，否则返回default；

