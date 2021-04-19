---
title: 宝塔面板搭建YOURLS(yourls)-私人短链接地址服务
tags:
  - YOURLS
  - 短链接服务
id: '1329'
categories:
  - - 文章
abbrlink: 50773
date: 2020-04-15 12:21:18
---

## 1.前言

其实很早之前我就发现了**[YOURLS](https://github.com/YOURLS/YOURLS)**，遗憾的是由于英文的干扰，迫使我很难有行动下去的动力。这阵子有点时间，遂就随手搭建了一下。

官方文档：[https://yourls.org](https://yourls.org/)

项目：

### 1.1什么是**YOURLS**？

**YOURLS** stands for **Your Own URL Shortener**. It is a small set of PHP scripts that will allow you to run your own URL shortening service (_a la_ TinyURL or Bitly).

Running your own URL shortener is fun, geeky and useful: you own your data and don't depend on third-party services. It's also a great way to add branding to your short URLs, instead of using the same public URL shortener everyone uses.

大概意思就是说**YOURLS**，这个基于PHP开发的短链接服务，适用于私有，且不依赖第三方公共短链接生成。你只需要有一定的耐心和一个短域名，就可以感受到短链接生成的乐趣。

### 1.2特点

*   免费而且开源
*   使用具有两面性：私有的（仅自己使用）或者生成公共的（每个人都可以创建短链接，适用于Intranet）
*   顺序排列(从1到n)或自定义URL关键字
*   十分方便的类书签模式记录，可轻松缩短和共享链接
*   出色的统计信息：历史点击报告，引荐来源跟踪，访问者地理位置
*   整洁的Ajaxed界面
*   出色的插件架构，可轻松实现新功能
*   支持开发人员API
*   全面的jsonp支持
*   安装十分友好
*   示例文件可创建您自己的公共界面等

## 2.安装(这里以Centos7为例)

环境说明：

1.  至少**PHP 5.6  如果要使用api  还需要有curl拓展**，推荐7.2-7.3
2.  至少**MYSQL 5**，推荐5.6

### 2.1宝塔面板安装

Centos安装脚本:

`yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh`

然后登录宝塔面板，默认安装稍微改一下，按照上面的环境来！！！这里不多说。

### 2.2新建站点

主要就是填写你的**域名**，再设置添加一个**mysql数据库**

![](https://cdn.jsdelivr.net/gh/plutotz/cdn/picturebed/Snipaste_2020-04-15_11-05-00.png)

### 2.3伪静态

这一步重写路由，就是Rewrites（我这里是**nginx**）

添加一下代码：

```
  location / {
    try_files $uri $uri/ /yourls-loader.php$is_args$args;
  }
```

![](https://cdn.jsdelivr.net/gh/plutotz/cdn/picturebed/Snipaste_2020-04-15_11-06-09.png)

#### 倘若是 **apache**

(1)配置里开启  mod\_rewrite 模块

(2)创建 .htaccess  文件（这里注意分安装路径）

```
#.htaccess 文件内容，如果是根目录下  http://yoursite/ 
# BEGIN YOURLS
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^.*$ /yourls-loader.php [L]
</IfModule>
# END YOURLS

#如果是二级目录下 http://yoursite/somedir/
# BEGIN YOURLS
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /somedir/
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^.*$ /somedir/yourls-loader.php [L]
</IfModule>
# END YOURLS
```

### 2.4安装**YOURLS**

```
cd 你创建的根目录地址 如：/www/wwwroot/XXX
wget https://github.com/YOURLS/YOURLS/archive/1.7.6.tar.gz
tar -xvf YOURLS-1.7.6.tar.gz
```

或者直接下载上传，解压。

下载地址：

[https://github.com/YOURLS/YOURLS/releases](https://github.com/YOURLS/YOURLS/releases)

**2.4.1将user目录下的config-sample.php 重命名 为 config.php**

**2.4.2修改config.php里面的配置参数**

以我这里的注释为例：

```
define( 'YOURLS_DB_USER', '数据库用户名' );
define( 'YOURLS_DB_PASS', '数据库密码' );
define( 'YOURLS_DB_NAME', '数据库名字' );
define( 'YOURLS_DB_HOST', 'localhost' );
define( 'YOURLS_DB_PREFIX', 'yourls_' );
//上面是数据信息不用多说
define( 'YOURLS_SITE', 'http://' ); //你自己服务器的域名 用最短的，短地址也是基于这个生成。
define( 'YOURLS_HOURS_OFFSET', '+8'); 　　　//时区偏移　
define( 'YOURLS_LANG', '' ); 　　　　　//这个语言默认是英文
define( 'YOURLS_UNIQUE_URLS', true );　　　//短地址是否唯一　
define( 'YOURLS_PRIVATE', true );         //是否私有，如果私有的，则进行api调用生成短地址时需要传递用户名和密码
define( 'YOURLS_COOKIEKEY', 'A2C7&H~r80pTps{nIfI8VFpTxnfF3c)j@J#{nDUh' );//加密cookie 去 http://yourls.org/cookie 获取
$yourls_user_passwords = array(
    'admin' => '123456' /* Password encrypted by YOURLS */ ,  //用户名=>密码  可填多个  登录成功后这里的明文密码会被加密，这里默认是没有的，我们为了安全加上一个，如我这里是用户名是admin，密码是123456
    );
define( 'YOURLS_DEBUG', false );　　　　　　//是否开启调试　　
define( 'YOURLS_URL_CONVERT', 62 );　　　　//使用36进制 还是62进制  这个最好一开始设好不要修改，避免地址冲突，建议62进制
$yourls_reserved_URL = array(
    'porn', 'faggot', 'sex', 'nigger', 'fuck', 'cunt', 'dick',  //排除一下短地址，这些地址是不会生成的
);
```

**2.4.3添加中文语言包**

遗憾的是语言包停止更新了，现适用于此时的最新版本(1.7.3)。不过在1.7.6版本也能用。不知道作者后续会不会进行更新，可以关注一下。

**解压完毕上传到 user/languages 里面**。

## **3.访问**

**浏览器 输入 **http://域名/admin**，然后选择安装即可安装成功**。

![](https://cdn.jsdelivr.net/gh/plutotz/cdn/picturebed/Snipaste_2020-04-15_10-04-18.png)

![](https://cdn.jsdelivr.net/gh/plutotz/cdn/picturebed/Snipaste_2020-04-15_11-42-23.png)

![](https://cdn.jsdelivr.net/gh/plutotz/cdn/picturebed/Snipaste_2020-04-15_11-48-50.png)

## 4.最后

你可能遇到一些问题:

4.1如果安装报错，或者不跳转，那有可能是你的php 或者 mysql 版本过低

4.2.短链接后面从1开始，很不爽，可以推荐用时间戳来生成

只需要修改 includes/functions.php  272行左右

将 $id = yourls\_get\_next\_decimal(); 改为 $id = time();

![](https://cdn.jsdelivr.net/gh/plutotz/cdn/picturebed/Snipaste_2020-04-15_10-21-50.png)

4.3.一些YOURLS的拓展

![](https://cdn.jsdelivr.net/gh/plutotz/cdn/picturebed/Snipaste_2020-04-15_11-50-58.png)

你可以使用官方自带的插件库：

[https://github.com/YOURLS/awesome-yourls#plugins](https://github.com/YOURLS/awesome-yourls#plugins)

多的吓人！！！从0-9到A-Z排列.

4.4.api接口生成   

可以参考

[https://yourls.org/#API](https://yourls.org/#API)

请求地址：http://域名//yourls-api.php

参数：username(用户名)、password（密码）、format（格式 json）、url（长地址）、action（功能，shorturl）

返回（示例）：

```
{
  "url": {
    "keyword": "ozh",
    "url": "http:\/\/ozh.org",
    "title": "Ozh RICHARD \u00ab ozh.org",
    "date": "2014-10-24 16:01:39",
    "ip": "127.0.0.1"
  },
  "status": "success",
  "message": "http:\/\/ozh.org added to database",
  "title": "Ozh RICHARD \u00ab ozh.org",
  "shorturl": "http:\/\/sho.rt\/1f",
  "statusCode": 200
}
```

上面的  shorturl 就是生成的短链接，也可以在后台直接生成指定的短链接。

4.5.查看统计某个链接的点击情况

**可以在生成的每个短链接后面加上一个 +**

如：http://你的域名/j7fk2+ 进行访问

![](https://cdn.jsdelivr.net/gh/plutotz/cdn/picturebed/Snipaste_2020-04-15_12-06-09.png)