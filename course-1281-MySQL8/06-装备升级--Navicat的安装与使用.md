# 第 6 章 装备升级--Navicat 的安装与使用

## 6-1 Navicat 安装

### 为什么使用 Navicat

Navicat 是一套快速、可靠并价格相当便宜的数据库管理工具，专为简化数据库的管理及降低系统管理成本而设。它的设计符合数据库管理员、开发人员及中小企业的需要。Navicat 是以直觉化的图形用户界面而建的，让你可以以安全并且简单的方式创建、组织、访问并共用信息。

### Navicat 的安装

- Navicat 官网：[Navicat GUI](https://www.navicat.com/en/)
  - Navicat 下载（英文版）：[Navicat | 下载 Navicat Premium](https://www.navicat.com/en/download/navicat-premium)
- 国内代理商网站：[Navicat](http://www.navicat.com.cn/)
  - Navicat 下载（简体中文版）：[Navicat | 下载 Navicat Premium](http://www.navicat.com.cn/download/navicat-premium)
- 激活版下载：[Navicat Premium 激活版 - 果核剥壳)](https://www.ghpym.com/navicat.html)
- 安装视频教程：[Navicat 的安装，MySQL8.0 零基础入门之从青铜到钻石教程-慕课网](https://www.imooc.com/video/22559)
- 激活视频教程：[【果核视频】最新版 Navicat15.x 激活教程 - 果核剥壳](https://www.ghpym.com/ghvideo07.html)

## 6-2 Navicat 练习

1. 创建 db 数据库

   ![navicat-create-database](https://img.zxj.guru/mysql/img/navicat-create-database.png)

2. 使用 db 数据库。

   右键选择『打开数据库』。

3. 创建 hero 表，表中包括：id，name，age, sex，location，life，magic，is_hot,grounding_date,max_score 字段。

   - 命令行界面：打开 Navicat 的命令行界面，输入 SQL 语句回车运行即可。

     ![navicat-database-console](https://img.zxj.guru/mysql/img/navicat-database-console.png)

     ```sql
     CREATE TABLE hero (
         id INT,
         name VARCHAR(5),
         age INT,
         sex CHAR(1),
         attack INT,
         location VARCHAR(3),
         life INT,
         magic INT,
         is_hot INT,
         grounding_date DATE,
         max_score DOUBLE
     );
     ```

   ![navicat-database-console-sql](https://img.zxj.guru/mysql/img/navicat-database-console-sql.png)

   - 图形化界面使用教程：[Navicate 练习，MySQL8.0 零基础入门之从青铜到钻石教程-慕课网](https://www.imooc.com/video/22560)

4. 添加一条数据，对数据进行增删改操作

   - 命令行界面：打开 Navicat 的查询窗口，输入 SQL 语句运行即可。

     ```sql
     INSERT into hero VALUES (2, '阿珂', 19, '女', 470, '刺客', 1500, 1100, 0, '2019-06-11', 15.6);
     INSERT into hero VALUES (3, '老夫子', 75, '男', 430, '战士', 2500, 0, 1, '2016-11-18', 17.7);
     INSERT into hero VALUES (4, '吕布', 40, '男', 500, '战士', 2700, 1000, 1, '2015-04-22', 12.2);

     DELETE FROM hero WHERE name = '老夫子' AND location = '战士';
     DELETE FROM hero WHERE name = '吕布' OR location = '刺客';

     UPDATE hero SET is_hot = 0 WHERE name = '亚瑟';

     SELECT * FROM hero;
     ```

     ![navicat-database-select-sql](https://img.zxj.guru/mysql/img/navicat-database-select-sql.png)

   - 图形化界面使用教程：[Navicate 练习，MySQL8.0 零基础入门之从青铜到钻石教程-慕课网](https://www.imooc.com/video/22560)
