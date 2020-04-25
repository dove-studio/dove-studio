---
title: Ubuntu+LAMP服务器搭建
categories:
  - Web
tags:
  - LAMP
  - Ubuntu
mp3:
  - 'http://link.hhtjim.com/163/425570952.mp3'
  - 'http://link.hhtjim.com/163/425570952.mp3'
cover: /img/cover.jpg
abbrlink: 6b81465a
date: 2019-05-12 00:29:12
---

#### 安装Apache
1 安装
``` bash
sudo apt-get install apache2
```
安装完成后打开http://localhost 如果能访问说明服务器已经能够运行

2 设置index.php优先
``` bash
sudo gedit /etc/apache2/mods-enabled/dir.conf
```
更改为首先列出index.php，修改配置文件以后，我们需要重新启动一下Apache服务器。
``` bash
sudo systemctl restart apache2
```
3 为了方便后续使用，修改`/var/www/html`的权限
``` bash
sudo chmod 777 /var/www/html
```

#### 安装MySQL
1 安装
``` bash
sudo apt-get install mysql-server
```
2 修改mysql root密码
``` bash
sudo gedit /etc/mysql/debian.cnf
```
复制debian-sys-maint的密码，等一会登陆的时候粘贴
``` bash
mysql -u debian-sys-maint -p

use mysql;
update user set authentication_string=password('Pwd@1234') where user='root';
update user set plugin="mysql_native_password"; 
flush privileges;
quit;
```

#### 安装PHP
1 安装
``` bash
sudo apt-get install php
```
2 打开mysqli扩展
``` bash
sudo apt-get install php-mysql
sudo gedit /etc/php/7.2/apache2/php.ini
```
去掉注释：extension=mysqli，保存后重启Apache即可
3 在`/var/www/html`下建立`index.php`，内容如下：
``` php
<?php
phpinfo();
?>
```
之后再打开http://localhost 可以查看PHP配置情况

#### 安装phpMyAdmin
去https://www.phpmyadmin.net/downloads/ 下载最新版程序并解压到`/var/www/html`目录下
然后登陆http://localhost/phpmyadmin 进入数据库管理网站
