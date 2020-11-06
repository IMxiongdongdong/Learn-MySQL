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

`WEEK(d)`计算日期`d`是一年中的第几周。`WEEK()`的双参数形势允许指定该星期是否起始于周日或周一，以及返回值的范围是否为从0~53或从1~53。

~~~mysql
SELECT WEEK('2011-02-20');
~~~

### 8. 获取天数的函数`DAYOFYEAR(d)`和`DAYOFMONTH(d)`

`DAYOFYEAR(d)`函数返回`d`是一年中的第几天，范围是从1~366。

~~~mysql
SELECT DAYOFYEAR('2011-02-20');
~~~

`DAYOFMONTH(d)`函数返回指定日期在一个月中的位置，范围是从1~31。

~~~mysql
SELECT DAYOFMONTH('2011-02-20');
~~~

### 9. 获取年份、季度、小时、分钟和秒钟的函数

`YEAR(date)`返回`date`对应的年份，范围是1970~2069。

~~~mysql
SELECT YEAR('11-02-03');
~~~

`QUARTER(date)`返回`date`对应的一年中的季度值，范围是从1~4。

~~~mysql
SELECT QUARTER('11-04-01');
~~~

`MINUTE(time)`返回`time`对应的分钟数，范围是从0~59。

~~~mysql
SELECT MINUTE('11-02-03 10:10:03');
~~~

`SECOND(time)`返回`time`对应的秒数，范围是从0~59。

~~~mysql
SELECT SECOND('10:05:03');
~~~

### 10. 获取日期的指定值的函数`EXTRACT(type FROM date)`

`EXTRACT(type FROM date)`函数所使用的时间间隔函数类型说明符同`DATE_ADD()`或`DATE_SUB()`的相同，但它从日期中提取一部分，而不是执行日期运算。

~~~mysql
SELECT EXTRACT(YEAR FROM '2011-07-02') AS col1,
EXTRACT(YEAR_MONTH FROM '2011-07-12 01:02:03')AS col2,
EXTRACT(DAY_MINUTE FROM '2011-07-12 01:02:03')AS col3;
~~~

### 11. 时间和秒钟转换的函数

`TIME_TO_SEC(time)`返回已转化为秒的`time`参数。

~~~mysql
SELECT TIME_TO_SEC('23:23:00');
~~~

`SEC_TO_TIME(seconds)`返回被转化为小时、分钟和秒数的`seconds`参数值，其格式为`HH:MM:SS`或`HHMMSS`。

~~~mysql
SELECT TIME_TO_SEC('23:23:00'),SEC_TO_TIME(84180);
~~~

### 12. 计算日期和时间函数

计算日期和时间的函数有：`DATE_ADD()`、`ADDDATE()`、`DATE_SUB()`、`SUBDATE()`、`ADDTIME()`、`SUBTIME()`、`DATE_DIFF()`。

### 13. 将日期和时间格式化的函数

`DATE_FORMAT(date,format)`根据`format`指定的格式显示`date`值。