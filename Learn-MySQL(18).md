# 第十八天

## 数据备份与还原

### 1. 数据备份

#### 1.1 使用`mysqldump`命令备份

`mysqldump`是`MySQL`提供的一个非常有用的数据库备份工具，该命令执行时，可以将数据库备份成一个文本文件，该文件中实际上包含了多个`CREATE和INSERT语句`，使用这些语句可以重新创建表和插入数据，语法格式如下：

~~~mysql
mysqldump -u user -h host -ppassword dbname[tbname,[tbname]]>filename.sql
~~~

`user`表示用户名称；`host`表示登录用户的主机名称；`password`为登录密码；`dbname`为需要备份的数据库名称；`tbname`为`dbname`数据库中需要备份的数据表，可以指定多个需要备份的表；右箭头符号`>`告诉`mysqldump`将备份数据表的定义和数据写入备份文件；`filename.sql`为备份文件的名称。
* 使用`mysqldump`备份单个数据库中的所有表
* 使用`mysqldump`备份数据库中的某个表

语法格式如下：

~~~mysql
mysqldump -u user -h host -p dbname [tbname,[tbname]]>filename.sql
~~~

`tbname`表示数据库中的表名，多个表名之间用空格隔开。

备份表和备份数据库中所有表的语句中不同的地方在于，要在数据库名称`dbname`之后指定需要备份的表名称。

* 使用`mysqldump`备份多个数据库

需要使用`--databases`参数，语法格式如下：

~~~mysql
mysqldump -u user -h host -p --database [dbname,[dbname...]]>filename.sql
~~~

使用`--database`参数之后，必须指定至少一个数据库的名称，多个数据库名称之间用空格隔开。

#### 1.2 直接复制整个数据库目录

因为`MySQL`表保存为文件方式，所以可以直接复制`MySQL`数据库的存储目录及文件进行备份。

#### 1.3 使用`mysqlhotcopy`工具快速备份

`mysqlhotcopy`是一个`Perl`脚本，它是备份数据库或单个表最快的途径，但它只能运行在数据库目录所在的机器上，并且只能备份`MyISAM`类型的表。`mysqlhotcopy`在`Unix`系统中运行。语法格式如下：

~~~mysql
mysqlhotcopy db_name_1,...db_name_n /path/to/new_directory
~~~

`db_name_1,...,db_name_n`分别为需要备份的数据库的名称；`/path/to/new_directory`指定备份文件目录。

### 2. 数据还原

#### 2.1 使用`mysql`命令还原

备份的`sql`文件中包含`CREATE、INSERT`语句（有时也会有`DROP`语句）。`mysql`命令可以直接执行文件中的这些语句，语法格式如下：

~~~mysql
mysql -u user -p [dbname]<filename.sql
~~~

`user`是执行`backup.sql`中语句的用户名；`-p`表示输入用户密码；`dbname`是数据库名。如果`filename.sql`文件为`mysqldump`工具创建的包含创建数据库语句的文件，执行的时候不需要指定数据库名。

#### 2.2 直接复制到数据库目录

如果数据库通过复制数据库文件备份，可以直接复制备份的文件到`MySQL`数据目录下实现还原。通过这种方式还原时，必须保存备份数据的数据库和待还原的数据库服务器的主版本号相同。而且这种方式只对`MyISAM`引擎的表有效，对于`InnoDB`引擎的表不可用。

执行还原以前关闭`mysql`服务，将备份的文件或目录覆盖`MySQL`的`data`目录，启动`mysql`服务。对于`Linux/Unix`操作系统来说，复制完文件需要将文件的用户和组更改为`mysql`运行的用户和组，通常用户是`mysql`，组也是`mysql`。

#### 2.3 `mysqlhotcopy`快速恢复

`mysqlhotcopy`备份后的文件也可以用来恢复数据库，在`MySQL`服务器停止运行时，将备份的数据库文件复制到`MySQL`存放数据的位置（`MySQL`的`data`文件夹），重新启动`MySQL`服务即可。如果以根据用户执行该操作，必须指定数据库文件的所有者，输入语句如下：

~~~mysql
chown -R mysql,mysql/var/lib/mysql/dbname
~~~

### 3. 数据库迁移

