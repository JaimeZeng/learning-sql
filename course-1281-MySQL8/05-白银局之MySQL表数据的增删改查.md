# 第 5 章 白银局之 MySQL 表数据的增删改查

## 5-1 DML 语句之插入表数据

DML(Data Manipulation Language)：数据操作语言，用来对数据库中表的数据进行增删改。关键字：insert, delete, update 等

### 关键字说明

```mysql
INSERT INTO 表名 – 表示往哪张表中添加数据
(字段名1, 字段名2, …)  --  要给哪些字段设置值
VALUES (值1, 值2, …); -- 设置具体的值
```

**注意：**

- 值与字段必须对应，个数相同，类型相同。
- 值的数据大小必须在字段的长度范围内。
- 除了数值类型外，其它的字段类型的值必须使用引号引起。（建议单引号）
- 如果要插入空值，可以不写字段，或者插入 null。

### 插入全部字段数据

#### 语法

- 所有的字段名都写出来：`INSERT INTO 表名 (字段名1, 字段名2, 字段名3…) VALUES (值1, 值2, 值3);`
- 不写字段名：`INSERT INTO 表名 VALUES (值1, 值2, 值3…);`

#### 具体操作

1. 创建 db2 数据库并使用。

   ```sql
   CREATE DATABASE IF NOT EXISTS db2;
   USE db2;
   SELECT DATABASE();
   ```

   ```mysql
   mysql> CREATE DATABASE IF NOT EXISTS db2;
   Query OK, 1 row affected (0.01 sec)

   mysql> USE db2;
   Database changed
   mysql> SELECT DATABASE();
   +------------+
   | DATABASE() |
   +------------+
   | db2        |
   +------------+
   1 row in set (0.00 sec)
   ```

2. 创建学生信息表 student，字段有：学号 id, 姓名 name, 年龄 age, 性别 sex, 家庭地址 address, 电话号码 phone, 生日 birthday, 数学成绩 math, 英语成绩 english。

   ```sql
   CREATE TABLE student (
       id INT,
       name VARCHAR(100),
       age INT,
       sex CHAR(1),
       address VARCHAR(100),
       phone VARCHAR(11),
       birthday DATE,
       math DOUBLE,
       english DOUBLE
   );
   DESC student;
   ```

   ```mysql
   mysql> CREATE TABLE student (
       ->     id INT,
       ->     name VARCHAR(100),
       ->  age INT,
       ->  sex CHAR(1),
       ->  address VARCHAR(100),
       ->     phone VARCHAR(11),
       ->  birthday DATE,
       ->  math DOUBLE,
       ->  english DOUBLE
       -> );
   Query OK, 0 rows affected (0.04 sec)

   mysql> DESC student;
   +----------+--------------+------+-----+---------+-------+
   | Field    | Type         | Null | Key | Default | Extra |
   +----------+--------------+------+-----+---------+-------+
   | id       | int          | YES  |     | NULL    |       |
   | name     | varchar(100) | YES  |     | NULL    |       |
   | age      | int          | YES  |     | NULL    |       |
   | sex      | char(1)      | YES  |     | NULL    |       |
   | address  | varchar(100) | YES  |     | NULL    |       |
   | phone    | varchar(11)  | YES  |     | NULL    |       |
   | birthday | date         | YES  |     | NULL    |       |
   | math     | double       | YES  |     | NULL    |       |
   | english  | double       | YES  |     | NULL    |       |
   +----------+--------------+------+-----+---------+-------+
   9 rows in set (0.01 sec)
   ```

3. 在学生信息表 student 中插入以下数据。

   | 字段     | 字段类型     | 数据 1      | 数据 2      |
   | -------- | ------------ | ----------- | ----------- |
   | id       | INT          | 10086       | 10000       |
   | name     | VARCHAR(20)  | Jaime       | 电信        |
   | age      | INT          | 23          | 22          |
   | sex      | char(1)      | 男          | 女          |
   | address  | VARCHAR(100) | 西安市      | 北京市      |
   | phone    | VARCHAR(11)  | 13888888888 | 15333333333 |
   | brithday | date         | 19997-07-07 | 1998-03-03  |
   | math     | double       | 97.8        | 50          |
   | english  | double       | 2.2         | 50          |

   ```mysql
   INSERT INTO student(id, name, age, sex, address, phone, birthday, math, english) VALUES (10086, 'Jaime', 23, '男', '西安市', '13888888888', '1997-07-07', 97.8, 2.2);
   INSERT INTO student VALUES (10000, '电信', 22, '女', '北京市', '15333333333', '1998-03-03', 50, 50);
   ```

   ```mysql
   mysql> INSERT INTO student(id, name, age, sex, address, phone, birthday, math, english) VALUES (10086, 'Jaime', 23, '男', '西安市', '13888888888', '1997-07-07', 97.8, 2.2);
   Query OK, 1 row affected (0.01 sec)

   mysql> INSERT INTO student VALUES (10000, '电信', 22, '女', '北京市', '15333333333', '1998-03-03', 50, 50);
   Query OK, 1 row affected (0.01 sec)
   ```

