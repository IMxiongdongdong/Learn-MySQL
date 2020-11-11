# 第五天

## 数据类型

* 数据类型主要有数值类型、日期/时间类型和字符串类型

  数值数据类型：包括整数类型`TINYINT`、`SMALLINT`、`MEDIUMINT`、`INT`、`BIGINT`

、浮点小数数据类型`FLOAT`和`DOUBLE`、定点小数类型`DECIMAL`。

​		日期/时间类型：包括`YEAR`、`TIME`、`DATE`、`DATETIME`和`TIMESTAMP`。

​		字符串类型：包括`CHAR`、`VARCHAR`、`BINARY`、`VARBINARY`、`BLOB`、`TEXT`、`ENUM`和`SET`等。

### 1. 整数类型

|    类型名称    |      说明      | 存储需求 |
| :------------: | :------------: | :------: |
|   `TINYINT`    |   很小的整数   | 1个字节  |
|   `SMALLINT`   |    小的整数    | 2个字节  |
|  `MEDIUMINT`   | 中等大小的整数 | 3个字节  |
| `INT(INTEGER)` | 普通大小的整数 | 4个字节  |
|    `BIGINT`    |     大整数     | 8个字节  |

### 2. 浮点类型和定点数类型

浮点类型有两种，单精度浮点类型`FLOAT`和双精度浮点类型`DOUBLE`。定点类型只有一种`DECIMAL`。

浮点类型和定点类型都可以用（`M`，`N`）来表示，其中`M`称为精度，表示总共的位数；`N`称为标度，是表示小数的位数。

|      类型名称      |        说明        |  存储需求   |
| :----------------: | :----------------: | :---------: |
|      `FLOAT`       |    单精度浮点数    |   4个字节   |
|      `DOUBLE`      |    双精度浮点数    |   8个字节   |
| `DECIMAL(M,D),DEC` | 压缩的“严格”定点数 | `M`+2个字节 |

`DECIMAL`类型不同于`FLOAT`和`DOUBLE`，`DECIMAL`实际是以串存放的，`DECIMAL`可能的最大取值范围与`DOUBLE`一样，但是其有效的取值范围由`M`和`D`的值决定。如果改变`M`而固定`D`，则其取值范围将随`M`的变大而变大。`DECIMAL`的存储空间并不是固定的，而由其精度值`M`决定，占用`M`+2个字节。

`FLOAT`和`DOUBLE`在不指定精度时，默认会按照实际的精度（由计算机硬件和操作系统决定），`DECIMAL`如不指定精度默认为（10，0）。

浮点数相对于定点数的优点是在长度一定的情况下，浮点数能够表示更大的数据范围，它的缺点是会引起精度问题。

### 3. 日期与时间类型

* `YEAR`类型是一个单字节类型用于表示年，在存储时只需要1个字节。

* `TIME`类型用在只需要时间信息的值，在存储时需要3个字节。格式为`HH:MM:SS`。

* `DATE`类型用在仅需要日期值时，没有时间部分，在存储时需要3个字节。日期格式为`YYYY-MM-DD`，其中`YYYY`表示年，`MM`表示月，`DD`表示日。

* `DATETIME`类型用在需要同时包含日期和时间信息的值，在存储时需要8个字节。

* `TIMESTAMP`的显示格式与`DATETIME`相同，显示宽度固定在19个字符，日期格式为`YYYY-MM-DD HH:MM:SS`，在存储时需要4个字节。但是`TIMESTAMP`列的取值范围小于`DATETIME`的取值范围。

### 4. 字符串类型

* `CHAR`和`VARCHAR`类型

  `CHAR(M)`为固定长度字符串，在定义时指定字符串列长。当保存时在右侧填充空格以达到指定的长度。`M`表示列长度，`M`的范围是0~255个字符。

  `VARCHAR(M)`是长度可变的字符串，`M`表示最大列长度。`VARCHAR`的最大世家长度由最长的行的大小和使用的字符集确定，而其实际占用的空间为字符串的实际长度加1。

