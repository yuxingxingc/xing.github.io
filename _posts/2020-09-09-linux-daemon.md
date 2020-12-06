---
layout: post
title: linux系统之系统服务daemon
tags: linux
---



# 认识系统服务 daemon

[toc]



## 1,什么是daemon和服务



- ​	**`Service服务`：<u>是常驻与内存中的进程，他提供了系统或者网络的功能，就叫做服务，也称作service。</u>**

- ​	**`daemon`：<u>为了支持service而运行的进程我们称之为daemon</u>**



### 早期system V的init的管理行为中daemon的分类 



​	在早期unix系统下，system V的版本里，系统启动第一个调用的程序就是init，然后init再去调用其他的服务/service。



​	init的管理机制如下：



> - 服务的启动，关闭和观察：
>   - 启动：/etc/init.d/daemon start
>   - 关闭：/etc/init.d/daemon stop
>   - 重启：/etc/init.d/daemon restart
>   - 观察状态：/etc/init.d/daemon status
>   
> - 服务的分类：
>   - 独立启动服务：此类服务常驻于内存中，可以为本机用户提供服务，响应速度快。
>   - 超级服务：此类服务不会常驻于内存当中，当用户需要socket时，xinetd和inetd这两个超级服务会提供socket，但平时超级服务是不会启动的，服务结束后响应的服务也会被关闭，所以超级服务的响应速度稍慢。
>   
> - 服务的相互依赖问题：
>   
>   - 例如服务A的启动需要服务B已经启动，那么称作这两个服务之间有依赖关系，注意！通过init手动启动的服务不会启动其相关依赖性的服务。
>   
> - 执行优先级分类：
>   - init可以根据用户设定的执行等级来启动不同的服务，进入不同的界面，linux提供七个执行等级：1，单人维护模式，3，纯文本模式，5，文字加图形界面。
>   - 执行等级是通过/etc/rc.d/rc[0-6]/SXXdaemon link到/etc/init.d/daemon/，其中S为启动该服务，XX是优先级数字。
>   
> - 为服务设置执行优先级：
>   - 预设启动的服务：chkconfig daemon on
>   - 预设不启动的服务：chkconfig daemon off
>   - 查询是否为预设服务：chkconfig --list daemon
>   
> - 执行等级的行为切换：
>   
>   - init5即可以从其他等级切换到图像+文字的操作界面。



##### systemd的配置文件路径



​	基本算，systemd通过daemon去执行一个服务单元unit。每种unit可分为不同的类型type，下面是几种常用的服务设置路径：



> 1. /usr/lib/systemd/system/: 每个服务主要的设置文件路径。
> 2. /run/systemd/system/: 执行过程中产生的服务脚本
> 3. /etc/systemd/system/：设置服务优先级的路径

​	

### systemd使用unit的分类



​	CentOS 7.X之后Redhat公司放弃了System V的启动服务方式，采用了Systemd这种机制。systemd的优点如下：



> 1. 支持并行处理服务， 比init这种串行处理服务的方式更高效。
> 2. systemd只有一个命令，使用起来更容易，而且systemd常驻于内存中，响应速度快，可以及时响应用户的需求。
> 3. 服务依赖性检查，systemd会帮助你启动存在相关依赖性的服务。
> 4. systemd把服务分成更多的`种类`(unit)，方便用户使用和记忆。
> 5. systemd向下兼容init的脚本程序。



##### systemd的unit类型介绍



​	通过下列命令可以查询/usr/lib/systemd/system/下服务的类型，主要看文件的副名档。



```bash
ll /usr/lib/systemd/system/ | grep -E '(vsftpd|multi|cron)'
```



---

| 副名档   | 主要服务类型           |
| -------- | ---------------------- |
| .service | 网络等等系统服务类型   |
| .socket  | 内部程序交换数据的服务 |
| .target  | 一堆服务的集合         |
| .mout    | 挂载服务               |
| .path    | 检测目录和路径的服务   |
| .timer   | 定时循环执行服务类型   |

---



## 2,通过systemctl管理进程



### systemctl管理单一进程的启动，开机启动和状态观察



​	服务的启动流程有两个阶段，1，开机时需要不需要启动服务？2，现在要不要立刻启动服务？

systemctl命令的用法和选项。



```bash
systemctl [command] [unit]
#command
#start:	立刻启动unit服务
#stop:	立刻结束unit服务
#restart:	重启服务
#reload:	不关闭服务的情况下，重新加载服务的配置文件
#enable:	开机启动服务
#disable:	开机不启动服务
#mask:		禁用服务
#unmask:	取消禁用服务
#status:	查询服务状态
#is-active:	查询服务是否正在运行
#is-enable:	查询服务是否开机启动
```



服务状态的查看和输出说明解释：



```bash
[root@study ~]# systemctl status atd.service
atd.service - Job spooling tools
   Loaded: loaded (/usr/lib/systemd/system/atd.service; enabled)
   Active: inactive (dead) since Tue 2015-08-11 01:04:55 CST; 4s ago
  Process: 1350 ExecStart=/usr/sbin/atd -f $OPTS (code=exited, status=0/SUCCESS)
 Main PID: 1350 (code=exited, status=0/SUCCESS)
```