就是把数据从一个系统移动到另一个系统上，数据迁移有一下原因：

* 需要安装新的数据库服务器

* `MySQL`版本更新

* 数据库管理系统的变更

#### 3.1 相同版本的`MySQL`数据库之间的迁移

就是在主版本号相同的`MySQL`数据库之间进行数据库移动。迁移过程其实就是在源数据库备份和目标数据库还原过程的组合。

对于`InnoDB`表，不能用直接复制文件的方式备份数据库，因此最常用和最安全的方式是使用`mysqldump`命令导出数据，然后在目标数据库服务器使用`mysql`命令导入。

#### 3.2 不同版本的`MySQL`数据库之间的迁移

因为数据库升级等原因，需要将较旧版本`MySQL`数据库中的数据迁移到较新版本的数据库中。`MySQL`服务器升级时，需要先停止服务，然后卸载旧版本，并安装新版的`MySQL`，这种更新方法很简单，如果想保留旧版本中的用户访问控制信息，则需要备份`MySQL`中的`mysql`数据库，在新版本`MySQL`安装完成之后，重新读入`mysql`备份文件中的信息。

#### 3.3 不同数据库之间的迁移

迁移之前，需要了解不同数据库的架构，比较它们之间的差异。不同数据库中定义相同类型的数据关键字可能会不同。数据库迁移可以使用一些工具，例如在`Windows`系统下，可以使用`MyODBC`实现`MySQL`和`SQL Server`之间的迁移。`MySQL`官方提供的工具`MySQL Migration Toolkit`也可以在不同数据库间进行数据迁移。

### 4. 表的导出和导入

#### 4.1 用`SELECT...INTO OUTFILE`导出文本文件

`MySQL`数据库导出数据时，允许使用包含导出定义的`SELECT`语句进行数据的导出操作。该文件被创建到服务器主机上，因此必须拥有文件写入权限（`FILE`权限），才能使用此语法。`SELETC ...INTO OUTFILE 'filename'`形式的`SELECT`语句可以把被选择的行写入一个文件中，`filename`不能是一个已经存在的文件，语法格式如下：

~~~msyql
SELECT columnlist FROM table WHERE condition INTO OUTFILE 'filename' [OPTIONS]

--OPTIONS 选项
	FIELDS TERMINATED BY 'value'
	FIELDS [OPTIONALLY] ENCLOSED BY 'value'
	FIELDS ESCAPED BY 'value'
	LINES STARTING BY 'value'
	LINES TERMINATED BY 'value'
~~~

可以看到`SELECT columnlist FROM table WHERE condition`为一个查询语句，查询结果返回满足指定条件的一条或多条记录；`INTO OUTFILE`语句的作用就是把前面`SELECT`语句查询出来的结果导出到名称为`filename`的外部文件中。`[OPTIONS]`为可选参数选项。

#### 4.2 用`mysqldump`命令导出文本文件

该工具不仅可以将数据导出为包含`CREATE、INSERT`的`sql`文件，也可以导出为纯文本文件。

#### 4.3 用`mysql`命令导出文本文件

相比于`mysqldump`，`mysql`工具导出的结果可读性更强，语法格式如下：

~~~mysql
mysql -u root -p --execute="SELECT 语句"dbname>filename.txt
~~~

该命令使用`--excute`选项，表示执行该选项后面的语句并退出，后面的语句必须用双引号括起来，`dbname`为要导出的数据库名称；导出的文件中不同列之间使用制表符分隔，第一行包含了各个字段的名称。

#### 4.4 使用`LOAD DATA INFILE`方式导入文本文件

`MySQL`允许将数据导出到外部文件，也可以从外部文件导入数据。`MySQL`提供了一些导入数据的工具，这些工具有`LOAD DATA`语句、`source`命令和`mysql`命令。

#### 4.5 使用`mysqlimport`命令导入文本文件

`mysqlimport`命令提供许多与`LOAD DATA INFILE`语句相同的功能，大多选项直接对应`LOAD DATA INFILE`子句。使用`mysqlimport`语句需要指定所需的选项、导入的数据库名称以及导入的数据文件的路径和名称，语法格式如下：

~~~mysql
mysqlimport -u root -p dbname filename.txt [OPTIONS]
~~~