# 第十天

## 加密函数

### 1. 加密函数`PASSWORD(str)`

`PASSWORD(str)`从原明文密码`str`计算并返回加密后的密码字符串，当参数为`NULL`时，返回`NULL`。

~~~mysql
SELECT PASSWORD('newpwd');
~~~

### 2. 加密函数`MD5(str)`

`MD5(str)`为字符串算出一个`MD5 128`比特校验和。该值以32位十六进制数字的二进制字符串形式返回，若参数为`NULL`，则会返回`NULL`。

~~~mysql
SELECT MD5('mypwd');
~~~

### 3. 加密函数`ENCODE(str,pswd_str)`

`ENCODE(str,pswd_str)`使用`pswd_str`作为密码，加密`str`。使用`DECODE()`解密结果，结果是一个和`str`长度相同的二进制字符串。

~~~mysql
SELECT ENCODE('secret','cry'),LENGTH(ENCODE('secret','cry'));
~~~

### 4. 解密函数`DECODE(crypt_str,pswd_str)`

`DECODE(crypt_str,pswd_str)`使用`pswd_str`作为密码，解密加密字符串`crypt_str`,`crypt_str`是由`ENCODE()`返回的字符串。

~~~mysql
SELECT DECODE(ENCODE('secret','cry'),'cry');
~~~

***

## 其他函数

### 1. 格式化函数`FORMAT(x,n)`

`FORMAT(x,n)`将数字`x`格式化，并以四舍五入的方式保留小数点后`n`位，结果以字符串的形式返回。若`n`为0，则返回结果函数不含小数部分。

~~~mysql
SELECT FORMAT(12332.123456,4),FORMAT(12332.1,4),FORMAT(12332.2,0);
~~~

### 2. 不同进制的数字进行转换的函数

`CONV(N,from_base,to_base)`函数进行不同禁止间的转换。返回值为数值`N`的字符串表示，由`from_base`进制转化为`to_base`进制。如有任意一个参数为`NULL`，则返回值为`NULL`。自变量`N`被理解为一个整数，但是可以被指定为一个整数或字符串。最小基数为2，而最大基数则为36。

~~~mysql
SELECT CONV('a',16,2);
~~~

### 3. `IP`地址与数字相互转换的函数

`INET_ATON(expr)`给出一个作为字符串的网络地址的点地址表示，返回一个代表该地址值的整数。地址可以是4或8比特地址。

~~~mysql
SELECT INET_ATON('209.207.224.40');
~~~

### 4. 加锁函数和解锁函数

`GET_LOCK(str,timeout)`设法使用字符串`str`给定的名字得到一个锁，超时为`timeout`秒。若成功得到锁，则返回1；若操作超时，则返回0；若发生错误，则返回`NULL`。假如有一个用`GET_LOCK()`D得到的锁，当执行`RELEASE_LOCK()`或连接断开（正常或非正常）时，这个锁就会解除。

`RELEASE_LOCK()`解开被`GET_LOCK()`获取的，用字符串`str`所命名的锁。若锁被解开，则返回1；若该线程尚未创建锁，则返回0（此时锁没有被解开）；若命名的锁不存在，则返回`NULL`。若该锁从未被`GET_LOCK()`的调用获取，或锁已经被提前打开，则该锁不存在。

`IS_FREE_LOCK(str)`检查名为`str`的锁是否可以使用（换言之没有被封锁）。若锁可以使用，则返回1（没有人在用这个锁）；若这个锁正在被使用，则返回0；出现错误，则返回`NULL`（诸如不正确的参数）。

`IS_USED_LOCK(str)`检查名为`str`的锁是否正在被使用（换言之，被封锁）。若被封锁，则返回使用该锁的客户端的连接标识符（`connection ID`)；否则，返回`NULL`。

### 5. 重复执行指定操作的函数

`BENCHMARK(count,expr)`函数重复`count`次执行表达式`expr`。它可以用于计算`MySQL`处理表达式的速度。结果值通常为0（0只是表示处理过程很快，并不是没有花费时间）。另一个作用时它可以在`MySQL`客户端内部报告语句执行的时间。

### 6. 改变字符集的函数

`CONVERT(...USING...)`带有`USING`的`CONVERT()`函数被用来在不同的字符集之间转化数据。

### 7. 改变数据类型的函数

`CAST(x,AS type)`和`CONVERT(x,type)`函数将一个类型的值转换为另一个类型的值，可转换的`type`有`BINARY,CHAR(n),DATE,TIME,DATETIME,DECIMAL,SIGNED,UNSIGED`。

