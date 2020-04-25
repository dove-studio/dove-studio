---
title: 基于Ubuntu的STM32调试环境搭建
categories:
  - STM32
tags:
  - STM32
  - STLINK
  - JLINK
  - VSCODE
  - STM32CubeMX
mp3:
  - 'http://link.hhtjim.com/163/425570952.mp3'
  - 'http://link.hhtjim.com/163/425570952.mp3'
cover: /img/cover.jpg
abbrlink: 743468b
date: 2019-05-12 23:37:42
---
### 安装gcc-arm-none-eabi
``` bash
sudo apt-get install gcc-arm-none-eabi
```
上述安装没有gdb，使用vscode调试需要使用用以下方式进行安装
```
sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa
sudo apt-get update
sudo apt-get install gcc-arm-embedded
```

### 安装STM32CubeMX
1 STM32CubeMX是32-bit程序，如果是64-bit系统，需要安装32-bit compliant packages such as ia32-libs.
在Ubuntu18.04中已经没有ia32-libs，使用lib32ncurses5 lib32z1替代
``` bash
sudo apt-get install lib32ncurses5 lib32z1
```

2 STM32CubeMX是java程序，需要JRE支持，可以安装Oracle JDK或者OpenJDK
[Oracle Java](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

OpenJDK is a free and open-source implementation of the Java Platform, Standard Edition licensed under the GNU General Public License version 
```
sudo apt install default-jre
sudo apt install openjdk-11-jre-headless
sudo apt install openjdk-8-jre-headless 
```

Oracle JDK
```
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java11-set-default
```

测试是否安装成功
```
java --version
```

3 进入软件目录开始安装
```
sudo ./SetupSTM32CubeMX-5.0.0.linux
```

### 调试环境准备
#### 安装openocd
```
sudo apt-get install openocd
```

#### 如果使用stlink，安装stlink驱动
1 准备stlink驱动安装环境
```
sudo apt-get install libusb-dev
sudo apt-get install libusb-1.0
sudo apt-get install cmake
sudo apt-get install libgtk-3-dev
```

2 下载源码
```
git clone https://github.com/texane/stlink.git
cd stlink
```

3 编译及安装

```
make clean
make release
make debug
cd build
cmake -DCMAKE_BUILD_TYPE=Debug ..
make
cd Release
sudo make install
ldconfig
```

#### 如果使用Jlink，安装Jlink驱动
1 首先安装readline
```
sudo apt-get install libreadline6-dev
```
2 去[jlink](https://www.segger.com/downloads/jlink/)下载安装程序，然后安装
```
sudo dpkg -i JLink_Linux_V644h_x86_64.deb
```

#### 配置udev
1 生成`49-link.rules`文件
```
sudo gedit /etc/udev/rules.d/49-link.rules
```
输入以下内容：
```
SUBSYSTEMS=="usb", ATTRS{idVendor}=="1366", ATTRS{idProduct}=="0102", \
    MODE:="0666", \
    SYMLINK+="jlink_%n"
 
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="3744", \
    MODE:="0666", \
    SYMLINK+="stlinkv1_%n"
 
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="374b", \
    MODE:="0666", \
    SYMLINK+="stlinkv2-1_%n"
 
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="3748", \
    MODE:="0666", \
    SYMLINK+="stlinkv2_%n"
```

2 使udev生效
```
sudo /etc/init.d/udev restart
```

### 下载程序
#### 使用stlink
```
openocd -f interface/stlink-v2.cfg -f target/stm32f1x.cfg 2>/dev/null &
```
#### 使用Jlink
```
openocd -f interface/jlink.cfg -f target/stm32f1x.cfg 2>/dev/null &
```
#### 下载
```
telnet localhost 4444
halt
flash write_image erase *.hex
reset
```
下载过程有时候会提示失败，需要reset后再下载

### 配置vscode
#### 安装相关插件
* 安装插件 ARM；
* 安装插件 Cortex-Debug;
* 安装插件 C/C++; 必要插件，否则无法调试。
* 安装插件 C/C++ Clang Comamnd Adapter; 用来补全和诊断，需要同时安装Clang，参考官方文档。
* 安装插件 Uncrustify; 用来格式化代码， shift+alt+f，非常方便。缺点是代码中有Unicode可能会导致乱码，然后配置文件有点多。
* 安装插件 Bracket Pair Colorizer; 不同颜色高亮显示匹配括号，爱护视力必备。


#### 编辑launch.json
```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Cortex Debug",
            "cwd": "${workspaceRoot}",
            "executable": "./build/*.elf",
            "request": "launch",
            "type": "cortex-debug",
            "servertype": "openocd",
            "BMPGDBSerialPort": "/dev/ttyACM0",
            "runToMain": true,
            "configFiles": [
                "interface/jlink.cfg",
                "target/stm32f1x.cfg"
            ]
        }
    ]
}
```

### 安装串口软件
推荐cutecom，界面化，支持HEX收发
```
sudo apt-get install cutecom
```