### 插入部分字段数据

#### 语法

- `INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...);`

- 没有添加数据的字段会使用 NULL。

#### 具体操作：

- 在学生信息表 student 中插入以下数据。

  | 字段     | 字段类型     | 数据 3 |
  | -------- | ------------ | ------ |
  | id       | INT          | 10010  |
  | name     | VARCHAR(20)  | 联通   |
  | age      | INT          | 67     |
  | sex      | char(1)      | 男     |
  | address  | VARCHAR(100) | 北京市 |
  | phone    | VARCHAR(11)  |        |
  | birthday | date         |        |
  | math     | double       |        |
  | english  | double       |        |

  ```mysql
  mysql> INSERT INTO student(id, name, age, sex, address) VALUES (10010, '联通', 67, '男', '北京市');
  Query OK, 1 row affected (0.01 sec)

  mysql> SELECT * FROM student;
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | id    | name  | age  | sex  | address | phone       | birthday   | math | english |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | 10086 | Jaime |   23 | 男   | 西安市  | 13888888888 | 1997-07-07 | 97.8 |     2.2 |
  | 10000 | 电信  |   22 | 女   | 北京市  | 15333333333 | 1998-03-03 |   50 |      50 |
  | 10010 | 联通  |   67 | 男   | 北京市  | NULL        | NULL       | NULL |    NULL |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  3 rows in set (0.00 sec)
  ```

## 5-2 蠕虫复制

蠕虫复制：在已有的数据基础之上，将原来的数据进行复制，插入到对应的表中。

#### 语法

- 复制全部数据：` INSERT INTO 表名1 SELECT * FROM 表名2;`
- 复制部分数据：`INSERT INTO 表名1 (字段名1, 字段名2, ...) SELECT (字段名1, 字段名2, ...) FROM 表名2;`

#### 作用

将表名 2 中的数据复制到表名 1 中

#### 具体操作

- 创建 student2 表，student2 结构和 student 表结构一样。

  ```sql
  CREATE TABLE student2 LIKE student;
  DESC student2;
  ```

  ```mysql
  mysql> CREATE TABLE student2 LIKE student;
  Query OK, 0 rows affected (0.04 sec)

  mysql> DESC student2;
  +----------+--------------+------+-----+---------+-------+
  | Field    | Type         | Null | Key | Default | Extra |
  +----------+--------------+------+-----+---------+-------+
  | id       | int          | YES  |     | NULL    |       |
  | name     | varchar(100) | YES  |     | NULL    |       |
  | age      | int          | YES  |     | NULL    |       |
  | sex      | char(1)      | YES  |     | NULL    |       |
  | address  | varchar(100) | YES  |     | NULL    |       |
  | phone    | varchar(11)  | YES  |     | NULL    |       |
  | birthday | date         | YES  |     | NULL    |       |
  | math     | double       | YES  |     | NULL    |       |
  | english  | double       | YES  |     | NULL    |       |
  +----------+--------------+------+-----+---------+-------+
  9 rows in set (0.01 sec)
  ```

- 将 student 表中的数据添加到 student2 表中。

  ```sql
  INSERT INTO student2 SELECT * FROM student;
  SELECT * FROM student2;
  ```

  ```mysql
  mysql> INSERT INTO student2 SELECT * FROM student;
  Query OK, 3 rows affected (0.01 sec)
  Records: 3  Duplicates: 0  Warnings: 0

  mysql> SELECT * FROM student2;
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | id    | name  | age  | sex  | address | phone       | birthday   | math | english |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | 10086 | Jaime |   23 | 男   | 西安市  | 13888888888 | 1997-07-07 | 97.8 |     2.2 |
  | 10000 | 电信  |   22 | 女   | 北京市  | 15333333333 | 1998-03-03 |   50 |      50 |
  | 10010 | 联通  |   67 | 男   | 北京市  | NULL        | NULL       | NULL |    NULL |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  3 rows in set (0.00 sec)
  ```

