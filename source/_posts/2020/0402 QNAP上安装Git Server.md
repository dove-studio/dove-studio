---
title: QNAP上安装Git Server
categories:
  - NAS
tags:
  - QNAP
  - Git
mp3:
  - 'http://link.hhtjim.com/163/425570952.mp3'
  - 'http://link.hhtjim.com/163/425570952.mp3'
cover: /img/cover.jpg
abbrlink: 6baf6858
date: 2020-04-02 23:38:00
---
### 安装Git

    opkg update
    opkg install git

初始化git服务器端仓库,git仓库务必存放在非系统自带的目录下，否则系统重启之后数据会被抹掉

    cd /share/CACHEDEV1_DATA
    mkdir git
    cd git
    git init --bare python.git

为NAS添加名称为git的用户和用户群，用于所有的git仓库访问。这里最好通过NAS自带的WEB界面创建用户和用户群，且git用户无需其他目录的权限。创建之后，通过WEB界面使用git用户进行登陆，这样NAS系统会自动设置好git用户的默认目录（这个目录是/share/homes/git）。

更改git仓库目录的所有者为git用户。运行如下命令：

    chown -R git:git python.git
