---
title: linux下使用screen或则nohup将任务放到后台运行
tags:
  - nohup
  - screen
id: '555'
categories:
  - - 转载
abbrlink: 12937
date: 2019-10-27 19:14:38
---

1、简介  
Screen是一款由GNU计划开发的用于命令行终端切换的自由软件。用户可以通过该软件同时连接多个本地或远程的命令行会话，并在其间自由切换。GNU Screen可以看作是窗口管理器的命令行界面版本。它提供了统一的管理多个会话的界面和相应的功能。

在Screen环境下，所有的会话都独立的运行，并拥有各自的编号、输入、输出和窗口缓存。用户可以通过快捷键在不同的窗口下切换，并可以自由的重定向各个窗口的输入和输出。

2、语法  
$> screen \[-AmRvx -ls -wipe\]\[-d <作业名称>\]\[-h <行数>\]\[-r <作业名称>\]\[-s \]\[-S <作业名称>\]

\-A 　将所有的视窗都调整为目前终端机的大小。  
\-d <作业名称> 　将指定的screen作业离线。  
\-h <行数> 　指定视窗的缓冲区行数。  
\-m 　即使目前已在作业中的screen作业，仍强制建立新的screen作业。  
\-r <作业名称> 　恢复离线的screen作业。  
\-R 　先试图恢复离线的作业。若找不到离线的作业，即建立新的screen作业。  
\-s 　指定建立新视窗时，所要执行的shell。  
\-S <作业名称> 　指定screen作业的名称。  
\-v 　显示版本信息。  
\-x 　恢复之前离线的screen作业。  
\-ls或--list 　显示目前所有的screen作业。  
\-wipe 　检查目前所有的screen作业，并删除已经无法使用的screen作业。  
3、常用screen参数  
screen -S yourname -> 新建一个叫yourname的session  
screen -ls -> 列出当前所有的session  
screen -r yourname -> 回到yourname这个session  
screen -d yourname -> 远程detach某个session  
screen -d -r yourname -> 结束当前session并回到yourname这个session  
4、在Session下，使用ctrl+a(C-a)   
C-a ? -> 显示所有键绑定信息  
C-a c -> 创建一个新的运行shell的窗口并切换到该窗口  
C-a n -> Next，切换到下一个 window  
C-a p -> Previous，切换到前一个 window  
C-a 0..9 -> 切换到第 0..9 个 window  
Ctrl+a \[Space\] -> 由视窗0循序切换到视窗9  
C-a C-a -> 在两个最近使用的 window 间切换  
C-a x -> 锁住当前的 window，需用用户密码解锁  
C-a d -> detach，暂时离开当前session，将目前的 screen session (可能含有多个 windows) 丢到后台执行，并会回到还没进 screen 时的状态，此时在 screen session 里，每个 window 内运行的 process (无论是前台/后台)都在继续执行，即使 logout 也不影响。  
C-a z -> 把当前session放到后台执行，用 shell 的 fg 命令则可回去。  
C-a w -> 显示所有窗口列表  
C-a t -> time，显示当前时间，和系统的 load  
C-a k -> kill window，强行关闭当前的 window  
C-a \[ -> 进入 copy mode，在 copy mode 下可以回滚、搜索、复制就像用使用 vi 一样  
C-b Backward，PageUp  
C-f Forward，PageDown  
H(大写) High，将光标移至左上角  
L Low，将光标移至左下角  
0 移到行首  
$ 行末  
w forward one word，以字为单位往前移  
b backward one word，以字为单位往后移  
Space 第一次按为标记区起点，第二次按为终点  
Esc 结束 copy mode  
C-a \] -> paste，把刚刚在 copy mode 选定的内容贴上  
5、常用操作  
创建会话（-m 强制）：

screen -dmS session\_name

# session\_name session名称

关闭会话：

