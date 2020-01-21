---
layout: post
title: "Something about BIOS"
img: bios.jpg
date: 2018-03-04 23:16:00
category: blog
author: ykx
description: When I want to realize the multibooting in USB device, I met some problems about the concept of BIOS. This is the note when I learn it.
tag: [Learning, Computer]
---

### BIOS & EFI
* BIOS：Basic Input Output System，基本输入输出系统。**BIOS是个人电脑启动时加载的第一个软件。它是一组固化到计算机内主板上一个ROM芯片上的程序，它保存着计算机最重要的基本输入输出的程序、开机后自检程序和系统自启动程序，它可从CMOS中读写系统设置的具体信息。** 其主要功能是为计算机提供最底层的、最直接的硬件设置和控制。此外，BIOS还向作业系统提供一些系统参数。系统硬件的变化是由BIOS隐藏，程序使用BIOS功能而不是直接控制硬件。现代作业系统会忽略BIOS提供的抽象层并直接控制硬件组件。

* EFI：Extensible Firmware Interface，可拓展固件接口。一种在未来的类PC的电脑系统中替代BIOS的升级方案。EFI技术源于Intel Itanium平台的推出，它是一个新生的处理器架构，系统固件和操作系统之间的接口都可以完全重新定义。

* 比较EFI BIOS和Legacy BIOS：EFI BIOS是用模块化、C语言风格的参数堆栈传递方式，动态链接的形式构建的系统，运行于32位或64位模式乃至未来增强的处理器模式下，突破传统16位代码的寻址能力，达到处理器的最大寻址。它利用加载EFI驱动的形式，识别及操作硬件，不同于BIOS利用挂载实模式中断的方式增加硬件功能。EFI系统下的驱动并不是由可以直接运行在CPU上的代码组成的，而是用EFI Byte Code(EBC)编写而成的，这是一组专用于EFI驱动的虚拟机器指令，必须在EFI驱动运行环境(Driver Execution Environment，或DXE)下被解释运行，这就保证了充分的向下兼容性。

* BIOS主要程序：中断例程，系统设置程序，POST上电自检，自检程序

  BIOS功能：自检及初始化，程序服务处理，硬件中断处理

  EFI组成：Pre-EFI初始化模块，EFI驱动执行环境，EFI驱动程序，兼容性支持模块（CSM），EFI高层应用，GUID磁盘分区

* BIOS进入方式

  Award BIOS：按“Del”键

  AMI BIOS：按“Del”或“ESC”键

  Phoenix BIOS：按“F2”键

  acer：按“Del”键

  ibm（冷开机按f1，部分新型号可以在重新启动时启动按f1）

  hp（启动和重新启动时按f2）

  sony（启动和重新启动时按f2）

  dell（启动和重新启动时按f2）

  acer（启动和重新启动时按f2）

  toshiba（冷开机时按esc然后按f1）

  hp compaq（开机到右上角出现闪动光标时按f10，或者开机时按f10）

  fujitsu（启动和重新启动时按f2）

  绝大多数国产和台湾品牌（启动和重新启动时按f2）


### MBR & GPT
* MBR：Master Boot Record，即硬盘主引导记录，是位于磁盘最前边的一段引导（Loader）代码。包含MBR引导代码的扇区称为主引导扇区，或MBR扇区，不属于磁盘上的任何分区，由三部分组成：1. 主引导程序即主引导记录（MBR），2. 磁盘分区表项（DPT）， 3. 结束标志。

  MBR分区表只支持容量在 **2.1TB(2TiB)** 以下的硬盘，超过2.1TB的硬盘只能管理2.1TB，最多只支持 **4个主分区或三个主分区和一个扩展分区，扩展分区下可以有多个逻辑分区** 。

* GPT：GUID Partition Table，全局唯一标识分区表，是EFI标准的一部分，被用于替代BIOS系统中的一32bits来存储逻辑块地址和大小信息的主开机纪录（MBR）分区表。

  与MBR最大4个分区表项的限制相比，**GPT对分区数量没有限制**，但Windows最大仅支持128个GPT分区，GPT可管理硬盘大小达到了 **18EB**，只有基于 **UEFI** 平台的主板才支持GPT分区引导启动。

* MBR分区结构：引导代码、Windows磁盘签名、MBR分区表和MBR结束标志共计4部分。位于磁盘的0柱面、0磁头、1扇区。

