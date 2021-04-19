---
title: 最新CloudreveV3以及Go语言安装教程
tags:
  - Cloudreve V3
  - 网盘
id: '1065'
categories:
  - - 转载
abbrlink: 51310
date: 2020-03-19 23:34:32
---

> [Cloudreve](https://cloudreve.org/) 是个公有网盘程序，你可以用它快速搭建起自己的网盘服务，公有云/私有云都可。作者用了六个月的时间，把 Cloudreve 用 Go 语言重构了一遍，除了修复 V2 版本被诟病很多的 Bug 外，还增加了很多令人兴奋的新特性：

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/Snipaste_2020-03-19_23-08-13.png?x-oss-process=image/resize,m_fill,w_640,h_297#)

*   支持本机、从机、七牛、阿里云 OSS、腾讯云 COS、又拍云、OneDrive (包括世纪互联版) 作为存储端
*   上传/下载 支持客户端直传，支持下载限速
*   可对接 Aria2 离线下载（支持所有存储策略，下载完成后自动中转）
*   在线 压缩/解压缩、多文件打包下载（支持所有存储策略）
*   覆盖全部存储策略的 WebDAV 协议支持
*   拖拽上传、目录上传、流式上传处理
*   文件拖拽管理
*   多用户、用户组
*   创建文件、目录的分享链接，可设定自动过期
*   视频、图像、音频、文本、Office 文档在线预览
*   自定义配色、黑暗模式、PWA 应用、全站单页应用
*   All-In-One 打包，开箱即用

这篇文章就来尝鲜这个最新go版本的Cloudreve，老规矩还是用宝塔面板来部署。

**具体的安装和部署**

* * *

## 1、前言

*   官网：[https://cloudreve.org/](https://cloudreve.org/)
*   下载：[https://github.com/cloudreve/Cloudreve/releases](https://github.com/cloudreve/Cloudreve/releases)
*   安装文档：[https://docs.cloudreve.org/getting-started/install](https://docs.cloudreve.org/getting-started/install)
*   演示：[https://demo.cloudreve.org](https://demo.cloudreve.org)

## 2、准备

安装之前你需要准备好环境：

1.  宝塔面板安装好
2.  nginx安装好
3.  mysql安装好
4.  域名准备一个
5.  新建网站

## 3、安装

### go语言环境安装 ：

安装环境：CentOS Linux 7.6、宝塔面板6.9.3、golang：go1.12.5.linux-amd64.tar.gz

这篇文章就来水一下如何在宝塔面板Linux环境下安装Go语言环境和程序的如何运行。

#### 一：简介

下载之前先去官网溜达下，点击【download go】就可进入下载页面：

[![宝塔面板Linux环境-安装Golang:Go语言环境安装以及程序如何运行](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632073-golang-1-min.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632073-golang-1-min.png)

官网：[https://golang.google.cn/](https://golang.google.cn/)

下载：[https://dl.google.com/go/go1.12.5.linux-amd64.tar.gz](https://dl.google.com/go/go1.12.5.linux-amd64.tar.gz)

[![宝塔面板Linux环境-安装Golang:Go语言环境安装以及程序如何运行](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632074-golang-2-min.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632074-golang-2-min.png)

根据自己的系统环境下载相应的版本，这里选择的是go1.12.5.linux-amd64.tar.gz。

#### 二：下载安装

宝塔面板可以直接在面板里面下载安装，这里为了方便，直接就在命令环境下面下载安装配置了。

_**2.1下载**_

SSH工具连接服务器开始操作：

```
cd /www/server && wget -O golang.tar.gz https://dl.google.com/go/go1.12.5.linux-amd64.tar.gz
```

这些可以直接在面板环境里操作，也很方便。

[![宝塔面板Linux环境-安装Golang:Go语言环境安装以及程序如何运行](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632074-golang-3-min.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632074-golang-3-min.png)

_**2.2解压**_

下载好之后解压：

```
ar -xzvf golang.tar.gz
```

_**2.3添加环境变量**_

添加环境变量，使用vim 打开/etc/profile 文件。

```
vim /etc/profile
```

在profile 最底部添加：

```
export GOROOT=/www/server/goexport GOBIN=$GOROOT/binexport GOPKG=$GOROOT/pkg/tool/linux_amd64export GOARCH=amd64export GOOS=linuxexport GOPATH=/www/wwwroot/Golangexport PATH=$PATH:$GOBIN:$GOPKG:$GOPATH/bin
```

丢一张图：

[![宝塔面板Linux环境-安装Golang:Go语言环境安装以及程序如何运行](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632075-golang-4-min.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632075-golang-4-min.png)

添加好之后，保存退出，然后执行如下命令使其生效：

```
source /etc/profile
```

_**2.4测试是否生效**_

使用如下命令来测试Go语言环境是否安装成功。

```
go version
```

丢一张图：

[![宝塔面板Linux环境-安装Golang:Go语言环境安装以及程序如何运行](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632076-golang-5-min.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632076-golang-5-min.png)

图上所示，我们已经安装成功了。

#### 三：创建GOROOT目录

使用如下命令来创建，不过我们可以用面板环境来可视化操作：

```
mkdir /www/wwwroot/Golang
```

丢一张图看看：

[![宝塔面板Linux环境-安装Golang:Go语言环境安装以及程序如何运行](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632076-golang-6-min.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632076-golang-6-min.png)

以上就完全安装好go了，因为是宝塔面板的环境以上所以操作都可直接在面板里操作。

#### 四：go run

我们可以测试一段代码来验证go语言的运行。我们到Golang里面新建一个文件命名为test.go

```
touch test.go
```

之后用vi简单编辑下，也可以直接到宝塔面板里新建编辑：

```
vi test.go
```

复制一段代码进去：

```
package main import "fmt" func main() {   /* 这是我的第一个简单的程序 */   fmt.Println("Hello, 大鸟博客!")}
```

丢一张图看看：

[![宝塔面板Linux环境-安装Golang:Go语言环境安装以及程序如何运行](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632077-golang-7-min.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632077-golang-7-min.png)

代码保存好之后，我们执行 Go 程序，如何执行呢，打开命令行，并进入程序文件保存的目录中。

```
go run test.go
```

我们看图：

[![宝塔面板Linux环境-安装Golang:Go语言环境安装以及程序如何运行](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632078-golang-8-min.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632078-golang-8-min.png)

成功执行了这一段代码，输出了“hello，大鸟博客！”

### 第二种快速安装方法：

**环境：centos7**

1.下载安装包，选择如下版本  
Linux 2.6.23 or later, Intel 64-bit processor

```
wget https://dl.google.com/go/go1.12.5.linux-amd64.tar.gz
```

2.校验  
官网给出的SHA256 Checksum是 aea86e3c73495f205929cfebba0d63f1382c8ac59be081b6351681415f4063cf

自己校验看看是否正确

```
sha256sum go1.12.5.linux-amd64.tar.gz
```

3.解压安装包

```
sudo tar -C /usr/local -xzf  go1.12.5.linux-amd64.tar.gz
```

4.添加环境变量

编辑`~/.bash_profile`文件

```
vim ~/.bash_profile
```

添加

```
export PATH=$PATH:/usr/local/go/bin
```

加载新的环境变量

```
source ~/.bash_profile
```

5.测试安装结果

```
mkdir -p ~/go/src/hello

#编辑文件hello.go
vim ~/go/src/hello/hello.go
```

添加

```
package main

import "fmt"

func main() {
   fmt.Printf("Hello, World\n")
}
```

构建`hello.go`文件

```
cd ~/go/src/hello
go build
```

运行测试

```
./hello
```

如果有输出`Hello, World`表示Go安装成功。

#### 五：总结

整个环境的安装和简单的测试运行代码就说完了，希望对想学习Go语言的同学能有一点帮助。

Go 是一个开源的编程语言，它能让构造简单、可靠且高效的软件变得容易。

Go是从2007年末由Robert Griesemer, Rob Pike, Ken Thompson主持开发，后来还加入了Ian Lance Taylor, Russ Cox等人，并最终于2009年11月开源，在2012年早些时候发布了Go 1稳定版本。现在Go的开发已经是完全开放的，并且拥有一个活跃的社区。

Go 语言特色

*   简洁、快速、安全
*   并行、有趣、开源
*   内存管理、数组安全、编译迅速

Go 语言用途

1.  Go 语言被设计成一门应用于搭载 Web 服务器，存储集群或类似用途的巨型中央服务器的系统编程语言。
2.  对于高性能分布式系统领域而言，Go 语言无疑比大多数其它语言有着更高的开发效率。它提供了海量并行的支持，这对于游戏服务端的开发而言是再好不过了。

### 安装Cloudreve，安装命令如下：

```
cd /opt
wget https://github.com/cloudreve/Cloudreve/releases/download/3.0.0-rc1/cloudreve_3.0.0-rc1_linux_amd64.tar.gz
tar -zxvf cloudreve_3.0.0-rc1_linux_amd64.tar.gz   #解压获取到的主程序
chmod +x ./cloudreve  #赋予执行权限
./cloudreve   #启动 Cloudreve
```

分别复制命令回车执行，安装成功截图如下：

[![宝塔面板安装Cloudreve V3(go版本) - 支持六大云存储存/OneDrive世纪互联/aria2等](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632079-Cloudreve-V3-go-1.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632079-Cloudreve-V3-go-1.png)

Cloudreve 在首次启动时，会创建初始管理员账号，请注意保管管理员密码，此密码只会在首次启动时出现。如果您忘记初始管理员密码，需要删除同级目录下的“cloudreve.db”，重新启动主程序以初始化新的管理员账户。

Cloudreve 默认会监听“5212”端口。你可以在浏览器中访问“http://服务器IP:5212”进入 Cloudreve。如果宝塔面板需要在安全中放行“5212”端口。注意用默认的管理账号和密码登录。

## 4、进程守护

以上步骤操作完后，最简单的部署就完成了。你可能需要一些更为具体的配置，才能让Cloudreve更好的工作，宝塔面板我们可以使用Supervisor管理器来设置进程守护，具体流程请参考下面的配置流程。

### **4.1 安装Supervisor管理器**

软件商店→系统工具 ，找到Supervisor管理器安装即可。

[![宝塔面板安装Cloudreve V3(go版本) - 支持六大云存储存/OneDrive世纪互联/aria2等](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632080-Cloudreve-V3-go-2.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632080-Cloudreve-V3-go-2.png)

### **4.2 添加守护进程**

打开Supervisor管理器添加守护进程，看图：

[![宝塔面板安装Cloudreve V3(go版本) - 支持六大云存储存/OneDrive世纪互联/aria2等](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632080-Cloudreve-V3-go-3.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632080-Cloudreve-V3-go-3.png)

**注意**：路径修改为自己的。添加完成后，守护进程就会启动成功，如图：

[![宝塔面板安装Cloudreve V3(go版本) - 支持六大云存储存/OneDrive世纪互联/aria2等](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632081-Cloudreve-V3-go-4.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632081-Cloudreve-V3-go-4.png)

**注意**：设置守护进程之前，请先停止掉命令模式。

## 5、域名访问

新建网站，之后在网站设置中，配置反向daili，如图：

[![宝塔面板安装Cloudreve V3(go版本) - 支持六大云存储存/OneDrive世纪互联/aria2等](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632081-Cloudreve-V3-go-5.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632081-Cloudreve-V3-go-5.png)

## 6、效果展示

现在就可以用域名打开Cloudreve 访问了：

[![宝塔面板安装Cloudreve V3(go版本) - 支持六大云存储存/OneDrive世纪互联/aria2等](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632083-Cloudreve-V3-go-8.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632083-Cloudreve-V3-go-8.png)

#### **管理面板：**

[![宝塔面板安装Cloudreve V3(go版本) - 支持六大云存储存/OneDrive世纪互联/aria2等](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632084-Cloudreve-V3-go-9.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632084-Cloudreve-V3-go-9.png)

#### **支持的存储策略：**

[![宝塔面板安装Cloudreve V3(go版本) - 支持六大云存储存/OneDrive世纪互联/aria2等](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632084-Cloudreve-V3-go-10.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632084-Cloudreve-V3-go-10.png)

#### **添加oneindex存储策略时详细的引导：**

[![宝塔面板安装Cloudreve V3(go版本) - 支持六大云存储存/OneDrive世纪互联/aria2等](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632085-Cloudreve-V3-go-11.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632085-Cloudreve-V3-go-11.png)

#### **创建WebDAV：**

[![宝塔面板安装Cloudreve V3(go版本) - 支持六大云存储存/OneDrive世纪互联/aria2等](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632085-Cloudreve-V3-go-12.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584632085-Cloudreve-V3-go-12.png)

注意：目前 V3 仍处于 RC 版本阶段，V2版本的升级措施会随着正式版一起发布。

## 7、一些细节

首次启动时，Cloudreve 会在同级目录下创建名为“conf.ini”的配置文件，你可以修改此文件进行一些参数的配置，保存后需要重新启动 Cloudreve 生效。

默认情况下，Cloudreve 会使用内置的 SQLite 数据库，并在同级目录创建数据库文件“cloudreve.db”，如果您想要使用 MySQL，请在配置文件中加入以下内容，并重启 Cloudreve。

```
[Database]
#数据库类型，目前支持 sqlite  mysql
Type = mysql
#用户名
User = root
#密码
Password = root
#数据库地址
Host = 127.0.0.1
#数据库名称
Name = v3
#数据表前缀
TablePrefix = cd
```

注意：更换数据库配置后，Cloudreve 会重新初始化数据库，原有的数据将会丢失。

## 8、最后

从使用体验来看，效果很不错，功能强大，支持存储种类也多，唯一不足的地方竟然不支持Google Drive 。作者更是说目前不支持，未来也不会支持。

安装真的是很简单了，比之前的v2版本安装简单的多。不过目前还是测试版，所以有bug是很正常的。

场景使用：可以使用 Cloudreve 搭建个人用网盘、文件分享系统，亦或是针对大小团体的公有云系统。

摘自： https://www.daniao.org/8544.html https://www.daniao.org/5094.html

在此感谢，有部分删改
