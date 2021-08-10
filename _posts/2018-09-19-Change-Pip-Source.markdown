---
layout: post
title: "Change Pip Source"
img: python.jpg
date: 2018-09-19 00:00:00
category: blog
author: ykx
description: How to change Pip source in order to have a faster download speed.
tag: [Computer, Python, Linux]
---

> Ref: https://blog.csdn.net/chenghuikai/article/details/55258957

## Temporary Way

eg: pip install scrapy -i https://pypi.tuna.tsinghua.edu.cn/simple

## Long-term Way

modify '~/.pip/pip.conf':

~~~bash
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
~~~

## Chinese Pip Image

阿里云 http://mirrors.aliyun.com/pypi/simple/

中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/

豆瓣(douban) http://pypi.douban.com/simple/

清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/

中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/
