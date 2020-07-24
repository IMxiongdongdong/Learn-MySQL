# 第二天

## 1 数据库操作

### 1.1 新建数据库

* 查看数据库

  ~~~sql
  SHOW DATABASES; 
  ~~~

* 新建数据库

  ~~~sql
  CREATE DATABASE database_name;
  ~~~
* 查看数据库引擎
  ~~~sql
  SHOW CREATE DATABASE database_name \G;
  ~~~
### 1.2 删除数据库
* 删除数据库

  ~~~SQL
  DROP DATABASE database_name;
  ~~~

### 1.3 数据库引擎

* 查看引擎

  ~~~sql
SHOW ENGINES \G;
  ~~~

***

## 2 表操作

### 2.1 创建表

* 创建表

~~~sql
  CREATE TABLE 表名
  (
      字段名1， 数据类型 [列级别约束条件] [默认值]，
      字段名2， 数据类型 [列级别约束条件] [默认值]，
  );
~~~

~~~sql
  CREATE TABLE tb_emp1
  (
      id INT(11),
      name VARCHAR(25),
      deptId INT(11),
      salary FLOAT
  );
~~~

* 主键约束

  * 在定义列的同时指定主键

  ~~~sql
  字段名 数据类型 PRIMARY KEY [默认值]
  ~~~

  ~~~sql
    CREATE TABLE tb_emp2
    (
        id INT(11) PRIMARY KEY,
        name VARCHAR(25),
        deptId INT(11),
        salary FLOAT
    );
  ~~~

  * 在定义完所有列之后指定主键

  ~~~sql
  [CONSTRAINT<约束名>]PRIMARY KEY[字段名]
  ~~~

  ~~~sql
  CREATE TABLE tb_emp3
  (
    id INT(11),
      name VARCHAR(25),
      deptId INT(11),
      salary FLOAT,
      PRIMARY KEY(id)
  );
  ~~~
  
  