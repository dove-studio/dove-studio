---
title: QNAP上安装Python3
categories:
  - NAS
  - Python
tags:
  - QNAP
  - Python
mp3:
  - 'http://link.hhtjim.com/163/425570952.mp3'
  - 'http://link.hhtjim.com/163/425570952.mp3'
cover: /img/cover.jpg
abbrlink: e2563005
date: 2020-04-02 22:02:00
---
### 通过应用商店安装
安装应用商店里面的python3之后，没有任何作用。
通过shell链接到NAS上找到安装包的位置
```
[~] # cat /share/CACHEDEV1_DATA/.qpkg/Python3/README.md
Run the following command to enter Python3 environment:
$ . /etc/profile.d/python3.bash
```

有可能会报告没有运行权限
```
[~] # ls -als /etc/profile.d/python3.bash
0 lrwxrwxrwx 1 admin administ 48 Feb 9 12:54 /etc/profile.d/python3.bash -> /share/CACHEDEV1_DATA/.qpkg/Python3/python3.bash
```

执行如下命令添加权限即可
```
[~] # chmod 744 /share/CACHEDEV1_DATA/.qpkg/Python3/python3.bash
[~] # . /etc/profile.d/python3.bash
```
但是似乎不能开机就运行这个脚本。每次重新开机都要去再运行一次，但是似乎也不是什么大问题了。

### 通过Entware-ng安装
```
opkg install python3
opkg install python3-pip
```
pip也可以通过以下命令安装
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
```