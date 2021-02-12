# 第 3 章 新手入门之 SQL 基础语法

## 3-1 dos 连接 MySQL

MySQL 是一个需要账户名密码登录的数据库，登陆后使用，它提供了一个默认的 root 账号，使用安装时设置的密码即可登录。退出 MySQL：`exit`。

1. `mysql -u用户名 -p密码`。这样会暴露数据库密码。

   ```powershell
   > mysql -uroot -proot
   ```

2. `mysql -u用户名 -p回车`。回车后输入数据库密码。这样数据库密码会隐藏。

   ```powershell
   > mysql -uroot -p
   ****
   ```

3. `mysql -hip地址 -u用户名 -p密码`。指定数据库安装地址。本机安装为 `127.0.0.1`。

   ```powershell
   > mysql -h127.0.0.1 -uroot -proot
   # or
   > mysql -h127.0.0.1 -uroot -p
   ****
   ```

4. `mysql --host=ip地址 --user=用户名 --password=密码`。指定数据库安装地址。本机安装为 `localhost`。

   ```powershell
   > mysql --host=localhost --user=root --password=root
   # or
   > mysql --host=localhost --user=root --password
   ****
   ```

5. `exit`。退出 MySQL。

## 3-2 DBMS、数据库以及表之间的关系

数据库管理系统（DataBase Management System，**DBMS**）：指一种操作和管理数据库的大型软件，用于建立、使用和维护数据库，对数据库进行统一管理和控制，以保证数据库的安全性和完整性。用户通过数据库管理系统访问数据库中表内的数据

数据库管理程序(DBMS)可以管理多个数据库，一般开发人员会针对每一个应用创建一个数据库。为保存应用中实体的数据，一般会在数据库创建多个表，以保存程序中实体的数据。数据库管理系统、数据库和表的关系如图所示：

![dbms-database-table](https://img.zxj.guru/mysql/img/dbms-database-table.png)

先有数据库 → 再有表 → 再有数据。一个库包含多个表。

## 3-3 SQL 的概述以及分类

### 什么是 SQL

结构化查询语言(Structured Query Language)简称 SQL, SQL 语句就是对数据库进行操作的一种语言。

### SQL 作用

通过 SQL 语句我们可以方便的操作数据库中的数据库、表、数据。SQL 是数据库管理系统都需要遵循的规范。不同的数据库生产厂商都支持 SQL 语句，但都有特有内容。

### SQL 语句分类

- DDL(`Data Definition Language`)数据定义语言。用来定义数据库对象：数据库，表，列等。关键字：`create`, `drop`, `alter` 等。
- DML(`Data Manipulation Language`)数据操作语言。用来对数据库中表的数据进行增删改。关键字：`insert`, `delete`, `update` 等。
- DQL(`Data Query Language`)数据查询语言。用来查询数据库中表的记录(数据)。关键字：`select`, `where` 等。
- DCL(`Data Control Language`)数据控制语言(了解)。用来定义数据库的访问权限和安全级别，及创建用户。关键字：`GRANT`， `REVOKE`等。

## 3-4 SQL 通用语法

1. SQL 语句可以单行或多行书写，以分号结尾。

2. 可使用空格和缩进来增强语句的可读性。

3. MySQL 数据库的 SQL 语句不区分大小写，关键字建议使用大写。

   ```sql
   SELECT * FROM student;
   select * from student;    -- 语句会自动转换为下面这条大写语句
   SELECT * FROM student;
   ```
