---
title: "Git Instructions -- I"
layout: post
date: 2019-05-04 00:00
img: git.jpg
headerImage: True
tag: [Computer, Git, Learning]
category: blog
author: ykx
description: Some Git instructions I've used.
---

> Ref:
> [删除远程分支](https://www.cnblogs.com/luosongchao/p/3408365.html)，
> [新建分支与合并](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)，
> [分支管理](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E7%AE%A1%E7%90%86)，
> [**fork的仓库与原仓库保持同步**](https://blog.csdn.net/starter_____/article/details/79321962)，
> [远程仓库的使用](https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E7%9A%84%E4%BD%BF%E7%94%A8)


### 分支相关操作

* 显示分支：

~~~
git branch
~~~

* 显示所有分支：

~~~
git branch -a
~~~

* 查看分支信息：

~~~
git branch -v
~~~

* 新建分支：

~~~
git checkout -b [new-branch-name]
~~~

相当于两条命令：

~~~
git branch [new-branch-name]
git checkout [new-branch-name]
~~~

* 切换分支（不合并）：

~~~
git checkout [branch-name]
~~~

* 删除远程分支：

~~~
git push origin --delete [remote-branch-name]
~~~

* 删除本地分支：

~~~
git branch -d [branch-name]
~~~


### fork的仓库与原仓库保持同步

* 查看远程仓库：

~~~
git remote -v
~~~

* 添加指向原仓库的upstream：

~~~
git remote add upstream [repo-address]
~~~

* 从原仓库拉取代码：

~~~
git pull upstream master
~~~

由此也可以看出完整的push/pull命令应该是：

~~~
git push/pull origin/upstream/... [branch-name]
~~~

