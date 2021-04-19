---
title: Simple Torrent：一个支持边下边播、无版权限制和自动上传的BT离线下载程序
tags:
  - Simple Torrent
id: '639'
categories:
  - - 分享
abbrlink: 53733
date: 2020-02-15 13:57:58
---

\[toc\]

![simple_torrent1](https://www.gitiu.com/wp-content/uploads/2020/02/simple_torrent1.png)

![simple_torrent2](https://www.gitiu.com/wp-content/uploads/2020/02/simple_torrent2.png)

## 安装

使用`SSH`客户端登录服务器，运行命令：

```
bash <(wget -qO- https://raw.githubusercontent.com/boypt/simple-torrent/master/scripts/quickinstall.sh)
```

然后使用`ip:3000`访问即可。

顺便提供个博主经常用的`BT-Trackers`服务器地址，效果不错，如下：

```
https://trackerslist.com/all.txt
```

直接在`Web`界面修改即可。

相关命令：

```
启动：systemctl start cloud-torrent
重启：systemctl restart cloud-torrent
停止：systemctl stop cloud-torrent
查看状态：systemctl status cloud-torrent
```

## Docker安装

**1、安装Docker**

```
#CentOS 6系统
rpm -iUvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
yum update -y
yum -y install docker-io
service docker start
chkconfig docker on

#CentOS 7、Debian、Ubuntu系统
curl -sSL https://get.docker.com/  sh
systemctl start docker
systemctl enable docker
```

**2、安装Simple Torrent**

```
docker run --restart=always --name simple-torrent -d \
-p 3000:3000 \
-v ~/downloads:/downloads \
-v ~/torrents:/torrents \
boypt/cloud-torrent
```

然后使用`ip:3000`访问即可。

最后如果你访问不了`Web`端，可能要检查下防火墙端口，有安全组的也要放行下相关端口。

这里提供个`CentOS`系统防火墙开启命令，大致如下：

```
#CentOS 6
iptables -I INPUT -p tcp --dport 3000 -j ACCEPT
service iptables save
service iptables restart

#CentOS 7
firewall-cmd --zone=public --add-port=3000/tcp --permanent
firewall-cmd --reload
```

## API使用

关于`API`的用法，官方文档说的很详细了，这里就大概列举几个，如下：

```
#通过远程地址添加种子
curl --data "http://domain.com/file.torrent" "http://localhost:3000/api/url"
#通过本地文件添加种子
curl --data-binary "my.torrent" "http://localhost:3000/api/url"
#通过磁力链接添加种子
curl --data "magnet:?xt=urn:btih:..." "http://localhost:3000/api/url"

#开始种子任务
curl --data "start:${HASH}" "http://localhost:3000/api/torrent"
#停止种子任务
curl --data "stop:${HASH}" "http://localhost:3000/api/torrent"
#删除种子任务
curl --data "delete:${HASH}" "http://localhost:3000/api/torrent"

#查看文件和种子信息
/api/files和/api/torrents
```

## 外部程序调用

先修改配置文件，通过上面脚本安装的配置文件在你的主目录，比如`/root`目录，配置文件`cloud-torrent.json`。

修改以下参数：

```
#外部程序调用参数
"donecmd": "",

#比如我要下载完成后，直接运行/home目录下的rats.sh脚本
"donecmd": "/home/rats.sh",
```

那么下载完成后就会运行该脚本。

一般种子下载完成后，会返回以下参数变量，这里列举下主要的：

```
CLD_DIR为下载路径，且为绝对路径
CLD_PATH为下载文件名称
CLD_SIZE为文件大小
CLD_TYPE为调用事件类型，分为files和torrent，分别为种子里单个文件和整体文件
CLD_HASH为文件HASH值
```

这里随便放一个下载后自动移动的脚本，针对`rclone`挂载的文件夹。

```
#!/bin/bash

#下载后移动的文件夹路径
RemoteDIR="/down/moerats";  

if [[ ${CLD_TYPE} == "torrent" ]]; then
eval mv \'"${CLD_DIR}/${CLD_PATH}"\' "${RemoteDIR}";
#移动后停止该任务
curl --data "stop:${CLD_HASH}" "http://127.0.0.1:3000/api/torrent";
#停止后清除该任务，也就是不会出现在Web界面了
curl --data "delete:${CLD_HASH}" "http://127.0.0.1:3000/api/torrent";
fi
```

这里还可以结合`TG`机器人啥的一起使用，玩法很多，可以自行结合`API`一起使用。

要注意的是，配置调用脚本的时候，需要给予脚本可执行权，并重启程序生效，比如：

```
#给予可执行权，脚本路径/root/rats.sh
chmod +x /root/rats.sh
#重启程序
systemctl restart cloud-torrent
```

> 转 本文链接：[https://www.moerats.com/archives/1023/](https://www.moerats.com/archives/1023/)