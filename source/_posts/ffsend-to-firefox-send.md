---
title: '安装ffsend命令行客户端通过FireFox Send分享文件-[超详细]'
tags:
  - ffsend
  - Firefox Send
id: '1222'
categories:
  - - 文章
abbrlink: 125
date: 2020-04-01 21:34:00
---

## 1.前言

Linux 用户偏爱使用 `scp` 或 `rsync` 来进行文件或目录的复制拷贝。不过在 Linux 上正出现了许多新的可选方式，因为 Linux 是开源的，所以任何人都可以为 Linux 开发一个安全软件。 于此同时有  [OnionShare](https://linux.cn/article-9177-1.html)、[Magic Wormhole](https://www.2daygeek.com/wormhole-securely-share-files-from-linux-command-line/)、[Transfer.sh](https://www.2daygeek.com/transfer-sh-easy-fast-way-share-files-over-internet-from-command-line/) 和 [Dcp – Dat Copy](https://linux.cn/article-10516-1.html)这些可用的工具。

但是今天我们来认识[ffsend](https://www.gitiu.com/article/ffsend-to-firefox-send) ，它是 [Firefox Send](https://send.firefox.com/) 的命令客户端。 允许用户通过命令行来传递和接收文件或目录。 甚至可以允许我们通过一个安全、私密、加密的链接，使用一个简单的命令来轻易安全地分享文件。

## 2.了解Firefox Send

![](https://www.gitiu.com/wp-content/uploads/2020/10/1602954606-dfd26e958931ee3cae9896cd22fb5881.png)

**FireFox Send** 与传统的网盘不太一样，它是一种类似“**阅后即焚**”的简单且私密的临时个人文件共享工具 (网络服务)，用户只需通过任意浏览器 (包括 Chrome、Edge、火狐等) 即可快速上传一个或多个文件与他人分享。 匿名用户可以上传最大 1GB、最长 1 天的临时文件、被下载 1 次后自动删除文件；而注册用户 (同样完全免费) 则最大可以上传 2.5 GB 文件、最长可以保留 7 天的时间、最多允许 100 次下载次数。

目前已经推出Android端，可在Google Play商店下载体验！！

### 2.1 **FireFox Send**的特点

*   简单安全的一次性临时文件分享服务
*   跨平台、跨设备
*   完全开源
*   通过源代码「**自己架设一套私人专属的 FireFox Send 网盘**」

\[mdx\_github author="mozilla" project="send"\]\[/mdx\_github\]

## 3.引出ffsend

得益于 FireFox Send 完全开源 (基于 Node.js 开发)，甚至还有开发者推出了命令行版本的工具，可以通过命令一键上传并分享文件ffsend ，这对于运维或开发人员，可以非常方便地通过服务器传输文件或者编写脚本整合到自己的工作流中去。

\[mdx\_github author="timvisee" project="ffsend"\]\[/mdx\_github\]

### 3.1 ffsend 的特点：

*   全功能且使用友好的命令行工具
*   可以安全地上传和下载文件与目录
*   总是在客户端加密
*   可用额外的密码进行保护、密码生成和可配置下载次数限制
*   内置的文件或目录的打包和解压
*   可以轻松地管理你的历史分享记录
*   能够使用你自己的 Send 主机
*   审查或者删除共享文件
*   精准的错误报告
*   低内存消耗，用于加密或上传下载
*   无需交互，可以集成在脚本中

![](https://www.gitiu.com/wp-content/uploads/2020/10/1602954606-51f3bf91f8e39f8bed0626e870a8a4b2.gif)

### 3.2安装 ffsend 

```
#下载压缩包
$  wget https://github.com/timvisee/ffsend/releases/download/v0.2.59/ffsend-v0.2.59-linux-x64.tar.gz
PS:注意最新版版本号的修改这里是v0.2.59

#解压 tar 包
$  tar -xvf ffsend-v0.2.59-linux-x64.tar.gz

#查看你的 PATH 环境变量
echo $PATH
/home/daygeek/.cargo/bin:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/lib/jvm/default/bin:/usr/bin/site_perl:/usr/bin/vendor_perl:/usr/bin/core_perl

#将这个可执行文件放置到 PATH 环境变量中的某个目录中
$  sudo mv ffsend /usr/local/sbin

#直接运行ffsend
$  ffsend
```

这里就可以获取其基本使用信息了

```
ffsend 0.2.59
Usage: ffsend [FLAGS] ...
Easily and securely share files from the command line.
A fully featured Firefox Send client.
Missing subcommand. Here are the most used:
 ffsend upload ...
 ffsend download ...
To show all subcommands, features and other help:
 ffsend help [SUBCOMMAND]
```

当然也有Windows,MacOS,nupkg版

![](https://www.gitiu.com/wp-content/uploads/2020/10/1602954606-297c2fcf263ebda698eab31f51b0a670.png)

下载发布地址： [https://github.com/timvisee/ffsend/releases](https://github.com/timvisee/ffsend/releases)

### 3.3 Debian/Ubuntu系统

```
$  wget https://github.com/timvisee/ffsend/releases/download/v0.2.59/ffsend_0.2.59_amd64.deb

$  sudo dpkg -i ffsend_0.2.59_amd64.deb
```

甚至是 **Arch Linux系统**的用户， 可以简单地借助 [AUR 助手](https://www.2daygeek.com/category/aur-helper/)来安装它，因为这个包已经在 AUR 软件仓库中了 。

```
yay -S ffsend
```

## 4.使用 ffsend

**4.1 上传语法： ffsend upload \[/Path/to/the/file/name\]**

比如我上传一个名为BasicAuth.png的图片

```
#输入上传文件的名称
$  ffsend upload BasicAuth.png --copy
#下面会提示上传完成，出现唯一的 URL
Upload complete
Share link: https://send.firefox.com/download/11e17b4ea7c45d48/#jcN-NJh5m6O2oVkJiHqPLA
```

**4.2 下载语法： ffsend download \[Generated URL\]**

例如我要下载一个文件，只需要把文件URL改一改就好了

```
#输入命令加入下载文件的URL
$  ffsend download https://send.firefox.com/download/11e17b4ea7c45d48/#jcN-NJh5m6O2oVkJiHqPLA
#提示下载成功
Download complete
```

**4.3 那么同样的对于某个目录文件的直接上传语法： ffsend upload \[/Path/to/the/Directory\] --copy**

在下面的例子中，我们将上传一个名为 `test` 的目录：

```
$  ffsend upload /home/daygeek/test --copy
#出现提示
You've selected a directory, only a single file may be uploaded.
Archive the directory into a single file? [Y/n]: y   #这里输入 y 回车
Archiving...
Upload complete
Share link: https://send.firefox.com/download/90aa5cfe67/#hrwu6oXZRG2DNh8vOc3BGg
```

同样是下载这个目录文件,通过链接URL：

```
$  ffsend download https://send.firefox.com/download/90aa5cfe67/#hrwu6oXZRG2DNh8vOc3BGg

You're downloading an archive, extract it into the selected directory? [Y/n]: y
Extracting...
Download complete
```

## 5.**为文件添加密码**

上面已经通过安全、私密和加密过的链接来发送了文件，这里是继续操作的命令。

```
#输入命令：
$  ffsend upload file-copy-rsync.sh --copy --password
Password:
Upload complete
Share link: https://send.firefox.com/download/0742d24515/#P7gcNiwZJ87vF8cumU71zA
```

下载该文件时，它将要求你输入密码：

```
$  ffsend download https://send.firefox.com/download/0742d24515/#P7gcNiwZJ87vF8cumU71zA

This file is protected with a password.
Password:
Download complete
```

## 6\. 限制文件被下载的次数

```
$ ffsend upload file-copy-scp.sh --copy --downloads 10
Upload complete
Share link: https://send.firefox.com/download/23cb923c4e/#LVg6K0CIb7Y9KfJRNZDQGw
```

下载同上一样

## 7.查看下载链接的各种信息

**语法： ffsend info \[Generated URL\]**

```
#输入命令
$ ffsend info https://send.firefox.com/download/23cb923c4e/#LVg6K0CIb7Y9KfJRNZDQGw
ID: 23cb923c4e
Name: file-copy-scp.sh
Size: 115 B
MIME: application/x-sh
Downloads: 3 of 10
Expiry: 23h58m (86280s)
```

## 8.查看传输历史

**语法： ffsend history**

```
#输入命令：
$  ffsend history
# LINK EXPIRY
1 https://send.firefox.com/download/23cb923c4e/#LVg6K0CIb7Y9KfJRNZDQGw 23h57m
2 https://send.firefox.com/download/0742d24515/#P7gcNiwZJ87vF8cumU71zA 23h55m
3 https://send.firefox.com/download/90aa5cfe67/#hrwu6oXZRG2DNh8vOc3BGg 23h52m
4 https://send.firefox.com/download/a4062553f4/#yy2_VyPaUMG5HwXZzYRmpQ 23h46m
5 https://send.firefox.com/download/74ff30e43e/#NYfDOUp_Ai-RKg5g0fCZXw 23h44m
6 https://send.firefox.com/download/69afaab1f9/#5z51_94jtxcUCJNNvf6RcA 23h43m
```

## 9.删除分享链接

**语法：ffsend delete \[Generated URL\]**

```
$ ffsend delete https://send.firefox.com/download/69afaab1f9/#5z51_94jtxcUCJNNvf6RcA
File deleted
```