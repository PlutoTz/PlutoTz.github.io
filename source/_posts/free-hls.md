---
title: 部署Free-HLS – 视频切片后自动上传至国内免费CDN
tags:
  - Free-HLS
  - 视频切片
  - 视频床
id: '874'
categories:
  - - 转载
abbrlink: 24613
date: 2020-03-11 09:10:51
---

\[toc\]

Free-HLS是一个免费的 HLS 视频解决方案，即所谓的视频床。该项目提供一整套集成化解决方案，囊括了各环节所需的切片、转码、上传、即时分享等套件。让你可以以更方便、更低廉的方式分享你的视频到任意地方。

这个项目需要搭建服务端和客户端，当然你可以都放到一起。这篇教程就来说说如何利用宝塔面板来搭建一个自己的视频床。

###   
**1、前言**

Github地址：

参考视频部署：[https://sxyz.gitee.io/free-hls/usage.html](https://sxyz.gitee.io/free-hls/usage.html)

## 2.安装

### 2.1安装python项目管理器（软件商店里安装）

宝塔面板默认的python版本是python2，但是这个教程是需要python3来部署的，如何解决呢？这里提供一个思路。

1.**首先安装python项目管理器，这个自己在软件商店里自行安装。**

2.**版本管理中安装python3.7.2。**

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1583890714-httpswww.daniao.orgwp-contentuploads202003free-hls-bt-1.png)

### 2.2**随便添加一个项目**

随便添加一个项目，项目添加好后，宝塔会给我们生成一个虚拟环境这个虚拟环境默认就是python3的环境，所以问题就解决了。比如说我们就添加free-hls这个项目。先进入命令模式git项目，如下：

```
git clone https://github.com/sxyazi/free-hls.git
```

之后，我们进入到python项目管理新建一个项目，具体如图：

