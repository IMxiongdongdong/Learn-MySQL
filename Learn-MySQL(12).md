# 第十二天

## 插入、更新和删除数据

### 1. 插入数据

#### 1.1 为表的所有字段插入数据

使用基本的`INSERT`语句插入数据要求指定表名称和插入到新记录中的值，格式为：

~~~mysql
INSERT INTO table_name(column_list) VALUES(value_list);
~~~

`table_name`指定要插入数据的表名，`column_list`指定要插入数据的那些列，`value_list`指定每个列应对应插入的数据。使用该语句时字段列和数据值的数量必须相同。

#### 1.2 为表的指定字段插入数据

就是在`INSERT`语句中只向部分字段中插入值，而其他字段的值为表定义时的默认值。

#### 1.3 同时插入多条记录

`INSERT`语句可以同时向数据表中插入多条记录，插入时指定多个值列表，每个值列表之间用逗号分隔开，格式如下：

~~~mysql
INSERT INTO table_name(column_list)
VALUES(value_list1), (value_list2),...,(value_listn);
~~~

使用`INSERT`同时插入多条记录时，`MySQL`会返回一些在执行单行插入时没有的额外信息，这些包含数的字符串的意思分别是：

`Records`：表明插入的记录条数。

`Duplicates`：表明插入时被忽略的记录，原因可能是这些记录包含了重复的主键值。

`Warnings`：表明有问题的数据值，例如发生数据类型转换。

#### 1.4 将查询结果插入到表中

`INSERT`语句用来给数据表插入记录时，指定插入记录的列值。`INSERT`还可以将`SELECT`语句查询的结果插入到表中，语法格式如下：

~~~mysql
INSERT INTO table_name1 (column_list1)
SELECT (column_list2) FROM table_name2 WHERE (condition)
~~~

### 2. 更新数据

`MySQL`中使用`UPDATE`语句更新表中的记录，可以更新特定的行或者同时更新所有的行，基本语法格式如下：

~~~mysql
UPDATE table_name
SET column_name1 = value1,column_name2=value2,......,column_namen=valuen
WHERE (condition);
~~~

`column_name1,column_name2,......,column_namen`为指定更i性能的字段的名称；`value1,value2,......,valuen`为相对应的指定字段的更新值；`conditon`指定更新的记录需要满足的条件。更新多个列时，每个“列-值”对之间用逗号隔开，最后一列之后不需要逗号。

### 3. 删除数据

从数据表中删除数据使用`DELETE`语句，`DELETE`语句允许`WHERE`子句指定删除条件。`DELETE`语句基本语法格式如下：

~~~mysql
DELETE FROM table_name [WHERE <condition>];
~~~

`table_name`指定要执行删除操作的表；`[WHERE <condition>]`为可选参数，指定删除条件，如果没有`WHERE`子句，`DELETE`语句将删除表中的所有记录。

