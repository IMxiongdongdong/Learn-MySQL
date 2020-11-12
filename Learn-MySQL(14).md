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

* `IF`语句

  `IF`语句包含多个条件判断，根据判断的结果为`TRUE`或者`FALSE`执行相应的语句，格式如下：

  ~~~mysql
  IF expr_condition THEN statement_list
  	[ELSEIF expr_condition THEN statement_list]...
  	[ELSE statement_list]
  END IF
  ~~~

  `IF`实现了一个基本的条件构造。如果`expr_condition`求值为真，相应的`SQL`语句列表被执行；如果没有`expr_condition`匹配，则`ELSE`子句里的语句列表被执行。`statement_list`可以包含一个或多个语句。

* `CASE`语句

  `CASE`是另一个进行条件判断的语句，该语句有2中语句格式，第一种如下：

  ~~~mysql
  CASE case_expr
  	WHEN when_value THEN statement_list
  	[WHEN when_value THEN statement_list]...
  	[ELSE statement_list]
  END CASE
  ~~~

  其中，`case_expr`参数表示条件判断的表达式，决定了哪一个`WHEN`子句会被执行；`when_value`参数表达式可能的值，如果某个`when_value`表达式与`case_expr`表达式结果相同，则执行对应`THEN`关键字后的`statement_list`中的语句；`statement_list`参数表示不同`when_value`值的执行语句。

* `LOOP`语句

  `LOOP`循环语句用来重复执行某些语句，与`IF`和`CASE`语句相比，`LOOP`只是创建一个循环操作的过程，并不进行条件判断。`LOOP`内的语句一直重复执行直到循环被退出，跳出循环过程，使用`LEAVE`子句，`LOOP`语句的基本格式如下：

  ~~~mysql
  [loop_label:]LOOP
  	statement_list
  END LOOP[loop_label]
  ~~~

  `loop_label`表示`LOOP`语句的标注名称，该参数可以省略；`statement_list`参数表示需要循环执行的语句。

* `LEAVE`语句

  `LEAVE`语句用来退出任何被标注的流程控制构造，格式如下：

  ~~~mysql
  LEAVE label
  ~~~

* `ITERATE`语句

  `ITERATE`语句将执行顺序转到语句段开头处，格式如下：

  ~~~mysql
  ITERATE label
  ~~~

  `ITERATE`纸可以出现在`LOOP`、`REPEAT`、`WHILE`语句内，`ITERATE`的意思为再次循环，`label`参数表示循环的表只。`ITERATE`语句必须跟在循环标志前面。

* `REPEAT`语句

  `REPEAT`语句创建一个带条件判断的循环过程，每次语句执行完毕之后，会对条件表达式进行判断，如果表达式为真，则循环结束；否则重复执行循环中的语句。基本格式如下：

  ~~~mysql
  [repeat_label:]REPEAT
  	statement_list
  UNTIL expr_condition
  END REPEAT [repeat_label]
  ~~~

  `repeat_label`为`REPEAT`语句的标注名称，该参数可以省略；`REPEAT`语句内的语句或语句群被重复，直至`expr_condition`为真。

* `WHILE`语句

  `WHILE`语句创建一个带条件判断的循环过程，与`REPEAT`不同，`WHILE`在执行语句执行时，先对指定的表达式进行判断，如果为真，则执行循环内的语句，否则退出循环。基本格式如下：

  ~~~mysql
  [while_label:]WHILE expr_condtion DO
  	statement_list
  END WHILE [while_label]
  ~~~

  `while_label`为`WHILE`语句的标注名称；`	expr_condition`为进行判断的表达式，如果表达式结果为真，`WHILE`语句内的语句或语句群被执行，直至`expe_condition`为假，退出循环。
  
  ### 2. 调用存储过程和函数
  
  #### 2.1 调用存储过程
  
  存储过程是通过`CALL`语句进行调用的，语法如下：
  
  ~~~mysql
  CALL sp_name([parameter[,...]])
  ~~~
  
  `CALL`语句调用一个先前用`CREATE PROCEDURE`创建的存储过程，其中`sp_name`为存储过程名称，`parameter`为存储过程的参数。
  
  #### 2.2 调用存储函数
  
  在`MySQL`中，存储函数的使用方法与`MySQL`内部函数的使用方法是一样的。换言之，用户自己定义的存储函数与`MySQL`内部函数是一个性质的。区别在于，存储函数是用户自己定义的，而内部是`MySQL`的开发者定义的。
  
  ### 3. 查看存储过程和函数
  
  `MySQL`存储了存储过程和函数的状态信息，用户可以使用`SHOW STATUS`语句或`SHOW CREATE`语句来查看，也可以直接从系统的`information_shema`数据库中查询。
  
  #### 3.1 `SHOW STATUS`语句查看存储过程和函数的状态
  
  语法格式如下：
  
  ~~~mysql
  SHOW {PROCEDURE|FUNCTION} STATUS [LIKE 'pattern']
  ~~~
  
  这个语句是一个`MySQL`的扩展。它返回子程序的特征，如数据库、名字、类型、创建者及创建和修改日期。如果没有指定样式，根据使用的语句，所有存储程序或存储函数的信息都被列出。`PROCEDURE`和`FUNCTION`分别表示查看存储过程和函数；`LIKE`语句表示匹配存储过程或函数的名称。
  
  #### 3.2 `SHOW CREATE`语句查看存储过程和函数的定义
  
  ~~~mysql
  SHOW CREATE {PROCEDURE|FUNCTION} sp_name
  ~~~
  
  这个语句是一个`MySQL`的扩展。类似于`SHOW CREATE TABLE`，它返回一个可用来重新创建已命名子程序的确切字符串。`PROCEDURE`和`FUNCTION`分别表示查看存储过程和函数；`LIKE`语句表示匹配存储过程或函数的名称。
  
  #### 3.3 从`information_schema.Routines`表中查看存储过程和函数的信息
  
  `MySQL`中存储过程和函数的信息存储在`information_schema`数据库下的`Routines`表中。可以通过查询该表的记录查询存储过程和函数的信息。
  
  ~~~mysql
  SELECT * FROM information_shcema.Routines
  WHERE ROUTINE_NAME='sp_name';
  ~~~
  
  ### 4. 修改存储过程和函数
  
  使用`ALTER`语句可以修改存储过程或函数的特性。
  
  ~~~mysql
  ALTER {PROCEDURE|FUNCTION} sp_name {characteristic...}
  ~~~
  
  其中，`sp_name`参数表示存储过程或函数的名称；`characteristic`参数指定存储函数的特性。
  
  ### 5. 删除存储过程和函数
  
  可以使用`DROP`语句，格式如下：
  
  ~~~mysql
  DROP {PROCEDURE|FUNCTION} [IF EXISTS] sp_name
  ~~~
  
  这个语句被用来移除一个存储过程或函数。`sp_name`为要移除的存储过程或函数的名称。
  
  `IF EXISTS`子句是一个`MySQL`的扩展。如果程序或函数不存储，它可以防止发生错误，产生一个用`SHOW WARNINGS`查看的警告。