---
title: CENTOS服务器使用rclone开机自动挂载微软onedrive onedrive挂载为服务器磁盘方法
tags:
  - rclone挂载OneDrive
id: '511'
categories:
  - - 分享
abbrlink: 50369
date: 2019-09-10 22:55:25
---

1.先把rclone的可执行文件复制到/usr/bin：（我安装的时候它就在，所以我没动这一步）

cp /root/rclone-v\*/rclone /usr/bin/rclone(自己看路径对不对)

2.新建一个rclone.service文件： vi /usr/lib/systemd/system/rclone.service    (关键) 3.写入：

\[Unit\]
Description=rclone
    
\[Service\]
User=root
ExecStart=/usr/bin/rclone mount xxx: /xxx/xxx --allow-other --allow-non-empty --vfs-cache-mode writes
Restart=on-abort
    
\[Install\]
WantedBy=multi-user.target

4.重载daemon，让新的服务文件生效：

systemctl daemon-reload

5.现在就可以用systemctl来启动rclone了：

systemctl start rclone

6.设置开机启动：

systemctl enable rclone

7.停止、查看状态可以用：

systemctl stop rclone
systemctl status rclone

8.重启你的VPS，然后查看一下rclone的服务起来没，接着查看一下盘子挂上去没：

reboot
systemctl status rclone
df -h