# 第三天

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

  * 联合主键

    ~~~sql
    CREATE TABLE tb_emp4
    (
        name VARCHAR(25),
        deptId INT(11),
        salary FLOAT,
        PRIMARY KEY(name,deptId)
    );
    ~~~

* 外键约束

  外键用来在两个表的数据之间建立链接，它可以是一列或者多列。一个表可以有一个或多个外键。外键对应的是参照完整性，一个表的外键可以为空值，若不为空值，则每一个外键值必须等于另一个表中主键的某个值。

  外键是表中的一个字段，它可以不是本表的主键，但对应另外一个表的主键。外键主要作用是保证数据引用的完整性，定义外键后，不允许删除在另一个表中具有关联关系的行。外键的作用是保持数据的一致性、完整性。

  主表（父表）：对于两个具有关联关系的表而言，相关联字段中主键所在的那个表即是主表。

  从表（子表）：对于两个具有关联关系的表而言，相关联字段中外键所在的那个表即是从表。

  ~~~sql
  [CONSTRAINT <外键名>] FOREIGN KEY 字段名1[,字段名2,...] REFERENCES <主表名> 主键列1[,主键列2,...]
  ~~~

  ~~~sql
  CREATE TABLE tb_emp5
  (
  	id INT(11)PRIMARY KEY,
      name VARCHAR(25),
      deptId INT(11),
      salary FLOAT,
      CONSTRAINT fk_emp_dept1 FOREIGN KEY(deptId) REFERENCES tb_dept1(id)
  );
  ~~~

  在表`tb_emp5`上添加了名称为`fk_emp_dept1`的外键约束，外键名称为`deptId`，其依赖于表`tb_dept1`的主键`id`。

* 使用非空约束

  非空约束指的是字段的值不能为空，如果用户在添加数据时没有指定值，数据库系统会报错。

  ~~~sql
  CREATE TABLE tb_emp6
  (
  	id INT(11)PRIMARY KEY,
      name VARCHAR(25) NOT NULL,
      deptId INT(11),
      salary FLOAT
  );
  ~~~

* 使用唯一性约束

  唯一性约束要求该列唯一，允许为空，但只能出现一个空值。唯一约束可以确保一列或者几列不出现重复值。

  ~~~sql
  CREATE TABLE tb_dept7
  (
  	id INT(11),
      name VARCHAR(22)UNIQUE,
      location VARCHAR(50)
  );
  ~~~

* 使用默认约束

  默认约束指定某列的默认值。

  ~~~sql
  CREATE TABLE tb_emp8
  (
  	id INT(11),
      name VARCHAR(25),
      deptId INT(11) DEFAULT 1111,
      salary FLOAT
  );
  ~~~

* 设置表属性值自动增加

  一个表只能有一个字段使用`AUTO_INCREMENT`约束，且该字段必须为主键的一部分。它约束的字段可以是任何整数类型。

  ~~~sql
  CREATE TABLE tb_emp9
  (
  	id INT(11) PRIMARY KEY AUTO_INCREMENT,
      name VARCHAR(25),
      deptId INT(11)
  );
  ~~~

  