- 将 student 表中的 name, age 字段数据添加到 student2 表中。

  ```sql
  INSERT INTO student2(name, age) SELECT name,age FROM student;
  SELECT * FROM student2;
  ```

  ```mysql
  mysql> INSERT INTO student2(name, age) SELECT name,age FROM student;
  Query OK, 3 rows affected (0.00 sec)
  Records: 3  Duplicates: 0  Warnings: 0

  mysql> SELECT * FROM student2;
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | id    | name  | age  | sex  | address | phone       | birthday   | math | english |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | 10086 | Jaime |   23 | 男   | 西安市  | 13888888888 | 1997-07-07 | 97.8 |     2.2 |
  | 10000 | 电信  |   22 | 女   | 北京市  | 15333333333 | 1998-03-03 |   50 |      50 |
  | 10010 | 联通  |   67 | 男   | 北京市  | NULL        | NULL       | NULL |    NULL |
  |  NULL | Jaime |   23 | NULL | NULL    | NULL        | NULL       | NULL |    NULL |
  |  NULL | 电信  |   22 | NULL | NULL    | NULL        | NULL       | NULL |    NULL |
  |  NULL | 联通  |   67 | NULL | NULL    | NULL        | NULL       | NULL |    NULL |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  6 rows in set (0.00 sec)
  ```

## 5-3 DML 语句之表数据的修改和删除

### 修改表记录

#### 关键字说明

```sql
UPDATE: 修改数据
SET: 修改哪些字段
WHERE: 指定条件
```

#### 语法

- **不带条件修改数据：** `UPDATE 表名 SET 字段名=值;`
- **带条件修改数据：** `UPDATE 表名 SET 字段名=值 WHERE 字段名=值;`
- **带条件修改多个字段数据：** `UPDATE 表名 SET 字段名1=值1,字段名2=值2 WHERE 字段名=值;`

#### 具体操作

- 不带条件修改数据，将所有的性别改成女。

  ```sql
  UPDATE student SET sex='男';
  SELECT * FROM student;
  ```

  ```mysql
  mysql> UPDATE student SET sex='男';
  Query OK, 1 row affected (0.01 sec)
  Rows matched: 3  Changed: 1  Warnings: 0

  mysql> SELECT * FROM student;
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | id    | name  | age  | sex  | address | phone       | birthday   | math | english |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | 10086 | Jaime |   23 | 男   | 西安市  | 13888888888 | 1997-07-07 | 97.8 |     2.2 |
  | 10000 | 电信  |   22 | 男   | 北京市  | 15333333333 | 1998-03-03 |   50 |      50 |
  | 10010 | 联通  |   67 | 男   | 北京市  | NULL        | NULL       | NULL |    NULL |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  3 rows in set (0.00 sec)
  ```

- 带条件修改数据，将 id 号为 10000 的学生性别改成女

  ```sql
  UPDATE student SET sex='女' WHERE id=10000;
  SELECT * FROM student WHERE id=10000;
  ```

  ```mysql
  mysql> UPDATE student SET sex='女' WHERE id=10000;
  Query OK, 1 row affected (0.00 sec)
  Rows matched: 1  Changed: 1  Warnings: 0

  mysql> SELECT * FROM student WHERE id=10000;
  +-------+------+------+------+---------+-------------+------------+------+---------+
  | id    | name | age  | sex  | address | phone       | birthday   | math | english |
  +-------+------+------+------+---------+-------------+------------+------+---------+
  | 10000 | 电信 |   22 | 女   | 北京市  | 15333333333 | 1998-03-03 |   50 |      50 |
  +-------+------+------+------+---------+-------------+------------+------+---------+
  1 row in set (0.00 sec)
  ```

- 带条件修改多个字段数据，将 id 号为 10010 的学生电话号码修改为 16666666666，数学成绩修改为 49.8，英语成绩修改为 50.2。

  ```sql
  UPDATE student SET phone='16666666666', math=49.8, english=50.2;
  -- SELECT * FROM student WHERE id=10010;
  SELECT id, phone, math, english FROM student WHERE id=10010;
  ```

  ```mysql
  mysql> UPDATE student SET phone='16666666666', math=49.8, english=50.2;
  Query OK, 3 rows affected (0.01 sec)
  Rows matched: 3  Changed: 3  Warnings: 0

  mysql> SELECT id, phone, math, english FROM student WHERE id=10010;
  +-------+-------------+------+---------+
  | id    | phone       | math | english |
  +-------+-------------+------+---------+
  | 10010 | 16666666666 | 49.8 |    50.2 |
  +-------+-------------+------+---------+
  1 row in set (0.00 sec)
  ```

### 删除表记录

#### 语法

- 不带条件删除数据
  - 使用 DELETE 删除：`DELETE FROM 表名;`
  - 使用 TRUNCATE 删除：`TRUNCATE TABLE 表名;`
- 带条件删除数据：`DELETE FROM 表名 WHERE 字段名=值;`
- 带多个条件删除数据：`DELETE FROM 表名 WHERE 字段名1=值1 and 字段名2=值2;`

#### TRUNCATE 和 DELETE 的区别

