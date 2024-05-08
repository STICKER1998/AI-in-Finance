Chapter 1 操作系统该市有
=================================
## Section 1.1 Linux概述
### 1.Linux 系统的组成如下：
- Linux系统内核：其提供最核心的功能，如调度CPU，调度内存，调度文件系统，调度网络通讯，调度IO等；
- 系统级应用程序：可以理解为出厂最基础的软件，比如文件管理器，任务管理器，图片查看，音乐播放等；

### 2.Linux内核
Linux内核可以从 [https://www.kernel.org/] 下载。内核是免费开源的，这也代表了
- 任何人都可以获得，修改内核源码，并且自行集成系统级程序；
- 提供了内核+系统级程序的完整封装，就可以称之为Linux发行版；

**注意**：我们主要基于CentOS进行演示，并辅助Ubuntu进行演示。

## Section 1.2 虚拟机介绍
### 1.什么是虚拟机？
通过虚拟化技术，在电脑内虚拟出计算机硬件并给出虚拟的硬件安装操作系统，即可得到一台虚拟的电脑，称之为虚拟机。虚拟机是由虚拟硬件和操作系统组成的。

VMware 官网下载：
[https://www.vmware.com/products/desktop-hypervisor.html]

建议通过CSDN找资源下载。

CentOS7下载:
[https://pan.baidu.com/s/19hRg2LI1AuqXTQL9IoG5dw?pwd=9987#list/path=%2Fsharelink3232509500-712911551426175%2F1%E3%80%81Linux%E9%9B%B6%E5%9F%BA%E7%A1%80%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8%E5%88%B0%E7%B2%BE%E9%80%9A%2F%E8%AF%BE%E7%A8%8B%E8%B5%84%E6%96%99%2F%E8%BD%AF%E4%BB%B6%E5%8C%85%2FCentOS%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85%E5%8C%85&parentPath=%2Fsharelink3232509500-712911551426175]

各类软件均可从此下载
[https://pan.baidu.com/s/19hRg2LI1AuqXTQL9IoG5dw?pwd=9987]

## Section 1.3 远程连接Linux: FinalShell
- 在VMware虚拟机的桌面的空白位置右键'open terminal'打开终端；
- 在终端中输入`ifconfig`，定位到`ens33`位置找到主机ip；
- 在FinalShell中点击左上角文件夹的图标通过SSH创建和VMware的连接；

注意事项：
- 如果重启虚拟机，ip地址有可能变化，此时你需要在FinalShell中修改你的ip地址；
- 在使用FinalShell时，请勿关闭虚拟机，只能最小化；

