# 第十七天

## `MySQL`用户管理

`MySQL`是一个多用户数据库，具有功能强大的访问控制系统，可以为不同用户指定允许的权限。`MySQL`用户可以分为普通用户和`root`用户。`root`用户是超级管理员，拥有所有权限，包括新建用户、删除用户和修改用户的密码等管理权限；普通用户只拥有被授予的各种权限。用户管理包括管理用户章虎、权限等。

### 1. 权限表

`MySQL`服务器通过权限表来控制用户对数据库的访问，权限表存放在`mysql`数据库中，由`mysql_install_db`脚本初始化。存储账户权限信息表主要有：`user`、`db`、`table_priv`、`columns_priv`、`procs_priv`。

#### 1.1 `user`表

`user`表是`MySQL`中最重要的一个权限表，记录允许连接到服务器的账号信息，里面的权限是全局级的。

#### 1.2 `db`表和`host`表

`db`表和`host`表是`MySQL`数据中非常重要的权限表。`db`表中存储了用户对某个数据库的操作权限，决定用户能从哪个主机存储哪个数据库。`host`表中存储了某个主机对数据库的操作权限，配合`db`权限表对给定主机上数据库级操作权限做更细致的控制。这个权限表不受`GRANT`和`REVOKE`语句的影响。`db`表比较常用，`host`表一般很少使用。`db`表和`host`表结构相似，字段大致可以分为两类：用户列和权限列。

#### 1.3 `table_priv`表和`columns_priv`表

`table_priv`表用来对表设置操作权限，`columns_priv`表用来对表的某一列设置权限。

#### 1.4 `procs_priv`表

可以对存储过程和存储函数设置操作权限。

### 2. 账户管理

#### 2.1 登录和退出`MySQL`服务器

#### 2.2 新建普通用户

有两种方式，一种是使用`CREATE USER`或`GRANT`语句，另一种是直接操作`MySQL`授权表。

* 使用`CREATE USER`语句创建新用户

  ~~~mysql
  CREATE USER user_specification
  	[,user_specification]...
  
  user_specification:
  	user@host
  	[
          IDENTIFIED BY [password]'password'
          |IDENTIFIED WITH auth_plugin[AS 'auth_string']
      ]
  ~~~

  `user`表示创建的用户的名称；`host`表示允许登录的用户主机名称；`IDENTIFIED BY`表示用来设置用户的密码；`[PASSWORD]`表示使用哈希值设置密码，该参数可选；`password`表示用户登录时使用的普通明文密码；`IDENTIFIED WITH `语句为用户指定一个身份验证插件；`auth_plugin`时插件的名称，插件的名称可以是一个带单引号的字符串，或者带引号的字符串；`auth_string`是可选的字符串参数，该参数将床底给身份验证插件，由该插件解释该参数的意义。

* 使用`GRANT`语句创建新用户

  `CREATE USER`语句可以用来创建账户，通过该语句可以在`user`表中添加一条新的记录，但是`CREATE USER`语句创建的新用户没有任何权限，还需要使用`GRANT`语句赋予用户权限。而`GRANT`语句不仅可以创建新用户，还可以在创建的同时对用户授权。`GRANT`还可以指定账户的其他特点，如使用安全连接、限制使用服务器资源等。使用`GRANT`语句创建新用户时必须有`GRANT`权限。`GRANT`语句是添加新用户并授权他们访问`MySQL`对象的首选方法，`GRANT`语句的基本语法格式如下：

  ~~~mysql
  GRANT priviliges ON db.table
  TO user@host [IDENTIFIED BY 'password'][,user[IDENTIFIED BY 'password']]
  [WITH GRANT OPTION];
  ~~~

  其中，`priviliges`表示赋予用户的权限类型；`db.table`表示用户的权限所作用的数据库中的表；`IDENTIFIED BY`关键字用来设置密码；`password`表示用户密码；`WITH GRANT OPTION`为可选参数，表示对新建立的用户赋予`GRANT`权限，即该用户可以对其他用户赋予权限。

* 直接操作`MySQL`用户表

  不管是`CREATE USER`或者`GRANT`，在创建新用户时，实际上都是在`user`表中添加一条新的记录。因此，可以使用`INSERT`语句向`user`表中直接插入一条记录来创建一个新的用户。使用`INSERT`语句，必须拥有对`mysql.user`表的`INSERT`权限。使用`INSERT`语句来创建新用户的基本语法格式如下：

  ~~~mysql
  INSERT INTO mysql.user(Host,User,Password,[privilegelist])
  VALUES('host','username',PASSWORD('password')，privilegevaluelist);
  ~~~

  `Host`、`User`、`Password`分别为`user`表中的主机、用户名和密码字段；`privilegelist`表示用户的权限，可以用多个权限；`PASSWORD()`函数为密码加密函数；`privilegevaluelist`为对应的权限的值，只能取`Y`或者`N`。

  #### 2.3 删除普通用户

  可以使用`DROP USER`语句删除用户，也可以直接通过`DELETE`从`mysql.user`表中删除对应的记录来删除用户。

* 使用`DROP USER`语句删除用户

  语法如下：

  ~~~mysql
  DROP USER user [,user];
  ~~~

  `DROP USER`语句用于删除一个或多个`MySQL`账户。要使用`DROP USER`，必须拥有`MySQL`数据库的全局`CREATE USER`权限或`DELETE`权限。

* 使用`DELETE`语句删除用户

  语法如下：

  ~~~mysql
  DELETE FROM MySQL.user WHERE host='hostname' and user='username'
  ~~~

  `host`和`user`为`user`表中的两个字段，两个字段的组合确定所要删除的账户记录。

  #### 2.4 `root`用户修改自己的密码、

* 使用`mysqladmin`命令在命令行指定新密码

  语法格式如下：

  ~~~mysql
  mysqladmin -u username -h localhost -p password 'newpwd'
  ~~~

  `username`为要修改密码的用户名称，在这里指定为`root`用户；参数`-h`指需要修改的、对应哪个主机用户的密码，该参数可以不写，默认是`localhost`；`-p`表示输入当前密码；`password`为关键字，后面双引号内的内容`newpwd`为新设置的密码。执行完上面的语句，`root`用户的密码将被修改为`newpwd`。

* 修改`mysql`数据库的`user`表

  因为所有账户信心都保存在`user`表中，因此可以通过直接修改`user`表来改变用户的密码。`root`用户登录到`MySQL`服务器后，使用`UPDATE`语句修改`mysql`数据库的`user`表的`password`字段，从而修改用户的密码，语句如下：

  ~~~mysql
  UPDATE mysql.user set Password=PASSWORD('rootpwd') WHERE User='root' and 'Host'='localhost';
  ~~~

  `PASSWORD()`函数用来加密用户密码。执行`UPDATE`语句后，需要执行`FLUSH PRIVILEGES`语句重新加载用户权限。

* 使用`SET`语句修改`root`用户的密码

  `SET PASSWORD`语句可以用来重新设置其他用户的登录密码或者自己使用的账户的密码，语法结构如下：

  ~~~mysql
  SET PASSWORD=PASSWORD('rootpwd');
  ~~~

  