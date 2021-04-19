---
title: 多种功能强大的BT离线下载程序Docker镜像及安装
tags:
  - BT离线下载、
  - Docker镜像
id: '623'
categories:
  - - 转载
abbrlink: 44574
date: 2020-01-13 14:12:28
---

## 安装Docker

首先安装下面程序之前，需要在服务器上安装`Docker`环境，使用命令：

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

## 安装Aria2

**镜像来源：**[https://hub.docker.com/r/onisuly/aria2-with-webui](https://hub.docker.com/r/onisuly/aria2-with-webui)

先安装`Docker`，然后执行以下命令：

```
docker run --restart=always --name aria2-ariang -d \
-p 6060:80 \
-p 6800:6800 \
-e SECRET=moerats \
-v ~/aria2/down:/data \
-v ~/aria2/conf:/conf \
onisuly/aria2-with-webui
```

安装完成后，相关信息如下：

```
AriaNg地址：http://ip:6060
aria2连接端口：6800
aria2连接密匙：moerats
下载/配置目录：~/aria2
```

`CentOS`系统安装后，可能还需要开启相应的端口，大致如下：

```
#CentOS 6
iptables -I INPUT -p tcp --dport 6060 -j ACCEPT
iptables -A INPUT -p tcp --dport 6800 -j ACCEPT
service iptables save
service iptables restart

#CentOS 7
firewall-cmd --zone=public --add-port=6060/tcp --permanent
firewall-cmd --zone=public --add-port=6800/tcp --permanent
firewall-cmd --reload
```

如果你不想用了，可以使用以下命令卸载：

```
#删掉容器
ContainerID=`docker psgrep onisuly/aria2-with-webuiawk '{print $1}'`
docker kill ${ContainerID}
docker rm ${ContainerID}
docker rmi `docker imagesgrep onisuly/aria2-with-webuiawk '{print $3}'`
#删掉下载文件夹
rm -rf ~/aria2
```

## 安装utorrent

**镜像来源：**[https://hub.docker.com/r/ekho/utorrent](https://hub.docker.com/r/ekho/utorrent)

先安装`Docker`，然后执行以下命令：

```
docker run --restart=always --name utorrent -d \
-p 8080:8080 \
-p 6881:6881 \
-v ~/utorrent:/utorrent/data \
ekho/utorrent
```

安装完成后，相关信息如下：

```
utorrent地址：http://ip:8080/gui
访问用户名：admin
访问密码：为空
下载目录：~/utorrent
```

`CentOS`系统安装后，可能还需要开启相应的端口，大致如下：

```
#CentOS 6
iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
iptables -A INPUT -p tcp --dport 6881 -j ACCEPT
service iptables save
service iptables restart

#CentOS 7
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --zone=public --add-port=6881/tcp --permanent
firewall-cmd --reload
```

如果你不想用了，可以使用以下命令卸载：

```
#删掉容器
ContainerID=`docker psgrep ekho/utorrentawk '{print $1}'`
docker kill ${ContainerID}
docker rm ${ContainerID}
docker rmi `docker imagesgrep ekho/utorrentawk '{print $3}'`
#删掉下载文件夹
rm -rf ~/utorrent
```

## 安装Deluge

**镜像来源：**[https://hub.docker.com/r/linuxserver/deluge](https://hub.docker.com/r/linuxserver/deluge)

先安装`Docker`，然后执行以下命令：

```
docker run --restart=always --name deluge -d \
--net=host \
-v ~/deluge/config:/config \
-v ~/deluge/downloads:/downloads \
linuxserver/deluge
```

安装完成后，相关信息如下：

```
deluge地址：http://ip:8112
访问密码：deluge
配置/下载目录：~/deluge
```

`CentOS`系统安装后，可能还需要开启相应的端口，大致如下：

```
#CentOS 6
iptables -I INPUT -p tcp --dport 8112 -j ACCEPT
service iptables save
service iptables restart

#CentOS 7
firewall-cmd --zone=public --add-port=8112/tcp --permanent
firewall-cmd --reload
```

进入界面后，记得点击上方的`Preferences`，将下载目录设置为`/downloads`。

如果你不想用了，可以使用以下命令卸载：

```
#删掉容器
ContainerID=`docker psgrep linuxserver/delugeawk '{print $1}'`
docker kill ${ContainerID}
docker rm ${ContainerID}
docker rmi `docker imagesgrep linuxserver/delugeawk '{print $3}'`
#删掉下载文件夹
rm -rf ~/deluge
```

## 安装Transmission

**镜像来源：**[https://hub.docker.com/r/linuxserver/transmission](https://hub.docker.com/r/linuxserver/transmission)

先安装`Docker`，然后执行以下命令：

```
docker run --restart=always --name transmission -d \
-e TRANSMISSION_WEB_HOME=/transmission-web-control/ \
-e USER=moerats \
-e PASS=moerats \
-p 9091:9091 \
-p 51413:51413 \
-p 51413:51413/udp \
-v ~/transmission/config:/config \
-v ~/transmission/downloads:/downloads \
-v ~/transmission/watch:/watch \
linuxserver/transmission
```

安装完成后，相关信息如下：

```
transmission地址：http://ip:9091
访问用户名：moerats
访问密码：moerats
配置/下载目录：~/transmission
```

`CentOS`系统安装后，可能还需要开启相应的端口，大致如下：

```
#CentOS 6
iptables -I INPUT -p tcp --dport 9091 -j ACCEPT
iptables -A INPUT -p tcp --dport 51413 -j ACCEPT
iptables -A INPUT -p udp --dport 51413 -j ACCEPT
service iptables save
service iptables restart

#CentOS 7
firewall-cmd --zone=public --add-port=9091/tcp --permanent
firewall-cmd --zone=public --add-port=51413/tcp --permanent
firewall-cmd --zone=public --add-port=51413/udp --permanent
firewall-cmd --reload
```

如果你不想用了，可以使用以下命令卸载：

```
#删掉容器
ContainerID=`docker psgrep linuxserver/transmissionawk '{print $1}'`
docker kill ${ContainerID}
docker rm ${ContainerID}
docker rmi `docker imagesgrep linuxserver/transmissionawk '{print $3}'`
#删掉下载文件夹
rm -rf ~/transmission
```

## 安装Rutorrent

**镜像来源：**[https://hub.docker.com/r/linuxserver/rutorrent](https://hub.docker.com/r/linuxserver/rutorrent)

先安装`Docker`，然后执行以下命令：

```
docker run --restart=always --name rutorrent -d \
-p 2222:80 \
-p 5000:5000 \
-p 51413:51413 \
-p 6881:6881/udp \
-v ~/rutorrent/config:/config \
-v ~/rutorrent/downloads:/downloads \
linuxserver/rutorrent
```

安装完成后，相关信息如下：

```
rutorrent地址：http://ip:2222
配置/下载目录：~/rutorrent
```

`CentOS`系统安装后，可能还需要开启相应的端口，大致如下：

```
#CentOS 6
iptables -I INPUT -p tcp --dport 2222 -j ACCEPT
iptables -A INPUT -p tcp --dport 5000 -j ACCEPT
iptables -A INPUT -p tcp --dport 51413 -j ACCEPT
iptables -A INPUT -p udp --dport 6881 -j ACCEPT
service iptables save
service iptables restart

#CentOS 7
firewall-cmd --zone=public --add-port=2222/tcp --permanent
firewall-cmd --zone=public --add-port=5000/tcp --permanent
firewall-cmd --zone=public --add-port=51413/tcp --permanent
firewall-cmd --zone=public --add-port=6881/udp --permanent
firewall-cmd --reload
```

如果你不想用了，可以使用以下命令卸载：

```
#删掉容器
ContainerID=`docker psgrep linuxserver/rutorrentawk '{print $1}'`
docker kill ${ContainerID}
docker rm ${ContainerID}
docker rmi `docker imagesgrep linuxserver/rutorrentawk '{print $3}'`
#删掉下载文件夹
rm -rf ~/rutorrent
```

## 安装Qbittorrent

**镜像来源：**[https://hub.docker.com/r/linuxserver/qbittorrent](https://hub.docker.com/r/linuxserver/qbittorrent)

先安装`Docker`，然后执行以下命令：

```
docker run --restart=always --name qbittorrent -d \
-p 6881:6881 \
-p 6881:6881/udp \
-p 8080:8080 \
-v ~/qbittorrent/config:/config \
-v ~/qbittorrent/downloads:/downloads \
linuxserver/qbittorrent
```

安装完成后，相关信息如下：

```
qbittorrent地址：http://ip:6666
用户名：admin
密码：adminadmin
配置和/下载目录：~/qbittorrent
```

`CentOS`系统安装后，可能还需要开启相应的端口，大致如下：

```
#CentOS 6
iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
iptables -A INPUT -p tcp --dport 6881 -j ACCEPT
iptables -A INPUT -p udp --dport 6881 -j ACCEPT
service iptables save
service iptables restart

#CentOS 7
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --zone=public --add-port=6881/tcp --permanent
firewall-cmd --zone=public --add-port=6881/udp --permanent
firewall-cmd --reload
```

如果你不想用了，可以使用以下命令卸载：

```
#删掉容器
ContainerID=`docker psgrep linuxserver/qbittorrentawk '{print $1}'`
docker kill ${ContainerID}
docker rm ${ContainerID}
docker rmi `docker imagesgrep linuxserver/qbittorrentawk '{print $3}'`
#删掉下载文件夹
rm -rf ~/qbittorrent
```

这里顺便推荐个磁力链接聚合搜索`magnetW`，有兴趣的可以下载`Windows/Mac`端应用程序，下载地址→[传送门](https://magnetw.app/guide/install.html)。

最后这里只列举简单的安装，更深层次的可以访问镜像地址使用，如果还有其它好用没有列举的，可以留言提下。

* * *

> 本文链接：[https://www.moerats.com/archives/1015/](https://www.moerats.com/archives/1015/)