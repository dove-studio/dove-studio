---
title: Python3.8安装Scrapy爬虫框架
categories:
  - Windows
tags:
  - Python
  - Scrapy
mp3:
  - 'http://link.hhtjim.com/163/425570952.mp3'
  - 'http://link.hhtjim.com/163/425570952.mp3'
cover: /img/cover.jpg
abbrlink: 3e9ab994
date: 2020-04-05 00:11:00
---
步骤概览：
1、网络安装wheel、pyWin32
2、本地安装lxml；[lxml下载地址](https://www.lfd.uci.edu/~gohlke/pythonlibs/#lxml)
3、本地安装pyWin32；[pyWin32下载地址](https://www.lfd.uci.edu/~gohlke/pythonlibs/#pywin32)
4、本地安装Twisted；[Twisted下载地址](https://www.lfd.uci.edu/~gohlke/pythonlibs/#twisted)
5、网络安装Scrapy；

1、网络安装wheel(实际发现pyWin32、lxml也可以网络安装）

用管理员身份打开cmd > 输入

    pip install wheel
    pip install pyWin32
    pip install lxml

2、本地安装Twisted（网络安装过程中出错，没有详细研究）

    pip install xxx.whl

3、网络安装Scrapy

以管路员身份打开cmd > 输入

    pip install scrapy