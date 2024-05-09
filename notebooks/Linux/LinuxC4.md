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

## Section 4.5 日期与时区
### 1.date命令
```
date [-d] [+格式化字符串]
```
- 选项-d，按照给定的字符串显示日期，一般用于日期计算；
- 格式化字符串；

### 2.Linux时区
系统默认为UTC时区，我们可以使用root用户修改时区。
```
rm -f /etc/localtime
sudo ln -s /user/share/zoneinfo/Asia/Shanghai /etc/localtime
```
首先将系统自带的localtime删除，再将`/user/share/zoneinfo/Asia/Shanghai`软链接上` /etc/localtime`。

### 3.校准时间
我们可以通过ntp程序自动校准系统时间；首先下载安装ntp软件并将其ntpd服务设置为开机自动启动；
```
yum -y install ntp
systemctl start ntpd
systemctl enable ntpd
```
我们也可以手动校准时间，将ntp连接到一个提供时间校准的服务器。
```
ntpdate -u ntp.aliyun.com
```

## Section 4.6 IP地址和主机名
### 1.IP地址
每个电脑都有一个地址，用于和其他计算机进行通讯。IP地址主要有两个版本，V4和V6版本。目前主要使用的是IPv4版本`a.b.c.d`；

我们可以通过如下命令查看IP地址：
```
ifconfig
```

### 2.特殊IP地址
除了标准IP地址之外，还有几个特殊的IP地址需要我们了解；

本地回环IP 127.0.0.1 这个IP地址用于指代本机；

0.0.0.0特殊IP地址
— 可以用指代本机
- 可以在端口中绑定关系
- 在一些IP地址限制中，表示所有IP的意思，如放行规则设置为0.0.0.0，表示允许访问任意IP地址；

### 3.主机名
每一台电脑除了对外联络地址（IP地址）以外，也可以有一个名字，称为主机名；
**查看主机名**
```
hostname
```
**修改主机名**
```
hostnamectl set-hostname 新名字(例子里设为centos)
```

### 4.域名解析
IP地址非常复杂，难以记忆，我们可以通过域名解析来解决这个问题；
- 先查看本机的记录：Linux系统中在/etc/hosts
- 再联网去DNS服务器询问
  
### 5.配置主机名映射
FinalShell通过IP地址连接到了Linux服务器，我们也可以通过主机名（域名）来连接。

- 以管理员身份运行记事本；
- 打开`C:\Windows\System32\drivers\etc\hosts`；
- 在最后编写`Linux系统的IP地址 主机名`并保存；
- 在FinalShell中将IP地址连接改成主机名(`centos`)连接；


