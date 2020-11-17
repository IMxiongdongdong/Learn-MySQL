# 第十六天

## 1. `MySQL`函数

`MySQL`的触发器和存储过程一样，都是嵌入到`MySQL`的一段程序。触发器是由事件来触发某个操作，这些事件包括`INSERT`、`UPDATE`和`DELETE`语句。如果定义了触发程序，当数据库执行这些语句的时候就会激发触发器执行相应的操作，触发程序是与表相关的命名数据库对象，当表上出现特定事件时，将激活该对象。

### 1.1 创建触发器

触发器是个特殊的存储过程，不同的是，执行存储过程要使用`CALL`语句来调用，而触发器的执行不需要使用`CALL`语句来调用，也不需要手工启动，只要当一个预定义的事件发生的时候，就会被`MySQL`自动调用。创建只有一个执行语句的触发器语法如下：

  ~~~mysql
  CREATE TRIGGER trigger_name trigger_time trigger_event
  ON tbl_name FOR EACH ROW trigger_stmt
  ~~~

  其中，`tirgger_name`标识触发器名称，用户自行指定；`trigger_time`标识触发时机，可以指定为`before`或者`after`；`trigger_event`标识触发事件，包括`INSERT`、`UPDATE`和`DELETE`；`tbl_name`标识建立触发器的表名，即在哪张表上建立触发器；`trigger_stmt`是触发程序体。触发器程序可以使用`begin`和`end`作为开始和结束，中间包含多条语句。

### 1.2 创建有多个执行语句的触发器

创建多个执行语句的触发器的语法如下：

~~~mysql
CREATE TRIGGER trigger_name trigger_time trigger_event
ON tbl_name FOR EACH ROW trigger_stmt
~~~

  其中，`tirgger_name`标识触发器名称，用户自行指定；`trigger_time`标识触发时机，可以指定为`before`或者`after`；`trigger_event`标识触发事件，包括`INSERT`、`UPDATE`和`DELETE`；`tbl_name`标识建立触发器的表名，即在哪张表上建立触发器；`trigger_stmt`是触发程序体。触发器程序可以使用`begin`和`end`作为开始和结束，中间包含多条语句。

## 2. 查看触发器

查看触发器是指查看数据库中已存在的触发器的定义、状态和语法信息等。可以通过命令来查看已经创建的触发器。

### 2.1 `SHOW TRIGGERS`语句查看触发器信息

语句如下：

~~~mysql
SHOW TRIGGERS;
~~~

### 2.2 在`triggers`表中查看触发器信息

在`MySQL`中所有触发器的定义都存在`INFORMATION_SCHEMA`数据库的`TRIGGERS`表格中，可以通过查询命令`SELECT`来查看，语法如下：

~~~mysql
SELECT * FROM INFORMATION_SCHEMA.TRIGGERS WHERE condition;
~~~

## 3. 触发器的作用

触发程序是与表有关的命名数据库对象，当表上出现特定事件时，将激活该对象。在某些触发程序的用法中，可用于检查插入到表中的值，或对更新设计的值进行计算。

触发程序与表相关，当对表执行`INSERT`、`DELETE`或`UPDATE`语句时，将激活触发程序。可以将触发程序设置为在执行语句之前或之后激活。

## 4. 删除触发器

使用`DROP TRIGGER`语句可以删除已经定义的触发器，删除触发器语句基本语法格式如下：

~~~mysql
DROP TRIGGER [schema_name.]trigger_name
~~~

其中，`schema_name`表示数据库名称，是可选的。如果省略了`schema`，将从当前数据库中舍弃触发程序。`trigger_name`是要删除的触发器的名称。