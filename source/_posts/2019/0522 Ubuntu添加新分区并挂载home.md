---
title: Ubuntu添加新分区并挂载home
categories:
  - Linux
tags:
  - Ubuntu
mp3:
  - 'http://link.hhtjim.com/163/425570952.mp3'
  - 'http://link.hhtjim.com/163/425570952.mp3'
cover: /img/cover.jpg
abbrlink: b0b4f55e
date: 2019-05-22 15:15:38
---
### 需求
由于一开始安装只设置了/和swap分区，很快就发现空间不足，需要扩展home分区
### 磁盘准备 
查看已有的磁盘，可以看到sdb还没有分区。
```
sudo fdisk -l
```
进入sdb进行分区,输入m可以查看帮助信息
```
sudo fdisk /dev/sdb
```
输入n新建分区，输入分区号1，然后输入大小我输入的是sector扇区的开始和结束位置，也可以输入以K M G T P为单位的大小。
然后查看要创建的分区表，这时还没有创建，按w保存退出后才成功。
可以再次执行 sudo fdisk -l 查看是否创建。

然后将新分区格式化为ext4
```
sudo mkfs -t ext4 /dev/sdb6
```
### 为/home挂载新分区
1 创建临时目录，用来临时挂载新分区
```
sudo mkdir /mnt/home
```
2 将新分区挂载到新文件夹
```
sudo mount /dev/sdb6 /mnt/home
```
3 将/home目录下的文件拷贝到新分区
```
cd /home
sudo cp -ax * /mnt/home
```
拷贝时间也许较长，耐心等待。

4 重命名原/home目录，并新建一个新的空/home目录，并将新分区挂载过来
```
cd /
sudo mv /home /home.old
sudo mkdir /home
sudo mount /dev/sdb6 /home
```
5 查看uuid，找到新分区id
```
sudo blkid
```
6 找到新分区的uuid，加入/etc/fstab
```
sudo gedit /etc/fstab
```
7 加入
```
UID=78ebdca7-7632-426a-93c5-fd95be0954e2 /home           ext4    defaults          0       2
```
8 最后修改权限问题，进入新挂载的/home 查看是否都是对应文件夹对应用户的权限，不是进行更改。
```
sudo chown -R will /will
```