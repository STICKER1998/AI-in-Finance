MySQL
=================================

# Chapter 1 MySQL 基础知识
## Section 1.1 MySQL概述
### 1. 数据库的相关概念
**数据库(DataBase, DB)**： 存储数据的仓库， 数据是有组织的进行储存

**数据管理系统(Database Management System, DBMS)**： 操纵和管理数据的大型软件

**SQL(Structed Query Language)**：操作关系型数据库的编程语言，定义了一套操作关系型数据库统一标准

### 2. MySQL下载
Windows: [MySQL](https://dev.mysql.com/downloads/installer/)
在安装时需要选择Full Install， 需要耗时几分钟

### 3. MySQL启动与停止
方法1： win+R -> services.msc -> 找到MySQL80 右键可以控制启动以及停止
方法2： cmd 以管理员身份运行 -> 输入命令行 'net start mysql80' / 'net stop mysql80'
