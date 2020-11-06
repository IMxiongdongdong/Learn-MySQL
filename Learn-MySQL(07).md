# 第七天

## `MySQL`函数

### 1. 数学函数

#### 1.1 绝对值函数

`ABS(x)`

~~~mysql
SELECT ABS(2),ABS(-3.3),ABS(-33);
~~~

#### 1.2 返回圆周率的函数

`PI()`

~~~mysql
SELECT pi();
~~~

#### 1.2 返回圆周率的函数
#### 1.3 平方根函数

`SQRT(x)`

~~~mysql
SELECT SQRT(9),SQRT(40),SQRT(-49);
~~~

#### 1.4 求余函数

`MOD(x,y)`

~~~mysql
SELECT MOD(31,8),MOD(234,10),MOD(45.5,6);
~~~

#### 1.5 获取整数的函数

`CEIL(x),CEILING(x),FLOOR(x)`

~~~mysql
SELECT CEIL(-3.35),CEILING(3.35);
SELECT FLOOR(-3.35),FLOOR(3.35);
~~~

#### 1.6 获取随机数的函数

`RAND(),RAND(x)`

~~~mysql
SELECT RAND(),RAND(),RAND();
~~~

不带参数的`RAND()`每次产生的随机数值是不同的。

~~~mysql
SELECT RAND(10),RAND(10),RAND(11);
~~~

当`RAND(x)`的参数相同时，将产生相同的随机数，不同的`x`产生的随机数值不同。

#### 1.7 四舍五入函数

`ROUND(x),ROUND(x,y),TRINCATE(x,y)`

~~~mysql
SELECT ROUND(-1.14),ROUND(-1.67),ROUND(1.14),ROUND(1.66);
SELECT ROUND(1.38,1),ROUND(1.38,0),ROUND(232.38,-1);
~~~

`ROUND(x)`函数对操作数进行四舍五入操作。

`ROUND(x,y)`返回最接近于`x`的数，其值保留到小数点后面`y`位，若`y`为负值，则将保留`X`值到小数点左边`y`位。

~~~mysql
SELECT TRUNCATE(1.31,1)TRUNCATE(1.99,1),TRUNCATE(1.99,0),TRUNCATE(19.99,-1);
~~~

`TRUNCATE(x,y)`返回舍去至小数点后`y`位的数字`x`。若`y`的值为0，则结果不带有小数点或不带有小数部分。若`y`设为负数，则截去（归零）`x`小数点左起第`y`位开始后面所有低位的值。

#### 1.8 符号函数

`SIGN(x)`

~~~mysql
SELECT SIGN(-21),SIGN(0),SIGN(21);
~~~

返回参数的符号，`X`的值为负、零或正时返回结果依次为-1，0或1。

#### 1.9 幂运算函数

`POW(x,y),POWER(x,y),EXP(x)`

~~~mysql
SELECT POW(2,2),POWER(2,2),POW(2-2),POWER(2,-2);
~~~

#### 1.10 对数运算函数

`LOG(x),LOG10(x)`

~~~mysql
SELECT LOG(3),LOG(-3);
SELECT LOG10(2),LOG10(100),LOG10(-100);
~~~

#### 1.11 角度与弧度相互转换函数

`RADIANS(x)`将参数由角度转化为弧度。

`DEGRESS(x)`将参数由弧度转化为角度。

#### 1.12 正弦函数和反正弦函数

`SIN(x)`返回`x`正弦，其中`x`为弧度值。

`ASIN(x)`返回`x`的反正弦，即正弦为`x`的值。

#### 1.13 余弦函数和反余弦函数

`COS(x)`返回`x`余弦，其中`x`为弧度值。

`ACOS(x)`返回`x`的反余弦，即余弦为`x`的值。

#### 1.14 正切函数、反正切函数和余切函数

`TAN(x)`返回`x`正切，其中`x`为弧度值。

`ATAN(x)`返回`x`的反正切，即正切为`x`的值。