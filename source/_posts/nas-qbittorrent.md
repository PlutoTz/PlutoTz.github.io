---
title: NAS  群晖安装第三方 qBittorrent 套件
tags:
  - Nas
  - qBittorrent
id: '1249'
categories:
  - - 文章
  - - 转载
abbrlink: 9217
date: 2020-11-10 21:59:31
---

双十一期间入了一款入门级白裙DS220+，考虑到下载问题以及还差不多能用但是后台挖矿的“玩物下载”，就迫不得已玩起来PT，遂就找到了一篇教程，权当是备份，然后转载补充一波，这几个月来越来越懒的写文章了！照例还是一篇有内容的水文，哈哈

下面是正文 来自 [NAS 群晖安装 qBittorrent 套件并优化设置、替换 UI（非 docker 安装 )](https://blog.zuiyu1818.cn/posts/NAS_qBittorrent.html)

 有部分删改，在此感谢！

本文你将看到：

*   Download Station、Transmission、qBittorrent 三款软件的对比
*   qBittorrent 套件的安装和配置
*   qBittorrent 替换 WebUI

## Download Station

[![DownloadStation](https://www.gitiu.com/wp-content/uploads/2020/11/DownloadStation.jpg "DownloadStation")](https://files.zuiyu1818.cn/NAS/DownloadStation.jpg)

Download Station

emmm，这货一言难尽吧，用的比较少一言难尽吧。这货其实就是个套着壳的 Tr。

优点：

*   群晖自带，安装简单
*   有着原生 GUI 界面，适合新手
*   自带管理软件 `DS get`

缺点：

*   看不到啥详细数据，数据控难受啊

## Transmission

[![Transmission_WebUI](https://www.gitiu.com/wp-content/uploads/2020/11/Transmission_WebUI.jpg "Transmission_WebUI")](https://files.zuiyu1818.cn/NAS/Transmission_WebUI.jpg)

Transmission\_WebUI

保种神器，占用资源小，是几乎所有 PT 站点保种首选

优点：

*   非常适合保种，万种大佬就是用的它

缺点：

*   不能跳过校验
*   校验时不能下载
*   多站点辅种时，文件块不对会爆红，挺烦的

### 权限

有人反映 tr 无法下载到其他磁盘，表现为没有下载速度，这其中有权限不对的锅。这里简单说下权限问题

[![Tr_Auth1](https://www.gitiu.com/wp-content/uploads/2020/11/Tr_Auth1.jpg "Tr_Auth1")](https://files.zuiyu1818.cn/NAS/Tr_Auth1.jpg)

[![Tr_Auth2](https://www.gitiu.com/wp-content/uploads/2020/11/Tr_Auth2.jpg "Tr_Auth2")](https://files.zuiyu1818.cn/NAS/Tr_Auth2.jpg)

tr 权限

### tracker 替换教程

站点有时会更换 tracker 服务器，经常见小伙伴们询问怎样批量替换 tracker，这里就说一下

1.  获取当前 tracker，若无属性选项卡，点击右下角即可展开
    
    [![tr_tacker1](https://www.gitiu.com/wp-content/uploads/2020/11/tr_tacker1.jpg "tr_tacker1")](https://files.zuiyu1818.cn/NAS/tr_tacker1.jpg)
    
    此处记得完整复制 ↓ ，不要只复制个开头！！！
    
    [![tr_tacker2](https://www.gitiu.com/wp-content/uploads/2020/11/tr_tacker2.jpg "tr_tacker2")](https://files.zuiyu1818.cn/NAS/tr_tacker2.jpg)
    
2.  替换 tracker
    
    [![tr_tacker3](https://www.gitiu.com/wp-content/uploads/2020/11/tr_tacker3.jpg "tr_tacker3")](https://files.zuiyu1818.cn/NAS/tr_tacker3.jpg)
    
    此功能要求字段完全匹配，所以必须完整复制过来
    
    [![tr_tacker4](https://www.gitiu.com/wp-content/uploads/2020/11/tr_tacker4.jpg "tr_tacker4")](https://files.zuiyu1818.cn/NAS/tr_tacker4.jpg)
    
3.  点击确定，等待完成即可
    

> 若种子较多，替换时间较长，此时 WebUI 将无响应不要乱动。
> 
> 替换后红种为正常现象，请耐心等待。若几 h 还是红种状态，手动暂停重开试试，若还是有问题注意关注站点公告

## qBittorrent

热种抢上传必备神器，推荐使用 4.1.x (4.1.7) 版本

### 下载地址

这里的版本适配 DSM6.2.x，x86 架构 CPU，其他自测

> 已更新至 4.2.5，后面带 `+` 为增强版，个人建议 pt 慎用

软件名称

下载地址

qBittorrent(4.1.x)

[点击下载](https://pan.baidu.com/s/1zrPk2K6joV22WwQ3pDL4kw) `r532`

qBittorrent(4.2.x)

[点击下载](https://pan.baidu.com/s/1Rp2mBbPEByD5jSGmsROmCA) `a1hm`

国旗数据

[点击下载](https://pan.baidu.com/s/1t8GpP5pZRxmj6m4lGRLEbw) `27g2`

个人使用时间最长，感觉最稳定的版本为 4.1.7，四个月守株待兔上传了 18T+，仅供参考

4.1.x 版本也是可以修改高级参数的，有一定的动手能力即可

> 4.2.x 版本不是同一个作者编译，安装过以前版本（4.1.x）的用户记得卸载以前版本，并删除 `/homes/admin/.config` 里的 qBittorrent 文件夹，再安装
> 
> 实测卸载老版本装上 4.2.x 后，种子信息都在，放心更新
> 
> 卸载后设置都会重置，默认用户及密码：admin/adminadmin
> 
> 4.1.x 的作者地址→[点击前往](http://ssd.dlinkddns.com/pub/synology/qbittorrent/)
> 
> 感谢隔壁网 [Auska](http://www.gebi1.com/space-uid-345461.html)大佬编译 4.2.x 版本

#### 注意

升级到 4.2.x 后再卸载降级到 4.1.x，会强制重新校验所有种子文件  
4.2.x 版本若出现种子信息界面无信息，请清空浏览器缓存，不建议使用修改版，出现问题的话，后果自负

#### 关于 HTTPS

4.2.x 版本，已经由以前的复制证书内容改为文件路径的模式，所以请将证书文件上传后填入`完整的路径`  
请确保 qb 对证书文件拥有读取权限，所以建议放置到 qb 的配置文件夹路径，如 `/homes/admin/.config/qBittorrent` 下

### 安装方法

1.  首先确认已启动 `admin` 用户。此套件默认运行用户为 admin，所以不启用可能会导致安装后无法运行。
    
    [![admin_sure](https://www.gitiu.com/wp-content/uploads/2020/11/admin_sure.jpg "admin_sure")](https://files.zuiyu1818.cn/NAS/admin_sure.jpg "确认admin用户已经开启")
    
    确认 admin 用户已经开启
    
2.  开启家目录。因为套件的配置文件、种子文件、校验文件等都是默认存放在家目录
    
    [![qB_home](https://www.gitiu.com/wp-content/uploads/2020/11/qB_home.jpg "qB_home")](https://files.zuiyu1818.cn/NAS/qB_home.jpg "启用家目录")
    
    启用家目录
    
3.  检查家目录下的 `admin` 文件夹权限是否正确。不放心的小伙伴可以在权限选项卡再确认下 admin 是否拥有`完全控制`的权限
    
    [![admin_auth](https://www.gitiu.com/wp-content/uploads/2020/11/admin_auth.jpg "admin_auth")](https://files.zuiyu1818.cn/NAS/admin_auth.jpg "检查权限")
    
    检查权限
    
4.  在套件中心手动安装 qBittorrent 套件
    
    [![qB_install](https://www.gitiu.com/wp-content/uploads/2020/11/qB_install.jpg "qB_install")](https://files.zuiyu1818.cn/NAS/qB_install.jpg)
    

安装完后，在套件列表里点击即可打开网页控制界面（或者手动使用 8085 端口访问）。默认用户名 `admin`密码 `adminadmin`

**\[alert\] 这里一定要访问注意端口号后的' / ',一定不能忘记\[/alert\]**

如：127.0.0.1:8085/

若8085端口打不开使用 8999 端口

\[alert\]去掉斜杠的干扰，请在qBittorrent web页面中修改，在设置-web用户界面-关闭启用跨站点请求伪造(CSRF)保护\[/alert\]

[![qB_start](https://www.gitiu.com/wp-content/uploads/2020/11/qB_start.jpg "qB_start")](https://files.zuiyu1818.cn/NAS/qB_start.jpg)

### 权限设置

有小伙伴反应，使用套件安装的 qBittorrent 下载无速度，这是因为软件读写需要文件夹拥有 admin 权限，所以给文件下载路径相应的权限即可

[![qb_auth](https://www.gitiu.com/wp-content/uploads/2020/11/qb_auth.jpg "qb_auth")](https://files.zuiyu1818.cn/NAS/qb_auth.jpg)

### 国旗数据

因为 GeoIP 修改过规则，导致 qb 无法自动下载相应数据，这时只需要在 `/homes/admin/.local/share/data/qBittorrent` 目录新建 `GeoIP` 文件夹，将国家数据 `GeoLite2-Country.mmdb` 拷贝进去，重启 qb 即可

[![GeoIP](https://www.gitiu.com/wp-content/uploads/2020/11/GeoIP.jpg "GeoIP")](https://files.zuiyu1818.cn/NAS/GeoIP.jpg)

### 替换 WebUI

目前备用 UI 尚不完善，对我而言仅仅是多了个方便分类查看 tracker 的功能，**不推荐使用**，当然随便尝鲜

这里提供两个大佬的项目

\[github author="CzBiX" project="qb-web" /\] \[github author="miniers" project="qb-web"/\]

> 若出现乱码，在地址栏后面加入 `/api/v2/app/setPreferences?json=%7B%22alternative_webui_enabled%22:false%7D` 进行返回原本 UI
> 
> 乱码解决方式，查看自己路径是否正确

#### 1\. 下载备用 WebUI

[![qB_web1](https://www.gitiu.com/wp-content/uploads/2020/11/qB_web1.jpg "qB_web1")](https://files.zuiyu1818.cn/NAS/qB_web1.jpg)

[![qB_web2](https://www.gitiu.com/wp-content/uploads/2020/11/qB_web2.jpg "qB_web2")](https://files.zuiyu1818.cn/NAS/qB_web2.jpg)

#### 2\. 解压文件至群晖

找到所在位置

[![qB_web3](https://www.gitiu.com/wp-content/uploads/2020/11/qB_web3.jpg "qB_web3")](https://files.zuiyu1818.cn/NAS/qB_web3.jpg)

在 Web 用户界面启用备用 Web UI

**补充 这里是解压后public 所在的上一级目录 然后至根目录 路径 比如是/volume1/xx/xx/xx/  最后要有' / '**

[![qB_web4](https://www.gitiu.com/wp-content/uploads/2020/11/qB_web4.jpg "qB_web4")](https://files.zuiyu1818.cn/NAS/qB_web4.jpg)

保存即可大功告成啦

[![qB_web5](https://www.gitiu.com/wp-content/uploads/2020/11/qB_web5.jpg "qB_web5")](https://files.zuiyu1818.cn/NAS/qB_web5.jpg)

### qBittorrent 高级参数设置

##### qBittorrent4.2.x 版本已经支持网页上修改高级参数了

配置文件目录：`/volume3/homes/admin/.config/qBittorrent/qBittorrent.conf`

种子文件目录：`/volume3/homes/admin/.local/share/data/qBittorrent/BT_backup`

> 请根据自己情况自己调整文件路径

默认的 web 界面很多参数都无法修改，尤其是想修改磁盘缓存。打开配置文件，在 \[Preferences\] 字段新增一行 `Downloads\DiskWriteCacheSize=XXXX`，其中 XXXX 是缓存大小，单位是 MB。

[![qb_ini](https://www.gitiu.com/wp-content/uploads/2020/11/qb_ini.jpg "qb_ini")](https://files.zuiyu1818.cn/NAS/qb_ini.jpg)

#### 内存溢出

若出现内存溢出现象，可以尝试禁用系统缓存

找到 `Advanced\osCache` 设置为 `false`  
没有的话就手动添加 `Advanced\osCache=false`

[![qb_osCache](https://www.gitiu.com/wp-content/uploads/2020/11/qb_osCache.jpg "qb_osCache")](https://files.zuiyu1818.cn/comment/qb_osCache.jpg)

> 记得设置为 false，不是图片上的 true