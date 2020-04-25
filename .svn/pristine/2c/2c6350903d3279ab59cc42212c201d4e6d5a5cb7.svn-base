---
title: QNAP上安装Aria2
categories:
  - NAS
tags:
  - QNAP
  - Aria2
mp3:
  - 'http://link.hhtjim.com/163/425570952.mp3'
  - 'http://link.hhtjim.com/163/425570952.mp3'
cover: /img/cover.jpg
abbrlink: 7bf42b13
date: 2020-04-02 23:43:00
---
安装Aria2

    opkg update
    opkg install aria2
    aria2c -v

安装成功后会打印以下信息

    aria2 version 1.33.0

    Copyright (C) 2006, 2017 Tatsuhiro Tsujikawa

    This program is free software; you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation; either version 2 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    ** Configuration **
    Enabled Features: BitTorrent, GZip, HTTPS, Message Digest, Metalink, XML-RPC, SFTP
    Hash Algorithms: sha-1, sha-224, sha-256, sha-384, sha-512, md5, adler32
    Libraries: zlib/1.2.11 libxml2/2.9.7 OpenSSL/1.0.2n libssh2/1.7.0
    Compiler: gcc 6.3.0
    built by  x86_64-pc-linux-gnu
    targeting x86_64-openwrt-linux-gnu
    on        Jan 11 2018 07:55:53

    System: Linux 4.14.24-qnap #1 SMP Fri Feb 14 09:52:34 CST 2020 x86_64

    Report bugs to https://github.com/aria2/aria2/issues
    Visit https://aria2.github.io/

By default, Aria2 is configured to start as a daemon, download any content to /opt/var/aria2/torrents, listen RPC control port 6800, which can be accessed with Passw0rd token. You can change this settings in /opt/etc/aria2.conf if necessary.

修改配置

    vim /opt/etc/aria2.conf

配置文件说明
```
## 文件保存相关 ##
 
# 文件保存目录
dir=/home/naonao/Downloads
# 启用磁盘缓存, 0为禁用缓存, 需1.16以上版本, 默认:16M
disk-cache=32M
# 断点续传
continue=true
 
# 文件预分配方式, 能有效降低磁盘碎片, 默认:prealloc
# 预分配所需时间: none < falloc ? trunc < prealloc
# falloc和trunc则需要文件系统和内核支持
# NTFS建议使用falloc, EXT3/4建议trunc, MAC 下需要注释此项
file-allocation=trunc
 
## 下载连接相关 ##
 
# 最大同时下载任务数, 运行时可修改, 默认:5
#max-concurrent-downloads=100
# 同一服务器连接数, 添加时可指定, 默认:1
# 官方的aria2最高设置为16, 如果需要设置任意数值请重新编译aria2
max-connection-per-server=256
# 整体下载速度限制, 运行时可修改, 默认:0（不限制）
#max-overall-download-limit=0
# 单个任务下载速度限制, 默认:0（不限制）
#max-download-limit=0
# 整体上传速度限制, 运行时可修改, 默认:0（不限制）
#max-overall-upload-limit=0
# 单个任务上传速度限制, 默认:0（不限制）
#max-upload-limit=0
# 禁用IPv6, 默认:false
# disable-ipv6=true
 
# 最小文件分片大小, 添加时可指定, 取值范围1M -1024M, 默认:20M
# 假定size=10M, 文件为20MiB 则使用两个来源下载; 文件为15MiB 则使用一个来源下载
min-split-size=10M
# 单个任务最大线程数, 添加时可指定, 默认:5
# 建议同max-connection-per-server设置为相同值
split=256
 
## 进度保存相关 ##
 
# 从会话文件中读取下载任务
input-file=/etc/aria2/aria2.session
# 在Aria2退出时保存错误的、未完成的下载任务到会话文件
save-session=/etc/aria2/aria2.session
# 定时保存会话, 0为退出时才保存, 需1.16.1以上版本, 默认:0
save-session-interval=60
 
## RPC相关设置 ##
 
# 启用RPC, 默认:false
enable-rpc=true
# 允许所有来源, 默认:false
rpc-allow-origin-all=true
# 允许外部访问, 默认:false
rpc-listen-all=true
# RPC端口, 仅当默认端口被占用时修改
# rpc-listen-port=6800
# 设置的RPC授权令牌, v1.18.4新增功能, 取代 --rpc-user 和 --rpc-passwd 选项
rpc-secret=yourpassword
# 启动SSL
# rpc-secure=true
# 证书文件, 如果启用SSL则需要配置证书文件, 例如用https连接aria2
# rpc-certificate=
# rpc-private-key=
 
## BT/PT下载相关 ##
 
# 当下载的是一个种子(以.torrent结尾)时, 自动开始BT任务, 默认:true
follow-torrent=true
# 客户端伪装, PT需要
peer-id-prefix=-TR2770-
user-agent=Transmission/2.77
# 强制保存会话, 即使任务已经完成, 默认:false
# 较新的版本开启后会在任务完成后依然保留.aria2文件
#force-save=false
# 继续之前的BT任务时, 无需再次校验, 默认:false
bt-seed-unverified=true
# 保存磁力链接元数据为种子文件(.torrent文件), 默认:false
# bt-save-metadata=true
# 单个种子最大连接数, 默认:55 0表示不限制
bt-max-peers=0
# 最小做种时间, 单位:分
# seed-time = 60
# 分离做种任务
bt-detach-seed-only=true
```

启动

    /opt/etc/init.d/S81aria2 start

关闭

    /opt/etc/init.d/S81aria2 stop

下载文件

    aria2c xxx -d dir

如果下载遇到以下错误：

    [SocketCore.cc:1015] errorCode=1 SSL/TLS handshake failure: unable to get local issuer certificate

请安装以下软件：

    opkg install ca-certificates

安装WebUI

[Aria2 WebUI by @ziahamza](http://ziahamza.github.io/webui-aria2/)
[YAAW WebUI by @binux](http://binux.github.io/yaaw/demo/)

下载Aria2 WebUI，然后解压到Web目录，文件夹命名为aria2，然后访问 http://xxx.xxx.xxx.xxx/aria2 即可

参考 https://github.com/Entware/Entware-ng/wiki/Using-Aria2