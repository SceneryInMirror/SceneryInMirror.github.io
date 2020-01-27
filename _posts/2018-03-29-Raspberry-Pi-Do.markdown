---
layout: post
title: "Raspberry Pi. Do"
img: raspberrypi.jpg
date: 2018-03-29 21:00:00
category: blog
author: ykx
description: One of this semester's lessons is to develop with Raspberry Pi. 1-Wire bus is a simple way to use peripheral. To begin with it, there are some problems you may meet and handle with.
tag: [Computer, RPi]
---
## 1-Wire Bus

* 实验中使用的传感器是DS18B20，不能接反否则会迅速发热烫手甚至烧坏器件；

* 使用1-wire总线需要先进入 raspi-config ，在 interfacing options 里面enable 1-wire（SSH、SPI、I2C、VNC等都采用同样的方式设置，都可利用lsmod检查）；

* 利用 lsmod 检查 w1-gpio 模块和 w1-therm 模块是否存在，若没有则：

  > sudo modprobe w1-gpio

  > sudo modprobe w1-therm

* 进入 /sys/bus/w1/devices 目录，可以看到文件夹 w1_bus_master1 和 28-* ， cat 28-*/w1_slave 可以读到温度数据；

* 如果 ls 没有任何结果，则可能是内核版本问题，需要修改 /boot/config.txt 文件，在末尾添加：

  > dtoverlay=w1-gpio-pullup, gpioin=4


**DATE: 2018-03-29**

-----

## 树莓派中文环境配置

参考文献：[http://shumeipai.nxez.com/2016/03/13/how-to-make-raspberry-pi-display-chinese.html](http://shumeipai.nxez.com/2016/03/13/how-to-make-raspberry-pi-display-chinese.html)

* scim中文输入法：

1. 安装中文字体：
  
    > sudo apt-get install ttf-wqy-zenhei

2. 安装中文输入法：
  
    > sudo apt-get install scim-pinyin
    
3. 修改树莓派配置：

    > sudo raspi-config

   在change_locale中选择zh_CN.UTF-8

4. 重启后即可

**DATE: 2018-03-30**



参考文献：[http://shumeipai.nxez.com/2015/03/11/raspberry-pi-to-install-chinese-input-method-fcitx-and-google-pinyin-ime.html](http://shumeipai.nxez.com/2015/03/11/raspberry-pi-to-install-chinese-input-method-fcitx-and-google-pinyin-ime.html)

* 另一种安装fcitx输入法及google拼音支持的方法（现在自己都是用的这种方法，沉迷google不能自拔...）

1. 安装fcitx输入法：

   > sudo apt-get install fcitx

2. 下载google拼音支持：

   > sudo apt-get install fcitx-googlepinyin

----

## 修改用户密码

参考文献：[https://blog.csdn.net/guanmaoming/article/details/78760428](https://blog.csdn.net/guanmaoming/article/details/78760428)

1. 修改pi用户密码

   > sudo passwd pi

2. 修改root用户密码

   > sudo passwd root

3. 用户切换

   > su root
   >
   > su pi

---

## 修改镜像源

参考文献：[https://blog.csdn.net/la9998372/article/details/77886806](https://blog.csdn.net/la9998372/article/details/77886806)

1. 使用管理员权限编辑`/etc/apt/sources.list`文件：

   > sudo vim /etc/apt/sources.list

2. `#`注释掉原来的内容，写入以下内容：

   ~~~Bash
   deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi
   deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi
   ~~~

3. 使用管理员权限编辑`/etc/apt/sources.list.d/raspi.list`文件：

   > sudo vim /etc/apt/sources.list.d/raspi.list

4. `#`注释掉原来的内容，写入以下内容：

   ~~~Bash
   deb http://mirror.tuna.tsinghua.edu.cn/raspberrypi/ stretch main ui
   deb-src http://mirror.tuna.tsinghua.edu.cn/raspberrypi/ stretch main ui
   ~~~

5. 更新软件源列表：

   > sudo apt-get update

---

## 需要安装的包

1. vim
2. lftp
3. curl
4. ctags

**DATE: 2018-05-22**


