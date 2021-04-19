---
title: uptime-status一款漂亮的网站监控面板
tags:
  - uptime-status
  - UptimeRobot
  - 网站监控面板
id: '955'
categories:
  - - 分享
abbrlink: 6982
date: 2020-03-15 20:48:22
---


## 前言

无意之间在[hostloc](https://www.hostloc.com)看到一贴，介绍一款比较适合监控站点的多功能网站监控面板，于是遂就水一篇文档。干就完了！uptime-status是对接 [UptimeRobot](https://uptimerobot.com) API 开发而成的。所以说部署就简单起来了。

项目地址：

项目演示站点：[https://status.org.cn/](https://status.org.cn/)

我的Demo:[https://www.gitiu.com/monitor/](https://www.gitiu.com/monitor/)

或者直接看图？？

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584267915-8a5d86c5b1db6.png)

监测界面

## 开始

1.动动你的小手去[这里](https://uptimerobot.com/signUp)注册一个账号，默认开通免费服务，虽说是免费的但是绝对够你用了。不够用上钱！！

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/uptime-status2.png)

免费额度

2.注册成功后在 [My Settings](https://uptimerobot.com/dashboard#mySettings) 中找到 API Settings开启 Monitor-Specific API Keys ，先搜索你之前起的 Friendly Name ，点击选择搜索到的项目名字，然后会得到一串数字。复制它后面有用。

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584287650-5c87954175136.png)

搜索名字

## 配置 UptimeRobot

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584276545-2802dcbd868a3.png)

监控协议选择

我们找到New Monitor,我们可以看到支持的协议，好像免费版不支持https。你根据自己的需求开始操作吧。

Tips:我建议先在 [UptimeRobot](https://uptimerobot.com/) 里面弄好你想要监控的站点，然后再安装uptime-status

![](https://cdn.gitiu.com/wp-content/uploads/2020/03/1584276545-79258c3cddd86.png)

我配置的http协议的监控

## 环境说明

目前已经在 GitHub 上放出编译好的文件，什么环境都不需要！  
不需要PHP！不需要nodejs！不需要宝塔！只需要一个支持html的纯静态空间都可以！  
包括又拍云！OSS！七牛！啥都行！cloudflare worker 什么乱七八糟的都可以！

### 安装

最**简单的食用方式**：  
1.下载最新版 **[https://github.com/yb/uptime-status/releases/latest](https://github.com/yb/uptime-status/releases/latest)**  
2\. 解压缩压缩包；  
3\. 打开config.js  
4\. 修改你的 ApiKeys（就是上面获取的 Monitor-Specific API Keys） 和其它配置（已经做了注释，去掉了代码压缩）  

```
// 配置
window.Config = {

  // 站点名
  SiteName: 'Uptime Status',

  // 站点链接
  SiteUrl: '/',

  // UptimeRobot Api Keys
  // 支持 Monitor-Specific 和 Read-Only 两只 Api Key
  ApiKeys: [
    'm784488775-dd1ad84b209c05f8e185c33e',
    'm784490063-7b5da437e7f1e0d67613714d',
    'm784497419-de55aa09902ccb3ab22d548a',
    'm784496436-71a4bf7b1e3bdf7756be131b',
  ],

  // 是否显示监测站点的链接
  ShowLink: true,

  // 日志天数
  // 虽然免费版说仅保存60天日志，但测试好像API可以获取90天的
  // 不过时间不要设置太长，容易卡，接口请求也容易失败
  CountDays: 60,

  // 导航栏菜单
  Navi: [
    {
      text: 'Homepage',
      url: 'https://status.org.cn/'
    },
    {
      text: 'GitHub',
      url: 'https://github.com/yb/uptime-status'
    }
  ]
};
```

若是想麻烦一点自己**编译安装**那么就这样

食用方法：  
1. 下载或 clone 代码；  
2. 第一次下载之后先执行 npm i 安装依赖；  
3. 改 src/config.js 中的 apikeys；  
4. 执行 npm run build 打包；  
5. 把 build 目录下的静态文件随便找个地方扔就完事了;

## 最后

  
这个代码是依赖 UptimeRobot 的，你需要先注册个 UptimeRobot。拿到apikey。检测线路的问题不是这个代码检测是 UptimeRobot 去检测的。
