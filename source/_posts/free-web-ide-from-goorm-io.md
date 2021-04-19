---
title: goorm.io一款来自韩国的免费在线Web IDE平台
tags:
  - goorm.io
  - Web IDE平台
id: '1050'
categories:
  - - 转载
abbrlink: 40018
date: 2020-03-07 11:38:30
---

## 前言

对于类似的在线的WEB IDE之前也介绍过几款[Cloud9](https://51.ruyo.net/3446.html) ,[Coding.net](https://51.ruyo.net/3380.html) ！这里给大家分享一款韩国思密达的一款[Web IDE](https://www.gitiu.com/reprinted_articles/free-web-ide-from-goorm-io/（在新窗口中打开）)！

\[toc\]

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584502824-goorm.io_.png)

## 官网地址

[https://www.goorm.io/](https://www.goorm.io/)

账号登录：[https://accounts.goorm.io/signup](https://accounts.goorm.io/signup)

登录地址：[https://account](https://51.ruyo.net/go/index.html?u=https://accounts.goorm.io/login)[s.goorm.io/login](https://accounts.goorm.io/login)

控制面板：[https://ide.goorm.io/my](https://ide.goorm.io/my)

## 免费额度

CPU使用率： 低

内存：1GB

储存：10GB

容器数量：5

协作者：5个/容器

宾客：3/容器

域名：3/容器

虚拟化：KVM

**同时在线运行1个容器**

不支持永久在线运行

不支持自定义域名

付费套餐：[https://ide.goorm.io/pricing](https://ide.goorm.io/pricing)

## 操作步骤

1）访问【[注册地址](https://accounts.goorm.io/signup)】，填写相关信息注册账号！

先需要邮箱接受验证URL，然后完成注册。

2）访问【[控制面板](https://ide.goorm.io/my)】，点击【New container】可以新建一个容器！

3）填写相关内容！直接创建即可！

Region ：美国，韩国，德国，印度

Stack：包含目前主流的开发语言以及框架！

支持添加Mysql，MangoDB！

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584503172-0170451ad5f67c12d2a0b1159eec9ec6.png)

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584503172-902051ef77060a089264a5e284245649.png)

4）点击容器右上角的【⚙】可以看见的配置信息！

Basic Information（基本信息）

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584503173-3d31e4faed7be9d7e6d9c14d87cddda0.png)

Setting（设置信息）

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584503173-19805c15e287fa2e0e3cc65642419669.png)

网络端口映射设置

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584503173-dfd98a08990afaf347d9beb6c173c22a.png)

剩下的是一些协作的配置，这里不多做说明了！

4）终端中可以编写代码了~

测试命令

```
python -m http.server 443
```

然后访问域名机可以了！

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584503173-bea34f3e4e077facf74c37352eda3c07.png)

## 测试IP

54.180.119.192 （韩国）

## 性能跑分

```
----------------------------------------------------------------------
CPU model            : Intel(R) Xeon(R) Platinum 8175M CPU @ 2.50GHz
Number of cores      : 2
CPU frequency        : 2500.000 MHz
Total size of Disk   : 77.9 GB (63.0 GB Used)
Total amount of Mem  : 7687 MB (2408 MB Used)
Total amount of Swap : 8191 MB (18 MB Used)
System uptime        : 0 days, 2 hour 24 min
Load average         : 0.38, 0.80, 1.20
OS                   : Ubuntu 18.04.2 LTS
Arch                 : x86_64 (64 Bit)
Kernel               : 4.4.0-1102-aws
----------------------------------------------------------------------
I/O speed(1st run)   : 68.2 MB/s
I/O speed(2nd run)   : 51.2 MB/s
I/O speed(3rd run)   : 45.5 MB/s
Average I/O speed    : 55.0 MB/s
----------------------------------------------------------------------
Node Name                       IPv4 address            Download Speed
CacheFly                        204.93.150.152          53.0MB/s
Linode, Tokyo2, JP              139.162.65.37           75.5MB/s
Linode, Singapore, SG           139.162.23.4            24.6MB/s
Linode, London, UK              176.58.107.39           9.25MB/s
Linode, Frankfurt, DE           139.162.130.8           8.87MB/s
Linode, Fremont, CA             50.116.14.9             17.6MB/s
Softlayer, Dallas, TX           173.192.68.18           14.6MB/s
Softlayer, Seattle, WA          67.228.112.250          17.6MB/s
Softlayer, Frankfurt, DE        159.122.69.4            4.93MB/s
Softlayer, Singapore, SG        119.81.28.170           25.5MB/s
Softlayer, HongKong, CN         119.81.130.170          37.0MB/s
----------------------------------------------------------------------
```

## 最后说明

该免费的WEBIDE适合测试学习使用，不太适合酸酸等！CPU使用率不能太高，否则有封号的风险！

容器能在线多久？目测是只要你链的终端就不会离线！

摘自：[goorm.io一款来自韩国的免费在线Web IDE平台](https://51.ruyo.net/15623.html) 在此感谢