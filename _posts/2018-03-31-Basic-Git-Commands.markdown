---
layout: post
title: "Basic Git Commands"
img: git.jpg
date: 2018-03-31 12:49:00
category: blog
author: yxk
description: The basic git commands commonly used.
tag: [Git, Computer, Learning]
---

### 新建库

> git init

### 关联远程库

> git remote add origin git@github.com:[user-name]/[repo-name].git

### 推送关联当前master分支

> git push -u origin master

此后每次提交不需要参数-u

### 更新上传

> git add -A

> git commit -m "[message]"

> git push

### 用户名邮箱配置

> git config [--global] user.name "[yourname]"

> git config [--global] user.email "[youremail]"

或者直接修改 .git/config 文件

### 克隆远程库

> git clone git@github.com:[user-name]/[repo-name].git 