> 转载于：[MYSQL 中 TRUNCATE 和 DELETE 的区别 - 简书 (jianshu.com)](https://www.jianshu.com/p/ddc5b65e63af)

|            | TRUNCATE | DELETE |
| ---------- | -------- | ------ |
| 条件删除   | 不支持   | 支持   |
| 事务回滚   | 不支持   | 支持   |
| 清理速度   | 快       | 慢     |
| 高水位重置 | 是       | 否     |

1. 条件删除。因为 DELETE 是可以带 WHERE 的，所以支持条件删除；而 TRUNCATE 只能删除整个表。

   ```sql
   -- delete - 条件删除
   DELETE FROM student WHERE id = 1;
   -- delete - 删除整个表的数据
   DELETE FROM student;
   -- truncate - 删除整个表的数据
   TRUNCATE TABLE student;
   ```

2. 事务回滚。由于 DELETE 是数据操作语言（DML - Data Manipulation Language），每次从表中删除一行，并且同时将该行的删除操作作为事务记录在日志中保存以便进行进行回滚操作，操作时原数据会被放到 rollback segment 中，事务提交后才生效。如果有相应的 tigger,执行的时候将被触发，可以被回滚；

   而 TRUNCATE 是数据定义语言（DDL - Data Definition Language)，一次性地从表中删除所有的数据并不把单独的删除操作记录记入日志保存，操作时不会进行存储，不能进行回滚。

   ```sql
   CREATE TABLE stu (
       id INT,
       name VARCHAR(22),
       age INT
   );

   INSERT INTO stu VALUES(1, 'student1', 12);
   INSERT INTO stu VALUES(2, 'student2', 13);
   INSERT INTO stu VALUES(3, 'student3', 14);
   INSERT INTO stu VALUES(4, 'student4', 15);

   -- DELETE
   SELECT * FROM stu;
   -- 开启事务
   START TRANSACTION;
   DELETE FROM stu;
   SELECT * FROM stu;
   -- 回滚
   ROLLBACK;
   SELECT * FROM stu;

   -- TRUNCATE
   SELECT * FROM stu;
   -- 开启事务
   START TRANSACTION;
   TRUNCATE TABLE stu;
   SELECT * FROM stu;
   -- 回滚
   ROLLBACK;
   SELECT * FROM stu;
   ```

   ```mysql
   mysql> SELECT * FROM stu;
   +------+----------+------+
   | id   | name     | age  |
   +------+----------+------+
   |    1 | student1 |   12 |
   |    2 | student2 |   13 |
   |    3 | student3 |   14 |
   |    4 | student4 |   15 |
   +------+----------+------+
   4 rows in set (0.00 sec)

   mysql> START TRANSACTION;
   Query OK, 0 rows affected (0.00 sec)

   mysql> DELETE FROM stu;
   Query OK, 4 rows affected (0.00 sec)

   mysql> SELECT * FROM stu;
   Empty set (0.00 sec)

   mysql> ROLLBACK;
   Query OK, 0 rows affected (0.00 sec)

   mysql> SELECT * FROM stu;
   +------+----------+------+
   | id   | name     | age  |
   +------+----------+------+
   |    1 | student1 |   12 |
   |    2 | student2 |   13 |
   |    3 | student3 |   14 |
   |    4 | student4 |   15 |
   +------+----------+------+
   4 rows in set (0.00 sec)
   ```

   **DELETE 回滚成功。**

   ```mysql
   mysql> SELECT * FROM stu;
   +------+----------+------+
   | id   | name     | age  |
   +------+----------+------+
   |    1 | student1 |   12 |
   |    2 | student2 |   13 |
   |    3 | student3 |   14 |
   |    4 | student4 |   15 |
   +------+----------+------+
   4 rows in set (0.00 sec)

   mysql> START TRANSACTION;
   Query OK, 0 rows affected (0.00 sec)

   mysql> TRUNCATE TABLE stu;
   Query OK, 0 rows affected (0.05 sec)

   mysql> SELECT * FROM stu;
   Empty set (0.00 sec)

   mysql> ROLLBACK;
   Query OK, 0 rows affected (0.00 sec)

   mysql> SELECT * FROM stu;
   Empty set (0.00 sec)
   ```

   **TRUNCATE 回滚失败。**

3. 清理速度。在数据量比较小的情况下，DELETE 和 TRUNCATE 的清理速度差别不是很大。但是数据量很大的时候就能看出区别。由于第二项中说的，TRUNCATE 不需要支持回滚，所以使用的系统和事务日志资源少。DELETE 语句每次删除一行，并在事务日志中为所删除的每行记录一项，固然会慢，但是相对来说也较安全。

4. 高水位重置。随着不断地进行表记录的 DML 操作，会不断提高表的高水位线（HWM），DELETE 操作之后虽然表的数据删除了，但是并没有降低表的高水位，随着 DML 操作数据库容量也只会上升，不会下降。所以如果使用 DELETE，就算将表中的数据减少了很多，在查询时还是很和 DELETE 操作前速度一样。
   而 TRUNCATE 操作会重置高水位线，数据库容量也会被重置，之后再进行 DML 操作速度也会有提升。

