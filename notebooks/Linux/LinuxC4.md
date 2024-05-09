Chapter 4 Linux实用操作
===========================================
## Section 4.1 快捷键
**强制停止**
`Ctrl+c`：可以强制停止一些命令以及退出一些输入错误的命令；

**退出或登出**
`Ctrl+d`：可以退出账户的登陆以及某些特定程序的专属界面；

注意：`Ctrl+d`不能用于退出`vi/vim`编辑器；

**搜索历史命令**
- `history`可以查看历史输入过的命令；
- `!+命令前缀`：会自动执行上一次匹配命令前缀的命令；注意，最后不要用此命令使用很久远的命令；
- `Ctrl+r`：输入内容去匹配历史命令；


**光标快捷键**
- `Ctrl+a`：跳到命令开头；
- `Ctrl+e`：跳到命令结尾；
- `Ctrl+左键`：向左跳一个单词；
- `Ctrl+右键`：向右跳一个单词；

**清空**
`Ctrl+a`/`clear`：清空屏幕

## Section 4.2 软件安装
### 1.`yum`命令
在CentOS系统下，软件安装包的后缀为`.rpm`，我们可以使用`yum`命令进行安装。
```
yum [-y] [install | remove | search] 软件名称
```
|选项|功能|
|:---:|:-:|
|install| 安装|
|remove|卸载|
|search|搜索|

**注意**
- `yum`命令需要root权限，可以`su`切换到`root`或者使用`sudo`临时提权；
- `yum`命令需要联网；

**例子**：利用`yum`软件搜索并安装`wget`程序
```
yum [-y] search wget
yum [-y] install wget
```
如果使用`yum -y install wget`，在安装过程中所有选项都会选择确定；



### 2.`apt`命令
下面拓展一下在`Ubuntu`系统下如何安装文件。在Ubuntu系统下，软件安装包的后缀为`.deb`，我们可以使用`apt`命令进行安装。
```
apt [-y] [install | remove | search] 软件名称
```
`apt`命令同样需要root权限；


## Section 4.3 控制软件启动和关闭
### 1.`systemctl`命令
Linux系统很多软件（内置或第三方）均支持使用systemctl命令控制：启动，停止，开机自动启动。能够被 管理的软件，一般称之为**服务**。
```
systemctl start|stop|status|enable|disable 服务名
```
系统内置的服务比较多，比如
|名称|服务|
|:---:|:---:|
|NetworkManager|主网络服务|
|network|副网络服务|
|firewalld|防火墙服务|
|sshd|ssh服务|

大部分软件安装后会自动集成到systemctl中，但对于部分软件需要手动添加。

## Section 4.4 软链接
在系统中创建软链接，可以将文件，文件夹链接到其他位置，类似于windows中的快捷方式。

```
ln -s 参数1 参数2
```
- 选项-s，创建软链接
- 参数1，被链接的文件或者文件夹；
- 参数2，要链接去的目的地；

