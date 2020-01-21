---
layout: post
title: "SSH Under Campus Network"
img: ssh.jpg
date: 2018-03-30 22:42:00
category: blog
author: ykx
description: As I have more than one computers, I want to ssh one by another one. Here are the ways to realize ssh under campus network.
tag: [Computer, Network]
---

### 两台机器都在路由器下

* 直接使用路由器局域网下分配的 IP 192.168.*.* 访问，端口22

* 树莓派用了有线和无线两种不同的连接方式，对应路由器下的IP不同

* 利用MAC地址固定IP：
	
  * lenovo-z41-70: 192.168.1.101

  * thinkpad: 192.168.1.102

  * raspberry pi(eth0): 192.168.1.100

  * raspberry pi(wlan0): 192.168.1.104

* 除了SSH还可以使用VNC，图形化远程

### 一台机器在路由器下一台机器在校园网下

* 两台机器可以互相访问

* 路由器下机器访问校园网下机器直接SSH对方IP即可，无线 10.2.*.*，有线 10.128.*.*

* 校园网下机器访问路由器下机器要通过设置路由器的端口映射实现：

  * 192.168.1.102 -> 21

  * 192.168.1.104 -> 80

### 两台机器都在校园网下

* 无法通过IP地址SSH访问对方，不论是IPv4还是IPv6

* 互访的一种间接方法是：先SSH到路由器下的机器上，再SSH到对方机器

### 上传下载

参考文献：[https://www.cnblogs.com/magicc/p/6490566.html](https://www.cnblogs.com/magicc/p/6490566.html)

* 上传本地文件到服务器

  > scp /path/filename username@servername:/path/

  > eg: scp /var/www/test.php root@192.168.0.101:/var/www/

* 从服务器上下载文件

  > scp username@servername:/path/filename /var/www/local_dir

* 上传目录到服务器

  > scp -r local_dir username@servername:remote_dir

  > eg: scp -r test root@192.168.0.101:/var/www/

* 从服务器下载整个目录

  > scp -r username@servername:/var/www/remote_dir/ /var/www/local_dir


### 额外发现

* Fugoes同学提醒，连接Wireless PKU不做网页登录时是可以访问IPv6的，并且ifconfig发现此时也已经分配到IPv4地址，但IPv4不通

* Linux主机做server之前需要apt-get install openssh-server，RPi做server之前要在raspi-config里面enable SSH

