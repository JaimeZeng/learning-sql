# 第 4 章 青铜局之操作 MySQL 数据库、数据表

## 4-1 DDL 语句之操作数据库

### 创建数据库

**语法：**

- 直接创建数据库：`CREATE DATABASE 数据库名;`
- 判断是否存在并创建数据库：`CREATE DATABASE IF NOT EXISTS 数据库名;`
- 创建数据库并指定字符集(编码表)：`CREATE DATABASE 数据库名 CHARACTER SET 字符集;`

**具体操作：**

- 直接创建数据库 db1；
- 创建数据库 db2，在创建之前先确认是否存在。若不存在就创建；
- 创建数据库 db3 并制定字符集编码为 gbk。

```mysql
> mysql -uroot -proot
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.23 MySQL Community Server - GPL

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE DATABASE db1;
Query OK, 1 row affected (0.01 sec)

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| db1                |
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| world              |
+--------------------+
7 rows in set (0.01 sec)

mysql> CREATE DATABASE IF NOT EXISTS db2;
Query OK, 1 row affected (0.01 sec)

mysql> SHOW CREATE DATABASE db2;
+----------+-------------------------------------------------------------------------------------------------------------------------------+
| Database | Create Database                                                                                                               |
+----------+-------------------------------------------------------------------------------------------------------------------------------+
| db2      | CREATE DATABASE `db2` /*!40100 DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci */ /*!80016 DEFAULT ENCRYPTION='N' */ |
+----------+-------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

mysql> CREATE DATABASE db3 CHARACTER SET gbk;
Query OK, 1 row affected (0.01 sec)

mysql> SHOW CREATE DATABASE db3；
    -> ;
ERROR 1049 (42000): Unknown database 'db3；'
mysql> SHOW CREATE DATABASE db3;
+----------+------------------------------------------------------------------------------------------------+
| Database | Create Database                                                                                |
+----------+------------------------------------------------------------------------------------------------+
| db3      | CREATE DATABASE `db3` /*!40100 DEFAULT CHARACTER SET gbk */ /*!80016 DEFAULT ENCRYPTION='N' */ |
+----------+------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

### 查看数据库

**语法：**

- 查看所有的数据库：`SHOW DATABASES;`
- 查看某个数据库的定义信息：`SHOW CREATE DATABASE 数据库名;`

**具体操作：** 查看所有的数据库以及 db3 数据库的定义信息。

```mysql
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| db1                |
| db2                |
| db3                |
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| world              |
+--------------------+
9 rows in set (0.00 sec)

mysql> SHOW CREATE DATABASE db3;
+----------+------------------------------------------------------------------------------------------------+
| Database | Create Database                                                                                |
+----------+------------------------------------------------------------------------------------------------+
| db3      | CREATE DATABASE `db3` /*!40100 DEFAULT CHARACTER SET gbk */ /*!80016 DEFAULT ENCRYPTION='N' */ |
+----------+------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

### 修改数据库

**语法：**

- 修改数据库字符集格式：`ALTER DATABASE 数据库名 DEFAULT CHARACTER SET 字符集;`

**具体操作：** 将 db3 数据库的字符集改成 utf8。`ALTER DATABASE db3 DEFAULT CHARACTER SET utf8;`

```mysql
mysql> ALTER DATABASE db3 DEFAULT CHARACTER SET utf8;
Query OK, 1 row affected, 1 warning (0.01 sec)
mysql> SHOW CREATE DATABASE db3;
+----------+-------------------------------------------------------------------------------------------------+
| Database | Create Database                                                                                 |
+----------+-------------------------------------------------------------------------------------------------+
| db3      | CREATE DATABASE `db3` /*!40100 DEFAULT CHARACTER SET utf8 */ /*!80016 DEFAULT ENCRYPTION='N' */ |
+----------+-------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

### 删除数据库

**语法：**

- 删除数据库：`DROP DATABASE 数据库名;`

**具体操作：** 删除 db2 数据库。`DROP DATABASE db2;`

```mysql
mysql> DROP DATABASE DB2;
Query OK, 0 rows affected (0.03 sec)
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| db1                |
| db3                |
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| world              |
+--------------------+
8 rows in set (0.01 sec)
```

### 使用数据库

**语法：**

- 查看正在使用的数据库：`SELECT DATABASE();`
- 使用/切换数据库：`USE 数据库名;`

**具体操作：**

- 查看正在使用的数据库：`SELECT DATABASE();`
- 使用 db1 数据库：`USE db1;`

```mysql
mysql> SELECT DATABASE();
+------------+
| DATABASE() |
+------------+
| NULL       |
+------------+
1 row in set (0.00 sec)