#### 具体操作

- 查看 student2 表中数据；

  ```mysql
  mysql> SELECT * FROM student2;
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | id    | name  | age  | sex  | address | phone       | birthday   | math | english |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | 10086 | Jaime |   23 | 男   | 西安市  | 13888888888 | 1997-07-07 | 97.8 |     2.2 |
  | 10000 | 电信  |   22 | 女   | 北京市  | 15333333333 | 1998-03-03 |   50 |      50 |
  | 10010 | 联通  |   67 | 男   | 北京市  | NULL        | NULL       | NULL |    NULL |
  |  NULL | Jaime |   23 | NULL | NULL    | NULL        | NULL       | NULL |    NULL |
  |  NULL | 电信  |   22 | NULL | NULL    | NULL        | NULL       | NULL |    NULL |
  |  NULL | 联通  |   67 | NULL | NULL    | NULL        | NULL       | NULL |    NULL |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  ```

- 带条件删除数据，删除 student2 表中 id 为空的学生数据。

  ```sql
  DELETE FROM student2 WHERE id is NULL;
  SELECT * FROM student2;
  ```

  ```mysql
  mysql> DELETE FROM student2 WHERE id is NULL;
  Query OK, 3 rows affected (0.01 sec)

  mysql> SELECT * FROM student2;
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | id    | name  | age  | sex  | address | phone       | birthday   | math | english |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | 10086 | Jaime |   23 | 男   | 西安市  | 13888888888 | 1997-07-07 | 97.8 |     2.2 |
  | 10000 | 电信  |   22 | 女   | 北京市  | 15333333333 | 1998-03-03 |   50 |      50 |
  | 10010 | 联通  |   67 | 男   | 北京市  | NULL        | NULL       | NULL |    NULL |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  3 rows in set (0.00 sec)
  ```

- 带条件删除数据，删除 student2 表中性别为男且家庭地址为北京市的学生数据。

  ```sql
  DELETE FROM student2 WHERE sex = '男' and address = '北京市';
  SELECT * FROM student2;
  ```

  ```mysql
  mysql> DELETE FROM student2 WHERE sex = '男' and address = '北京市';
  Query OK, 1 row affected (0.00 sec)

  mysql> SELECT * FROM student2;
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | id    | name  | age  | sex  | address | phone       | birthday   | math | english |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | 10086 | Jaime |   23 | 男   | 西安市  | 13888888888 | 1997-07-07 | 97.8 |     2.2 |
  | 10000 | 电信  |   22 | 女   | 北京市  | 15333333333 | 1998-03-03 |   50 |      50 |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  2 rows in set (0.00 sec)
  ```

- 使用 DELETE 删除 student2 表中所有数据。

  ```sql
  SELECT * FROM student2;
  -- DELETE
  -- 开启事务
  START TRANSACTION;
  DELETE FROM student2;
  -- 回滚
  ROLLBACK;
  SELECT * FROM student2;
  ```

  ```mysql
  mysql> SELECT * FROM student2;
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | id    | name  | age  | sex  | address | phone       | birthday   | math | english |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | 10086 | Jaime |   23 | 男   | 西安市  | 13888888888 | 1997-07-07 | 97.8 |     2.2 |
  | 10000 | 电信  |   22 | 女   | 北京市  | 15333333333 | 1998-03-03 |   50 |      50 |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  2 rows in set (0.00 sec)

  mysql> START TRANSACTION;
  Query OK, 0 rows affected (0.00 sec)

  mysql> DELETE FROM student2;
  Query OK, 2 rows affected (0.00 sec)

  mysql> ROLLBACK;
  Query OK, 0 rows affected (0.00 sec)

  mysql> SELECT * FROM student2;
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | id    | name  | age  | sex  | address | phone       | birthday   | math | english |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | 10086 | Jaime |   23 | 男   | 西安市  | 13888888888 | 1997-07-07 | 97.8 |     2.2 |
  | 10000 | 电信  |   22 | 女   | 北京市  | 15333333333 | 1998-03-03 |   50 |      50 |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  2 rows in set (0.00 sec)
  ```

- 使用 TRUNCATE 删除 student2 表中所有数据。

  ```sql
  SELECT * FROM student2;
  -- TRUNCATE
  TRUNCATE TABLE student2;
  SELECT * FROM student2;
  ```

  ```mysql
  mysql> SELECT * FROM student2;
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | id    | name  | age  | sex  | address | phone       | birthday   | math | english |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  | 10086 | Jaime |   23 | 男   | 西安市  | 13888888888 | 1997-07-07 | 97.8 |     2.2 |
  | 10000 | 电信  |   22 | 女   | 北京市  | 15333333333 | 1998-03-03 |   50 |      50 |
  +-------+-------+------+------+---------+-------------+------------+------+---------+
  2 rows in set (0.00 sec)

  mysql> TRUNCATE TABLE student2;
  Query OK, 0 rows affected (0.04 sec)

  mysql> SELECT * FROM student2;
  Empty set (0.01 sec)
  ```

