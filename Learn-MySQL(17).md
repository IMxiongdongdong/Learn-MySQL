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

#### 2.5 `root`用户修改普通用户密码

  `root`用户拥有很高的权限，不仅可以修改自己的密码，还可以修改其他用户的密码。`root`用户登录`MySQL`服务器后，可以通过`SET`语句修改`mysql.user`表，以及`GRANT`语句修改用户的密码。

* 使用`SET`语句修改普通用户的密码，语法格式如下：

  ~~~mysql
  SET PASSWORD FOR 'user'@'host' = PASSWORD('somepassword');
  ~~~

  只有`root`可以通过更新`mysql`数据库的用户来更改其他用户的密码。如果使用普通用户修改，可省略`FOR`子句更改自己的密码。
  
* 使用`UPDATE`语句修改普通用户的密码

  使用`root`用户登录`MySQL`服务器后，可以使用`UPDATE`语句修改`mysql`数据库的`user`表的`password`字段，从而修改普通用户的密码，语法格式如下：

  ~~~mysql
  UPDATE MySQL.user SET Password=PASSWORD("pwd")
  WHERE User="username" AND Host="hostname";
  ~~~

  `PASSWORD()`函数用来加密用户密码。执行`UPDATE`语句后，需要执行`FLUSH PRIVILEGES`语句重新加载用户权限。

* 使用`GRANT`语句修改普通用户密码

  除了前面介绍的方法，还可以在全局级别使用`GRANT USAGE`语句`(*.*)`指定某个账户的密码而不影响账户当前的权限，使用`GRANT`语句修改密码，必须拥有`GRANT`权限。一般情况下最好使用该方法来指定或修改密码：

  ~~~mysql
  MySQL>GRANT USAGE ON *.* TO 'someuser'@'%'IDENTIFIED BY 'somepassword';
  ~~~

#### 2.6 普通用户修改密码

普通用户登录`MySQL`服务器后，通过`SET`语句设置自己的密码，语法格式如下：

~~~mysql
SET PASSWORD = PASSWORD("newpassword");
~~~

其中，`PASSWORD()`函数对密码进行加密，`newpassword`是设置的新密码。

#### 2.7 `root`用户密码丢失的解决办法

对于`root`用户密码丢失这种特殊情况，`MySQL`实现了对应的处理机制。可以通过特殊方法登录到`MySQL`服务器，然后在`root`用户下重新设置密码，步骤如下：

* 使用`--skip-grant-tables`选项启动`MySQL`服务

  以此选项启动时，`MySQL`服务器将不加载权限判断，任何用户都能访问数据库。在`windows`操作系统种，可以使用`mysqld`或`mysqld-nt`来启动`MySQL`服务进程。如果`MySQL`的目录已经添加到环境变量中，可以直接使用`mysqld、mysqld-nt`命令启动`MySQL`服务。否则需要先在命令行下切换到`MySQL`的`bin`目录。

  `mysqld`命令如下：

  ~~~mysql
  mysqld --skip-grant-tables
  ~~~

  `mysqld-nt`命令如下：

  ~~~mysql
  mysqld-nt --skip-grant-tables
  ~~~

* 使用`root`用户登录，重新设置密码

  步骤如下：

  使用`net stop mysql`命令停止`MySQL`服务进程；

  在命令行输入`mysqld --skip-grant-tales`选项启动`MySQL`服务；

  打开另一个命令行窗口，输入不加密码的登录命令`mysql -u root`；

  登录成功后，可以使用`UPDATE`语句或者使用`mysqladmin`命令重新设置`root`密码。设置语句如下：

  ~~~mysql
  mysql> UPDATE mysql.user SET Password=PASSWORD('newpwd') WHERE User='root' and Host='localhost';
  ~~~

* 加载权限表

  修改密码完成后，必须使用`FLUSH PRIVILEGES`语句加载权限表。加载权限表后，新的密码才会生效，同时`MySQL`服务器开始权限验证。输入语句如下：

  ~~~mysql
  mysql>FLUSH PRIVILEGES;
  ~~~

  修改密码完成后，将输入`mysqld --skip-grant-tables`命令的命令行窗口关闭，接下来就可以使用新设置的密码登录`MySQL`了。