screen -X -S \[session # you want to kill\] quit  
查看所有会话：

screen -ls  
进入会话：

screen -r session\_name

## 如何安装 screen

screen 在一些流行的发行版上已经预安装了。你可以使用下面的命令检查是否已经在你的服务器上安装了。

```
screen -v 
Screen version 4.00.03 (FAU)  
```

如果在 Linux 中还没有 screen，你可以使用系统提供的包管理器很简单地安装它。

CentOS/RedHat/Fedora

```
yum -y install screen 
```

Ubuntu/Debian

```
apt-get -y install screen
```

## 1、screen使用举例

1.1、创建一个新screen窗口

```
[root@imzcy ~]# screen -S jboss-blog
[root@imzcy ~]# java -jar blog-imzcy.jar
[root@imzcy ~]# 
```

1.2、使用Ctrl + A + D组合键退出screen窗口

```
[detached from 9832.jboss-blog]
[root@localhost ~]# 
```

1.3、列出已经创建的screen窗口

```
[root@localhost ~]# screen -ls
There is a screen on:
        9832.jboss-blog (Detached)
1 Socket in /var/run/screen/S-root.
 
[root@imzcy ~]# 
```

1.4、恢复到指定的screen窗口

```
[root@imzcy ~]# screen -r jboss-blog
[root@imzcy ~]# screen -r 9832
```

1.5、退出并删除当前screen窗口

```
screen -S session_name -X quit
```

```
[root@localhost ~]# exit
exit
 
[screen is terminating]
[root@localhost ~]# screen -ls
No Sockets found in /var/run/screen/S-root.
 
[root@localhost ~]# 
```

常用选项：  
**\-ls** ：列出所有screen窗口  
**\-S** ：自定义一个名称，创建一个新的screen窗口并自动切换进去  
**\-r** ：进入到指定screen窗口，可以指定窗口名称或窗口ID  
**\-d** ：将screen窗口与在连接的会话分离并重新连接到当前会话  
**Ctrl + A + D** ：退出当前screen窗口  
**exit** ：退出并删除当前的screen窗口

#### screen使用中常见问题之：There is no screen to be resumed matching jboss-blog.

  正常创建的screen窗口没有被使用的话，使用screen -ls查出来的状态是(Detached); 如果你创建的screen窗口被其他连接到当前服务器的用户使用了，那么查出来的状态是(Attached)。这个时候你直接使用screen -r 窗口名称是进不去该窗口的。必须先使用-d选项将screen窗口和对方的会话断开。才能继续使用。

```
[root@imzcy ~]# screen -ls
There is a screen on:
        9849.jboss-blog (Attached)        //可以看到当前状态是Attached，说明可能正在被其他人使用
1 Socket in /var/run/screen/S-root.
 
[root@imzcy ~]# 
[root@imzcy ~]# screen -r jboss-blog        //这个使用直接使用screen -r是进入不到这个窗口的，会报下面的错误
There is a screen on:
        9849.jboss-blog (Attached)
There is no screen to be resumed matching jboss-blog.
[root@imzcy ~]# 
 
 
[root@imzcy ~]# screen -d jboss-blog        //使用screen -d将制定screen窗口与会话分离
[9849.jboss-blog detached.]
[root@imzcy ~]# 
[root@imzcy ~]# screen -r jboss-blog        //这个是后就可以使用screen -r进入窗口了
 
 
 
[remote detached from 9849.jboss-blog]        //对方那边会提示screen窗口被远程分离了
[root@imzcy ~]# 
```

## 2、nohup使用举例

将jar包放到后台运行，并且重定向其错误输出和标准输出到当前目录下的blog.log文件中

```
[root@imzcy ~]# nohup java -jar blog-imzcy.jar >blog.log 2>&1 &
[1] 9900
[root@imzcy ~]#
 
使用tail -f查看运行状态
[root@imzcy ~]# tail -f blog.log 
 
查看nohup运行jar包的进程号
[root@imzcy ~]# ps -ef grep "\-jar"
```

参考原文链接：[Linux中的screen命令使用](https://blog.csdn.net/han0373/article/details/81352663)

[linux下使用screen或则nohup将任务放到后台运行](https://www.imzcy.cn/1258.html)