## 5-4 DQL 语句之简单查询

DQL(Data Query Language)：数据查询语言，用来查询数据库中表的记录(数据)。关键字：`select`, `where`等。

**注意：查询不会对数据库中的数据进行修改.只是一种显示数据的方式。**

```sql
--为 student准备数据
INSERT INTO student2 VALUES (1, '莫博文', 43, '女', '于码市', '12222222222', '2019-02-12', 92.5, 88);
INSERT INTO student2 VALUES (2, '王皓轩', 21, '男', '子涵沙市', '1666666666', '1948-3-11', 97.5, 65.5);
INSERT INTO student2 VALUES (3, '覃擎宇', 44, '女', '浩轩京市', '13333333333', '2012-07-15', 42.5, 74.5);
INSERT INTO student2 VALUES (4, '廖熠彤', 34, '男', '昊然市', '17777777777', '1987-07-31', 69, 65);
INSERT INTO student2 VALUES (5, '沈立辉', 51, '男', '振家口市', '15555555555', '1949-11-11', 88, 97);
INSERT INTO student2 VALUES (6, '朱烨华', 57, '男', '赖乡县', '19999999999', '1988-12-16', 74.5, 92.5);
INSERT INTO student2 VALUES (7, '万绍辉', 42, '女', '明都市', '18888888888', '1962-01-24', 86.5, 71.5);
INSERT INTO student2 VALUES (8, '任晟睿', 49, '男', '浩轩京市', '11111111111', '1954-10-09', 77.5, 60);
```

```mysql
mysql> SELECT * FROM student2;
+------+--------+------+------+----------+-------------+------------+------+---------+
| id   | name   | age  | sex  | address  | phone       | birthday   | math | english |
+------+--------+------+------+----------+-------------+------------+------+---------+
|    1 | 莫博文 |   43 | 女   | 于码市   | 12222222222 | 2019-02-12 | 92.5 |      88 |
|    2 | 王皓轩 |   21 | 男   | 子涵沙市 | 1666666666  | 1948-03-11 | 97.5 |    65.5 |
|    3 | 覃擎宇 |   44 | 女   | 浩轩京市 | 13333333333 | 2012-07-15 | 42.5 |    74.5 |
|    4 | 廖熠彤 |   34 | 男   | 昊然市   | 17777777777 | 1987-07-31 |   69 |      65 |
|    5 | 沈立辉 |   51 | 男   | 振家口市 | 15555555555 | 1949-11-11 |   88 |      97 |
|    6 | 朱烨华 |   57 | 男   | 赖乡县   | 19999999999 | 1988-12-16 | 74.5 |    92.5 |
|    7 | 万绍辉 |   42 | 女   | 明都市   | 18888888888 | 1962-01-24 | 86.5 |    71.5 |
|    8 | 任晟睿 |   49 | 男   | 浩轩京市 | 11111111111 | 1954-10-09 | 77.5 |      60 |
+------+--------+------+------+----------+-------------+------------+------+---------+
8 rows in set (0.00 sec)
```

### 查询表所有数据

#### 语法

- 使用 \* 表示所有列。`SELECT * FROM 表名;`
- 写出查询每列的名称。`SELECT 字段名1, 字段名2, 字段名3, ... FROM 表名;`

#### 具体操作

查询 student2 表中所有数据。

```sql
SELECT * FROM student2;
SELECT id, name, age, sex, address, phone, birthday, math, english FROM student2;
```

