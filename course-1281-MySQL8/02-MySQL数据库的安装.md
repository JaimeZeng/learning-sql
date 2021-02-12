# 第 2 章 MySQL 数据库的安装

## 2-1 数据库的卸载

1. 『我的电脑』右键『服务』打开服务窗口，关闭 『MySQL 服务』。

   ![uninstall-mysql-01](https://img.zxj.guru/mysql/img/uninstall-mysql-01.png)

2. 在『设置』中的『应用和功能』搜索『MySQL』，然后把所有『MySQL 模块』全部卸载。

   ![uninstall-mysql-02](https://img.zxj.guru/mysql/img/uninstall-mysql-02.png)

   ![uninstall-mysql-03](https://img.zxj.guru/mysql/img/uninstall-mysql-03.png)

3. 在『我的电脑』中的『系统盘』(默认为 C 盘)找到『MySQL 的安装目录』（64 位电脑默认为『C:/Program Files』，32 位默认为 『C:/Program Files (x86)』），查看是否还有相关残留文件夹（文件夹名为 『MySQL』）。如果有删除即可。

   ![uninstall-mysql-04](https://img.zxj.guru/mysql/img/uninstall-mysql-04.png)

4. 在『我的电脑』菜单栏中『查看』选项卡中勾选『显示隐藏的项目』选项，显示系统盘中的『ProgramData』应用数据文件夹。

   ![uninstall-mysql-05](https://img.zxj.guru/mysql/img/uninstall-mysql-05.png)

5. 在『ProgramData』应用数据文件夹中删除文件夹名为『MySQL』的文件夹。

   ![uninstall-mysql-06](https://img.zxj.guru/mysql/img/uninstall-mysql-06.png)
   ![uninstall-mysql-05](https://img.zxj.guru/mysql/img/uninstall-mysql-05.png)

## 2-2 数据库的安装

- 官网：[MySQL :: Developer Zone](https://dev.mysql.com/)

- 官方文档：[MySQL :: MySQL 8.0 Reference Manual](https://dev.mysql.com/doc/refman/8.0/en/)

- 下载地址：[MySQL :: Download MySQL Installer](https://dev.mysql.com/downloads/installer/)

- 下载直链：[mysql-installer-community-8.0.23.0.msi](https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-8.0.23.0.msi)

  **注意：使用迅雷等工具下载完成记得校验 MD5。**

  ```ini
  FileName: mysql-installer-community-8.0.23.0.msi
  MD5: 8de85ced955631901829a1a363cdbf50
  ```

- 官方安装说明：[MySQL :: MySQL 8.0 Reference Manual :: 2.3 Installing MySQL on Microsoft Windows](https://dev.mysql.com/doc/refman/8.0/en/windows-installation.html)

- 视频教程：[数据库的安装，MySQL8.0 零基础入门之从青铜到钻石教程-慕课网 (imooc.com)](https://www.imooc.com/video/22547)

## 2-3 启动 MySQL 服务

MySQL 启动方式和普通的 windows 程序双击启动方式不同，分为以下 3 种：

1. Windows 服务方式启动

   ![start-mysql-services-01](https://img.zxj.guru/mysql/img/start-mysql-services-01.png)
   ![start-mysql-services-02](https://img.zxj.guru/mysql/img/start-mysql-services-02.png)

2. 命令方式启动：`windows`+`r` 键调出运行窗口，输入 `services.msc` 命令。随后在服务中找到 `MySQL80` 服务启动即可。

   ![start-mysql-services-03](https://img.zxj.guru/mysql/img/start-mysql-services-03.png)
   ![start-mysql-services-04](https://img.zxj.guru/mysql/img/start-mysql-services-04.png)

3. 以 **管理员身份** 运行 `cmd` 打开 dos 窗口，输出 `net start mysql80`。

   > 停止 MySQL 服务方法：`net stop mysql80`。

   ![start-mysql-services-05](https://img.zxj.guru/mysql/img/start-mysql-services-05.png)

4. 使用 `cmd` 命令打开 dos 窗口，输出 `mysql -V` 查看当前 MySQL 版本。

   ![start-mysql-services-06](https://img.zxj.guru/mysql/img/start-mysql-services-06.png)

   ```powershell
   > mysql -V
   mysql  Ver 8.0.23 for Win64 on x86_64 (MySQL Community Server - GPL)
   ```

   如果出现错误将 MySQL 程序添加进系统环境变量 `PATH` 中。

   ```powershell
   # 64位
   C:\Program Files\MySQL\MySQL Server 8.0\bin\
   # 32位
   C:\Program Files (x86)\MySQL\MySQL Server 8.0\bin\
   ```
