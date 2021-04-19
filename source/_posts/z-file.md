---
title: 记一次在线网盘程序Z-File安装
tags:
  - Z-File
  - 在线网盘程序
id: '629'
categories:
  - - 转载
abbrlink: 50193
date: 2020-02-04 15:43:45
---


## 前言

此项目是一个在线文件目录的程序, 支持各种对象存储和本地存储, 使用定位是个人放常用工具下载, 或做公共的文件库. 不会向多账户方向开发.

前端基于 [h5ai](https://larsjung.de/h5ai/) 的原有功能使用 Vue 重新开发了一遍. 后端采用 SpringBoot, 数据库采用内嵌数据库.

最近开发了一个在线网盘程序 ZFile, 支持各种对象存储、OneDrive、FTP、本地存储. 本文包含普通用户和宝塔用户的安装方式.

预览地址 [https://zfile.jun6.net](https://zfile.jun6.net/)

## 系统特色

*   内存缓存 (免安装)
*   内存数据库 (免安装)
*   个性化配置
*   自定义目录的 header 说明文件
*   自定义 JS, CSS
*   文件夹密码
*   支持在线浏览文本文件, 视频, 图片, 音乐. (支持 FLV 和 HLS)
*   文件/目录二维码
*   缓存动态开启, 缓存自动刷新
*   全局搜索
*   支持 阿里云 OSS, FTP, 华为云 OBS, 本地存储, MINIO, OneDrive 国际/家庭/个人版, OneDrive 世纪互联版, 七牛云 KODO, 腾讯云 COS, 又拍云 USS.

安装依赖环境:

```
# CentOS系统
yum install -y java-1.8.0-openjdk unzip

# Debian/Ubuntu系统
apt update
apt install -y openjdk-8-jre-headless unzip
```

如为更新程序, 则请先执行 `rm -rf ~/zfile` 清理旧程序. 首次安装请忽略此选项.

下载项目

```
wget -P ~ https://c.jun6.net/ZFILE/zfile-1.2.war
cd ~
mkdir zfile && unzip zfile-1.2.war -d zfile && rm -rf zfile-1.2.war
chmod +x ~/zfile/bin/*.sh
```

## 目录结构

1  
2  
3  
4  
5  
6  
7  

├── zfile  
├── META-INF  
├── WEB-INF  
└── bin  
├── start.sh # 启动脚本  
└── stop.sh # 停止脚本  
├── restart.sh # 重启脚本

**启动项目**

```
~/zfile/bin/start.sh
```

**停止项目**

```
~/zfile/bin/stop.sh
```

**重启项目**

```
~/zfile/bin/restart.sh
```

**修改配置文件**

```
vim ~/zfile/WEB-INF/classes/application.yml
```

默认启动端口为 8080, 如需请配置文件请编辑上述文件, 修改后重启程序生效.

## 开放端口 (重点)

如部署后无法访问, 请检查防火墙是否开启此端口:

访问地址：

用户前台：[http](http://127.0.0.1:8080/#/main) : [//127.0.0.1](http://127.0.0.1:8080/#/main) : [8080/#/main](http://127.0.0.1:8080/#/main)

初始安装：[http](http://127.0.0.1:8080/#/install) : [//127.0.0.1](http://127.0.0.1:8080/#/install) : [8080/](http://127.0.0.1:8080/#/install) #/install[](http://127.0.0.1:8080/#/install)

管理后台：[http](http://127.0.0.1:8080/#/admin) : [//127.0.0.1](http://127.0.0.1:8080/#/admin) : [8080/#/admin](http://127.0.0.1:8080/#/admin)

## [](https://github.com/zhaojun1998/zfile#onedrive-%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B)OneDrive使用教程。

访问地址进行授权，获取accessToken和refreshToken：

国际/家庭/个人版：

[https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client\_id=09939809-c617-43c8-a220-a93c1513c5d4&response\_type=code&redirect\_uri=https://zfile.jun6.net/onedirve/callback&scope=offline\_access% 20User.Read％20Files.ReadWrite.All](https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=09939809-c617-43c8-a220-a93c1513c5d4&response_type=code&redirect_uri=https://zfile.jun6.net/onedirve/callback&scope=offline_access%20User.Read%20Files.ReadWrite.All)

世纪互联版：

[https://login.chinacloudapi.cn/common/oauth2/v2.0/authorize?client\_id=4a72d927-1907-488d-9eb2-1b465c53c1c5&response\_type=code&redirect\_uri=https://zfile.jun6.net/onedirve/china-callback&scope= offline\_access％20User.Read％20Files.ReadWrite.All](https://login.chinacloudapi.cn/common/oauth2/v2.0/authorize?client_id=4a72d927-1907-488d-9eb2-1b465c53c1c5&response_type=code&redirect_uri=https://zfile.jun6.net/onedirve/china-callback&scope=offline_access%20User.Read%20Files.ReadWrite.All)

然后分别填充至访问令牌和刷新令牌即可：

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/687474703a2f2f63646e2e6a756e362e6e65742f323032302d30312d32345f31382d35372d30362e706e67.jpg?x-oss-process=image/resize,m_fill,w_640,h_214#)

## [](https://github.com/zhaojun1998/zfile#%E8%BF%90%E8%A1%8C%E7%8E%AF%E5%A2%83)运行环境

*   JDK： `1.8`
*   缓存： `caffeine`
*   数据库： `h2/mysql`

## [](https://github.com/zhaojun1998/zfile#%E9%A2%84%E8%A7%88)预览

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/687474703a2f2f63646e2e6a756e362e6e65742f323032302f30312f32392f613235326135636563373133342e706e67.jpg?x-oss-process=image/resize,m_fill,w_640,h_302#)

  

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/687474703a2f2f63646e2e6a756e362e6e65742f323032302f30312f32392f363062303533386535306639662e706e67.jpg?x-oss-process=image/resize,m_fill,w_300,h_180#)

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/687474703a2f2f63646e2e6a756e362e6e65742f323032302f30312f32392f643563383532323162636666632e706e67.jpg?x-oss-process=image/resize,m_fill,w_640,h_592#)

## [](https://github.com/zhaojun1998/zfile#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)常见问题

### [](https://github.com/zhaojun1998/zfile#%E6%95%B0%E6%8D%AE%E5%BA%93)数据库

缓存默认支持`h2`和`mysql`，前者为嵌入式数据库，无需安装，但相对相对更好。

### [](https://github.com/zhaojun1998/zfile#%E9%BB%98%E8%AE%A4%E8%B7%AF%E5%BE%84)默认路径

默认H2数据库文件地址：`~/.zfile/db/`，`~`表示用户目录，windows为`C:/Users/用户名/`，linux为`/home/用户名/`，root用户为`/root/`

### [](https://github.com/zhaojun1998/zfile#%E5%A4%B4%E5%B0%BE%E6%96%87%E4%BB%B6%E5%92%8C%E5%8A%A0%E5%AF%86%E6%96%87%E4%BB%B6)头尾文件和加密文件

*   目录清单显示文件清单 `header.md`
*   目录需要密码访问，添加文件`password.txt`（无法拦截此文件被下载，但可以改名文件）

转载**本文链接：** [http://www.zhaojun.im/zfile-install/](http://www.zhaojun.im/zfile-install/)