* GPT分区结构：保护MBR、GPT头、分区表、分区区域、分区表备份、GPT头备份（http://yuedu.biz/wp-content/uploads/2014/04/147.jpg ）。解决了MBR只能分4个主分区的缺点，并且支持大硬盘，分区结构清晰简单而且有备份。

* MBR和GPT在硬盘上的区别：在MBR硬盘中，分区信息直接存储于主引导记录（MBR）中。但在GPT硬盘中，分区表的位置信息存储在GPT头中。但出于兼容性考虑，磁盘的第一扇区仍然用作MBR，之后才是GPT头。跟现代的MBR一样，GPT也使用逻辑区块位址（LBA）取代了早期的CHS寻址方式。传统MBR信息存储于LBA 0,GPT头存储于LBA 1，接下来才是分区表本身。64位Windows使用32扇区作为GPT分区表，接下来的LBA 34才是硬盘上第一个分区的开始。为了减少分区表损坏的风险，GPT在硬盘最后保存了一份分区表的副本。

* LBA 0：在GPT分区表的最开头，出于兼容性考虑仍然存储了一份传统的MBR，用来防止不支持GPT的硬盘管理工具错误识别并破坏硬盘中的数据，这个MBR也叫做保护MBR。在支持从GPT启动的操作系统中，这里也用于存储第一阶段的启动代码。在这个MBR中，只有一个标识为0xEE的分区，以此来表示这块硬盘使用GPT分区表。不能识别GPT硬盘的操作系统通常会识别出一个未知类型的分区，并且拒绝对硬盘进行操作，除非用户特别要求删除这个分区。这就避免了意外删除分区的危险。另外，能够识别GPT分区表的操作系统会检查保护MBR中的分区表，如果分区类型不是0xEE或者MBR分区表中有多个项，也会拒绝对硬盘进行操作。

  在使用MBR/GPT混合分区表的硬盘中，这部分存储了GPT分区表的一部分分区（通常是前四个分区），可以使不支持从GPT启动的操作系统从这个MBR启动，启动后只能操作MBR分区表中的分区。如Boot Camp就是使用这种方式启动Windows。

  LBA 1：分区表头定义了硬盘的可用空间以及组成分区表的项的大小和数量。在使用64位Windows Server 2003的机器上，最多可以创建128个分区，即分区表中保留了128个项，其中每个都是128字节。（EFI标准要求分区表最小要有16,384字节，即128个分区项的大小）分区表头还记录了这块硬盘的GUID，记录了分区表头本身的位置和大小（位置总是在LBA 1）以及备份分区表头和分区表的位置和大小（在硬盘的最后）。它还储存着它本身和分区表的CRC32校验。固件、引导程序和操作系统在启动时可以根据这个校验值来判断分区表是否出错，如果出错了，可以使用软件从硬盘最后的备份GPT中恢复整个分区表，如果备份GPT也校验错误，硬盘将不可使用。所以GPT硬盘的分区表不可以直接使用16进制编辑器修改。主分区表和备份分区表的头分别位于硬盘的第二个扇区（LBA 1）以及硬盘的最后一个扇区。备份分区表头中的信息是关于备份分区表的。

  LBA 2-33：GPT分区表使用简单而直接的方式表示分区。一个分区表项的前16字节是分区类型GUID。例如，EFI系统分区的GUID类型是{C12A7328-F81F-11D2-BA4B-00A0C93EC93B}。接下来的16字节是该分区唯一的GUID（这个GUID指的是该分区本身，而之前的GUID指的是该分区的类型）。再接下来是分区起始和末尾的64位LBA编号，以及分区的名字和属性。

* 采用GPT格式分区装系统，所需要的系统必须是win7 x64位以上的，并且主板支持UEFI启动模式。GPT格式分区最少要分三个区：1.EFI系统保护区（默认隐藏不加载） 2.MSR微软保留分区 3.系统数据分区。

* EFI BIOS只能识别FAT32分区。

### BIOS/UEFI、MBR/GPT搭配情况

* BIOS+MBR：**可用，可启动系统**。最常见最传统的方法，系统都会支持；唯一的缺点就是不支持容量大于2T的硬盘。

* BIOS+GPT：**MBR/GPT混合分区表可用**。

~~~C
下面的说法存在疑问：

BIOS是可以使用GPT分区表内MBR分区表外的硬盘来作为资料盘的，但不能引导系统；若电脑同时带有荣了小于2T的硬盘和容量大于2T的硬盘，小于2T的可以用MBR分区表安装系统，而大于2T的可以使用GPT分区表来存放资料也没什么问题，但系统需使用64位系统（主硬盘BIOS+MBR装系统、软件，次硬盘BIOS+GPT存储文件，这是很多影音文件发烧友在使用的模式）。
~~~

