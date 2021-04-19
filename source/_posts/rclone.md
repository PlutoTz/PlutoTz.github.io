---
title: 如何在CentOS7使用rclone挂载OneDrive网盘
tags:
  - onedrive
  - rclone
id: '504'
categories:
  - - 分享
abbrlink: 41826
date: 2019-04-20 20:30:12
---

Rclone是一个多平台可以挂载OneDrive/Google Drive/Amazon Drive等云存储的软件，平台可以在Windows、Mac OS、Linux上进行使用。这篇文章主要分享CentOS7使用Rclone挂载OneDrive的过程，其它系统或者挂载其它网盘原理和方法大致相同。

**注意：**服务器最好是KVM的，OpenVZ需要给你的服务商发TK告诉他们开一下FUSE，如果没有FUSE是没办法挂载的。

首先我们需要先获取到onedrive的access\_token

获取方式：先到https://rclone.org/downloads/下载win版本的Rclone

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/BaiduShurufa_2019-8-2_15-5-16.png?x-oss-process=image/resize,m_fill,w_300,h_124#)

下载后解压，把文件夹更名为rclone

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/BaiduShurufa_2019-8-2_15-9-14.png?x-oss-process=image/resize,m_fill,w_300,h_127#)

使用CMD打开运行以下命令

```
cd /d d:\rclone 
rclone authorize "onedrive"
```

运行后会自动打开浏览器，然后登录帐号，登录后会跳转到授权成功的页面。

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/BaiduShurufa_2019-8-2_15-29-9.png?x-oss-process=image/resize,m_fill,w_300,h_183#)

授权成功后回到CMD窗口，会看到如下：

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/BaiduShurufa_2019-8-2_15-31-33.png?x-oss-process=image/resize,m_fill,w_300,h_157#)

把{}括号里面的内容复制下来保存好，后面需要用到（**包含括号一起复制保存**）

到这里我们都win下面就操作完了，现在需要到服务器去操作下。

#### 准备

先安装一下基本组件

```
yum -y install wget unzip screen fuse fuse-devel
```

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/BaiduShurufa_2019-8-2_15-44-37.png?x-oss-process=image/resize,m_fill,w_640,h_416#)

使用官方的一键安装命令即可，输入下面的命令：

```
curl https://rclone.org/install.sh  sudo bash
```

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/BaiduShurufa_2019-8-2_15-48-54.png?x-oss-process=image/resize,m_fill,w_232,h_300#)

安装完成后会提示

rclone v1.48.0 has successfully installed.  
Now run "rclone config" for setup. Check https://rclone.org/docs/ for more details.

这样我们就可以直接使用rclone config命令在设置

#### 配置

接着就输入以下命令进行rclone设置

```
rclone config
```

输入命令后

1.  2019/08/02 10:55:49 NOTICE: Config file "/root/.config/rclone/rclone.conf" not found \- using defaults
2.  No remotes found \- make a new one
3.  n) New remote
4.  s) Set configuration password
5.  q) Quit config
6.  n/s/q\> n

这里我们选择n，创建一个新的。

然后输入名字，回车

```
name> onedrive
```

这个名字随意就行，自己记住就好了

选择需要挂载的网盘

1.  Type of storage to configure.
2.  Enter a string value. Press Enter for the default ("").
3.  Choose a number from below, or type in your own value
4.  1 / A stackable unification remote, which can appear to merge the contents of several remotes
5.  \\ "union"
6.  2 / Alias for an existing remote
7.  \\ "alias"
8.  3 / Amazon Drive
9.  \\ "amazon cloud drive"
10.  4 / Amazon S3 Compliant Storage Provider (AWS, Alibaba, Ceph, Digital Ocean, Dreamhost, IBM COS, Minio, etc)
11.  \\ "s3"
12.  5 / Backblaze B2
13.  \\ "b2"
14.  6 / Box
15.  \\ "box"
16.  7 / Cache a remote
17.  \\ "cache"
18.  8 / Dropbox
19.  \\ "dropbox"
20.  9 / Encrypt/Decrypt a remote
21.  \\ "crypt"
22.  10 / FTP Connection
23.  \\ "ftp"
24.  11 / Google Cloud Storage (this is not Google Drive)
25.  \\ "google cloud storage"
26.  12 / Google Drive
27.  \\ "drive"
28.  13 / Hubic
29.  \\ "hubic"
30.  14 / JottaCloud
31.  \\ "jottacloud"
32.  15 / Koofr
33.  \\ "koofr"
34.  16 / Local Disk
35.  \\ "local"
36.  17 / Mega
37.  \\ "mega"
38.  18 / Microsoft Azure Blob Storage
39.  \\ "azureblob"
40.  19 / Microsoft OneDrive
41.  \\ "onedrive"
42.  20 / OpenDrive
43.  \\ "opendrive"
44.  21 / Openstack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
45.  \\ "swift"
46.  22 / Pcloud
47.  \\ "pcloud"
48.  23 / QingCloud Object Storage
49.  \\ "qingstor"
50.  24 / SSH/SFTP Connection
51.  \\ "sftp"
52.  25 / Webdav
53.  \\ "webdav"
54.  26 / Yandex Disk
55.  \\ "yandex"
56.  27 / http Connection
57.  \\ "http"
58.  Storage\> 19

