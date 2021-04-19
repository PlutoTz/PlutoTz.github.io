---
title: 非华为笔记本安装华为share一碰传功能
tags:
  - huaweishare
id: '545'
categories:
  - - 分享
abbrlink: 5585
date: 2019-12-13 22:46:47
---

软硬件要求：电脑端系统win10，支持无线网卡（5GWIFI）和蓝牙。手机端系统[EMUI](http://club.huawei.com/forum.php?gid=2867) 9.0/Magic UI 2.1,多屏协同需要EMUI10.0，处理器980以上。NFC标签可以用NTAG213、215、216。

## 1.**伪装SN码**

按下win+X，选择使用管理员权限打开powershell

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/222331ocj92bwkhuepzmf7-1.png)

输入wbemtest回车

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/2223318lkv4kowhofsyand-1.png)

点连接=》再点连接=》点击勾选启用所有特权=》打开类Win32\_BIOS=》增加属性Seria1Number， 数值选非NULL，填5EKPM18320000397,然后点保存对象。

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/222203ed4n03pdbwfd3u3g-1.png)

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/222204bbzldliebm1ipgc9-1.png)

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/222204xahfy6thnsgwnsmo-1.png)

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/222204dugq3atqqpvmt6cc-1.png?x-oss-process=image/resize,m_fill,w_640,h_334#)

## **2.安装电脑管家**

打开文件夹，双击PCManager\_Setup\_10.0.2.59.exe运行安装电脑管家程序，复制一份要替换的文件。右键点击任务栏，选择任务管理器，找到MateBookService.exe右键选择打开文件所在位置，然后粘贴，选择替换目标中的文件，点击继续提供管理员权限，然后这时系统会提示文件正在占用是否重试，这再次找到MateBookService.exe右键点击。先点击结束任务后迅速点击重试，替换成功后重启电脑（原教程里面没有重启电脑这一项，但是我在安装的时候，MateBookService.exe没办法自动重启，所以我通过重启电脑来让这个程序重启。

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/222205v7q6vyw1q1eyk7l6-1.png?x-oss-process=image/resize,m_fill,w_640,h_483#)

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/222205vbpnt6s5revcrvaz-1.png)

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/2222065wmfimpqgupz3sd9-1.png?x-oss-process=image/resize,m_fill,w_640,h_407#)

## **3.制作NFC标签**

按下WIN+R键，输入cmd回车.

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/222206kgpp5trrqfykom80-1.png)

输入ipconfig /all回车

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/2222072cryrpv4yo1phcks-1.png?x-oss-process=image/resize,m_fill,w_640,h_176#)

记录蓝牙网络连接的物理地址，比如我的就是这个48-E2-44-BC-7B-F6。然后把它填在

SN=5EKPM18320000397MAC=？？？？？？？？MODELID=00000505中间的“MAC=“后面，删去“-“，填充后就变成了

SN=5EKPM18320000397MAC=48E244BC7BF6MODELID=00000505。再把这串字符通过二维码转换字符转换一下，比如使用草料二维码。

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/222207ovug9tyqts7yki8m-1.png?x-oss-process=image/resize,m_fill,w_640,h_299#)

使用手机安装一碰传助手APP，然后用APP扫描一下这个二维码，根据提示用手机NFC扫一下准备好的NFC标签激活就可以正常使用了。

## **4.完成展示**

![](https://cdn.gitiu.com/wp-content/uploads/2020/02/2222072ssgcocoj9bbaz7a-1.jpg?x-oss-process=image/resize,m_fill,w_640,h_853#)

## **5.所需软件资源**
[https://pan.baidu.com/s/1MAdRdN6AE9b7yqHZU0Ce4A](https://pan.baidu.com/s/1MAdRdN6AE9b7yqHZU0Ce4A)
密码：it46
