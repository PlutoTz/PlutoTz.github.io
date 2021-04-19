---
title: 使用宝塔面板开启BasicAuth  认证加固安全
tags:
  - base64_encode
  - BasicAuth认证
  - 宝塔面板
id: '1028'
categories:
  - - 日志
abbrlink: 16148
date: 2020-03-18 10:54:39
---

## 前言

[宝塔面板](http://bt.cn)相对于Linux基础较为薄弱的人来说是非常方便的,但是有时候一些安全问题总是让人头疼，今天我们通过开启宝塔登陆BasicAuth 认证， 为了防御扫描和拦截一下恶意请求，来保证宝塔的第一道防线的稳固。

## 开始

\[toc\]

1.首先，进入“面板设置”，可以在下面找到“ BasicAuth  认证 ”。

![](https://image.gitiu.com/2020/03/18/57646643d83b3.png)

2.然后开始配置，面板会出现这样的窗口。要注意红色的提示，基本上就是下面的四行。后面我会针对黑体内容补充，比如说怎样关闭 BasicAuth认证 。

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/0318111.png)

*   必须要用到且了解此功能才决定自己是否要开启!
*   开启后，以任何方式访问面板，将先要求输入BasicAuth用户名和密码
*   开启后，能有效防止面板被扫描发现，但**并不能代替面板本身的帐号密码**
*   请牢记BasicAuth密码，一但忘记将无法访问面板
*   如**忘记密码**，**可在SSH通过bt命令来关闭BasicAuth验证**

3.确定之后是下面的用户名和密码的配置。

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/03183.png)

4.填写好之后我们，我们刷新宝塔面板，可能会出现这样的提示。

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/03185.png)

有两种解决办法

```
1.查看面板入口：/etc/init.d/bt default

2.关闭安全入口：rm -f /www/server/panel/data/admin_path.pl
```

你也可以用我的方法。在SSH控制台输入bt default

4.1找到宝塔面板的入口

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/03187.png)

5.复制链接打开输入之前设置 BasicAuth认证的信息，就可以正常进入面板，但是我们还是要注意：**我们把 BasicAuth认证通过了之后，进入面板还是需要之前初始的安全入口的账号密码。**

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/03184.png)

## 如何关闭

在SSH控制台输入 bt ,按提示数字操作。

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/03186.png)

## 最后补充

我们不妨简单了解一下 BasicAuth认证的原理。

可以参考文章：[https://blog.csdn.net/ai2000ai/article/details/85775484](https://blog.csdn.net/ai2000ai/article/details/85775484)

[https://www.leiue.com/what-is-basic-auth](https://www.leiue.com/what-is-basic-auth)

> BasicAuth是最简单的一种（户名+密码）认证方式，用base64\_encode加密，仅限在一些安全要求不是那么高的场景下使用。

BasicAuth 是最简单的一种（户名+密码）认证方式，这种方式有很多问题, 比如它通过网络发送用户名和密码, 而这些都是以一种很容易解码的形式表示的。

虽然它是用 base64\_encode 加密过, 但这种加密的作用也仅仅是让可信任的用户不太可能在进行网络观测时无意中看到密码, 而不能防止恶意用户. 所以也仅限在一些安全要求不是那么高的场景下使用。

Basic Auth 是开放平台的两种认证方式，简单点说明就是每次请求 API 时都提供用户的 username 和 password。

要明白 BasicAuth 的原理和实现. 简单来说, 它是检查你的 Headers 中的 Authorization. 从中解析出 username 和 password, 和服务器保存的进行对比, 如果一致则通过, 否则返回 401/403 状态码, 表示客户端没有携带或携带了错误的认证信息。

BasicAuth 也被用于服务器或者网站的防止扫面来使用。