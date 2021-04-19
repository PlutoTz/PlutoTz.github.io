---
title: 宝塔面板安装TCShare – 一款天翼云目录列表程序可以实现天翼云直链解析
tags:
  - TCShare
  - 天翼云盘目录列表
  - 程序
id: '869'
categories:
  - - 分享
abbrlink: 50649
date: 2020-03-08 00:29:33
---

# 由于某些原因我将不在提供任何AK SK信息，抱歉！！！！

\[toc\]

## 前言

TCShare这个程序是天翼云API目录列表程序，这盘文章说说如何利用宝塔面板来部署TCShare。

项目地址:

参考我的安装环境: 宝塔面板 /Nginx1.16/PHP7.3，你也可以用自己的。可能也行吧?

## 开始

1.新建站点?

找到配置文件把一下代码填入,可以解决列网盘图片 404??的问题

```
location ~ .*\.(gifjpgjpegpngbmpswf)$
    {
        expires      30d;
       error_log off;
        access_log off;
    }
```

2.在“网站设置”→“伪静态”中设置好伪静态，代码如下：

```
try_files $uri $uri/ /index.php$is_args$args;
location ~ /\.env {
    deny all;
}
```

3.使用宝塔上传程序到网站根目录

或者使用git命令和其他命令例如:

```
cd /www/wwwroot/yun.gitiu.com
git clone https://github.com/xytoki/TCShare.git
mv TCShare/* ./
rm -rf TCShare
```

4.最重要的一步进入网站根目录，新建一个空白文件，命名为.env，填入一下代码

```
XS 是前缀
#    -KEY 是配置种类，可选KEY，APP，SEC
#     - -ct是key的ID（类似config.php）
#     -  - something是配置名称
#     -  -  - - - - value在等号右边
#   XS_KEY_ct_something=value

    XS_KEY_ct=ctyun   #必填，值为ctyun
    XS_KEY_ct_FD=     #应用文件夹名
    XS_KEY_ct_AK=     #AK
    XS_KEY_ct_SK=     #SK

#   这里APP后面的可以是任意值，一般就123456下去
#          ↓
    XS_APP_1=/              #挂载路径
    XS_APP_1_NAME=TCShare   #网盘名称
    XS_APP_1_THEME=mdui     #界面主题
    XS_APP_1_BASE=/         #网盘内路径
    XS_APP_1_KEY=ct         #对应上面Key的ID
```

5.到软件商店里找到你下载的 PHP ，点击设置找到禁用函数，移除 putenv。不移除可能安装不了composer，?切换到程序目录?，然后执行composer install。注意国内的源可以切换到阿里的?。

```
composer config repo.packagist composer https://mirrors.aliyun.com/composer/
```

6.访问域名，点击 Click here to get a token。跳转登录，获取授权。

7.，登录 天翼云盘 APP，在 我的应用 目录创建 safebox。可以把自己想分享的文件移动过来。

8.访问域名即可看到文件list??

## 重要提示??

请每个月手动为每个网盘的token续期。如，你的网盘安装在http://tcshare.website/，你需要每个月访问一次http://tcshare.website/-renew。可以在宝塔的计划任务设置一下每月定时访问 /-renew ，以延长 token 的有效期。

#### 可能你有多个天翼账号想加多块盘的

在之前的.env配置中加入一下代码，需要自己修改一些相关的内容

```
XS_KEY_ct2=ctyun   #必填，值为ctyun
XS_KEY_ct2_FD=     #应用文件夹名
XS_KEY_ct2_AK=     #AK
XS_KEY_ct2_SK=     #SK
 
XS_APP_2=/disk2         #挂载路径
XS_APP_2_NAME=TCSecond  #网盘名称
XS_APP_2_THEME=mdui     #界面主题
XS_APP_2_BASE=/         #网盘内路径
XS_APP_2_KEY=ct2        #对应上面Key的ID
```

这里我们将第二个网盘挂载到/disk2，但是你只能通过/disk2访问。一个小技巧?是：在第一个网盘里新建一个disk2文件夹，就能点击进入了，以此类推，第三块第四块……

注意：????除非你知道自己在做什么，不要把两个网盘或者多个网盘挂载到相同路径。

多盘用户可根据挂载路径进行授权，续期token。比如授权访问http://tcshare.website/disk2/（改为你的域名就行，注意后面的路径不要忘记），续期也是一样在后面加入/-renew

## 最后?

关于AK SK FD天翼云盘已经关闭api开发者申请，目前能用的没有几个。而且极有可能后期api失效，感觉也是必然如果,实在有困难可以给我私密留言获取！！

# 由于某些原因我将不在提供任何AK SK信息，抱歉！！！！