### 3. 权限管理

  权限管理主要是对登录到`MySQL`的用户进行权限验证。所有用户的权限都存储在`MySQL`的权限表中，不合理的权限规划会给`MySQL`服务器带来安全隐患。数据库管理员要对所有用户的权限进行合理规划管理。`MySQL`权限系统的主要功能是证实连接到一台给定主机的用户，并且赋予该用户在数据库上的`SELECT、INSERT、UPDATE和DELETE`权限。

#### 3.1 `MySQL`的各种权限

  账户权限信息被存储在`MySQL`数据库的`user、db、host、tables_priv、columns_priv和procs_priv`表中。在`MySQL`启动时，服务器将这些数据库表中权限信息的内容读入内存。

#### 3.2 授权

  授权就是为某个用户授予权限。合理的授权可以保证数据库的安全。`MySQL`中可以使用`GRANT`语句为用户授予权限。

  授予的权限可以分为多个层级：

* 全局层级

  全局权限适用于一个给定服务器中的所有数据库，这些权限存储在`mysql.user`表中。`GRANT ALL ON *.*`和`REVOKE ALL ON *.*`只授予和撤销全局权限。

* 数据库层级

  数据库权限适用于一个给定数据库中的所有目标。这些权限存储在`mysql.db`和`mysql.host`表中。`GRANT ALL ON db_name.`和`REVOKE ALL ON db_name.*`只授予和撤销数据库权限。

* 表层级

  表权限适用于一个给定表中的所有列。这些权限存储在`mysql.tables_Priv`表中。`GRANT ALL ON db_name.tbl_name`和`REVOKE ALL ON db_name.tbl_name`只授予和撤销表权限。

* 列层级

  列权限适用于一个给定表中的单一列。这些权限存储在`mysql.columns_priv`表中。当使用`REVOKE`时，必须指定与被授权列相同的列。

* 子程序层级

  `CREATE ROUTINE、ALTER ROUTINE、EXECUTE和GRANT`权限适用于已存储的子程序。这些权限可以被授予为全局层级和数据库层级。而且，除了`CREATE ROUTINE`外，这些权限可以被授予子程序层级，并存储在`mysql.procs_priv`表中。

  在`MySQL`中，必须是拥有`GRANT`权限的用户才可以执行`GRANT`语句。

  要使用`GRANT`或`REVOKE`，必须拥有`GRANT OPTION`权限，并且必须用于正在授予或撤销的权限。

#### 3.3 收回权限

收回权限就是取消已经赋予用户的某些权限。收回用户不必要的权限可以在一定程度上保证系统的安全性。`MySQL`中使用`REVOKE`语句取消用户的某些权限。使用`REVOKE`收回权限之后，用户账户的记录将从`db、host、tables_priv和columns_priv`表中删除，但是用户账号记录仍然在`user`表中保存。

#### 3.4 查看权限

`SHOW GRANTS`语句可以显示指定用户的权限信息，使用`SHOW GRANT`查看账户信息的基本语法格式如下：

~~~mysql
SHOW GRANTS FOR 'user'@'host';
~~~

其中，`user`表示登录用户的名称，`host`表示登录的主机名称或者`IP`地址。在使用该语句时，要确保指定的用户名和主机名都要用单引号括起来，并使用`@`符号，将两个名字分隔开。

### 4. 访问控制

`MySQL`的访问控制分为两个阶段：连接核实阶段和请求核实阶段。

#### 4.1 连接核实阶段

当连接`MySQL`服务器时，服务器基于用户的身份以及用户是否能通过正确的密码身份验证，来接受或拒绝连接。即客户端用户连接请求中会提供用户名称、主机地址名和密码，`MySQL`使用`user`表中的3个字段（`Host、User和Password`）执行身份检查，服务器只在有`user`表记录的`Host`和`User`字段匹配客户端主机名和用户名，并且提供正确的密码时才接受连接。如果连接核实没有通过，服务器完全拒绝访问；否则，服务器接受连接，然后进入阶段2等待用户请求。

#### 4.2 请求核实阶段

建立了连接之后，服务器进入访问控制的阶段2。对在此连接上进来的每个请求，服务器检查用户执行的操作，然后检查是否有足够的权限来执行它。这正是在授权表中的权限列发挥作用的地方。这些权限可以来自`user、db、host、table_priv或column_priv`表。