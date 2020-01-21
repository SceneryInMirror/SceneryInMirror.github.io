---
layout: post
title: "Migrate Linux with rsync"
img: ubuntu.jpg
date: 2018-05-04 16:10:00
category: blog
author: ykx
description: For the reason that I installed a new SSD, I need to migrate my operating system from the old HDD to the new SSD. And Fugoes suggested me to try rsync to realize it. I think it is a such effective way to migrate or backup files. It has other useful functions and I will try them later.
tag: [Learning, Computer, Linux]
---

参考文献：

[https://blog.csdn.net/dk_mcu/article/details/51864117](
https://blog.csdn.net/dk_mcu/article/details/51864117)

[www.rdeeson.com/weblog/157/moving-arch-linux-to-a-new-ssd-with-rsync](www.rdeeson.com/weblog/157/moving-arch-linux-to-a-new-ssd-with-rsync)

[https://wiki.archlinux.org/index.php/Rsync#File_system_cloning](https://wiki.archlinux.org/index.php/Rsync#File_system_cloning)

[https://www.cnblogs.com/f-ck-need-u/p/7220009.html](https://www.cnblogs.com/f-ck-need-u/p/7220009.html)

[https://wiki.archlinux.org/index.php/GRUB](https://wiki.archlinux.org/index.php/GRUB)

-----

rsync能实现Linux系统下的数据备份，相比于`cp -u`,'scp','dd'有其特殊优势，有很多关于这几种命令对比的文章。

这里使用rsync实现不同硬盘间系统备份，并从新的硬盘引导启动系统的目标。

### 分区及格式化

使用parted和mkfs命令即可，可查阅相关教程，没有太多坑。

### 备份

1. 一定认真阅读 ArchWiki 里面关于 [Full system backup](https://wiki.archlinux.org/index.php/Rsync#Full_system_backup) 的内容！

2. 使用命令：

```Bash
rsync -aAXv --exclude={"/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","/lost+found"} / /path/to/backup/folder
```

注意：
rsync一定要有合适的`--exclude`参数，否则可能进入循环无限复制直至写满！！！

### 修改fstab

进入新硬盘，编辑`/etc/fstab`（需要sudo），注意UUID与硬盘的对应关系，将旧硬盘作为挂载，新硬盘作为根文件系统；

```Bash
# 一个显示硬盘UUID的命令：
ls -l /dev/disk/by-uuid
```
### 安装os-prober

根据教程的说法，os-prober与Grub相对应，使用Grub必须install os-prober。

原ubuntu已经有os-prober了，所以不太清楚不install会怎样。


### chroot

```Bash
sudo su
cd /mnt/newarch
#将原系统proc sys dev目录下的内容挂载到新系统下
mount -t proc proc proc/
mount --rbind /sys sys/
mount --rbind /dev dev/
#改变root路径
chroot /mnt/newarch /bin/bash
```

### 重新安装bootloader

1. 因为我的机器是x86_64，分区GPT，使用EFI启动，所以教程里给出的关于i386的BIOS方法不适合我。这里使用的命令是：

   ```Bash
   mount /dev/sdb2 /boot/efi
   grub-install --target=x86_64-efi --efi-directory=/boot/efi --recheck --debug
   grub-mkconfig -o /boot/grub/grub.cfg
   ```

   其中`/dev/sdb2`是EFI分区，挂载到`/boot/efi`下，`grub-install`里的`--efi-directory`对应的就是EFI分区挂载点路径。`grub-mkconfig`更新grub配置信息，从而可以在开机启动的目录中显示新的操作系统启动路径。

2. 从chroot后的su中退出，再使用一遍上面的`grub-mkconfig`命令，则两个系统都能发现对方。

### others

1. 完成之后误操作了一次，又使用了一遍rsync命令，导致新系统内的etc/fstab信息和grub信息和原系统相同，只能重复一遍上述操作。然后就考虑如果以后还有从原系统同步到新系统的需求（貌似没有[捂脸]），后续同步时可以改一下上面的命令：

   ```Bash
   sudo rsync -aAXv /* /mnt/newarch --exclude={/dev/*,/proc/*,/sys/*,/tmp/*,/var/tmp/*,/run/*,/mnt/*,/media/*,/lost+found,/etc/fstab,/boot/efi,/boot/grub/grub.cfg}
   ```

   不过没有尝试，只是这么想想，所以嗯...慎用慎用...
   
2. 做了一件更蠢的事情...原来照抄archwiki教程，在`grub-install`命令中加入了`--bootloader-id=grub`，导致生成的操作系统信息在一个名为grub的新文件夹里，但没有更新原grub中的信息。

   结果是，在BIOS里面看到了一个SSD上的grub，grub能正常引导，但在新操作系统中使用update-grub并没有效果，反而在原系统中使用有效。

   然后以为是BIOS只能从HDD引导启动，拔了HDD发现grub配置废了，就觉得事情没有这么简单...

   之后认真比较了两个系统里`/boot/efi`下的内容，发现新系统中`grub-install`只更新了grub目录下的内容但没有更新ubuntu目录下的内容，才明白`--bootloader`是啥意思...之后就用上面的命令，更新了ubuntu目录下的内容，删了grub目录，BIOS里面看不到那个神奇的grub引导，直接进ubuntu后就是grub界面，然后就可以正常使用了。

   ```Bash
   # 原来坑死自己的命令：
   grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=grub
   ```

   拔HDD造成的结果是windows半天认不出来HDD然后磁盘检查了好久...
