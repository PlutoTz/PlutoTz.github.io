---
title: 【Hellohao图床】基于Sping Boot开发的图床站，纯粹的响应式图片存放驿站
tags:
  - hellohao图床
id: '566'
categories:
  - - 转载
abbrlink: 16604
date: 2019-08-24 07:47:00
---

![](https://www.gitiu.com/wp-content/uploads/2020/02/870a40908091646-1.png)

这是一个基于多家对象存储源的Spring Boot开源图床项目。系统使用 Spring Boot 搭建, 针对用户更方便的管理自己的图片管理拓展功能, 目前已经支持对接`本地`、`网易`，`阿里`，`又拍`，`七牛`、`腾讯`，`FTP`对象存储.

## 前言

本源码是有java语言基于springboot开发的图床站，并且毫无保留的把它开源了出来。

## 主要功能支持：

*   支持 图片拖拽、截图软件直接(Ctrl+V)和图片URL地址上传。
*   对接本地、网易、阿里、又拍、七牛、腾讯、FTP等各大对象存储平台。
*   图片定期暂存（到期自动删除）
*   支持画廊分享模式(用户可把自己当前上传的图片以图片集的形式批量分享给好友)
*   支持上传者IP记录，并可配置IP黑名单操作
*   支持链接生成二维码。
*   支持开启/关闭API接口。
*   设置用户可用容量
*   扩容码生成（用户可使用扩容码进行容量扩充）
*   分发群组（配置用户群组，不同群组分发图片到不同对象存储）
*   首页背景动态/静态，以及简约模式设置
*   URL列表、缩略图。查看原图等功能。
*   图片鉴黄配置（开启后，每天固定时间进行非法图片监测）
*   游客、用户的上传管理
*   邮箱注册激活。
*   站点样式设置和上传规则配置等。
*   等等~~

### 地址向导

**Github开源:**[**https://github.com/Hello-hao/Tbed**](https://github.com/Hello-hao/Tbed)

**在线文档:** [http://doc.wwery.com/](http://doc.wwery.com/)

**主站地址:** [**http://tc.hellohao.cn/**](http://tc.hellohao.cn/)

**体验地址:** [**http://129.28.173.126:8088/**](http://129.28.173.126:8088/)（用户名/密码均为admin）

## 更新日志

**2019-11-29**

*   √ 新增ip黑名单功能。
*   √ 再次优化上传前后台验证。
*   √ 提高首页打开速度。

## 开发者交流群

**群名称：** Hellohao图床开发者交流群

**群 号：** 864800972

## 系统预览

![](https://www.gitiu.com/wp-content/uploads/2020/02/6071c0825062730-1.png)

![](https://www.gitiu.com/wp-content/uploads/2020/02/eeeed0825062727-1.png)

![](https://www.gitiu.com/wp-content/uploads/2020/02/c208e0825054822-1.png)

![](https://www.gitiu.com/wp-content/uploads/2020/02/6c7690825054822-1.png)

![](https://www.gitiu.com/wp-content/uploads/2020/02/2a79b0825054822-1.png)

![](https://www.gitiu.com/wp-content/uploads/2020/02/5c1800825054824-1.png)

## 运行环境

*   JDK 1.8
*   MySQL 5.5+

## 详细部署教程

> 部署教程分为：1.一键部署；2.手动部署。任选一种部署即可（手动能力强的推荐：手动部署）

### 前言

部署依赖于java环境：

```

yum install java-1.8*
```

### 一，自动部署

#### 创建数据库：

创建mysql数据库`picturebed`，并导入`picturebed.sql`文件建表。

#### 执行如下命令，按步骤操作即可。

```

yum install -y wget && wget -O hellohao.sh http://www.hellohao.cn/gg/hellohao.sh && bash hellohao.sh
```

![](https://www.gitiu.com/wp-content/uploads/2020/02/647e11120102053-1.png)

该方法简单粗暴，包括java环境，不需要去下载jar包，不需要自己去写命令运行。也不需要`screen`或者`nohub`之类的后台命令，让用户傻瓜式操作。

#### 注意事项：

*   **安装：**等项目完全部署成功后且确认图床可以访问，在控制台按`Ctrl+C`后退出即可。（若内有java环境，先在选择界面安装java环境，然后重新执行以上命名安装图床）
*   **如果之前存在脚本安装过的图床：**请先根据提示按**3**停止然后再按**2**卸载，才可以重新安装。
*   如果重启了服务器，则需要重新部署。没有开机自启。

### 二，手动部署

先下载编译包：**[https://github.com/Hello-hao/Tbed/releases](https://github.com/Hello-hao/Tbed/releases)**，做准备并进行如下操作。

全平台支持：Linux/Windows/unix（这里以linux宝塔环境举例子）：

#### 导入数据库

![](https://www.gitiu.com/wp-content/uploads/2020/02/150550716060806-1.png)

![](https://www.gitiu.com/wp-content/uploads/2020/02/942210716061116-1.png)

创建数据库`picturebed`, 字符集选择 `utf8`后  
导入`picturebed.sql`  
  
在应用商店搜索`tomcat`选择tomcat8版本下载安装（这里说明一下，其实我们并不需要tomcat，只是因为宝塔下载tomcat8自带java环境，而这个java环境正是我们需要的）

#### 配置文件

打开 `application.properties` 修改 `MySQL` 和 `服务器端口` 等连接信息改成你服务器的信息.（端口号改不改都行，前提是保证其他程序没有被占用）

```
#数据库账号picturebed
spring.datasource.username=root
#数据库密码
spring.datasource.password=root
#数据库链接地址
spring.datasource.url=jdbc:mysql://localhost:3306/picturebed?useUnicode=true&characterEncoding=utf8&serverTimezone=GMT%2B8
#端口
server.port=8088
#鉴黄周期表达式 下方表达式为每天七点半执行
#不懂请勿乱修改。具体可以参考官方文档http://doc.wwery.com
Expression=0 30 04 * * ?

#下边的配置项不需要修改。
mybatis.configuration.map-underscore-to-camel-case=true
mybatis.mapper-locations=classpath:mapper/*.xml
logging.level.cn.hellohao.dao=debug
spring.jackson.date-format=yyyy-MM-dd HH:mm:ss
spring.jackson.time-zone=GMT+8
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
spring.thymeleaf.cache=false
multipart.maxFileSize=10240KB
multipart.maxRequestSize=10240KB
spring.thymeleaf.mode = LEGACYHTML5
spring.http.multipart.location=/data/upload_tmp
```

#### 部署

![](https://www.gitiu.com/wp-content/uploads/2020/02/a84aa0716061624-1.png)

jdk1.8和mysql数据库、还有配置文件，我们都改好了。接下来开始部署吧。  
在系统目录`/home`下新建一个`javaweb`目录  
把`Tbed.jar`和`application.properties`放到服务器你刚才新建的目录下：`/home/javaweb`，注意这两个文件必须要在同一目录下不能分开。

#### 启动项目

在完成了上述步骤后，然后xshell链接你的服务器，进入你刚才的目录，然后执行一下命令.  
进入你的目录：

```

[root@VM_0_15_centos ~]# cd /home/javaweb
```

部署jar编译包：

```
[root@VM_0_15_centos javaweb]# java -jar Tbed.jar
```

ok!等项目跑起来即可，如图：

![](https://www.gitiu.com/wp-content/uploads/2020/02/241340827025109-1.jpg)

**这里注意：记得把项目所需要的端口（8088）放行，否则访问不了！！**

启动后访问地址为：[http://服务器IP:8088](http://xn--ip-fr5c86lx7z:8088/) , `8088`就是你配置`server.port=8088`的端口.  
这样，你的站点就启动了。`注意:`启动后命令行是不能关闭的，否则你的站点就会不能访问。可以用`nohup`或者`screen`之类的命令后台运行。

初始用户名：`admin`  
初始邮箱：`admin`  
初始密码`admin`

## 视频教程

**全套部署视频：[https://www.bilibili.com/video/av79137056/](https://www.bilibili.com/video/av79137056/)**

> 原创作者：[Hellohao](https://hellohao.cn)
> 
> 本文链接： [http://www.hellohao.cn/?p=201](http://www.hellohao.cn/?p=201)