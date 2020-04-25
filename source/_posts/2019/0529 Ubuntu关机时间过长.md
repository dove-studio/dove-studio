---
title: Ubuntu关机时间过长
categories:
  - Linux
tags:
  - Ubuntu
  - MYSQL
mp3:
  - 'http://link.hhtjim.com/163/425570952.mp3'
  - 'http://link.hhtjim.com/163/425570952.mp3'
cover: /img/cover.jpg
abbrlink: 6ef3944f
date: 2019-05-29 14:15:38
---
### 问题表现
在使用中发现 LinuxMint/Ubuntu 均存在关机（重启）时间过长的问题。经过搜索，此问题不光是 LinuxMint/Ubuntu 有，其他发行版如 Arch 也比较常见。

严格来说，这个“问题”不算问题，大体原因是由于关机时，某个进程没有结束，systemd 在为此等待 90 秒。在用户看来就是关机时间过长（重启也是同样表现）。具体表现为，在点击关机后，一直显示 Splash 画面
这时，我们按键盘 Esc 键可以发现，原来是 systemd 在等待90秒。屏幕显示: A stop job is running for Session……

经过一番搜索和研究找到了解决方案。目前分析的原因，可能是 systemd 与某些软件兼容性问题。有些国外网友采取 systemd 降级的方法，虽然有效，但是太麻烦。

### 安装watchdog
1 安装 watchdog
```
sudo apt install watchdog
```
2 开启 watchdog 服务
```
sudo systemctl enable watchdog.service
```
3 马上启用 watchdog 服务
```
sudo systemctl start watchdog.service
```

### 修改超时等待时间
编辑配置文件
```
sudo gedit /etc/systemd/system.conf
```
把里面的DefaultTimeoutStopSec的值改为3s，然后载入新的配置
```
sudo systemctl daemon-reload
```

### 修改开机等待时间及画面
1 编辑配置文件
```
sudo gedit /etc/default/grub
```
2 修改如下：
```
GRUB_TIMEOUT=1
GRUB_CMDLINE_LINUX_DEFAULT=""
```
3 更新配置
```
sudo update-grub
```

### MYSQL关机超时问题
关机时系统卡住，停在Logo画面，ESC查看控制台有输出 A stop job is running for MySQL Community Server (*min *s / 10min) ，要等待计时结束才能自动关闭。
解决步骤：
1 使MySQL用户具有 /etc/mysql/debian.cnf 的读权限：
```
sudo chgrp mysql /etc/mysql/debian.cnf 
sudo chmod 640 /etc/mysql/debian.cnf 
```
2 复制一份 mysql.service 文件，并修改其访问权限：
```
sudo cp /lib/systemd/system/mysql.service /etc/systemd/system/ 
sudo chmod 755 /etc/systemd/system/mysql.service 
```
3 编辑新复制的文件，在其中添加MySQL服务停止条件：
```
sudo gedit /etc/systemd/system/mysql.service 
```
4 在文件的Service中添加一行：
```
ExecStop=/usr/bin/mysqladmin --defaults-file=/etc/mysql/debian.cnf shutdown 
```
5 载入新的配置：
```
sudo systemctl daemon-reload 
```

#### 其他几种解决办法
1 设置MySQL超时：
```
sudo gedit /etc/systemd/system/multi-user.target.wants/mysql.service 
```
修改TimeoutSec=3 

2 关机前手动停止MySQL服务：
```
sudo service mysql stop
```