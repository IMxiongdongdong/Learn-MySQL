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
