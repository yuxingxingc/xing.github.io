---
layout: post
title: linux之常用指令
description: 一些linux实用的指令。
tags: linux
---



# Linux Command

[toc]

​	命令基本使用格式

```bash
command  [-options]  parameter1  parameter2
```

## 基础命令

### 语言设置 locate

```bash
#显示语言
locate

#设置语言
LANG=en_US.utf8
export LC_ALL=en_US.utf8
```

### 时间设置 date

```bash
#查询时间
date

#按照格式查询时间
date +%Y/%m/%d
2015/05/29

date +%H:%M
14:33
```

### 显示日历 cal

```bash
#显示当前月份
cal
      May 2015
Su Mo Tu We Th Fr Sa
                1  2
 3  4  5  6  7  8  9
10 11 12 13 14 15 16
17 18 19 20 21 22 23
24 25 26 27 28 29 30
31

#显示某年日历
cal 2015
                               2015

       January               February                 March
Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa
             1  2  3    1  2  3  4  5  6  7    1  2  3  4  5  6  7
 4  5  6  7  8  9 10    8  9 10 11 12 13 14    8  9 10 11 12 13 14
11 12 13 14 15 16 17   15 16 17 18 19 20 21   15 16 17 18 19 20 21
18 19 20 21 22 23 24   22 23 24 25 26 27 28   22 23 24 25 26 27 28
25 26 27 28 29 30 31                          29 30 31

        April                   May                   June
Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa
          1  2  3  4                   1  2       1  2  3  4  5  6
 5  6  7  8  9 10 11    3  4  5  6  7  8  9    7  8  9 10 11 12 13
12 13 14 15 16 17 18   10 11 12 13 14 15 16   14 15 16 17 18 19 20
19 20 21 22 23 24 25   17 18 19 20 21 22 23   21 22 23 24 25 26 27
26 27 28 29 30         24 25 26 27 28 29 30   28 29 30
                       31

#显示某年某月
cal 10 2015
    October 2015
Su Mo Tu We Th Fr Sa
             1  2  3
 4  5  6  7  8  9 10
11 12 13 14 15 16 17
18 19 20 21 22 23 24
25 26 27 28 29 30 31
```

### 计算器 bc

```bash
#启用计算器
bc
#支持+，-，*，/，^,%

#设置输出小数点
scale=number
```

### 帮助命令 man、info

```bash
#帮助选项 --help
date --help

#man page
man date

#查询所有文档，等于whatis
man -f man
man (1)              - an interface to the on-line reference manuals
man (1p)             - display system documentation
man (7)              - macros to format man pages

#调用指定文档
man 1 man

#查询含有某个关键字的文档，等于apropos
man -k man
```

​	关于man文档中数字代表的含义：

| 代号 | 代表内容                                                     |
| ---- | ------------------------------------------------------------ |
| 1    | 使用者在shell环境中可以操作的指令或可执行档                  |
| 2    | 系统核心可呼叫的函数与工具等                                 |
| 3    | 一些常用的函数(function)与函式库(library)，大部分为C的函式库(libc) |
| 4    | 装置档案的说明，通常在/dev下的档案                           |
| 5    | 设定档或者是某些档案的格式                                   |
| 6    | 游戏(games)                                                  |
| 7    | 惯例与协定等，例如Linux档案系统、网路协定、ASCII code等等的说明 |
| 8    | 系统管理员可用的管理指令                                     |
| 9    | 跟kernel有关的文件                                           |

​	关于man文档中各个部分的介绍

| 代号        | 内容说明                                                     |
| ----------- | ------------------------------------------------------------ |
| NAME        | 简短的指令、资料名称说明                                     |
| SYNOPSIS    | 简短的指令下达语法(syntax)简介                               |
| DESCRIPTION | 较为完整的说明，这部分最好仔细看看！                         |
| OPTIONS     | 针对 SYNOPSIS 部分中，有列举的所有可用的选项说明             |
| COMMANDS    | 当这个程式(软体)在执行的时候，可以在此程式(软体)中下达的指令 |
| FILES       | 这个程式或资料所使用或参考或连结到的某些档案                 |
| SEE ALSO    | 可以参考的，跟这个指令或资料有相关的其他说明！               |
| EXAMPLE     | 一些可以参考的范例                                           |

### 关机命令 shutdown，reboot，halt，poweroff

