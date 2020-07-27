# 第四天

## 2 表操作

### 2.2 修改数据表

* 修改字段的数据类型

  ~~~sql
  ALTER TABLE <表名> MODIFY <字段名> <数据类型>;
  ~~~

  ~~~sql
  ALTER TABLE tb_dept1 MODIFY name VARCHAR(30);
  ~~~

* 修改字段名

  ~~~sql
  ALTER TABLE <表名> CHANGE <旧字段名> <新字段名> <新数据类型>;
  ~~~

  ~~~sql
  ALTER TABLE tb_dept1 CHANGE location loc VARCHAR(50);
  ~~~

* 添加字段
  ~~~sql
  ALTER TABLE <表名> ADD <新字段名> <数据类型>
  [约束条件][FIRST | AFTER 已存在字段名]；
  ~~~
  `FIRST`或`AFTER`已存在字段名，用于指定新增字段在表中的位置。
  
  * 添加有完整性约束条件的字段
  
    ~~~sql
    ALTER TABLE tb_dept1 ADD column1 VARCHAR(12) not null;
    ~~~
  
  * 在表的第一列添加一个字段
  
    ~~~sql
    ALTER TABLE tb_dept1 ADD column2 INT(11) FIRST;
    ~~~
  
  * 在表的指定列之后添加一个字段
  
    ~~~sql
    ALTER TABLE tb_dept1 ADDcolumn3 INT(11) AFTER name;
    ~~~

   * 删除字段

     ~~~sql
     ALTER TABLE <表名> DROP <字段名>；
     ~~~

     ~~~sql
     ALTER TABLE tb_dept1 DROP column2;
     ~~~

* 修改字段的排列位置

  ~~~sql
  ALTER TABLE <表名> MODIFY <字段1> <数据类型> FIRST | AFTER <字段2>；
  ~~~

  `FIRST`为可选字段，指将“字段1”修改为表的第一个字段，“`AFTER`字段2”指将“字段1”插入到“字段2”后面。

  * 修改字段为表的第一个字段

    ~~~sql
    ALTER TABLE tb_dep1 MODIFY column1 VARCHAR(12) FIRST;
    ~~~

  * 修改字段到表的指定列之后

    ~~~sql
    ALTER TABLE tb_dept1 MODIFY column1 VARCHAR(12) AFTER location;
    ~~~


* 更改表的存储引擎

  ~~~sql
  ALTER TABLE <表名> ENGINE=<更改后的存储引擎名>；
  ~~~

  ~~~sql
  ALTER TABLE tb_deptment3 ENGINE=MyISAM;
  ~~~

* 删除表的外键约束

  ~~~sql
  ALTER TABLE <表名> DROP FOREIGN KEY <外键约束名>
  ~~~

  外键一旦删除，就会解除主表和从表之间的关联关系。

  * ~~~sql
    ALTER TABLE tb_emp9 DROP FOREIGN KEY fk_emp_dept;
    ~~~

* 删除没有被关联的表

  使用`DROP TABLE`可以一次删除一个或多个没有被其他表关联的数据表。

  ~~~sql
  DROP TABLE [IF EXITS]表1, 表2,...表n;
  ~~~

  参数`IF EXITS`用于删除前判断删除的表是否存在，加上该参数后，在删除表的时候，如果表不存在，`SQL`语句可以顺利执行，但是会发出警告。

* 删除被其他表关联的主表

  不能直接删除，需要先解除关联。