* UEFI+MBR：**没有意义**。可以把UEFI设置成Legacy模式（传统模式），打开CSM兼容模块，让其支持传统MBR启动，但纯属瞎折腾，带来的效果同BIOS+MBR。

* UEFI+GPT：**可用，可启动系统**。最常见，未来的趋势。如果要把大于2T的硬盘作为系统盘来安装系统的话，就必须如此。而且系统须使用64位系统，否则无法引导。但系统又不是传统在PE下安装后就能直接使用的，引导还得经过处理才行。UEFI和GPT是相辅相成的。

### GRUB

* GNU GRUB：GRand Unified Bootloader，是一个来自GNU项目的多操作系统启动程序。GRUB是多启动规范的实现，它允许用户可以在计算机内同时拥有多个操作系统，并再计算机启动时选择希望运行的操作系统。GRUB可用于选择操作系统分区上的不同内核，也可用于向这些内核传递启动参数。

* 在x86架构的机器中，Linux、BSD或其它Unix类的操作系统中GRUB、LILO是主流。

##### 参考文献

https://baike.baidu.com/item/bios/91424?fr=aladdin （BIOS百度百科）

https://baike.baidu.com/item/GPT/15413476 (GPT百度百科)

https://www.cnblogs.com/xiaochina/p/5812085.html （EFI+GPT装系统）

https://www.zhihu.com/question/28471913 （知乎关于BIOS/UEFI+MBR/GPT的问答）

https://www.itsk.com/thread-345631-1-1.html （使用 Easy Image X 在“UEFI+GPT”模式下装系统）

##### 图片来源

http://www.pconline.com.cn/images/html/viewpic_pconline.htm?http://img0.pconline.com.cn/pconline/1611/21/2474316_bios_chip_plcc_socket.jpg&channel=8967

-----
### Supplement from Wikipedia

* The MBR holds the information on how the logical partitions, containing file systems, are organized on that medium. The MBR also contains executable code to function as a loader for the installed operating system—usually by passing control over to the loader's second stage, or in conjunction with each partition's volume boot record (VBR). This MBR code is usually referred to as a boot loader.

  The organization of the partition table in the MBR limits the maximum addressable storage space of a disk to 2 TiB (232 × 512 bytes).[2] Approaches to slightly raise this limit assuming 33-bit arithmetics or 4096-byte sectors are not officially supported, as they fatally break compatibility with existing boot loaders and most MBR-compliant operating systems and system tools, and can cause serious data corruption when used outside of narrowly controlled system environments. Therefore, the MBR-based partitioning scheme is in the process of being superseded by the GUID Partition Table (GPT) scheme in new computers. **A GPT can coexist with an MBR in order to provide some limited form of backward compatibility for older systems.**

  Multiple forms of hybrid MBRs have been designed and implemented by third parties in order to maintain partitions located in the first physical 2 TiB of a disk in both partitioning schemes "in parallel" and/or to allow older operating systems to boot off GPT partitions as well. 

*  Although it forms a part of the Unified Extensible Firmware Interface (UEFI) standard (Unified EFI Forum proposed replacement for the PC BIOS), it is also used on some BIOS systems because of the limitations of master boot record (MBR) partition tables, which use 32 bits for storing logical block addresses (LBA) and size information on a traditionally 512-byte disk sector.

	All modern PC operating systems support GPT. Some, including macOS and Microsoft Windows on x86, support booting from GPT partitions only on systems with EFI firmware, but FreeBSD and most Linux distributions can boot from GPT partitions on systems with both legacy BIOS firmware interface and EFI.

* Protective MBR (LBA 0): For limited backward compatibility, the space of the legacy MBR is still reserved in the GPT specification, but it is now used in a way that prevents MBR-based disk utilities from misrecognizing and possibly overwriting GPT disks. This is referred to as a protective MBR.

  Hybrid MBR (LBA 0 + GPT): In operating systems that support GPT-based boot through BIOS services rather than EFI, the first sector is also still used to store the first stage of the bootloader code, but modified to recognize GPT partitions. The bootloader in the MBR must not assume a sector size of 512 bytes.

----


感谢 [Fugoes](https://github.com/Fugoes) 同学指出关于MBR及GPT概念上的错误～