```mysql
mysql> SELECT * FROM student2;
+------+--------+------+------+----------+-------------+------------+------+---------+
| id   | name   | age  | sex  | address  | phone       | birthday   | math | english |
+------+--------+------+------+----------+-------------+------------+------+---------+
|    1 | 莫博文 |   43 | 女   | 于码市   | 12222222222 | 2019-02-12 | 92.5 |      88 |
|    2 | 王皓轩 |   21 | 男   | 子涵沙市 | 1666666666  | 1948-03-11 | 97.5 |    65.5 |
|    3 | 覃擎宇 |   44 | 女   | 浩轩京市 | 13333333333 | 2012-07-15 | 42.5 |    74.5 |
|    4 | 廖熠彤 |   34 | 男   | 昊然市   | 17777777777 | 1987-07-31 |   69 |      65 |
|    5 | 沈立辉 |   51 | 男   | 振家口市 | 15555555555 | 1949-11-11 |   88 |      97 |
|    6 | 朱烨华 |   57 | 男   | 赖乡县   | 19999999999 | 1988-12-16 | 74.5 |    92.5 |
|    7 | 万绍辉 |   42 | 女   | 明都市   | 18888888888 | 1962-01-24 | 86.5 |    71.5 |
|    8 | 任晟睿 |   49 | 男   | 浩轩京市 | 11111111111 | 1954-10-09 | 77.5 |      60 |
+------+--------+------+------+----------+-------------+------------+------+---------+
8 rows in set (0.00 sec)

mysql> SELECT id, name, age, sex, address, phone, birthday, math, english FROM student2;
+------+--------+------+------+----------+-------------+------------+------+---------+
| id   | name   | age  | sex  | address  | phone       | birthday   | math | english |
+------+--------+------+------+----------+-------------+------------+------+---------+
|    1 | 莫博文 |   43 | 女   | 于码市   | 12222222222 | 2019-02-12 | 92.5 |      88 |
|    2 | 王皓轩 |   21 | 男   | 子涵沙市 | 1666666666  | 1948-03-11 | 97.5 |    65.5 |
|    3 | 覃擎宇 |   44 | 女   | 浩轩京市 | 13333333333 | 2012-07-15 | 42.5 |    74.5 |
|    4 | 廖熠彤 |   34 | 男   | 昊然市   | 17777777777 | 1987-07-31 |   69 |      65 |
|    5 | 沈立辉 |   51 | 男   | 振家口市 | 15555555555 | 1949-11-11 |   88 |      97 |
|    6 | 朱烨华 |   57 | 男   | 赖乡县   | 19999999999 | 1988-12-16 | 74.5 |    92.5 |
|    7 | 万绍辉 |   42 | 女   | 明都市   | 18888888888 | 1962-01-24 | 86.5 |    71.5 |
|    8 | 任晟睿 |   49 | 男   | 浩轩京市 | 11111111111 | 1954-10-09 | 77.5 |      60 |
+------+--------+------+------+----------+-------------+------------+------+---------+
8 rows in set (0.00 sec)
```

### 查询指定列

#### 语法

- 查询指定列的数据,多个列之间以逗号分隔：`SELECT 字段名1, 字段名2... FROM 表名;`

#### 具体操作

查询 student2 表中 name, age 字段所有数据。

```mysql
mysql> SELECT name, age FROM student2;
+--------+------+
| name   | age  |
+--------+------+
| 莫博文 |   43 |
| 王皓轩 |   21 |
| 覃擎宇 |   44 |
| 廖熠彤 |   34 |
| 沈立辉 |   51 |
| 朱烨华 |   57 |
| 万绍辉 |   42 |
| 任晟睿 |   49 |
+--------+------+
8 rows in set (0.00 sec)
```

### 别名查询

#### 语法

- 查询时给列、表指定别名需要使用 AS 关键字。`SELECT 字段名1 AS 别名, 字段名2 AS 别名... FROM 表名;`
- 使用别名的好处是方便观看和处理查询到的数据。

**注意：AS 可以省略不写。**

#### 具体操作

查询 sudent2 表中 name, age 字段数据，查询结果中将 name 列的别名设为姓名，age 列的别名设为年龄。

```mysql
mysql> SELECT name AS 姓名, age AS 年龄 FROM student2;
+--------+------+
| 姓名   | 年龄 |
+--------+------+
| 莫博文 |   43 |
| 王皓轩 |   21 |
| 覃擎宇 |   44 |
| 廖熠彤 |   34 |
| 沈立辉 |   51 |
| 朱烨华 |   57 |
| 万绍辉 |   42 |
| 任晟睿 |   49 |
+--------+------+
8 rows in set (0.00 sec)
```

### 清除重复值

- 查询指定列并且结果不出现重复数据：`SELECT DISTINCT 字段名 FROM 表名;`

#### 具体操作