```bash
#同步命令，将内存数据写入硬盘
sync

#常用关机命令
shutdown -k 'warning'
-r	:重启
-h	:停止服务后关机

#某个时间关机
shutdown -h 20:25
#立马关机
shutdown -h now
#30分钟后重启
shutdown -r +30

#其他关机命令
reboot

halt

poweroff

```

## 文件和权限命令

### 改变所属组 chgrp

```bash
#改变所属组命令
chgrp -R file
-R	:循环改权限，子文件夹权限也更改
```

### 改变所有者 chown

```bash
#改变权限，所属者
chown -R user file
-R	:循环改权限，子文件夹权限也更改
```

### 改变文件权限 chmod

```bash
#改变文件权限，用数字改权限
chmod -R 777 file
r=4		：可读
w=2		：可写
x=1		：可执行

#用符号改文件权限
chmod  u=rwx,go=rx  .bashrc
```

| 命令  | 用户组  | 编辑权限                | 具体权限 | 目标文件   |
| ----- | ------- | ----------------------- | -------- | ---------- |
| chmod | u g o a | +(加入) -(除去) =(设定) | r w x    | 档案或目录 |

### 权限对于文件和目录的意义

| 元件 | 内容         | 叠代物件   | r            | w            | x                     |
| ---- | ------------ | ---------- | ------------ | ------------ | --------------------- |
| 档案 | 详细资料data | 文件资料夹 | 读到文件内容 | 修改文件内容 | 执行文件内容          |
| 目录 | 档名         | 可分类抽屉 | 读到档名     | 修改档名     | 进入该目录的权限(key) |

​	使用者对于文件权限的意义

| 操作动作              | /dir1 | /dir1/file1 | /dir2 | 重点                                      |
| --------------------- | ----- | ----------- | ----- | ----------------------------------------- |
| 读取 file1 内容       | x     | r           | -     | 要能够进入 /dir1 才能读到裡面的文件资料！ |
| 修改 file1 内容       | x     | rw          | -     | 能够进入 /dir1 且修改 file1 才行！        |
| 执行 file1 内容       | x     | rx          | -     | 能够进入 /dir1 且 file1 能运作才行！      |
| 删除 file1 档案       | wx    | -           | -     | 能够进入 /dir1 具有目录修改的权限即可！   |
| 将 file1 複製到 /dir2 | x     | r           | wx    | 要能够读 file1 且能够修改 /dir2 内的资料  |

### linux目录的含义

