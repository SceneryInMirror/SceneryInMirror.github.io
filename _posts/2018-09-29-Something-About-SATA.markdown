---
layout: post
title: "Something About SATA"
img: computer.jpg
date: 2018-09-29 00:00:00
category: blog
author: ykx
description: The problem I met in helping install CUDA.
tag: [Learning, Computer]
---

While helping my brother to install Kaldi and CUDA, I firstly chose to use Virtualbox.



The first problem I met is that Ubuntu in the Virtualbox couldn't connect to the wireless network. According to this [blog](https://blog.csdn.net/dkfajsldfsdfsd/article/details/79403343), I should make Ubuntu connected to the wireless network though NAT.	After creating NatNetwork in the setting of Virtualbox and choosing NAT in the setting of Ubuntu, it worked.



The second problem I met is that the GPU in the Virtualbox is not the one in the host, but the virtual GPU created by Virtualbox. Due to this, I could not use CUDA in the Virtualbox.



So I turned to install dual system in the computer. However, the third problem came. The SATA mode in his computer is 'Intel RST', which may be a kind of RAID. Because of this, I cannot find the hard disk when I installed Ubuntu as the second system. I should change the SATA mode to 'AHCI', but if I do this without special settings, I may not open the Windows system correctly.



According to this [blog](https://blog.csdn.net/u012986673/article/details/80738933), I should follow the a series of instructions to change the SATA mode without disarraying Win:

1. In Windows admin CMD, input "bcdedit /set {current} safeboot minimal";
2. Reboot computer, go to BIOS, change the SATA mode to 'AHCI';
3. Reboot computer, in Windows admin CMD, input "bcdedit /deletevalue {current} safeboot";
4. Reboot computer, it will start in the mode AHCI, then install the dual system.



It worked, and I finally installed Kaldi and CUDA successfully.



------

Reference:

1. https://blog.csdn.net/dkfajsldfsdfsd/article/details/79403343


2. https://blog.csdn.net/u012986673/article/details/80738933
