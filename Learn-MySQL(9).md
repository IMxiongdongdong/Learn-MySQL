# 第九天

## 日期和时间函数

### 1. 获取当前日期的函数和获取当前时间的函数

`CURDATE()`

将当前日期按照`YYYY-MM-DD`或`YYYYMMDD`格式的值返回。

~~~mysql
SELECT CURDATE();
~~~

`CURRENT_DATE()`

同上。

~~~mysql
SELECT CURRENT_DATE();
~~~

### 2. 获取当前日期和时间的函数

`CURRENT_TIMESTAMP()`、`LOCALTIME()`、`NOW()`、`SYSDATE()`

4个函数作用相同，均返回当前日期和时间值，格式为`YYYY-MM-DD HH:MM:SS`或`YYYYMMDDHHMMSS`。

~~~mysql
SELECT CURRENT_TIMESTAMP(),LOCALTIME(),NOW(),SYSDATE();
~~~

### 3. `unix`时间戳函数

`UNIX_TIMESTAMP(date)`

若无参数调用，则返回一个`unix`时间戳作为无符号整数。`date`可以是一个`DATE`字符串，`DATETIME`字符串，`TIMESTAMP`或一个当地时间的`YYMMDD`或`YYYYMMDD`的数字。

~~~mysql
SELECT UNIX_TIMESTAMP(),UNIX_TIMESTAMP(NOW()),NOW();
~~~

`FROM_UNIXTIME(date)`函数把时间戳转换为普通格式的时间，与`UNIX_TIMESTAMP(date)`函数互为反函数。

~~~mysql
SELECT FROM_UNIXTIME('1311476091');
~~~

### 4. 返回`UTC`日期的函数和返回`UTC`时间的函数

`UTC_DATE()`函数返回当前`UTC`（世界标准时间）日期值，其格式为`YYYY-MM-DD`或`YYYYMMDD`。

~~~mysql
SELECT UTC_DATE();
~~~

`UTC_TIME()`函数返回当前`UTC`的时间值，其格式为`HH:MM:SS`或`HHMMSS`。

~~~mysql
SELECT UTC_TIME()；
~~~

### 5. 获取月份的函数`MONTH(date)`和`MONTHNAME(date)`

`MONTH(date)`函数返回`date`对应的月份，范围从1~12。

~~~mysql
SELECT MONTH('2011-02-13');
~~~

`MONTHNAME(date)`函数返回日期`date`对应月份的英文全名。

~~~mysql
SELECT MONTHNAME('2011-02-13')
~~~

### 6. 获取星期的函数`DAYNAME(d)`、`DAYOFWEEK(d)`、`WEEKDAY(d)`

`DAYNAME(d)`函数返回`d`对应的工作日的英文名称。

~~~mysql
SELECT DAYNAME('2011-02-13');
~~~

`DAYOFWEEK(d)`函数返回`d`对应的一周中的索引位置。

~~~mysql
SELECT DAYOFWEEK('2011-02-13');
~~~

`WEEKDAY(d)`返回`d`对应的工作日索引。

~~~mysql
SELECT WEEKDAY('2011-02-13 22:23:00');
~~~

### 7. 获取星期数的函数`WEEK(d)`和`WEEKOFYEAR(d)`





