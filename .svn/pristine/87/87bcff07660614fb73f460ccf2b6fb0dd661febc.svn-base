---
title: QNAP内网穿透之frpc
categories:
  - NAS
tags:
  - QNAP
  - FRP
mp3:
  - 'http://link.hhtjim.com/163/425570952.mp3'
  - 'http://link.hhtjim.com/163/425570952.mp3'
cover: /img/cover.jpg
abbrlink: 8ecaa049
date: 2020-04-05 13:33:00
---
### frps服务器
找到外网服务器，有公网IP可以自己搭建，我使用 https://waiwang.men/ 提供的
```
地址:aliyunsz.waiwang.men
特权端口:6666
Xtcp端口:6667
监控端口:7777
特权密码:waiwang.men
监控账户:waiwang.men
监控密码:waiwang.men
免费域名:*.aliyunsz.waiwang.men
单用户映射数:5
服务端版本:0.29.1
服务器宽带:独享5M
服务器流量:单向1T
支持: 88,8443,35000-50000端口穿透
服务器定时重启:每周1凌晨3点
服务器由WaiWang.Men官方提供
```

### 安装frpc
container中搜索frpc，我选择leonismoe/frpc:v0.29.1，注意frpc版本要与服务器版本一致
1 看到执行的命令为：/frpc -c /etc/frpc.ini
2 设置成自动启动，CPU和内存设置为最低
3 点开高级设置，网络模式选择Host
4 创建frpc.ini存放路径，我使用得是：/share/Document/frpc
5 设置共享文件夹/etc为：/share/Document/frpc
6 点击创建完成frpc的准备

### 配置frpc.ini
```
[common]
server_addr = aliyunsz.waiwang.men
server_port = 6666
token = waiwang.men

[xxx]
type = https                              # 也可以是http
local_ip = 192.168.0.100                  # QNAP本地IP
local_port = 443                          # QNAP提供服务的端口号
custom_domains = xxx.aliyunsz.waiwang.men # xxx为自定义子域名，有自己的域名可以替代成自己的

[xxx]
type = https
local_ip = 192.168.0.100
local_port = 5001
custom_domains = xxx.aliyunsz.waiwang.men

[xxx]
type = tcp
local_ip = 192.168.0.100
local_port = 22
remote_port = xxx # 35000-50000之间
```
xxx为自定义
重启frpc，在控制台中看到xxx start proxy success即说明穿透成功
访问： https://xxx.aliyunsz.waiwang.men:8443 (如果是http服务则是88)