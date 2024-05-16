Quant Finance & Data Analysis
==============================
该Github文件主要用于记录成长为一个quant/DA需要做的准备：
- 基础知识：MySQL，Linux，Python
- 因子模型及其金融解释；
- 机器学习方法，Transformer（金融预测）, 遗传规划（因子挖掘）等；


|Topics                     |         Update                             |                                              
| ---                       |---                                              |                                        
|[Transformer-based Models](./notebooks/Topic1.md)   |2024/02/21 |          
|Factor Models              |         coming soon                             |
|[MySQL](#mysql)                   |        2024/05/16           |                                   
| [Linux](#linux)                    |               2024/05/08                  |       
|Python                     |    coming soon                                  |
 

MySQL
==========================
|章|小节|
|:---:|:---:|
|[Chapter 1 MySQL基础](./notebooks/MySQL/MySQL.md) |[MySQL绪论](./notebooks/MySQL/MySQLC1S1.md)  [SQL基础语句](./notebooks/MySQL/MySQLC1S2.md)  [函数](./notebooks/MySQL/MySQLC1S3.md)  [约束](./notebooks/MySQL/MySQLC1S4.md)  [多表查询](./notebooks/MySQL/MySQLC1S5.md)  [事务](./notebooks/MySQL/MySQLC1S6.md)|
|Chapter 2 MySQL进阶 |存储引擎  索引  SQL优化  视图/存储过程/触发器 锁 InnoDB引擎 MySQL管理|
|Chapter 3 MySQL实战 |[行列转换问题](./notebooks/MySQL/MySQLC3S1.md) [窗口函数](./notebooks/MySQL/MySQLC3S2.md)|


Linux
==========================
|章节标题|主要内容|
|:---:|:---:|
|[Chapter 1 操作系统概述](./notebooks/Linux/LinuxC1.md)  |Linux操作系统，虚拟机(VMware)安装，CentOS 7 配置，FinalShell配置与使用|
|[Chapter 2 Linux命令基础](./notebooks/Linux/LinuxC2.md)|Linux目录结构， 路径与特殊路径符(`.`,`..`,`~`)，目录切换命令(`cd`/`pwd`)，创建目录命令(`mkdir`)， <br> 文件操作命令(`touch`/`cat`/`more`/`cp`/`mv`)，查找命令(`which`/`find`)，`grep`/`wc`与管道符(`\|`)，`echo`/`tail` 和重定向符)|
|[Chapter 3 用户和权限](./notebooks/Linux/LinuxC3.md) |root用户，用户与用户组管理(`groupadd`/`groupdel`/`useradd`/`userdel`/`id`/`gatent`)， 查看权限控制，修改权限控制(`chmod`/`chown`)|
|[Chapter 4 Linux实用操作](./notebooks/Linux/LinuxC4.md) |快捷键，软件安装，systemctl，软连接，日期与时区，IP地址与主机名，网络传输， 进程管理，主机状态，环境变量，上传与下载，压缩与解压|



git 使用注意事项
==========================
- `ipconfig/flushdns`：以刷新DNS缓存，常在`git pull`,`git push`前使用。