mysql> USE DB3;
Database changed
mysql> SELECT DATABASE();
+------------+
| DATABASE() |
+------------+
| db3        |
+------------+
1 row in set (0.00 sec)
```

## 4-2 DDL 语句之操作表和列

### 数据类型

常用数据类型：

| 类型    | 描述                 |
| :------ | :------------------- |
| int     | 整型                 |
| double  | 浮点型               |
| varchar | 字符串型             |
| data    | 日期类型：yyyy-MM-dd |

详细的数据类型：

| 分类             | 类型名称         | 说明                                                             |
| :--------------- | :--------------- | :--------------------------------------------------------------- |
| 整数类型         | `tinyInt`        | 很小的整数                                                       |
|                  | `smallint`       | 小的整数                                                         |
|                  | `mediumint`      | 中等大小的整数                                                   |
|                  | `int(integer)`   | 普通大小的整数                                                   |
| 小数类型         | `float`          | 单精度浮点数                                                     |
|                  | `double`         | 双精度浮点数                                                     |
|                  | `decimal（m,d）` | 压缩严格的定点数                                                 |
| 日期类型         | `year`           | YYYY 1901~2155                                                   |
|                  | `time`           | HH:MM:SS -838:59:59~838:59:59                                    |
|                  | `date`           | YYYY-MM-DD 1000-01-01~9999-12-3                                  |
|                  | `datetime`       | YYYY-MM-DD HH:MM:SS 1000-01-01 00:00:00~ 9999-12-31 23:59:59     |
|                  | `timestamp`      | YYYY-MM-DD HH:MM:SS 19700101 00:00:01 UTC~2038-01-19 03:14:07UTC |
| 文本、二进制类型 | `CHAR(M)`        | M 为 0~255 之间的整数                                            |
|                  | `VARCHAR(M)`     | M 为 0~65535 之间的整数                                          |
|                  | `TINYBLOB`       | 允许长度 0~255 字节                                              |
|                  | `BLOB`           | 允许长度 0~65535 字节                                            |
|                  | `MEDIUMBLOB`     | 允许长度 0~167772150 字节                                        |
|                  | `LONGBLOB`       | 允许长度 0~4294967295 字节                                       |
|                  | `TINYTEXT`       | 允许长度 0~255 字节                                              |
|                  | `TEXT`           | 允许长度 0~65535 字节                                            |
|                  | `MEDIUMTEXT`     | 允许长度 0~167772150 字节                                        |
|                  | `LONGTEXT`       | 允许长度 0~4294967295 字节                                       |
|                  | `VARBINARY(M)`   | 允许长度 0~M 个字节的变长字节字符串                              |
|                  | `BINARY(M)`      | 允许长度 0~M 个字节的定长字节字符串                              |

### 创建表

表的结构与 excel 相似。

| id  | name | age | address |
| --- | ---- | --- | ------- |
| 1   | 吕布 | 21  | 北京市  |
| 2   | 貂蝉 | 17  | 上海市  |
| 3   | 关羽 | 33  | 长沙市  |
| 4   | 刘备 | 45  | 杭州市  |
| 5   | 张飞 | 31  | 沈阳市  |

**语法：** `CREATE TABLE 表名 (字段名1 字段类型1, 字段名2 字段类型2…);`

**关键字说明：**

- CREATE -- 表示创建
- TABLE -- 表示创建一张表

建议写成如下格式:

```mysql
CREATE TABLE 表名 (
字段名1 字段类型1,
字段名2 字段类型2
);
```

**具体操作：**

创建 student 表包含 id,name,birthday 字段。

```sql
mysql> CREATE TABLE student (
    ->       id INT,
    ->       name VARCHAR(20),
    ->       birthday DATE
    -> );
Query OK, 0 rows affected (0.04 sec)
```

### 查看表

**语法：**

1. 查看某个数据库中的所有表：`SHOW TABLES;`

2. 查看表结构：`DESC 表名;`
3. 查看创建表的 SQL 语句：`SHOW CREATE TABLE 表名;`

**具体操作：**

查看前面创建的 student 表， student 表结构，以及 student 表的创建语句。

```mysql
mysql> SHOW TABLES;
+---------------+
| Tables_in_db3 |
+---------------+
| student       |
+---------------+
1 row in set (0.01 sec)

mysql> DESC student;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int         | YES  |     | NULL    |       |
| name     | varchar(20) | YES  |     | NULL    |       |
| birthday | date        | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
3 rows in set (0.01 sec)

mysql> SHOW CREATE TABLE student;
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table   | Create Table
            |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
| student | CREATE TABLE `student` (
  `id` int DEFAULT NULL,
  `name` varchar(20) DEFAULT NULL,
  `birthday` date DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.01 sec)
```

### 快速创建一个表结构相同的表

**语法：** `CREATE TABLE 新表名 LIKE 旧表名;`

**具体操作：**

1. 创建 s1 表，s1 表结构和 student 表结构相同。
2. 查看 s1 表结构。
3. 查看 s1 表的创建语句。

```mysql
mysql> CREATE TABLE s1 LIKE student;
Query OK, 0 rows affected (0.02 sec)

mysql> DESC s1;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int         | YES  |     | NULL    |       |
| name     | varchar(20) | YES  |     | NULL    |       |
| birthday | date        | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
3 rows in set (0.01 sec)

mysql> SHOW CREATE TABLE s1;
+-------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| Table | Create Table
     |
