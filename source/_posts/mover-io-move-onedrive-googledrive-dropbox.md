---
title: 使用MOVER.IO文件迁移服务互通OneDrive/GoogleDrive/Dropbox等网盘
tags:
  - Dropbox
  - GoogleDrive
  - MOVER.IO
  - onedrive
  - 文件迁移
  - 网盘
id: '1044'
categories:
  - - 转载
abbrlink: 62355
date: 2020-03-18 14:24:00
---

## 简介

可能在大家手里面早就有了几个[OneDrive](https://www.gitiu.com/share/rclone/)的号，有个人号，E5管理号，A1子号，A1P子号，世纪互联子号，可是都是空的，也不敢存太多东西，要是能够转存资源就好了，刚好发现了一个比较优秀的网站，可以转存多种主流网盘(注:国外的)，当然也包括[OneDrive](https://www.gitiu.com/share/systemctl_start_rclone/)不同域之间的转存，最重要的是，完全免费，只有一点小限制，而它这就是 [mover.io](https://www.gitiu.com/reprinted_articles/mover-io-move-onedrive-googledrive-dropbox/)。

\[toc\]

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584502432-f71c12dc94fbf0d8c44ba737517d30e1.png)

这个网站之前就是网盘搬家类的，就在去年2019年10月21号，被微软收购了(巨硬有钱任性)，现在用于将云文件迁移到OFFICE365(包括谷歌网盘，亚马逊等)，只进不出，接下来简单介绍一下!

## 官网地址

[https://mover.io](https://mover.io)

## 账号注册

使用任意邮箱即可注册，注册过程会验证邮箱​，需要去邮箱点击验证，电话号码不会验证！

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584502432-37ef6a74640a8790c4e79ebd5fc7774b.png)

## 连接器

Mover 将十几个云服务提供商、web服务和数据库链接到Office 365，包括OneDrive和SharePoint Online。我们亲切地称这些链接为我们的**连接器**。

你为什么要用我们的连接器？以下是我们最喜欢的两个理由：

*   快速、方便、安全地将多个用户和大数据量迁移到Microsoft。
*   通过我们基于web的服务复制文件而不是使用桌面同步工具来节省带宽。

**主持列表**  
Mover Agent / Amazon S3 / Amazon WorkDocs / Microsoft Azure / Box / Dropbox / Egnyte / FTP / **Google Drive /** **Office 365 /** **OneDrive /** SFTP / SharePoint Online / WebDAV

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584502432-f4e36fb558f7e36aa7574d5281e61fef.png)

## 使用方法

注册成功后，进入网页，务必使用​电脑打开网页，手机打开网页可能会被折叠，导致文件夹打不开，如下图，电脑打开网页和手机打开的网页(手机Chrome已开启桌面版网页)。

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584502432-9e7b0f996593c8e9ef50af5c50432395.png)

### 数据源

数据源基本上 连接器 支持的云服务商都支持！只要授权登录即可！非常方便！

### 目的地

目前仅支持微软家族的存储服务！

Azure Blob Storage / Office 365 (OneDrive/SharePoint Admin) / OneDrive Consumer / OneDrive for Business (Single User)

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584502432-19f126a714a227b6d3aa891068e34f19.png)

### 账号选择？

我的微软的账号，到底选择哪个授权登录呢？其实多试几次就知道了！

OneDrive Consumer  : 普通的微软账号

OneDrive for Business (Single User) ： A1 / A1P / E3 /E5 等商业版账号

Office 365 (OneDrive/SharePoint Admin) ：A1 / A1P / E3 /E5 等商业版全局管理账号以及SharePoint账号

### 数据迁移

操作比较简单，只需要将 复制源(Source) 和 目标(Destination) 都授权了，就能愉快的玩耍了！

传输的速度也是非常快的！！

我只测试了 OneDrive -> OneDrive 和 Google Drive -> OneDrive ，一般速度大概每分钟传输**1-2GB**。

更多功能自行探索(其实功能就那几个，多试试)​~~

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584502432-c462e460e5e983fe339b1d8ab435944d.png)

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584502432-166f4003b07f699955a6c7ce186bcd46.png)

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584502432-baedf92141d6aebd6e53ddaf3cd59e83.png)

## 使用缺点

1.  团队盘(A1，E5，A1P等等)限制单文件大小15G。
2.  个人盘限制单文件大小10g(我也不知道怎么回事，可能是网站规则限制吧)。
3.  生成的任务删除不了，只能暂停。
4.  只能转存到OneDrive，不能转到其他地方。
5.  无法自动同步更新，只能手动点击。

## 使用优点

1.  免费(不知道什么时候会收费，现在没有)。
2.  不限制网盘数，不限制总流量。
3.  可以自动跳过同目录重复文件。
4.  支持不同域的OneDrive。
5.  官宣是使用的Windows Azure来传输的。

## 其他说明

发现这个平台还支持FTP传输！这个可以用来做一些FTP备份使用，比如网站数据/数据库的备份！

摘自： https://51.ruyo.net/15587.html 有部分删改 感谢