![å®å¡é¢æ¿é¨ç½²Free-HLS - è§é¢åçåèªå¨ä¸ä¼ è³å½ååè´¹CDN](https://cdn.gitiu.com/wp-content/uploads/2020/03/1583890714-httpswww.daniao.orgwp-contentuploads202003free-hls-bt-2.png)

### 2.3 **进入虚拟环境安装**

项目建立之后，会给我们生成一个虚拟环境，进入方法如下，在命令行输入 ：

```
source 项目路径/项目名_venv/bin/activate
如：source /data/python/project1_venv/bin/activate
比如这个案列进入方法：

source /www/wwwroot/ee.daniao.org/OneList/666666_venv/bin/activate
```

截图：

[![宝塔面板部署Free-HLS - 视频切片后自动上传至国内免费CDN](https://cdn.gitiu.com/wp-content/uploads/2020/03/1583890715-httpswww.daniao.orgwp-contentuploads202003free-hls-bt-3.png)](https://cdn.gitiu.com/wp-content/uploads/2020/03/1583890715-httpswww.daniao.orgwp-contentuploads202003free-hls-bt-3.png)

这样就解决了python的版本问题，而且还是一个单独的虚拟环境，折腾坏了也不影响啥。

### 2.4也可以选择命令安装 安装最新的 Python3，以及必要包：

```
apt install -y ffmpeg python3 python3-pip
pip3 install requests python-dotenv
```

## 3.搭建服务端

服务端位于项目的 `web` 目录，负责向客户端提供视频发布所必要的 API 接口。以及最终目标视频的播放服务。

3.1 **安装依赖**

ps: 这里命令输入是接着上面，你需要进入到python3的虚拟环境里面执行（这里的命令是centos7系统。）！！！

```
yum install -y python3 python3-pip
yum install Flask gunicorn python-dotenv
```

3.2 ** 启动服务**

```
cd web
gunicorn app:app -b 0.0.0.0:3395 --workers=5 --threads=2 -D
```

### 3.3访问你的IP:3395

当看到“hello，world”表示客户端已经搭建成功。

## 4.注册语雀

点击进入 [语雀官网](https://www.yuque.com)

先前往语雀官网注册一个账号，然后获取`ctoken`和`session`的值，这里说下大概获取方法，以谷歌浏览器为例。

登录后，`F12`进入控制台选择`Network`，点击`dashboard`，再选择`Cookies`即可看到所需要的`2`个参数。

## 5.搭建服务端

这里注意： 服务端可以在自己电脑上，也可以在服务器上，这就以服务器搭建为列。

5.1**安装依赖**

依赖包括ffmpeg、python3、python-dotenv，因为在上面我们已经安装过了依赖，唯一的ffmpeg没有安装，这里说下宝塔安装ffmpeg的方法，如下：

```
wget http://download.bt.cn/install/ext/ffmpeg.sh && sh ffmpeg.sh
安装完后可输入以下命令是否安装成功。
ffmpeg -version
```

在PHP设置中取消掉禁用scandir，exec、system、shell\_exec函数

安装方法2

```
1.编译安装升级yum
cd /root
yum -y update
2.安装命令如下：#下载ffmpeg（x64二进制文件）
wget https://johnvansickle.com/ffmpeg/builds/ffmpeg-git-amd64-static.tar.xz
 
#解压文件
tar xvf ffmpeg-git-*-static.tar.xz && rm -rf ffmpeg-git-*-static.tar.xz
#将ffmpeg和ffprobe可执行文件移至/usr/bin方便系统直接调用
mv ffmpeg-git-*/ffmpeg  ffmpeg-git-*/ffprobe /usr/bin/
 
#也可以使用ffmpeg一键自动安装包，安装FFMPEG和相关依赖。（如果使用二进制文件，此步略过）
#https://www.ffmpegtoolkit.com/
#CentOS 7.* 64bit Latest
yum install git wget -y 
cd /opt
git clone https://github.com/hostsoft/ffmpegtoolkit.git ffmpegtoolkit
cd ffmpegtoolkit
sh latest.sh
```

安装的过程非常漫长，你需要慢慢等待。安装成功后可以用命令“`ffmpeg`”或者“`ffmpeg -version`”来测试是否安装成功

###  **5.**2配置语雀

将 `.env.example` 更名为 `.env`，修改其中的 `YUQUE_CTOKEN`、`YUQUE_SESSION` 配置。操作步骤：

1.  登录语雀；
2.  打开 Chrome 的开发者工具（F12），切换到 Network 面板；
3.  刷新页面，从 Cookie 中抓取 `ctoken`、`_yuque_session`，复制并替换到 `.env` 文件中；
4.  将你服务器的域名或 IP 地址修改到 `.env` 中的 `APIURL` 配置项。

![å®å¡é¢æ¿é¨ç½²Free-HLS - è§é¢åçåèªå¨ä¸ä¼ è³å½ååè´¹CDN](https://cdn.gitiu.com/wp-content/uploads/2020/03/1583890716-httpswww.daniao.orgwp-contentuploads202003free-hls-bt-5.png)

## 6.使用

准备好目标视频文件，输入如下指令开始切片、上传：

```
python3 up.py test.mp4               #默认标题
python3 up.py test.mp4 测试哦         #自定义标题
python3 up.py test.mp4 test 5        #自定义分段大小
python3 up.py test.mp4 test LIMITED  #限制码率（需重编码）

python3 ls.py    #列出已上传视频
python3 ls.py 3  #列出已上传视频（第3页，50每页）
```

比如视频在root目录中命名为test,输入 python3 up.py /root/test.mp4开始上传

成功之后，会显示地址

最后的下载地址域名绑定使用宝塔面板反向代理IP:3396

## 7.最后

推荐在自己的电脑上设置服务器，不然长时间压缩切片造成服务器资源占用太高，会被服务商关机。

你要切片视频的话你得上传一个视频到你的服务器，或者自己从别地方download一个过来。

同样还有很多项目：

*   https://github.com/sxzz/free-hls.js
*   https://github.com/sxzz/free-hls-live
*   https://github.com/MoeClub/Note/tree/master/ffmpeg

教程绝大部分参考https://www.daniao.org/8407.html 有部分修改