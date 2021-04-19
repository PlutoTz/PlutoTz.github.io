---
title: BT种子/磁力链接下载工具：Aria2一键安装管理脚本
tags:
  - aria2
id: '562'
categories:
  - - 转载
abbrlink: 53458
date: 2018-12-09 09:43:31
---

## 安装

这里只提到了搭建后端，前端可以使用我自己搭建好的：[https://www.moerats.com/Aria2/](https://www.moerats.com/Aria2/)，或者可以参考：[一个Aria2新的更好用的Web前端：AriaNg安装教程](https://www.moerats.com/archives/255/)。

**系统要求：**`CentOS 7+`、`Debian 6+`、`Ubuntu 14.04+`

执行下面的代码下载并运行脚本：

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubiBackup/doubi/master/aria2.sh && chmod +x aria2.sh && bash aria2.sh
#备用地址
wget -N --no-check-certificate https://www.moerats.com/usr/shell/Aria2/aria2.sh && chmod +x aria2.sh && bash aria2.sh
```

运行脚本后会出现脚本操作菜单，选择并输入`1`就会开始安装。

## 使用说明

进入下载脚本的目录并运行脚本：

```
./aria2.sh
```

然后选择你要执行的选项即可。

```
Aria2 一键安装管理脚本 [vx.x.x]
-- Toyo  doub.io/shell-jc4 --
 
0. 升级脚本
————————————
1. 安装 Aria2
2. 卸载 Aria2
————————————
3. 启动 Aria2
4. 停止 Aria2
5. 重启 Aria2
————————————
6. 修改 配置文件
7. 查看 日志信息
————————————
 
当前状态: 已安装 并 已启动
 
请输入数字 [0-7]:
```

## 其他操作

启动：`service aria2 start`  
停止：`service aria2 stop`  
重启：`service aria2 restart`  
查看状态：`service aria2 status`  
配置文件：`/root/.aria2/aria2.conf`（配置文件包含中文注释，但是一些系统可能不支持显示中文）  
下载目录：`/usr/local/caddy/www/aria2/Download`（该目录为`Github`下载安装的，而备用地址下载的默认为`/usr/local/caddy/www/file`）

* * *

> 版权声明：本文为原创文章，版权归 [Rat's Blog](https://www.moerats.com/) 所有，转载请注明出处！
> 
> 本文链接：[https://www.moerats.com/archives/251/](https://www.moerats.com/archives/251/)