其中`active:(xxx)`表示服务的状态，分别有以下几种状态：

* running：服务正在运行中。
* exited：服务正常结束退出。
* waiting：服务正在执行过程中。
* inactive：服务没有运行。



其中`loaded:(xxx)`表示服务的设置状态：

* enabled：开机此服务会自动启动
* disabled：开机此服务不会自动启动
* static：此服务不能自己启动（无法enable）
* mask：此服务无论如何都无法启动



### systemctl查看所有进程的状态



​	如何查看系统上所有的服务？利用list-units和list-unit-files观察：



```bash
systemctl [command] [--type=TYPE] [--all]
#list-units:	根据unit列出启动所有的服务，--all会列出没有启动的服务
#list-unit-files:	根据usr/lib/systemd/system下的文件，列出所有的说明文档
#--type=TYPE:		其中TYPE可以是target，socket，service等等类型
```



### systemctl管理不同的操作环境(图像界面或者命令界面)



​	首先查询所有target类型的服务：



```bash
systemctl list-units --type=target --all
```

​	

​	解读屏幕打印输出：

* graphical.target：文字+图像界面
* multi-user.target：纯文本模式
* rescure.target：自救模式
* emergency.target：紧急模式，在rescure不能使用的情况下，使用root登录
* shutdown.target：关机流程
* getty.target：设置tty的个数



​	设置linux操作模式（图像模式/命令行模式）：



```bash
systemctl [command] [unit.target]
#get-default:	读取目前操作环境的默认模式设置
#set-default:	设置默认操作环境操作模式
#isolate:		切换到某个操作模式

#实例-获取预设模式
systemctl get-default

#实例-改变预设模式
systemctl set-default multi-user.target

#实例-切换到文字模式
systemctl isolate multi-user.target
```



​	systemctl提供的其他操作参数

```bash
systemctl poweroff	#系统关机
systemctl reboot	#系统开机
systemctl suspend	#系统进入暂停模式
systemctl hibernate	#系统进入休眠模式
systemctl rescure	#系统进入自救模式
systemctl emergency #系统进入紧急模式
```



### systemctl分析服务之间的依赖关系



​	系统的服务通常互相之间有依赖关系，我们可以用`systemctl` 查询服务之间的依赖关系



```bash
systemctl list-dependencies [unit] [--reverse]
#参数选项
#--reverse	反向追踪谁使用这个服务

#实例-查询graphical使用了哪写服务
systemctl list-dependencies graphical.target

graphical.target
├─accounts-daemon.service
├─gdm.service
├─network.service
├─rtkit-daemon.service
├─systemd-update-utmp-runlevel.service
└─multi-user.target
  ├─abrt-ccpp.service
  ├─abrt-oops.service
.....(底下省略).....
```



### 和systemd相关的daemon的运作原理和相关目录介绍

​	

​	和系统daemon相关的目录及其解释：

* /usr/lib/systemd/system/
  * CentOS官方按照的软件所存放配置文件的路径
* /run/systemd/system/
  * 系统执行过程中产生的服务脚本
* /etc/systemd/system/
  * 管理员根据系统需求所建立的脚本存放路径
* /etc/sysconfig/*
  * 服务初始化读取预设配置的路径
* /var/lib
  * 服务在运行中产生的数据存放路径
* /run/
  * 存放daemon的暂存文件



​	**网络服务和端口映射的介绍**，`/etc/services`是系统用来将服务和端口做映射的服务。其中第一列表示服务，第二列表示端口和协议。

```bash
cat /etc/services

tcpmux          1/tcp                           # TCP port service multiplexer
echo            7/tcp
echo            7/udp
discard         9/tcp           sink null
discard         9/udp           sink null
```







### 关闭网络服务

​	

​	第一步，找到当前的所有网络服务和对应端口：

```bash
netstat -tlunp
```



​	第二步，找到你要查询的服务和端口

```bash
systemctl list-units --all | grep avahi-daemon
```



​	第三步，关闭服务，查询端口是否还在继续被使用

```bash
systemctl stop avahi-daemon.service
systemctl stop avahi-daemon.socket
systemctl disable avahi-daemon.socket avahi-daemon.socket
netstat -tlunp
```



## 3,systemctl对于服务类型的设置

​	

​	本章主要介绍如何建立一个系统服务。



### systemctl设置文件夹目录介绍



​	一般情况下，systemd的设置文件放置于`/usr/lib/systemd/system/`下，一般情况不要修改。建议修改`/etc/systemd/system`下的配置文件。



后续内容过于深入，测试人员视情况可选择深入学习，来源链接

[鸟哥linux私房菜]: https://linux.vbird.org/linux_basic/centos7/0560daemons.php#systemctl_files





### systemctl设置文件和设置项目介绍

### 两个vsftpd运行实例

### 多重重复设定：getty为例

### 自己的服务自己设定

## 4,systemtcl对于timer的设定

## 5,CentOS 7.X 开机启动服务的简易说明