我们今天主要是挂在onedrive，所以这里选择19，根据实际来选择，rclone版本不一样选择也不一样。

下面就是client\_id>和client\_secret> ，这两项直接回车，不管

1.  Microsoft App Client Id
2.  Leave blank normally.
3.  Enter a string value. Press Enter for the default ("").
4.  client\_id\>
5.  Microsoft App Client Secret
6.  Leave blank normally.
7.  Enter a string value. Press Enter for the default ("").
8.  client\_secret\>

是否配置高级设置，这里我们直接No，选择n

1.  Edit advanced config? (y/n)
2.  y) Yes
3.  n) No
4.  y/n\> n

是否使用自动设置，同样直接NO，选择n

1.  Use auto config?
2.  \* Say Y if not sure
3.  \* Say N if you are working on a remote or headless machine
4.  y) Yes
5.  n) No
6.  y/n\> n

这一步就需要用到我们刚才在win获取到的access\_token

1.  For this to work, you will need rclone available on a machine that has a web browser available.
2.  Execute the following on your machine:
3.  rclone authorize "onedrive"
4.  Then paste the result below:
5.  result\>

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/BaiduShurufa_2019-8-2_16-11-55.png?x-oss-process=image/resize,m_fill,w_300,h_195#)

按照图片格式把获取到的access\_token粘贴进去

这里选择1，onedrive个人版或是商业版

1.  Choose a number from below, or type in an existing value
2.  1 / OneDrive Personal or Business
3.  \\ "onedrive"
4.  2 / Root Sharepoint site
5.  \\ "sharepoint"
6.  3 / Type in driveID
7.  \\ "driveid"
8.  4 / Type in SiteID
9.  \\ "siteid"
10.  5 / Search a Sharepoint site
11.  \\ "search"
12.  Your choice\> 1

然后会提示找到一个驱动器

1.  Found 1 drives, please select the one you want to use:
2.  0: OneDrive (business) id\=b!tjyPr9WccUytxb9q4bgCz1Z8pnMGbxJCmV651K4oHDgyUnggpbbeTYTa
3.  Chose drive to use:> 0

输入前面的序号就行

下面直接yes，大概的意思是找到这个驱动器，不知道对不对

1.  Found drive 'root' of type 'business', URL: https://dddd290-my.sharepoint.com/personal/info\_vipiu\_net/Documents
2.  Is that okay?
3.  y) Yes
4.  n) No
5.  y/n\> y

然后确认

1.  y) Yes this is OK
2.  e) Edit this remote
3.  d) Delete this remote
4.  y/e/d\> y

然后退出配置

1.  e) Edit existing remote
2.  n) New remote
3.  d) Delete remote
4.  r) Rename remote
5.  c) Copy remote
6.  s) Set configuration password
7.  q) Quit config
8.  e/n/d/r/c/s/q\>

以上就配置好了，剩下的就是挂载了

#### 挂载

首先创建一个本地文件夹，我创建的是/home/onedrive ，路径可以你自己定， 也就是下面的LocalFolder

```
mkdir /home/onedrive
```

挂载为磁盘的命令如下

```
rclone mount DriveName:Folder LocalFolder --copy-links --no-gzip-encoding --no-check-certificate
--allow-other --allow-non-empty --umask 000
```

如果你和我一样设置的挂载命令是：

```
rclone mount onedrive: /home/onedrive --allow-other --allow-non-empty --vfs-cache-mode writes
```

在运行挂载命令后，SSH窗口会出现中断，光标丢失，此时关掉窗口即可。如需另外再挂载网盘，只需要重新连接。

不出问题的情况下，输入

```
df -h
```

就可以看到Onedrive成功挂载。

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/BaiduShurufa_2019-8-2_16-28-0.png?x-oss-process=image/resize,m_fill,w_300,h_153#)

*   **转载：**[CentOS7使用rclone挂载OneDrive网盘 爱游博客](https://vipiu.net/archives/2019/08/02/2172.html "本文固定链接 https://vipiu.net/archives/2019/08/02/2172.html")