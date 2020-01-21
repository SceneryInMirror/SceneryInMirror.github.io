---
layout: post
title: "PCB Design Debug"
img: pcb.jpg
date: 2018-04-28 23:00:00
category: blog
author: ykx
description: Yesterday and today's afternoons, I spent time with HXX to debug the design of PCB. Here are the conclusion we reached.
tag: [Learning, Computer, Diary]
---

> WARNING(SPMHNI-192): Device/Symbol check warning detected.
>
> WARNING(SPMHNI-337): Unable to load symbol ...
>
> WARNING(SPMHUT-127):Could not find padstack ...

元件symbol有问题，可能出错的地方有：

1. .dra文件设计出错，psmpath、padpath未包含对应.psm、.pad文件路径(文件名大小写不明感)；

2. .dra调用的padstack没有对应的.pad文件，可以用Tools->Padstack->Replace替换成可用的焊盘文件(输入原始焊盘名和新焊盘名后要点Replace，极有可能被挡住...)；

3. 对应的.pad文件因为版本问题不能用Pad Designer打开，需要使用DB Doctor更新。

焊盘的编号及数量要与电路图及真实模型相对应，修改对应text即可；

设计PCB板过程中的导入网表操作，需要的有关元件封装文件包括：.pad(焊盘文件)，.dra(封装模型)，.psm(元件数据)，.fsm(flash焊盘数据)以及其他一系列.Xsm。

封装的概念：从广义上讲，封装不仅仅是指 把集成电路做成芯片 这种操作，百度百科里面的定义是，用导线把电路管脚引出，方便与其他电路模块连接，同时起到密封保护、增强散热等作用的设计都可以叫封装。比如电阻，可能本身就是一段阻值较大的导线，而我们平时用的直插式电阻或者贴片电阻，都是看不到那段导线的，它们都是用外壳包起来的，然后用铜导线在外壳两端引出作为引脚。所以平时用到的所有分立器件还有各种芯片，都可以认为是封装好的元件，只不过直插式的外形可能比较多样，然后贴片式的都很接近芯片那种方块的形状。就理解成是，封装是实际使用的物理模型，电路图是逻辑模型，两个要一一对应起来。

------
向HXX同学表示感谢~
