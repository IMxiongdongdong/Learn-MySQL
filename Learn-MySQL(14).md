# 第十四天

## 存储过程和函数

存储过程就是一条或者多条`SQL`语句的集合，可视为批文件，但是其作用不仅限于批处理。

### 1. 创建存储过程和函数

存储程序可以分为存储过程和函数，`MySQL`中创建存储过程和函数使用的语句分别是：`CREATE PROCEDURE`和`CREATE FUNCTION`。使用`CALL`语句来调用存储过程，只能用输出变量返回值。函数可以从语句外调用（即通过引用函数名），也能返回标量值。存储过程也可以调用其他存储过程。

#### 1.1 创建存储过程

创建存储过程，需要使用`CREATE PROCEDURE`语句，语法格式如下：

~~~mysql
CREATE PROCEDURE sp_name([proc_parameter])
	[characteristics...]routine_body
~~~

`CREATE PROCEDURE`为用来创建存储函数的关键字；`sp_name`为存储过程的名称；`proc_parameter`为指定存储过程的参数列表，列表形式如下：

~~~mysql
[IN|OUT|INOUT]param_name type
~~~

其中，`IN`表示输入参数，`OUT`表示输出参数，`INOUT`表示既可以输入也可以输出，`param_name`表示参数名称；`type`表示参数的类型，该类型可以是`MySQL`数据库中的任意类型。

#### 1.2 创建存储函数

创建存储函数，需要使用`CREATE FUNCTION`语句，语法格式如下：

~~~mysql
CREATE FUNCTION func_name([func_parameter])
	RETURNS type
[characteristic...]routine_body
~~~

`CREATE FUNCTION`为用来创建存储函数的关键字；`func_name`表示存储函数的名称；`func_parameter`为存储过程的参数列表，参数列表形式如下：

~~~mysql
[IN|OUT|INOUT] param_name type
~~~

其中`IN`表示输入参数，`OUT`表示输出参数，`INOUT`表示既可以输入也可以输出，`param_name`表示参数名称；`type`表示参数的类型，该类型可以是`MySQL`数据库中的任意类型。

`RETURNS type`语句表示函数返回数据的类型；`characteristic`表示存储函数的特性，取值与创建存储过程时相同。

#### 1.3 变量的使用

变量可以在子程序中声明并使用，这些变量的作用范围时在`BAGIN...END`程序中。

* 定义变量

  在存储过程中使用`DECLARE`语句定义变量，语法格式如下：

  ~~~mysql
  DECLARE var_name[,varname]...date_type[DEFAULT value];
  ~~~

  `var_name`为局部变量的名称，`DEFAULT value`子句给变量提供一个默认值，值除了可以被声明为一个常数之外，还可以被指定为一个表达式。如果没有`DEFAULT`子句，初始值为`NULL`。

* 为变量赋值

  定义变量之后，为变量赋值可以改变变量的默认值，`MySQL`中使用`SET`语句为变量赋值，格式如下：

  ~~~mysql
  SET var_name=expr[,var_name=expr]...;
  ~~~

  在存储程序中的`SET`语句是一般`SET`语句的扩展版本。被参考变量可能是子程序内声明的变量，或者是全局服务器变量，如系统变量或者用户变量。

  在存储程序中的`SET`语句作为预先存在的`SET`语法的一部分来实现。这允许`SET a=x,b=y,...`这样的扩展语法。其中不同的变量类型（局域声明变量及全局变量）可以被混合起来。这也允许把局部变量和一些指对系统变量有意义的选项合并起来。

  #### 1.4 定义条件和处理程序

  特定条件需要特定处理。这些条件可以联系到错误，以及子程序中的一般流程控制。定义条件是事先定义程序执行过程中遇到的问题，处理程序定义了在遇到这些问题时应当采取的处理方式，并且保证存储过程或函数在遇到警告或者错误时能继续执行。这样可以增强存储程序处理问题的能力，避免程序异常停止运行。

* 定义条件

  定义条件用`DECLARE`语句，格式如下：

  ~~~mysql
  DECLARE condition_name CONDITION FOR [condition_type]
  
  [condition_type]:
  	SQLSTATE[VALUE] sqlstate_value|mysql_error_code
  ~~~

  其中，`condition_name`参数表示条件的名称；`condition_type`参数表示条件的类型；`sqlstate_value`和`mysql_error_code`都可以表示`MySQL`的错误，`sqlstate_value`为长度为5的字符串类型错误代码，`mysql_error_code`为数值类型错误代码。
  
* 定义处理程序

  定义处理程序时，使用`DECLARE`语句的语法如下：

  ~~~mysql
  DECLARE handle_type HANDLER FOR condition_value[,...]sp_statement
  handler_type:
  	CONTINUE|EXIT|UNDO
  
  condition_value:
  	SQLSTATE[VALUE]sqlstate_value
  |condition_name
  |SQLWARNING
  |NOT FOUND
  |SQLEXCEPTION
  |mysql_error_code
  ~~~

  其中，`handler_type`为错误处理方式，参数取3个值；`CONTINUE、EXIT和UNDO`。`CONTINUE`表示遇到错误不处理，继续执行；`EXIT`遇到错误马上退出；`UNDO`表示遇到错误后撤回之前的操作，`MySQL`中暂时不支持这样的操作。

  #### 1.5 光标的使用

  查询语句可能返回多条记录，如果数据量非常大，需要在存储过程和储存函数中使用光标来逐条读取查询结果集中的记录。应用程序可以根据需要滚动或浏览其中的数据。

  光标必须在声明处理程序之前被声明，并且变量和条件还必须在声明光标或处理程序之前被声明。

* 声明光标

  `MySQL`中使用`DECLARE`关键字来声明光标，基本形式如下：

  ~~~mysql
  DECLARE cursor_name CURSOR FOR select_statement
  ~~~

  其中，`cursor_name`参数表示光标的名称，`select_statement`参数表示`SELECT`语句的内容，返回一个用于创建光标的结果集。

* 打开光标

  语法如下：

  ~~~mysql
  OPEN cursor_name{光标名称}
  ~~~

* 使用光标
  语法如下：

  ~~~mysql
  FETCH cursor_name INTO var_name[,var_name]...{参数名称}
  ~~~

  其中，`cursor_name`参数表示光标的名称；`var_name`参数表示将光标中的`SELECT`语句查询出来的信息存入该参数中，`var_name`必须在声明光标之前就定义好。

* 关闭光标

  语法如下：

  ~~~mysql
  CLOSE cursor_name{光标名称}
  ~~~

  #### 1.6 流程控制的使用

  流程控制语句用来根据条件控制语句的执行。`MySQL`中的用来构造控制流程的语句有：`IF`语句、`CASE`语句、`LOOP`语句、`WHILE`语句、`LEAVE`语句、`ITERATE`语句、`REPEAT`语句和`WHITE`语句。