| 目录                               | 应放置档案内容                                               |
| ---------------------------------- | ------------------------------------------------------------ |
| 第一部份：FHS 要求必须要存在的目录 |                                                              |
| /bin                               | 系统有很多放置执行档的目录，但/bin比较特殊。因为/bin放置的是在单人维护模式下还能够被操作的指令。 在/bin底下的指令可以被root与一般帐号所使用，主要有：cat, chmod, chown, date, mv, mkdir, cp, bash等等常用的指令。 |
| /boot                              | 这个目录主要在放置开机会使用到的档案，包括Linux核心档案以及开机选单与开机所需设定档等等。 Linux kernel常用的档名为：vmlinuz，如果使用的是grub2这个开机管理程式， 则还会存在/boot/grub2/这个目录喔！ |
| /dev                               | 在Linux系统上，任何装置与周边设备都是以档案的型态存在于这个目录当中的。 你只要透过存取这个目录底下的某个档案，就等于存取某个装置囉～ 比要重要的档案有/dev/null, /dev/zero, /dev/tty, /dev/loop*, /dev/sd*等等 |
| /etc                               | 系统主要的设定档几乎都放置在这个目录内，例如人员的帐号密码档、 各种服务的启始档等等。一般来说，这个目录下的各档案属性是可以让一般使用者查阅的， 但是只有root有权力修改。FHS建议不要放置可执行档(binary)在这个目录中喔。比较重要的档案有： /etc/modprobe.d/, /etc/passwd, /etc/fstab, /etc/issue 等等。另外 FHS 还规范几个重要的目录最好要存在 /etc/ 目录下喔：/etc/opt(必要)：这个目录在放置第三方协力软体 /opt 的相关设定档/etc/X11/(建议)：与 X Window 有关的各种设定档都在这裡，尤其是 xorg.conf 这个 X Server 的设定档。/etc/sgml/(建议)：与 SGML 格式有关的各项设定档/etc/xml/(建议)：与 XML 格式有关的各项设定档 |
| /lib                               | 系统的函式库非常的多，而/lib放置的则是在开机时会用到的函式库， 以及在/bin或/sbin底下的指令会呼叫的函式库而已。 什麽是函式库呢？你可以将他想成是『外挂』，某些指令必须要有这些『外挂』才能够顺利完成程式的执行之意。 另外 FHS 还要求底下的目录必须要存在：/lib/modules/：这个目录主要放置可抽换式的核心相关模组(驱动程式)喔！ |
| /media                             | media是『媒体』的英文，顾名思义，这个/media底下放置的就是可移除的装置啦！ 包括软碟、光碟、DVD等等装置都暂时挂载于此。常见的档名有：/media/floppy, /media/cdrom等等。 |
| /mnt                               | 如果你想要暂时挂载某些额外的装置，一般建议你可以放置到这个目录中。 在古早时候，这个目录的用途与/media相同啦！只是有了/media之后，这个目录就用来暂时挂载用了。 |
| /opt                               | 这个是给第三方协力软体放置的目录。什麽是第三方协力软体啊？ 举例来说，KDE这个桌面管理系统是一个独立的计画，不过他可以安装到Linux系统中，因此KDE的软体就建议放置到此目录下了。 另外，如果你想要自行安装额外的软体(非原本的distribution提供的)，那麽也能够将你的软体安装到这裡来。 不过，以前的Linux系统中，我们还是习惯放置在/usr/local目录下呢！ |
| /run                               | 早期的 FHS 规定系统开机后所产生的各项资讯应该要放置到 /var/run 目录下，新版的 FHS 则规范到 /run 底下。 由于 /run 可以使用记忆体来模拟，因此效能上会好很多！ |
| /sbin                              | Linux有非常多指令是用来设定系统环境的，这些指令只有root才能够利用来『设定』系统，其他使用者最多只能用来『查询』而已。 放在/sbin底下的为开机过程中所需要的，裡面包括了开机、修复、还原系统所需要的指令。 至于某些伺服器软体程式，一般则放置到/usr/sbin/当中。至于本机自行安装的软体所产生的系统执行档(system binary)， 则放置到/usr/local/sbin/当中了。常见的指令包括：fdisk, fsck, ifconfig, mkfs等等。 |
| /srv                               | srv可以视为『service』的缩写，是一些网路服务启动之后，这些服务所需要取用的资料目录。 常见的服务例如WWW, FTP等等。举例来说，WWW伺服器需要的网页资料就可以放置在/srv/www/裡面。 不过，系统的服务资料如果尚未要提供给网际网路任何人浏览的话，预设还是建议放置到 /var/lib 底下即可。 |
| /tmp                               | 这是让一般使用者或者是正在执行的程序暂时放置档案的地方。 这个目录是任何人都能够存取的，所以你需要定期的清理一下。当然，重要资料不可放置在此目录啊！ 因为FHS甚至建议在开机时，应该要将/tmp下的资料都删除唷！ |
| /usr                               | 第二层 FHS 设定，后续介绍                                    |
| /var                               | 第二曾 FHS 设定，主要为放置变动性的资料，后续介绍            |
| 第二部份：FHS 建议可以存在的目录   |                                                              |
| /home                              | 这是系统预设的使用者家目录(home directory)。在你新增一个一般使用者帐号时， 预设的使用者家目录都会规范到这裡来。比较重要的是，家目录有两种代号喔：~：代表目前这个使用者的家目录~dmtsai ：则代表 dmtsai 的家目录！ |
| /lib<qual>                         | 用来存放与 /lib 不同的格式的二进位函式库，例如支援 64 位元的 /lib64 函式库等 |
| /root                              | 系统管理员(root)的家目录。之所以放在这裡，是因为如果进入单人维护模式而仅挂载根目录时， 该目录就能够拥有root的家目录，所以我们会希望root的家目录与根目录放置在同一个分割槽中。 |

* 其他目录

