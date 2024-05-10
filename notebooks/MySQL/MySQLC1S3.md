## Section 1.3 函数
**函数**是指一段可以直接被另一段程序调用的程序或者代码。与大多数其他计算机语言一样，SQL也支持利用函数来处理数据：
- 用于处理文本字符串文本函数；
- 用于在数值数据上进行算术操作（返回绝对值等）的数值函数；
- 用于处理日期和时间值并从这些值中提取特定成分（返回日期之差，检查日期有效性等）的日期和时间函数；
- 返回DBMS正使用的特殊信息（如返回用户登陆信息，检查版本细节等）的特殊函数；


> [!WARNING]
> **函数没有SQL的可移植性强**
> 能在多个系统上运行的代码称为可移植的（portable），相对来说多数SQL语句是可移植的，而函数的可移植性没有那么强。

### 1.文本函数
MySQL提供了很多字符串函数，常用的如下：
|文本函数|作用|
|:---:|:---:|
|`concat(s1,s2,...sn)`| 字符串拼接函数，将s1,s2,...sn拼接成一个字符串|
|`lower(str)/upper(str)`|将字符串str全部转换成小写/大写|
|`lpad(str,n,pad)/rpad(str,n,pad)`|左/右填充，用字符串pad对str的左边/右边进行填充，达到n个字符串的长度；|
|`trim/ltrim/rtrim(str)`|去掉字符串头部和尾部/头部/尾部的空格；|
|`length(str)`|返回字符串str的长度；|
|`left/right(str,n)`|输出字符串str从左边/右边开始的长度为n的子串；|
|`substring(str,start,len)`|返回字符串str从start位置起的len个长度的字符串，这里strat从1开始；|
|`locate(str1,str2)`|返回字符串str2中子字符串str1第一次出现的位置；|

下面给出一个文本函数的例子

|name|length(name)|upper(name)|lower(name)|right(name,2)|substring(name,1,2)|left(name,2)|locate('av',name)|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Java|4|JAVA|java|va|Ja|Ja|2|
|Python|6|PYTHON|python|on|Py|Py|0|


### 2.数值函数
|数值函数|作用|
|:---:|:---:|
|`ceil(x)\floor(x)`| 向上\向下取整；|
|`mod(x,y)`|返回x/y的模；|
|`rand()`|生成(0,1)之间的随机数；|
|`round(x)`|返回x的四舍五入取整；|
|`sqrt(x)`|返回x的算数平方根；|
|`sin/cos/tan(x)`|返回对应的三角函数值；|
|`pi()`|返回圆周率值；|

给出一些例子
```sql
SELECT PI();
```
返回是`3.141593`

```sql
SELECT SQRT(4);
```
返回是`2`


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

