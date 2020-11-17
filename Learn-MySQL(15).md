# 第十五天

## 视图

数据库的视图是一个虚拟表。同真实的表一样，视图包含一系列带有名称的行和列数据。行和列数据来自由定义视图查询所引用的表，并且在引用视图时动态生成。

### 1. 视图概述

视图是从一个或者多个表中导出的，视图的行为与表非常相似，但视图是一个虚拟表。在视图中用户可以使用`SELECT`语句查询数据，以及使用`INSERT`、`UPDATE`、`DELETE`修改记录。

#### 1.1 视图的含义

视图是一个虚拟表，是从数据库中一个或多个表中导出来的表。视图还可以从已经存在的视图的基础上定义。

视图一经定义便存储在数据库中，与其相对应的数据并没有像表那样在数据库中再存储一份，通过视图看到的数据只是存放再基本表中的数据。对视图的操作与对表的操作一样，可以对其进行查询、修改和删除。当对通过视图看到的数据进行修改时，相应的基本表的数据也要发生变化；同时，若基本表的数据发生变化，则这种变化也可以自动的反映到视图中。

#### 1.2 视图的作用

与直接从数据表中读取相比，视图有以下优点：

* 简单化

  看到的就是需要的，视图不仅可以简化用户对数据的理解，也可以简化他们的操作。那些被经常使用的查询可以被定义为视图，从而使得用户不必为以后的操作每次指定全部的条件。

* 安全性

  通过视图用户只能查询和修改他们所能见到的数据。数据库中的其他数据则既看不见也取不到。数据库授权命令可以使每个用户对数据库的检索限制到特定的数据库对象上，但不能授权到数据库特定行和特定的列上。
  
* 视图可以帮助用户屏蔽真是表结构变化带来的影响。

### 2. 创建视图

视图中包含了`SELECT`查询的结果，因此视图的创建基于`SELECT`语句和已存在的数据表，视图可以建立在一张表上，也可以建立在多张表上。

#### 2.1 创建视图的语法形式 

创建视图使用`CREATE VIEW`语句，基本语法格式如下：

~~~mysql
CREATE [OR REPLACE] [ALGORITHM = {UNDEFINED|MERGE|TEMPTABLE}]
	VIEW view_name [(column_list)]
	AS SELECT_statement
	[WITH[CASCADED|LOCAL]CHECK OPTION]
~~~

其中，`CREATE`表示创建新的视图；`REPLACE`表示替换已经创建的视图；`ALGORITHM`表示视图选择的算法；`view_name`为视图的名称，`column_list`为属性列；`SELECT_statement`表示`SELECT`语句；`WITH[CASCADED|LOCAL]CHECK OPTION`参数表示视图在更新时保证在视图的权限范围内。

`ALGORITHM`的取值有3个，分别是`UNDEFINED|MERGE|TEMPTABLE`，`UNDEFINED`表示`MySQL`将自动选择算法；`MERGE`表示将使用的视图语句与视图定义合并起来，使得视图定义的某一部分取代语句对应的部分，`TEMPTABLE`表示将视图的结果存入临时表，然后用临时表来执行语句。

`CASCADED`与`LOCAL`为可选参数，`CASCADED`为默认值，表示更新视图时要满足所有相关视图和表的条件；`LOCAL`表示更新视图时满足该视图本身定义的条件即可。

该语句要求具有针对视图的`CREATE VIEW`权限，以及针对由`SELECT`语句选择的每一列上的某些权限。对于在`SELECT`语句中其他地方使用的列，必须具有`SELECT`权限。如果还有`OR REPLACE`子句，必须在视图上具有`DROP`权限。

视图属于数据库。在默认情况下，将在当前数据库创建新视图。要想在给定数据库中明确创建视图，创建时应将名称指定为`db_name.view_name`。

#### 2.2 在单表上创建视图

#### 2.3 在多表上创建视图

可以在两个或两个以上的表上创建视图，可以使用`CREATE VIEW`语句实现。

### 3. 查看视图

查看视图是查看数据库中已存在的视图的定义。查看视图必须要有`SHWO VIEW `的权限，`MySQL`数据库下的`user`表中保存着这个信息。查看视图的方法包括：`DESCRIBE`、`SHOW TABLE STATUS`和`SHOW CREATE VIEW`。

#### 3.1 `DESCRIBE`语句查看视图基本信息

语法如下：

~~~mysql
DESCRIBE 视图名;
~~~

#### 3.2 `SHOW TABLE STATUS`语句查看视图基本信息

语法如下：

~~~mysql
SHOW TABLE STATUS LIKE '视图名';
~~~

#### 3.3 `SHOW CREATE VIEW`语句查看视图详细信息

语法如下：

~~~mysql
SHOW CREATE VIEW 视图名;
~~~

#### 3.4 在`VIEWS`表中查看视图详细信息

通过对`views`表的查询，可以查看数据库中所有视图的详细信息，语句如下：

~~~mysql
SELECT * FROM information_schema.views;
~~~

### 4. 修改视图

修改视图是指修改数据库中存在的视图，当基本表的某些字段发生变化的时候，可以通过修改视图来保持与基本表的一致性。

#### 4.1 `CRETAE OR REPLACE VIEW`语句修改视图

语法如下：

~~~mysql
CREATE [OR REPLACE] [ALGORITHM = {UNDEFINED|MERGE|TEMPTABLE}]
	VIEW view_name[(column_list)]
	AS SELECT_statement
	[WITH[CASCADED|LOCAL]CHECK OPTION]
~~~

#### 4.2 `ALTER`语句修改视图

语法如下：

~~~mysql
ALTER [ALGORITHM = {UNDEFINED|MERGE|TEMPTABLE}]
	VIEW view_name [(column_list)]
	AS SELECT_statement
	[WITH|CASCADED|LOCAL]CHECK OPTION
~~~

### 5. 更新视图

通过视图来插入、更新、删除表中的数据，因为视图是一个虚拟表，其中没有数据。通过视图更新的时候都是转到基本表上进行更新的，如果对视图增加或者删除记录，实际上是对其基本表增加或者删除记录。

### 6. 删除视图

当视图不在需要时，可以将其删除，删除一个或多个视图可以使用`DROP VIEW`语句，语法如下：

~~~mysql
DROP VIEW [IF EXISTS]
	view_name [,view_name]...
	[RESTRICT|CASCADE]
~~~

其中，`view_name`是要删除的视图名称，可以添加多个需要删除的视图名称，各个名称之间使用逗号隔开。删除视图必须拥有`DROP`权限。