1. 将 student2 表中所有数据插入到 student2 表中。

   ```mysql
   mysql> INSERT INTO student2 SELECT * FROM student2;
   Query OK, 8 rows affected (0.01 sec)
   Records: 8  Duplicates: 0  Warnings: 0

   mysql> SELECT * FROM student2;
   +------+--------+------+------+----------+-------------+------------+------+---------+
   | id   | name   | age  | sex  | address  | phone       | birthday   | math | english |
   +------+--------+------+------+----------+-------------+------------+------+---------+
   |    1 | 莫博文 |   43 | 女   | 于码市   | 12222222222 | 2019-02-12 | 92.5 |      88 |
   |    2 | 王皓轩 |   21 | 男   | 子涵沙市 | 1666666666  | 1948-03-11 | 97.5 |    65.5 |
   |    3 | 覃擎宇 |   44 | 女   | 浩轩京市 | 13333333333 | 2012-07-15 | 42.5 |    74.5 |
   |    4 | 廖熠彤 |   34 | 男   | 昊然市   | 17777777777 | 1987-07-31 |   69 |      65 |
   |    5 | 沈立辉 |   51 | 男   | 振家口市 | 15555555555 | 1949-11-11 |   88 |      97 |
   |    6 | 朱烨华 |   57 | 男   | 赖乡县   | 19999999999 | 1988-12-16 | 74.5 |    92.5 |
   |    7 | 万绍辉 |   42 | 女   | 明都市   | 18888888888 | 1962-01-24 | 86.5 |    71.5 |
   |    8 | 任晟睿 |   49 | 男   | 浩轩京市 | 11111111111 | 1954-10-09 | 77.5 |      60 |
   |    1 | 莫博文 |   43 | 女   | 于码市   | 12222222222 | 2019-02-12 | 92.5 |      88 |
   |    2 | 王皓轩 |   21 | 男   | 子涵沙市 | 1666666666  | 1948-03-11 | 97.5 |    65.5 |
   |    3 | 覃擎宇 |   44 | 女   | 浩轩京市 | 13333333333 | 2012-07-15 | 42.5 |    74.5 |
   |    4 | 廖熠彤 |   34 | 男   | 昊然市   | 17777777777 | 1987-07-31 |   69 |      65 |
   |    5 | 沈立辉 |   51 | 男   | 振家口市 | 15555555555 | 1949-11-11 |   88 |      97 |
   |    6 | 朱烨华 |   57 | 男   | 赖乡县   | 19999999999 | 1988-12-16 | 74.5 |    92.5 |
   |    7 | 万绍辉 |   42 | 女   | 明都市   | 18888888888 | 1962-01-24 | 86.5 |    71.5 |
   |    8 | 任晟睿 |   49 | 男   | 浩轩京市 | 11111111111 | 1954-10-09 | 77.5 |      60 |
   +------+--------+------+------+----------+-------------+------------+------+---------+
   16 rows in set (0.00 sec)
   ```

2. 查询 student2 表中 id，name，age 字段数据，结果中排除重复值。

   ```mysql
   mysql> SELECT DISTINCT id, name, age FROM student2;
   +------+--------+------+
   | id   | name   | age  |
   +------+--------+------+
   |    1 | 莫博文 |   43 |
   |    2 | 王皓轩 |   21 |
   |    3 | 覃擎宇 |   44 |
   |    4 | 廖熠彤 |   34 |
   |    5 | 沈立辉 |   51 |
   |    6 | 朱烨华 |   57 |
   |    7 | 万绍辉 |   42 |
   |    8 | 任晟睿 |   49 |
   +------+--------+------+
   8 rows in set (0.00 sec)
   ```

### 查询结果参与运算

#### 语法

- 某列数据和固定值运算：`SELECT 列名1 + 固定值 FROM 表名;`

- 某列数据和其他列数据参与运算：`SELECT 列名1 + 列名2 FROM 表名;`

> **注意: 参与运算的必须是数值类型。**

#### 具体操作

1. 查询 student2 表中 id，name，age 字段数据，结果中排除重复数据并将 age 字段全部增加 10。

   ```mysql
   mysql> SELECT DISTINCT id, name, age, age + 10 FROM student2;
   +------+--------+------+----------+
   | id   | name   | age  | age + 10 |
   +------+--------+------+----------+
   |    1 | 莫博文 |   43 |       53 |
   |    2 | 王皓轩 |   21 |       31 |
   |    3 | 覃擎宇 |   44 |       54 |
   |    4 | 廖熠彤 |   34 |       44 |
   |    5 | 沈立辉 |   51 |       61 |
   |    6 | 朱烨华 |   57 |       67 |
   |    7 | 万绍辉 |   42 |       52 |
   |    8 | 任晟睿 |   49 |       59 |
   +------+--------+------+----------+
   8 rows in set (0.00 sec)
   ```

2. 查询 student2 表中 id，name，math,，english 字段数据，结果中排除重复数据并将数学成绩与英语成绩相加，别名设置为 total。

   ```mysql
   mysql> SELECT DISTINCT id, name, math, english, math + english AS total FROM student2;
   +------+--------+------+---------+-------+
   | id   | name   | math | english | total |
   +------+--------+------+---------+-------+
   |    1 | 莫博文 | 92.5 |      88 | 180.5 |
   |    2 | 王皓轩 | 97.5 |    65.5 |   163 |
   |    3 | 覃擎宇 | 42.5 |    74.5 |   117 |
   |    4 | 廖熠彤 |   69 |      65 |   134 |
   |    5 | 沈立辉 |   88 |      97 |   185 |
   |    6 | 朱烨华 | 74.5 |    92.5 |   167 |
   |    7 | 万绍辉 | 86.5 |    71.5 |   158 |
   |    8 | 任晟睿 | 77.5 |      60 | 137.5 |
   +------+--------+------+---------+-------+
   8 rows in set (0.00 sec)
   ```