+-------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| s1    | CREATE TABLE `s1` (
  `id` int DEFAULT NULL,
  `name` varchar(20) DEFAULT NULL,
  `birthday` date DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
+-------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.01 sec)
```

### 修改表

#### 添加表列

**语法：** `ALTER TABLE 表名 ADD 列名 类型;`

**具体操作：**为学生表 student 添加一个新的字段 remark,类型为 varchar(20)。

```mysql
mysql> DESC student;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int         | YES  |     | NULL    |       |
| name     | varchar(20) | YES  |     | NULL    |       |
| birthday | date        | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
3 rows in set (0.00 sec)

mysql> ALTER TABLE student ADD remark VARCHAR(20);
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> DESC student;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int         | YES  |     | NULL    |       |
| name     | varchar(20) | YES  |     | NULL    |       |
| birthday | date        | YES  |     | NULL    |       |
| remark   | varchar(20) | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
4 rows in set (0.01 sec)
```

#### 修改表列类型

**语法：** `ALTER TABLE 表名 MODIFY列名 新的类型;`

**具体操作：** 将 student 表中的 remark 字段类型的改成 int。

```mysql
mysql> ALTER TABLE student MODIFY remark INT;
Query OK, 0 rows affected (0.07 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> DESC student;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int         | YES  |     | NULL    |       |
| name     | varchar(20) | YES  |     | NULL    |       |
| birthday | date        | YES  |     | NULL    |       |
| remark   | int         | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
4 rows in set (0.01 sec)
```

#### 修改表列名

**语法：** `ALTER TABLE 表名 CHANGE 旧列名 新列名 类型;`

**具体操作：** 将 student 表中的 remark 字段名改成 intro，类型改为 VACHAR(30)。

```mysql
mysql> ALTER TABLE student CHANGE remark intro VARCHAR(30);
Query OK, 0 rows affected (0.07 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> DESC student;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int         | YES  |     | NULL    |       |
| name     | varchar(20) | YES  |     | NULL    |       |
| birthday | date        | YES  |     | NULL    |       |
| intro    | varchar(30) | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
4 rows in set (0.01 sec)
```

#### 删除表列

**语法：** `ALTER TABLE 表名 DROP 列名;`

**具体操作：** 删除 student 表中的字段 intro。

```mysql
mysql> ALTER TABLE student DROP intro;
Query OK, 0 rows affected (0.05 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> DESC student;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int         | YES  |     | NULL    |       |
| name     | varchar(20) | YES  |     | NULL    |       |
| birthday | date        | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
3 rows in set (0.00 sec)

```

#### 修改表名

**语法：** `RENAME TABLE 表名 TO 新表名;`

**具体操作：** 将学生表 student 改名成 student2。

```mysql
mysql> RENAME TABLE student TO student2;
Query OK, 0 rows affected (0.02 sec)

mysql> DESC student2;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int         | YES  |     | NULL    |       |
| name     | varchar(20) | YES  |     | NULL    |       |
| birthday | date        | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
3 rows in set (0.01 sec)
```

#### 修改表字符集

**语法：** `ALTER TABLE 表名 character set 字符集;`

**具体操作：** 将 sutden2 表的编码修改成 gbk。

```mysql
mysql> ALTER TABLE student2 CHARACTER SET gbk;
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> SHOW CREATE TABLE student2;
+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table    | Create Table
                                |
+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| student2 | CREATE TABLE `student2` (
  `id` int DEFAULT NULL,
  `name` varchar(20) CHARACTER SET utf8 DEFAULT NULL,
  `birthday` date DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=gbk |
+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

### 删除表

**语法：**

1. 直接删除表：`DROP TABLE 表名;`

2. 判断表是否存在并删除表：`DROP TABLE IF EXISTS 表名;`

**具体操作：**

1. 删除 s1 表。

   ```mysql
   mysql> show TABLES;
   +---------------+
   | Tables_in_db3 |
   +---------------+
   | s1            |
   | student2      |
   +---------------+
   2 rows in set (0.01 sec)

   mysql> DROP TABLE s1;
   Query OK, 0 rows affected (0.02 sec)

   mysql> show TABLES;
   +---------------+
   | Tables_in_db3 |
   +---------------+
   | student2      |
   +---------------+
   1 row in set (0.01 sec)
   ```

2. 创建 student 表，表结构和 student2 表结构相同。

   ```mysql
   mysql> CREATE TABLE student LIKE student2;
   Query OK, 0 rows affected (0.03 sec)

   mysql> SHOW TABLES;
   +---------------+
   | Tables_in_db3 |
   +---------------+
   | student       |
   | student2      |
   +---------------+
   2 rows in set (0.01 sec)

   mysql> DESC TABLE student;
   +----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-------+
   | id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
   +----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-------+
   |  1 | SIMPLE      | student | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | NULL  |
   +----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-------+
   1 row in set, 1 warning (0.01 sec)
   ```

3. 判断 student2 表是否存在。若存在就删除 student2 表。

   ```mysql
   mysql> DROP TABLE IF EXISTS student2;
   Query OK, 0 rows affected (0.02 sec)

   mysql> SHOW TABLES;
   +---------------+
   | Tables_in_db3 |
   +---------------+
   | student       |
   +---------------+
   1 row in set (0.01 sec)
   ```
