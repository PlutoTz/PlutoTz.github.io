---
title: 超详细的NetData-轻量的性能监控工具安装教程
tags:
  - NetData
  - 性能监控工具
id: '1202'
categories:
  - - 日志
abbrlink: 19026
date: 2020-04-01 14:47:14
---

## 前言

对于服务器监控我相信对于一些人来说是感兴趣的，相比于繁琐的 [Grafana + Zabbix](https://2heng.xin/2019/09/10/docker-zabbix-grafana/)（白猫的docker安装教程）对于服务器资源的占用，较为轻便监控之全面的[NetData](https://www.netdata.cloud/)，它的优势就显现出来了。还有之前部署过 [uptime-status一款漂亮的网站监控面板](https://www.gitiu.com/share/uptime-status/) 就开始显得单一，不过后者对多站点多服务器更加友好。

https://www.gitiu.com/share/uptime-status/

## 介绍

**NetData** 是一个用于系统和应用的分布式实时性能和健康监控工具。它提供了对系统中实时发生的所有事情的全面检测。你可以在高度互动的 Web 仪表板中查看结果。使用 Netdata，你可以清楚地了解现在发生的事情，以及之前系统和应用中发生的事情。你无需成为专家即可在 Linux 系统中部署此工具。NetData 开箱即用，零配置、零依赖。只需安装它然后坐等，之后 NetData 将负责其余部分。

它有自己的内置 Web 服务器，以图形形式显示结果。NetData 非常快速高效，安装后可立即开始分析系统性能。它是用 C 编程语言编写的，所以它非常轻量。它占用的单核 CPU 使用率不到 3％，内存占用 10-15MB。我们可以轻松地在任何现有网页上嵌入图表，并且它还有一个插件 API，以便你可以监控任何应用。

### 特点

![](https://cdn.jsdelivr.net/gh/plutotz/cdn/picturebed/NETDATA.gif)

*   interactive bootstrap dashboards, 酷炫
*   所有请求每个metreic都在0.5ms内响应，即便是一台烂机器
*   非常高效，每秒采集数千个指标，但仅占cpu单核1%，少量MB的内存以及完全没有磁盘IO
*   提供复杂的、各种类型的告警，支持动态阈值、告警模板、多种通知方式等
*   可扩展，使用自带的插件API（比如bash, python, perl, node.js, java, go, ruby等）来收集任何可以衡量的数据
*   零配置：安装后netdata会自动的监测一切
*   零依赖：netdata有自己的web server， 提供静态web文件和web API 零维护：只管跑上！
*   支撑多种时间序列后端服务，比如graphite, opentsdb, prometheus, json document DBs

### NetData在Linux中的监控列表

*   CPU 使用率
*   RAM 使用率
*   交换内存使用率
*   内核内存使用率
*   硬盘及其使用率
*   网络接口
*   IPtables
*   Netfilter
*   DDoS 保护
*   进程
*   应用
*   NFS 服务器
*   Web 服务器 （Apache 和 Nginx）
*   数据库服务器 （MySQL），
*   DHCP 服务器
*   DNS 服务器
*   电子邮件服务
*   代理服务器
*   Tomcat
*   PHP
*   SNP 设备

## 开始部署

当然有最简单的安装 Netdata 的方法 ，那就是一键脚本适用于任何LInux发行版系统中。

```
bash <(curl -Ss https://my-netdata.io/kickstart-static64.sh)
```

![](https://image.gitiu.com/2020/04/01/9aba51a646853.png)

输入脚本命令

![](https://image.gitiu.com/2020/04/01/692a02045f3ee.png)

回车输入 y 确定安装

### 通过 Web 浏览器访问 NetData

打开 Web 浏览器，然后打开 `http://127.0.0.1:19999` 

或者 `http://localhost:19999/` 

或者 `http://ip-address:19999`

#### **若不能访问请开放19999端口**

```
在 Ubuntu/Debian 中： sudo ufw allow 19999

在 CentOS中： sudo firewall-cmd --permanent --add-port=19999/tcp
             sudo firewall-cmd --reload
```

当然有些用户可能不想在没有研究的情况下将某些东西直接注入到 Bash。如果你不喜欢此方法，可以按照以下步骤在系统上安装它

## 系统环境：

**Centos7**

## 下载安装netData

```
# 下载项目代码
 git clone https://github.com/firehol/netdata.git
# 安装变异所需要的包
 yum -y install zlib-devel libuuid-devel libmnl-devel gcc make git autoconf autogen automake pkgconfig
# 运行自带的安装启动脚本
 cd ./netdata
 ./netdata-installer.sh
```

安装启动脚本时，提示netData安装的详细目录，按下Enter键执行。

![](https://cdn.jsdelivr.net/gh/plutotz/cdn/picturebed/6662092-5ad085a1b6f4c081.png)

安装启动脚本

## 启动和配置

安装完成后，脚本输出一段信息，包括：KSM、端口、启动命令

### 开启 KSM 以节省储存占用

如果有下列信息，说明你的系统有 KSM，但是未启用，可以按照说明执行两句echo命令，节省 40-60% 的储存空间。

```
 --- Check KSM (kernel memory deduper) ---

Memory de-duplication instructions

You have kernel memory de-duper (called Kernel Same-page Merging,
or KSM) available, but it is not currently enabled.

To enable it run:

    echo 1 >/sys/kernel/mm/ksm/run
    echo 1000 >/sys/kernel/mm/ksm/sleep_millisecs

If you enable it, you will save 40-60% of netdata memory.
```

### web端口配置

默认的web访问端口为19999。

```
netdata by default listens on all IPs on port 19999,
so you can access it with:

  http://this.machine.ip:19999/
```

1.  如果修改端口，需要编辑配置文件`/etc/netdata/netdata.conf` 中的 `# default port = 19999`。去掉注释符号`#`，`端口尽量改掉默认的19999 !!!`
2.  修改端口后重启生效。
3.  如果有防火墙，需开放端口。

![](https://cdn.jsdelivr.net/gh/plutotz/cdn/picturebed/6662092-782456b08e0f4c1f.png)

修改web端口

### **对于 Ubuntu / Debian**

```
安装所需的依赖项：
 sudo apt-get install zlib1g-dev uuid-dev libuv1-dev liblz4-dev libjudy-dev libssl-dev libmnl-dev gcc make git autoconf autoconf-archive autogen automake pkg-config curl

Git 克隆 NetData 仓库： git clone https://github.com/netdata/netdata.git --depth=100

PS:上面的命令将会在当前工作目录中创建一个名为 netdata 的目录

切换到 netdata 目录： cd netdata/

使用命令安装并启动 NetData： sudo ./netdata-installer.sh
```

最后会输出：

![](https://cdn.gitiu.com/wp-content/uploads/2020/04/1585722669-090939ja4ibrtzpzp4oaqp.png)

## 最后的补充

### 启动／关闭netData

```
# 停止
systemctl stop netdata
# 启动
systemctl start netdata
# 重启
 systemctl restart netdata
# 开机启动
 systemctl enable netdata
```

### 卸载

```
# 卸载
./netdata-uninstaller.sh --force
或者sudo ./netdata-uninstaller.sh --force
```

可以随时打开 `http://localhost:19999/netdata.conf` 来下载和/或查看 NetData 默认配置文件。

![](https://cdn.gitiu.com/wp-content/uploads/2020/04/1585722669-090943m3ltlwzybhycltlu.png)

_Netdata 配置文件_

### 更新 NetData

```
切换到 netdata 目录：$ cd netdata
拉取最新更新：$ git pull
使用命令重新构建并更新它：$ sudo ./netdata-installer.sh
```

考虑到安全个人使用的私密性，对于netdata没有帐号密码体系，为保护服务器隐私，我们要使用nginx反向代理配置域名访问，并使用账号密码授权

### 使用Nginx配置域名访问，设置账号密码授权

准备：

*   如果服务器没有Nginx，安装: `yum install nginx`
*   netdata的域名，如: `netdata.example.com`

对于域名的绑定可以使用caddy或者是[宝塔面板](http://bt.cn)反向代理

可以参考：[利用 Caddy 轻松实现反向代理/镜像（支持自签SSL证书）](https://51.ruyo.net/3461.html)

### 生成Nginx密码文件

```
# 密码文件存放位置自定义，路径需记录下来，放在Nginx配置中。
printf "netdata:$(openssl passwd -apr1)" > /usr/local/nginx/conf/htpasswd
```

#### 配置nginx.conf

在 `...nginx/conf.d` 中创建`netdata.conf`文件，写入如下内容，`适当修改端口号、域名、auth_basic_user_file`。

```
upstream backend {
    # the netdata server，请修改具体端口号
    server 127.0.0.1:19999;
    keepalive 64;
}

server {
    # nginx listens to this
    listen 80;

    # the virtual host name of this，请求改具体域名
    server_name netdata.example.com;
   
   # auth password
   auth_basic "netdata Login";
   #  上一步生成的密码文件路径
   auth_basic_user_file /usr/local/nginx/conf/htpasswd;

    location / {
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://backend;
        proxy_http_version 1.1;
        proxy_pass_request_headers on;
        proxy_set_header Connection "keep-alive";
        proxy_store off;
    }
}
```

#### 重启nginx

```
# 密码文件存放位置自定义，路径需记录下来，放在Nginx配置中。
 systemctl reload nginx
```

重启Nginx后，可以直接通过域名`netdata.example.com`访问，并且需要输入账号和密码。但是依然可以通过http://IP:Port的方式访问，接下来禁用IP访问。

### NetData禁用外部IP请求

打开NetData配置文件：`/etc/netdata/netdata.conf`，web项的 `bind to`修改如下：

```
[web]
    bind to = 127.0.0.1 ::1
```

重启NetData： `systemctl restart netdata`

## 建议

\[toc\]

在Netdada的Dasherboard中使用Node ,通过Google或者Github账号注册登录可以使用同步服务，假设你再多台VPS上部署，可以绑定网址到云端上，查看十分方便。

![](https://cdn.jsdelivr.net/gh/plutotz/cdn/picturebed/Snipaste_2020-04-01_14-39-32.png)

![](https://cdn.jsdelivr.net/gh/plutotz/cdn/picturebed/Snipaste_2020-04-01_14-39-53.png)

![](https://cdn.jsdelivr.net/gh/plutotz/cdn/picturebed/Snipaste_2020-04-01_14-44-28.png)