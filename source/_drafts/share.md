---
abbrlink: ef069d5a
---
1. 在访问Windows共享资料之前，请确保Windows共享是可用的。Linux访问Windows共享或者LInux共享资料给Windows时，都使用Samba软件

rpm -qa | grep samba-client查看samba-client是否安装。如果没有，

sudo apt-get install smbclient安装

2. 查看该用户共享权限下的共享情况。

smbclient -L //ip地址 -U 用户名

3. 挂载

建立挂载点:mkdir -p /mnt/MYSHARE

使用mount命令挂载共享文件夹

mount -o username=,password=,vers=1.0 //ip/Share /mnt/MYSHARE或者

mount -t cifs -o user=用户名,password=密码 //ip地址/共享文件夹 /mnt/MYSHARE

如果此时不能进去，并报错mount error(112),Host id down ，那么可以在password后面加上vers=版本，1.0,2.0,3.0，主要是看服务器那端是什么版本。

mount -t cifs -o user=用户名,password=密码,vers=1.0 //ip地址/共享文件夹

4. 资料使用完毕后，可以使用umount命令卸载

umount /mnt/MYSHARE

5. 关于excel读写的问题需要注意的是，可能现在xlsx格式的excel还不能写，所以需要在挂载的时候，添加两个参数

gid,uid

可以用id,username查看当前状态的uid和gid，我的是1000.

mount -o username=,password=,gid="1000",uid="1000",vers=1.0 //ip/Share /mnt/MYSHARE