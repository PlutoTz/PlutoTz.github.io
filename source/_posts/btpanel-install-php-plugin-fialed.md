---
title: 解决宝塔面板安装php拓展失败的问题
tags:
  - btpanel
  - memcached安装失败
  - php拓展
  - 宝塔面板
id: '848'
categories:
  - - 文章
abbrlink: 46585
date: 2020-03-03 11:14:22
---

\[toc\]

## 前言

作为一个只有一点基础，甚至没有基础的Linux初学者来说使用宝塔面板无疑是一个更好的选择，方便实施管理预览，安装一些常用的软件环境，部署一些项目快速，当然缺点还是有的，比如说对小内存机器不友好，不去设置后台甚至不安全，但是总的来说，使用起来还是很稳定。

## 问题

其中我们对于安装php拓展时会遇到编译出错，运行日志报错，这甚至十分常见。

比如说安装fileinfo， exif ，imagemagick， memcached 等等

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1583203623-bt1.png?x-oss-process=image/resize,m_fill,w_640,h_241#)

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/bt2.png)

可能会去多安装几次，但是显示成功实际上并未安装成功。

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/bt4.png)

## 思考解决

多半是编译环境有问题，比如说安装 memcached 时，可能就是服务器编译器的问题，你可以试试这些代码

```
yum -y install gcc-c++
yum -y install glibc-headers
yum -y install m4
yum -y install autoconf
```

这时多半就会解决问题

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/bt7.png)

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/bt6.png)

也可以通过phpinfo查看

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/bt5-phpinfo.png)