| 目录        | 应放置档案内容                                               |
| ----------- | ------------------------------------------------------------ |
| /lost+found | 这个目录是使用标准的ext2/ext3/ext4档案系统格式才会产生的一个目录，目的在于当档案系统发生错误时， 将一些遗失的片段放置到这个目录下。不过如果使用的是 xfs 档案系统的话，就不会存在这个目录了！ |
| /proc       | 这个目录本身是一个『虚拟档案系统(virtual filesystem)』喔！他放置的资料都是在记忆体当中， 例如系统核心、行程资讯(process)、周边装置的状态及网路状态等等。因为这个目录下的资料都是在记忆体当中， 所以本身不佔任何硬碟空间啊！比较重要的档案例如：/proc/cpuinfo, /proc/dma, /proc/interrupts, /proc/ioports, /proc/net/* 等等。 |
| /sys        | 这个目录其实跟/proc非常类似，也是一个虚拟的档案系统，主要也是记录核心与系统硬体资讯较相关的资讯。 包括目前已载入的核心模组与核心侦测到的硬体装置资讯等等。这个目录同样不佔硬碟容量喔！ |

* usr目录

| 目录                               | 应放置档案内容                                               |
| ---------------------------------- | ------------------------------------------------------------ |
| 第一部份：FHS 要求必须要存在的目录 |                                                              |
| /usr/bin/                          | 所有一般用户能够使用的指令都放在这裡！目前新的 CentOS 7 已经将全部的使用者指令放置于此，而使用连结档的方式将 /bin 连结至此！ 也就是说， /usr/bin 与 /bin 是一模一样了！另外，FHS 要求在此目录下不应该有子目录！ |
| /usr/lib/                          | 基本上，与 /lib 功能相同，所以 /lib 就是连结到此目录中的！   |
| /usr/local/                        | 系统管理员在本机自行安装自己下载的软体(非distribution预设提供者)，建议安装到此目录， 这样会比较便于管理。举例来说，你的distribution提供的软体较旧，你想安装较新的软体但又不想移除旧版， 此时你可以将新版软体安装于/usr/local/目录下，可与原先的旧版软体有分别啦！ 你可以自行到/usr/local去看看，该目录下也是具有bin, etc, include, lib...的次目录喔！ |
| /usr/sbin/                         | 非系统正常运作所需要的系统指令。最常见的就是某些网路伺服器软体的服务指令(daemon)囉！不过基本功能与 /sbin 也差不多， 因此目前 /sbin 就是连结到此目录中的。 |
| /usr/share/                        | 主要放置唯读架构的资料档案，当然也包括共享文件。在这个目录下放置的资料几乎是不分硬体架构均可读取的资料， 因为几乎都是文字档案嘛！在此目录下常见的还有这些次目录：/usr/share/man：线上说明文件/usr/share/doc：软体杂项的文件说明/usr/share/zoneinfo：与时区有关的时区档案 |
| 第二部份：FHS 建议可以存在的目录   |                                                              |
| /usr/games/                        | 与游戏比较相关的资料放置处                                   |
| /usr/include/                      | c/c++等程式语言的档头(header)与包含档(include)放置处，当我们以tarball方式 (*.tar.gz 的方式安装软体)安装某些资料时，会使用到裡头的许多包含档喔！ |
| /usr/libexec/                      | 某些不被一般使用者惯用的执行档或脚本(script)等等，都会放置在此目录中。例如大部分的 X 视窗底下的操作指令， 很多都是放在此目录下的。 |
| /usr/lib<qual>/                    | 与 /lib<qual>/功能相同，因此目前 /lib<qual> 就是连结到此目录中 |
| /usr/src/                          | 一般原始码建议放置到这裡，src有source的意思。至于核心原始码则建议放置到/usr/src/linux/目录下。 |



- /var 的意义与内容：

如果/usr是安装时会佔用较大硬碟容量的目录，那麽/var就是在系统运作后才会渐渐佔用硬碟容量的目录。 因为/var目录主要针对常态性变动的档案，包括快取(cache)、登录档(log file)以及某些软体运作所产生的档案， 包括程序档案(lock file, run file)，或者例如MySQL资料库的档案等等。常见的次目录有：

| 目录                               | 应放置档案内容                                               |
| ---------------------------------- | ------------------------------------------------------------ |
| 第一部份：FHS 要求必须要存在的目录 |                                                              |
| /var/cache/                        | 应用程式本身运作过程中会产生的一些暂存档；                   |
| /var/lib/                          | 程式本身执行的过程中，需要使用到的资料档案放置的目录。在此目录下各自的软体应该要有各自的目录。 举例来说，MySQL的资料库放置到/var/lib/mysql/而rpm的资料库则放到/var/lib/rpm去！ |
| /var/lock/                         | 某些装置或者是档案资源一次只能被一个应用程式所使用，如果同时有两个程式使用该装置时， 就可能产生一些错误的状况，因此就得要将该装置上锁(lock)，以确保该装置只会给单一软体所使用。 举例来说，烧录机正在烧录一块光碟，你想一下，会不会有两个人同时在使用一个烧录机烧片？ 如果两个人同时烧录，那片子写入的是谁的资料？所以当第一个人在烧录时该烧录机就会被上锁， 第二个人就得要该装置被解除锁定(就是前一个人用完了)才能够继续使用囉。目前此目录也已经挪到 /run/lock 中！ |
| /var/log/                          | 重要到不行！这是登录档放置的目录！裡面比较重要的档案如/var/log/messages, /var/log/wtmp(记录登入者的资讯)等。 |
| /var/mail/                         | 放置个人电子邮件信箱的目录，不过这个目录也被放置到/var/spool/mail/目录中！ 通常这两个目录是互为连结档啦！ |
| /var/run/                          | 某些程式或者是服务启动后，会将他们的PID放置在这个目录下喔！至于PID的意义我们会在后续章节提到的。 与 /run 相同，这个目录连结到 /run 去了！ |
| /var/spool/                        | 这个目录通常放置一些伫列资料，所谓的『伫列』就是排队等待其他程式使用的资料啦！ 这些资料被使用后通常都会被删除。举例来说，系统收到新信会放置到/var/spool/mail/中， 但使用者收下该信件后该封信原则上就会被删除。信件如果暂时寄不出去会被放到/var/spool/mqueue/中， 等到被送出后就被删除。如果是工作排程资料(crontab)，就会被放置到/var/spool/cron/目录中！ |

### CentOS 系统信息查询 uname，lsb_release

```bash
#查看核心版本
uname -r

#查看系统位数
uname -m

#查看所有信息
uname -a

#安装redhat 红帽查询命令
yum install redhat-lsb
lsb_release -a
```

## 文件和目录管理命令

### 目录的操作 pwd，mkdir，rmdir

```bash
#此目录
.
#上级目录
..
#前一个目录
-
#家目录
~

#改变目录 cd
cd /tmp/

#显示绝对路径
pwd -P
-P	：显示绝对路径而不是连接

#新建目录
mkdir -mp /tmp/date/
-m	:设置权限
-p	:循环建立目录

#删除空目录
rmdir -p /tmp/date/
-P	:循环删除目录
```

### 环境变量，路径变量 $PATH

```bash
#命令查询的路径和顺序，环境变量
echo $PATH
/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin

#添加环境变量
PATH="${PATH}:/root"
```

### 查询文件命令 ls

```bash
ls [-aAdfFhilnrRSt] 档名或目录名称..
ls [--color={never,auto,always}] 档名或目录名称..
ls [--full-time] 档名或目录名称..

-a  ：全部的档案，连同隐藏档( 开头为 . 的档案) 一起列出来(常用)
-A  ：全部的档案，连同隐藏档，但不包括 . 与 .. 这两个目录
-d  ：仅列出目录本身，而不是列出目录内的档案资料(常用)
-f  ：直接列出结果，而不进行排序 (ls 预设会以档名排序！)
-F  ：根据档案、目录等资讯，给予附加资料结构，例如：
      *:代表可执行档； /:代表目录； =:代表 socket 档案； |:代表 FIFO 档案；
-h  ：将档案容量以人类较易读的方式(例如 GB, KB 等等)列出来；
-i  ：列出 inode 号码，inode 的意义下一章将会介绍；
-l  ：长资料串列出，包含档案的属性与权限等等资料；(常用)
-n  ：列出 UID 与 GID 而非使用者与群组的名称 (UID与GID会在帐号管理提到！)
-r  ：将排序结果反向输出，例如：原本档名由小到大，反向则为由大到小；
-R  ：连同子目录内容一起列出来，等于该目录下的所有档案都会显示出来；
-S  ：以档案容量大小排序，而不是用档名排序；
-t  ：依时间排序，而不是用档名。
--color=never  ：不要依据档案特性给予颜色显示；
--color=always ：显示颜色
--color=auto   ：让系统自行依据设定来判断是否给予颜色
--full-time    ：以完整时间模式 (包含年、月、日、时、分) 输出
--time={atime,ctime} ：输出 access 时间或改变权限属性时间 (ctime) 
                       而非内容变更时间 (modification time)
                       
#常用ls选项，按照时间来排序
ls -lrt
#按照文件大小来排序
ls -lSt
```

### 移动，复制，删除文件和目录 cp，rm，mv

* 复制cp

```bash
#复制命令
cp [-adfilprsu] 来源档(source) 目标档(destination)
cp [options] source1 source2 source3 .... directory

-a  ：相当于 -dr --preserve=all 的意思，至于 dr 请参考下列说明；(常用)
-d  ：若来源档为连结档的属性(link file)，则複製连结档属性而非档案本身；
-f  ：为强制(force)的意思，若目标档案已经存在且无法开启，则移除后再尝试一次；
-i  ：若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)
-l  ：进行硬式连结(hard link)的连结档建立，而非複製档案本身；
-p  ：连同档案的属性(权限、用户、时间)一起複製过去，而非使用预设属性(备份常用)；
-r  ：递迴持续複製，用于目录的複製行为；(常用)
-s  ：複製成为符号连结档 (symbolic link)，亦即『捷径』档案；
-u  ：destination 比 source 旧才更新 destination，或 destination 不存在的情况下才複製。
--preserve=all ：除了 -p 的权限相关参数外，还加入 SELinux 的属性, links, xattr 等也複製了。

#常用,强制覆盖，文件夹复制，连同属性一起复制
cp -r /src/ /dst/
cp -i /tmp/test.tar /dst/
cp -p /src/ /dst/
```

* 删除rm

```bash
#删除目录rm
rm [-fir] 档案或目录
-f  ：就是 force 的意思，忽略不存在的档案，不会出现警告讯息；
-i  ：互动模式，在删除前会询问使用者是否动作
-r  ：递迴删除啊！最常用在目录的删除了！这是非常危险的选项！！！

#常用，强制删除文件夹和文件
rm -rf file
```

* 移动，重命名命令mv

```bash
mv [-fiu] source destination
[root@study ~]# mv [options] source1 source2 source3 .... directory
选项与参数：
-f  ：force 强制的意思，如果目标档案已经存在，不会询问而直接覆盖；
-i  ：若目标档案 (destination) 已经存在时，就会询问是否覆盖！
-u  ：若目标档案已经存在，且 source 比较新，才会更新 (update)
```

### 获取文件名，获取路径名 basename，dirname

```bash
#从路径中获取文件的名字和路径的名字
basename /etc/sysconfig/network

dirname /etc/sysconfig/network
```

### 查询文本命令，查询文档命令 cat，tac，nl，more，less，head，tail，od

* cat

```bash
#cat命令
cat [-AbEnTv]
选项与参数：
-A  ：相当于 -vET 的整合选项，可列出一些特殊字符而不是空白而已；
-b  ：列出行号，仅针对非空白行做行号显示，空白行不标行号！
-E  ：将结尾的断行字元 $ 显示出来；
-n  ：列印出行号，连同空白行也会有行号，与 -b 的选项不同；
-T  ：将 [tab] 按键以 ^I 显示出来；
-v  ：列出一些看不出来的特殊字符

#常用命令，显示行号
cat -n file
```

* nl

```bash
nl [-bnw] 档案
选项与参数：
-b  ：指定行号指定的方式，主要有两种：
      -b a ：表示不论是否为空行，也同样列出行号(类似 cat -n)；
      -b t ：如果有空行，空的那一行不要列出行号(预设值)；
-n  ：列出行号表示的方法，主要有三种：
      -n ln ：行号在萤幕的最左方显示；
      -n rn ：行号在自己栏位的最右方显示，且不加 0 ；
      -n rz ：行号在自己栏位的最右方显示，且加 0 ；
-w  ：行号栏位的佔用的字元数。

#常用命令，不显示空行
nl -b t /etc/issue
```

* more

```bash
more /etc/man_db.conf
空白键 (space)：代表向下翻一页；
Enter         ：代表向下翻『一行』；
/字串         ：代表在这个显示的内容当中，向下搜寻『字串』这个关键字；
:f            ：立刻显示出档名以及目前显示的行数；
q             ：代表立刻离开 more ，不再显示该档案内容。
b 或 [ctrl]-b ：代表往回翻页，不过这动作只对档案有用，对管线无用。
```

* head

```bash
head [-n number] 档案 
选项与参数：
-n  ：后面接数字，代表显示几行的意思

#显示前100行
head -n 20 /etc/man_db.conf
```

* tail

```bash
tail [-n number] 档案 
选项与参数：
-n  ：后面接数字，代表显示几行的意思
-f  ：表示持续侦测后面所接的档名，要等到按下[ctrl]-c才会结束tail的侦测

#常用，显示后100行
tail -n 20 /etc/man_db.conf
#持续输出最后几行
tail -f /var/log/messages
```

### 修改文件时间，新建空文档 touch

#### mtime，ctime，atime

```bash
modification time (mtime)：
当该档案的『内容资料』变更时，就会更新这个时间！内容资料指的是档案的内容，而不是档案的属性或权限喔！

status time (ctime)：
当该档案的『状态 (status)』改变时，就会更新这个时间，举例来说，像是权限与属性被更改了，都会更新这个时间啊。

access time (atime)：
当『该档案的内容被取用』时，就会更新这个读取时间 (access)。举例来说，我们使用 cat 去读取 /etc/man_db.conf ， 就会更新该档案的 atime 了。
```

* touch

```bash
touch [-acdmt] 档案
选项与参数：
-a  ：仅修订 access time；
-c  ：仅修改档案的时间，若该档案不存在则不建立新档案；
-d  ：后面可以接欲修订的日期而不用目前的日期，也可以使用 --date="日期或时间"
-m  ：仅修改 mtime ；
-t  ：后面可以接欲修订的时间而不用目前的时间，格式为[YYYYMMDDhhmm]

#新建空文档
touch file
#修改文件时间
touch -d "2 days ago" bashrc
touch -t 201406150202 bashrc
```

### 设置文件预设权限 umask

```bash
#查询预设权限
umask
0022

umask -S
u=rwx,g=rx,o=rx

#设置权限，所属者和所属组有全部权限，其他人只有可写权限，r4，w2，x1
umask 002
```

### 查询文件类型 file

```bash
#file用法
file ~/.bashrc
/root/.bashrc: ASCII text
```

### 查找命令 which

```bash
#which用法
which -a command
-a	：查找所有路径下的command
```

### 文件名查找 whereis，locate，find

* whereis

```bash
#whereis用法，只在特定目录查找
whereis [-bmsu] 档案或目录名
选项与参数：
-l    :可以列出 whereis 会去查询的几个主要目录而已
-b    :只找 binary 格式的档案
-m    :只找在说明档 manual 路径下的档案
-s    :只找 source 来源档案
-u    :搜寻不在上述三个项目当中的其他特殊档案

#查找python
whereis python
```

* locate

```bash
#locate用法
locate [-ir] keyword
选项与参数：
-i  ：忽略大小写的差异；
-c  ：不输出档名，仅计算找到的档案数量
-l  ：仅输出几行的意思，例如输出五行则是 -l 5
-S  ：输出 locate 所使用的资料库档案的相关资讯，包括该资料库纪录的档案/目录数量等
-r  ：后面可接正规表示法的显示方式

#只显示5个结果
locate -l 5 passwd
#列出locate使用的数据库
locate -S
```

* find

```bash
find [PATH] [option] [action]
选项与参数：
1. 与时间有关的选项：共有 -atime, -ctime 与 -mtime ，以 -mtime 说明
   -mtime  n ：n 为数字，意义为在 n 天之前的『一天之内』被更动过内容的档案；
   -mtime +n ：列出在 n 天之前(不含 n 天本身)被更动过内容的档案档名；
   -mtime -n ：列出在 n 天之内(含 n 天本身)被更动过内容的档案档名。
   -newer file ：file 为一个存在的档案，列出比 file 还要新的档案档名

#按照名称查找
find / -name passwd

#查找包含
find / -name "*passwd*"

#查询24小时之内变过的文件
find / -mtime 0

#查出比某个文件新的文件
find /etc -newer /etc/passwd

#找出属于某用户的文件
find /home -user dmtsai

#找出不属于任何人的文件
find / -nouser

#查找大于某个大小的文件
find / -size +1M
```

## 磁盘信息查询命令 df，du

* df

```bash
#显示整体文件系统的磁盘使用情况
df [-ahikHTm] [目录或档名]
选项与参数：
-a  ：列出所有的档案系统，包括系统特有的 /proc 等档案系统；
-k  ：以 KBytes 的容量显示各档案系统；
-m  ：以 MBytes 的容量显示各档案系统；
-h  ：以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示；
-H  ：以 M=1000K 取代 M=1024K 的进位方式；
-T  ：连同该 partition 的 filesystem 名称 (例如 xfs) 也列出；
-i  ：不用磁碟容量，而以 inode 的数量来显示

#按照人类可读输出显示
df -h

#输出解释
Filesystem：代表该档案系统是在哪个 partition ，所以列出装置名称；
1k-blocks：说明底下的数字单位是 1KB 呦！可利用 -h 或 -m 来改变容量；
Used：顾名思义，就是使用掉的磁碟空间啦！
Available：也就是剩下的磁碟空间大小；
Use%：就是磁碟的使用率啦！如果使用率高达 90% 以上时， 最好需要注意一下了，免得容量不足造成系统问题喔！(例如最容易被灌爆的 /var/spool/mail 这个放置邮件的磁碟)
Mounted on：就是磁碟挂载的目录所在啦！(挂载点啦！)
```

* du

```bash
#显示文件系统扥使用量，目录剩余空间
du [-ahskm] 档案或目录名称
选项与参数：
-a  ：列出所有的档案与目录容量，因为预设仅统计目录底下的档案量而已。
-h  ：以人们较易读的容量格式 (G/M) 显示；
-s  ：列出总量而已，而不列出每个各别的目录佔用容量；
-S  ：不包括子目录下的总计，与 -s 有点差别。
-k  ：以 KBytes 列出容量显示；
-m  ：以 MBytes 列出容量显示；

#显示当前目录所有文件
du -a
#显示某个目录下的所有文件夹容量
du -sm /*
```

### 硬链接和软件类 ln

```bash
#硬链接，指向一个inode号码，删除一个硬链接文件并不影响其他有相同 inode 号的文件
ln old new
#软链接，删除软链接并不影响被指向的文件，但若被指向的原文件被删除，则相关软连接就变成了死链接。
#软链接可对不存在的文件或目录创建软链接；可交叉文件系统
ln -s old new
```

![img](http://img.hi-cat.cn/54ceb38017aa43253724fb275448123c)

### 挂载 mount

```bash
mount -a
[root@study ~]# mount [-l]
[root@study ~]# mount [-t 档案系统] LABEL=''  挂载点
[root@study ~]# mount [-t 档案系统] UUID=''   挂载点  # 鸟哥近期建议用这种方式喔！
[root@study ~]# mount [-t 档案系统] 装置档名  挂载点
选项与参数：
-a  ：依照设定档 /etc/fstab 的资料将所有未挂载的磁碟都挂载上来
-l  ：单纯的输入 mount 会显示目前挂载的资讯。加上 -l 可增列 Label 名称！
-t  ：可以加上档案系统种类来指定欲挂载的类型。常见的 Linux 支援类型有：xfs, ext3, ext4,
      reiserfs, vfat, iso9660(光碟格式), nfs, cifs, smbfs (后三种为网路档案系统类型)
-n  ：在预设的情况下，系统会将实际挂载的情况即时写入 /etc/mtab 中，以利其他程式的运作。
      但在某些情况下(例如单人维护模式)为了避免问题会刻意不写入。此时就得要使用 -n 选项。
-o  ：后面可以接一些挂载时额外加上的参数！比方说帐号、密码、读写权限等：
      async, sync:   此档案系统是否使用同步写入 (sync) 或非同步 (async) 的
                     记忆体机制，请参考档案系统运作方式。预设为 async。
      atime,noatime: 是否修订档案的读取时间(atime)。为了效能，某些时刻可使用 noatime
      ro, rw:        挂载档案系统成为唯读(ro) 或可读写(rw)
      auto, noauto:  允许此 filesystem 被以 mount -a 自动挂载(auto)
      dev, nodev:    是否允许此 filesystem 上，可建立装置档案？ dev 为可允许
      suid, nosuid:  是否允许此 filesystem 含有 suid/sgid 的档案格式？
      exec, noexec:  是否允许此 filesystem 上拥有可执行 binary 档案？
      user, nouser:  是否允许此 filesystem 让任何使用者执行 mount ？一般来说，
                     mount 仅有 root 可以进行，但下达 user 参数，则可让
                     一般 user 也能够对此 partition 进行 mount 。
      defaults:      预设值为：rw, suid, dev, exec, auto, nouser, and async
      remount:       重新挂载，这在系统出错，或重新更新参数时，很有用！
      
#单一档案系统不应该被重複挂载在不同的挂载点(目录)中；
#单一目录不应该重複挂载多个档案系统；
#要作为挂载点的目录，理论上应该都是空目录才是。
```

## 文件压缩，打包命令

* 常见格式

```bash
*.Z         compress 程式压缩的档案；
*.zip       zip 程式压缩的档案；
*.gz        gzip 程式压缩的档案；
*.bz2       bzip2 程式压缩的档案；
*.xz        xz 程式压缩的档案；
*.tar       tar 程式打包的资料，并没有压缩过；
*.tar.gz    tar 程式打包的档案，其中并且经过 gzip 的压缩
*.tar.bz2   tar 程式打包的档案，其中并且经过 bzip2 的压缩
*.tar.xz    tar 程式打包的档案，其中并且经过 xz 的压缩
```