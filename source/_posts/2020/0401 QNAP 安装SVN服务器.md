---
title: QNAP 安装SVN服务器
categories:
  - NAS
tags:
  - QNAP
  - SVN
mp3:
  - 'http://link.hhtjim.com/163/425570952.mp3'
  - 'http://link.hhtjim.com/163/425570952.mp3'
cover: /img/cover.jpg
abbrlink: dd326384
date: 2020-04-01 22:14:00
---
### 背景
买了一台QNAP，是x86 的机器，性能强大，最高能装16G内存。某日偶然网上发现有人做svn 服务器，于是打算试试。为啥不用Git lab呢。因为发现Container 的网络ip，与局域网不同，暂时还不知道怎么将局域网机器与之连接。再说Git lab 要8G内存，我也没升级的意思。另外我就管理下个人代码，遇不到太复杂的场景。最后svn 转 Git 也有方法，实在不行再转。重要的记录下修改历史，并保存代码。用什么也不重要。

### 安装
#### 安装Entware
我开始还以为有apt-get 或者 yum 可用，结果连rpm 都没有。网上就说用Entware，我们就用吧。
https://github.com/Entware/Entware
https://github.com/Entware/Entware/wiki/Install-on-QNAP-NAS
不过最新的Entware 是1.0.0了，http://bin.entware.net/other/Entware_1.00std.qpkg
还有一个http://bin.entware.net/other/Entware_1.00alt.qpkg
下载后手动在App Center里安装。

#### 安装Subversion
首先打开NAS上的ssh，然后Putty、Git Bash等登陆NAS。

    ssh xxx@xxx.xxx.xxx.xxx
    opkg update    
    opkg install subversion-server

通过 opkg 安装的软件启动脚本在 /opt/etc/init.d/ 目录
单个启动命令 /opt/etc/init.d/software_name start

### 配置
#### 创建仓库
我的存储路径是
/share/CACHEDEV1_DATA/svn
我在下面建了svn做为总的文件夹，仓库建在svn下面
添加仓库python
```
cd /share/CACHEDEV1_DATA/svn
mkdir python
svnadmin create python
```

#### 配置仓库
建议在svn目录下创建全局passwd，可以使所有仓库可用
```
cp python/conf/passwd passwd
vi passwd
```

在文件尾部加上用户和密码

    your_user = your_password

编辑配置文件
```
vi python/conf/svnserve.conf
```

打开文件中下列各句
```
[general]
anon-access = none
auth-access = write
password-db = ../../passwd
```

启动svn

    killall svnserve
    svnserve -d

这里启用默认端口，以及其他默认参数。

SVN客户端，windows一般选择乌龟客户端https://tortoisesvn.net/downloads.html。 
检出路径：

    svn://xxx.xxx.xxx.xxx/share/svn/python