* `TEXT`类型

  用于保存非二进制字符串，如文章内容、评论等。等保存或查询`TEXT`列的值时，不删除尾部空格。`TEXT`类型分为`TINYTEXT`、`TEXT`、`MEDIUMTEXT`、`LONGTEXT`，不同类型的存储空间和数据长度不同。

* `ENUM`类型

  时一个字符串对象，其值为表创建时在列规定中枚举的一列值。

  ~~~sql
  字段名 ENUM('值1','值2',...'值n')
  ~~~

### 4. `SET`类型

`SET`是一个字符串对象，可以有零或多个值，`SET`列最多可以用64个成员，其值为表创建时规定的一列值。指定包括多个`SET`成员的`SET`列值时，各成员之间用逗号间隔开。

~~~sql
SET('值1','值2',...'值n')
~~~

### 5. 二进制类型

二进制类型有`BIT`、`BINARY`、`VARBINARY`、`TINYBLOB`、`BLOB`、`MEDIUMBLOB`和`LONGBLOB`。

* `BIT`类型

  位字段类型

* `BINARY`类型

  长度是固定的，指定长度后，不足最大长度的，将在他们右边填充'\0'补充以达到指定长度。

* `VARBINARY`类型

  长度是可变的，指定好长度之后，其长度可以在0到最大值之间。

* `BLOB`类型

  是一个二进制大对象，用来存储可变数量的数据。

***

***

## 如何选择数据类型

### 1. 整数和浮点数

如果不需要小数部分，则使用整数来保存数据；如果需要表示小数部分，则使用浮点数类型。当要求浮点数存储精度较高时，应选择`DOUBLE`类型。

### 2. 浮点数和定点数

浮点数相对于定点数的优势是在长度一定的情况下，浮点数能表示更大的数据范围。但是由于浮点数容易产生误差，因此对精确度要求比较高时，建议使用定点数来存储。

定点数是以字符串存储的，用于定义货币等对精确度要求较高的数据。两个浮点数进行减法和比较运算时也容易出现问题，因此在进行计算的时候，一定要小心。如果进行数值比较，最好使用定点数类型。

### 3. 日期与时间类型

如果只要记录年份，使用`YEAR`即可，如果只记录时间，使用`TIME`类型。

如果同时需要记录日期和时间 ，则可以使用`TIMESTAMP`或`DATETIME`类型，由于`TIMESTAMP`列的取值范围小于`DATETIME`的取值范围，因此存储范围较大的日期最好使用`DATETIME`。

`TIMESTAMP`有一个`DATETIME`不具备的属性，默认情况下当插入一条记录但没有指定指定`TIMESTAMP`这个列值时，`MySQL`会把`TIMESTAMP`列设为当前的时间，另外`TIMESTAMP`在空间上比`DATETIME`更有效。

### 4. `CHAR`与`VARCHAR`

`CHAR`是固定长度字符，`VARCHAR`是可变长度字符，`CHAR`会自动删除插入数据的尾部空格，`VARCHAR`不会删除尾部空格。

`CHAR`是固定长度，所以它的处理速度比`VARCHAR`的速度要快，但是它的缺点就是浪费存储空间。

对于`MyISAM`存储引擎：最好使用固定长度的数据列代替可变长度的数据列。这样可以使整个表静态化，从而使数据检索更快，用空间换时间。

对于`InnoDB`存储引擎：使用可变长度的数据列，因为`InnoDB`数据表的存储格式不分固定长度和可变长度，因此使用`CHAR`不一定比使用`VARCHAR`更好，但由于`VARCHAR`是按照实际的长度存储，比较节省空间，所以对磁盘`I/O`和数据存储总量比较好。

### 5. `ENUM`和`SET`

`ENUM`只能取单值，它的数据列表是一个枚举集合。`SET`可取多值。

`ENUM`和`SET`的值是以字符串形式出现的，但在内部，`MySQL`以数值的形式存储它们。

### 6. `BLOB`和`TEXT`

`BLOB`是二进制字符串，`TEXT`是非二进制字符串，二者均可以存放大容量的信息。`BLOB`主要存储图片、音频信息等，`TEXT`只能存储纯文本文件。