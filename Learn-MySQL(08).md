# 第八天

## 字符串函数

### 1. 计算字符串字符数的函数和字符串长度的函数

`CHAR_LENGTH(str)`

返回值为字符串`str`所包含的字符个数，一个多字节字符算作一个单字符。

~~~mysql
SELECT CHAR_LENGTH('date'),CHAR_LENGTH('egg');
~~~

`LENGTH(str)`

返回值为字符串的字节长度，一个汉字是3个字节，一个数字或字母算一个字节。

~~~mysql
SELECT LENGTH('date'),LENGTH('egg')
~~~

### 2. 合并字符串函数

`CONCAT(s1,s2,...)`

返回结果为连接参数所产生的字符串，或许有一个或多个参数。如有任何一个参数为`NULL`，则返回值为`NULL`。如果所有参数均为非二进制字符串，则结果为非二进制字符串。如果自变量中含有任一二进制字符串，则结果为一个二进制字符串。

~~~mysql
SELECT CONCAT('My SQL','5.5'),CONCAT('My',NULL,'SQL');
~~~

`CONCAT_WS(x,s1,s2,...)`

代表`CONCAT With Separator`，是`CONCAT()`的特殊形式。第一个参数`x`是其他参数的分隔符，分隔符的位置放在要连接的两个字符串之间。分隔符可以是一个字符串，也可以是其他参数。如果分隔符为`NULL`，则结果为`NULL`。函数会忽略任何分隔符参数后的`NULL`值。

~~~mysql
SELECT CONCAT_WS('-','1st','2nd','3rd'),CONCAT_WS('*','1st',NULL,'3rd');
~~~

### 3. 替换字符串函数

`INSERT(s1,x,len,s2)`

返回字符串`s1`，其字符串起始于`x`位置和被字符串`s2`取代的`len`字符。如果`x`超过字符串长度，则返回值为原始字符串。假如`len`的长度大于其他字符串的长度，则从位置`x`开始替换。若任何一个参数为`NULL`，则返回值为`NULL`。

~~~mysql
SELECT INSERT('Quest'2,4,'What') AS col1,
INSERT('Quest',-1,4,'What') AS col2，
INSERT('Quest',3,100,'What') AS col3;
~~~

### 4. 字母大小写转换函数

`LOWER(str)`或者`LCASE(str)`

可以将字符串`str`中的字母字符全部转换成小写字母。

`UPPER(str)`或者`UCASE(str)`

可以将字符串`str`中的字母字符全部转换成大写字母。

### 5. 获取指定长度的字符串的函数

`LEFT(s,n)`

返回字符串`s`开始的最左边`n`个字符。

`RIGHT(s,n)`

返回字符串中右边的字符。

### 6. 填充字符串的函数

`LPAD(s1,len,s2)`

返回字符串`s1`，其左边由字符串`s2`填补到`len`字符长度。假如`s1`的长度大于`len`，则返回值被缩短至`len`字符。

`RPAD(s1,len,s2)`

返回字符串`s2`，其右边被字符串`s2`填补至`len`字符长度。假如字符串`s1`的长度大于`len`，则返回值被缩短到`len`字符长度。

### 7. 删除空格的函数

`LTRIM(s)`

返回字符串`s`，字符串左侧空格字符被删除。

`RTRIM(s)`

删除右边的空格。

`TRIM(s)`

删除字符串`s`两侧的空格。

### 8. 删除指定字符串的函数

`TRIM(s1 FROM s)`

删除字符串`s`中两端所有的子字符串`s1`。`s1`为可选项，在未指定的情况下，删除空格。

### 9. 重复生成字符串的函数

`REPEAT(s,n)`返回一个由重复的字符串`s`组成的字符串，字符串`s`的数目等于`n`。若`n<=0`，则返回一个空字符串。若`s`或`n`为`NULL`，则返回`NULL`。

### 10. 空格函数和替换函数

`SPACE(n)`返回一个由`n`个空格组成的字符串。

`REPLACE(s,s1,s2)`使用字符串`s2`替代字符串`s`中所有的字符串`s1`。

### 11. 比较字符串大小的函数

`STRAMP(s1,s2)`若所有的字符串均相同，则返回0，若根据当前分类次序，第一个参数小于第二个，则返回-1，其他情况返回1。

### 12. 获取子串的函数

`SUBSTRING(s,n,len)`带有`len`参数的格式，从字符串`s`返回一个长度同`len`字符相同的子字符串，起始于位置`n`。也可能对`n`使用一个负值。假若这样，则子字符串的位置起始于字符串结尾的`n`字符，即倒数第`n`个字符，而不是字符串的开头位置。

`MID(s,n,len)`与`SUBSTRING(s,n,len)`的作用相同。

### 13. 匹配子串开始位置的函数

`LOCATE(str1,str)`、`POSITION(str1 IN str)`和`INSTR(str,str1)`3个函数作用相同，返回子字符串`str1`在字符串`str`中的开始位置。

### 14. 字符串逆序的函数

`REVERSE(s)`将字符串`s`反转，返回的字符串顺序和`s`字符串顺序相反。

### 15. 返回指定位置的字符串的函数

`ELT(N,字符串1,字符串2,字符串3,...,字符串N)`

若`N=1`，则返回值为字符串1，若`N=2`，则返回值为字符串2，以此类推。若`N`小于1或大于参数的数目，则返回值为`NULL`。

### 16. 返回指定字符串位置的函数

`FIELD(s,s1,s2,...)`

返回字符串`s`在列表`s1,s2,...`中第一次出现的位置，在找不到`s`的情况下，返回值为0。如果`s`为`NULL`，则返回值为0，原因是`NULL`不能同任何值进行同等比较。

### 17. 返回子串位置的函数

`FIND_IN_SET(s1,s2)`

返回字符串`s1`在字符串列表`s2`中出现的位置，字符串列表是一个由多个逗号分开的字符串组成的列表。如果`s1`不在`s2`或`s2`为空字符，则返回值为0。如果任意一个参数为`NULL`，则返回值为`NULL`。这个函数在第一个参数包含一个逗号时将无法正常运行。

### 18. 选取字符串的函数

`MAKE_SET(x,s1,s2,...)`

返回由`x`的二进制数指定的相应位的字符串组成的字符串，`s1`对应比特1，`s2`对应比特01，以此类推。`s1,s2...`中的`NULL`值不会被添加到结果中。