---
layout: post
title: "Initrd and Initramfs"
img: initrd.jpg
date: 2018-08-09 00:00:00
category: blog
author: ykx
description: Learning about Initrd and Initramfs, which play important roles in switching on the computer and loading the operating system.
tag: [Learning, Computer]
---

### Initrd (Initial Ramdisk)

* 参考文献：https://en.wikipedia.org/wiki/Initial_ramdisk

In computing (specifically in regards to Linux computing), **initrd** (initial ramdisk) is a scheme for loading a temporary root file system into memory, which may be used as part of the Linux startup process. **initrd** and **initramfs** refer to two different methods of achieving this. Both are commonly used too make preparations before the real root file system can be mounted.

#### Rationale

Many Linux distributions ships a single, generic Linux kernel image - one that the distribution's developers create specifically to boot on a wide variety of hardware. The device drivers for this generic kernel image are included as loadable kernel modules because statically compiling many drivers into one kernel causes the kernel image to be much larger, perhaps too large to boot on computers with limited memory. This then raises the problem of detecting and loading the modules necessary to mount the root file system at boot time, or for that matter, deducing where or what the root file system is.

To further complicate matters, the root file system may be on a software RAID volume, LVM, NFS (on diskless workstations), or on an encrypted partition. All of these require special preparations to mount.

Another complication is kernel support for hibernation, which suspends the computer to disk by dumping an image of the entire contents of memory to a swap partition or a regular file, then powering off. On next boot, this image has to be made accessible before it can be loaded back into memory.

To avoid having to hardcode handling for so many special cases into the kernel, an initial boot stage with a temporary root file-system - now dubbed early user space - is used. This root file-system can contain user-space helpers which do the hardware detection, module loading and device discovery necessary to get the real root file-system mounted.

#### Implementation

An image of this initial root file system (along with the kernel image) must be stored somewhere accessible by the Linux bootloader or the boot firmware of the computer. This can be the root file system itself, a boot image on an optical disc, a small partition on a local disk (a boot partition, usually using ext2 or FAT file systems), or a TFTP server (on systems that can boot from Ethernet).

The bootloader will load the kernel and initial root file system image into memory and then start the kernel, passing in the memory address of the image. At the end of its boot sequence, the kernel tries to determine the format of the image from its first few blocks of data, which can lead either to the initrd or initramfs scheme.

In the initrd scheme, the image may be a file system image (optionally compressed), which is made available in a special block device (/dev/ram) that is then mounted as the initial root file system. The driver for that file system must be compiled statically into the kernel. Many distributions originally used compressed ext2 file system images. while the others (including Debian 3.1) used cramfs in order to boot on memory-limited systems, since the cramfs image can be mounted in-place without requiring extra space for decompression. Once the initial root file system is up, the kernel executes /linuxrc as its first process; when it exits, the kernel assumes that the real root file system has been mounted and executes /sbin/init to begin the normal user-space boot process.

In the initramfs scheme (available since the Linux kernel 2.6.13), the image may be a cpio archive (optionally compressed). The archive is unpacked by the kernel into a special instance of a tmpfs that becomes the initial root file system. This scheme has the advantage of not requiring an intermediate file system or block drivers to be compilted into the kernel. Some systems use the dracut package to create an initramfs image. In the initramfs scheme, the kernel executes /init as its first process that is not expected to exit. For some applications, initramfs can use the casper utility to create a writable environment using unionfs to overlay a persistence layer over a read-only root filesystem image. For example, overlay data can be stored on a USB flash drive, while a compressed SquashFS read-only image stored on a live CD acts as a root filesystem.

Depending on which algorithms were compiled statically into it, the kernel can unpack initrd/initramfs images compressed with gzip, bzip2, LZMA, XZ, LZO, and LZ4.

---

### Booting

* 参考文献：https://en.wikipedia.org/wiki/Booting

In computing, **booting** is starting up a computer or computer appliance until it can be used. It can be initiated by hardware such as a button press, or by software command. After the power is switched on the computer is relatively dumb, and can read only part of its storage called **Read-only memory**. There a small program is stored called **firmware**. It does power-on self-tests, and most importantly, allows accessing other types of memory, like **hard disk** and **main memory**. The firmware loads bigger programs into the computer's main memory and runs it. In general purpose computers, but as well in smart phones, tablets, optionally a **boot manager** is run. It lets a user choose which operating system to run, and set more complex parameters for it. The firmware of the boot manager then loads the **boot loader** into the memory and runs it. This piece of software is able to place an **operating system kernel** like Windows or Linux into the computers main memory, and run it. Afterwards, the kernel runs so called **user space software**, well known is the **graphical user interface** which lets the user log in to the computer or run some other applications. The whole process may take seconds to tenths of seconds on modern day general purpose computers.

Restarting a computer also is called reboot which can be "hard", e.g., after eletrical power to the CPU is switched from off to on, or "soft", where the poer is not cut. On some systems, a soft boot may optionally clear RAM to zero. Both hard and soft booting can be initiated by hardware such as a button press, or by software command. Booting is complete when the operative runtime system, typically operating system and some applications, is attained.

The process of returning a computer from a state of hibernation or sleep does not involve booting. Minimally, some embedded systems do not require a noticeable boot sequence to begin functioning and when turned on may simply run operational programs that are stored in ROM. All computing systems are state machines, and a reboot may be the only method to return to a designated zero-state from an unintended, locked state.

In addition to loading an operating system or stand-alone utility, the boot process can also load a storage dump program for diagnosing problems in an operating system.

Boot is short for bootstrap or bootstrap load and derives from the phrase to pull oneself up by one's bootstraps. The usage calls attention to the requirement that, if most software is loaded onto a computer by other software already running on the computer, some mechanism must exist to load the initial software onto the computer. Early computers used a variety of ad-hoc methods to get a small program into memory to solve this problem. The invention of read-only memory (ROM) of various types solved this paradox by allowing computers to be shipped with a start up program that could not be erased. Growth in the capacity of ROM has allowed ever more elaborate start up procedures to be implemented.

#### Modern boot loaders

When a computer is turned off, its software-including operating systems, application code, and data-remains stored on non-volatile memory. When the computer is powered on, it typically does not have an operating system or its loader in random-access memory (RAM). The computer first executes a relatively small program stored in read-only memory (ROM) along with a small amount of needed data, to access the nonvolatile device or devices from which the operating system programs and data can be loaded into RAM.

The small program that starts this sequence is known as a bootstrap loader, bootstrap or boot loader. This small program's only job is to load other data and programs which are then executed from RAM. Often, multiple-stage boot loaders are used, during which several programs of increasing complexity load one after the other in a process of chain loading.

##### First-stage boot loader

Boot loaders may face peculiar constraints, especially in size; for instance, on the IBM PC and compatibles, a boot sector should typically work in only 32KB (later relaxed to 64KB) of system memory and not use instructions not supported by the original 8088/8086 processors. The first stage of PC boot loaders (FSBL, first-stage boot loader) located on fixed disks and removable drives must fit into the first 446 bytes of the Master Boot Record in order to leave room for the default 64-byte partition table with four partition entries and the two-byte boot signature, which the BIOS requires for a proper boot loader - or even less, when additional features like more than four partition entries (up to 16 with 16 bytes each), a disk signature(6 bytes), a disk timestamp(6 bytes), and Advanced Active Partition(18 bytes) or special multi-boot loaders have to be supported as well in some environments. ...

Examples of first-stage bootloaders include coreboot, Libreboot and Das U-Boot.

##### Second-stage boot loader

Second -stage boot loaders, such as GNU GRUB, BOOTMGR, Syslinux, NTLDR or BootX, are not themselves operating systems, but are able to load an operating system properly and transfer execution to it; the operating system subsequently initializes itself and may load extra device drivers. The second-stage boot loader does not need drivers for its own operation, but may instead use generic storage access methods provided by system firmware such as the BIOS or Open Firmware, though typically with restricted hardware functionality and lower performance.

Many boot loaders (like GNU GRUB, Windows's BOOTMGR, and Windows NT/2000/XP's NTLDR) can be configured to give the user multiple booting choices. These choices can include different operating systems (for dual or multi-booting from different partitions or drives), different versions of the same operating system (in case a new version has unexpected problems), different operating system loading options (e.g., booting into a resecue or safe mode), and some standalone programs that can function without an operaing system, such as memory testers (e.g., memtest86+), **a basic shell (as in GNU GRUB)**, or even games (see List of PC Booter games). Some boot loaders can also load other boot loaders; for example, **GRUB loads BOOTMGR instead of loading Windows directly**. Usually a default choice is preselected with a time delay during which a user can press a key to change the choice; after this delay, the default choice is automatically run so normal booting can occur without interaction.

The boot process can be considered complete when the computer is ready to interact with the user, or the operating system is capable of running system programs or application programs.

Many embedded systems must boot immediately. For example, waiting a minute for a digital television or a GPS navigation device to start is generally unacceptable. Therefore, such devices have software systems in ROM or flash memory so the device can begin functioning immediately; little or no loading is necessary, because the loading can be precomputed and stored on the ROM when the device is made.

Large and complex systems may have boot procedures that proceed in multiple phases until finally the operating system and other programs are loaded and ready to execute. Because operating systems are designed as if they never start or stop, a boot loader might load the oeprating system, configure itself as a mere process within that system, and then irrevocably transfer control to the operating system. The boot loader then terminates normally as any other process would.

#### Personal computers (PC)

##### Boot devices

The boot device is the device from which the operating system is loaded. A modern PC's UEFI or BIOS firmware supports booting from various devices, typically a local solid state drive or hard disk drive via the **GPT** or **Master Boot Record (MBR)** on such a drive or disk, an optical disk drive (using El Torito), a USB mass storage device (FTL-based flash drive, SD card, or multi-media card slot; hard disk drive, optical disk drive, etc.), or a network interface card (using PXE). Older, less common BIOS-bootable devices include floppy disk drives, SCSI devices, Zip drives, and LS-120 drives.

##### Boot sequence

Upon starting, and IBM-compatible personal computer's x86 CPU executes, in real mode, the instruction located ate reset vector (the physical memory address FFFF0h on 16-bit x86 processors and FFFFFFF0h on 32-bit and 64-bit x86 processors), usually pointing to the **firmware (UEFI or BIOS)** entry point inside the ROM. This memory location typically contains a jump instruction that transfers execution to the location of the firmware (UEFI or BIOS) start-up program. This program runs a power-on self-test (POST) to check and initialize required devices such as DRAM and the PCI bus (including running embedded ROMs). The most complicated step is setting up DRAM over SPI, made more difficult by the fact that at this point memory is very limited.

After initializing required hardware, the firmware (UEFI or BIOS) goes through a pre-configured list of non-volatile storage devices ("boot device sequence") until it finds one that is bootable. A bootable MBR device is defined as one that can be read from, and where the last two bytes of the first sector contain the little-endian word AA55h, found as byte sequence 55h, AAh on disk (also known as the MBR boot signature), or where it is otherwise established that the code inside the sector is executable on x86 PCs.

Once the BIOS has found a bootable device it loads the boot sector to linear address 7C00h (usually segment:offset 0000h:7C00h, but some BIOSes erroneously use 07C0h:0000h) and transfers execution to the boot code. In the case of a hard disk, this is referred to as the Master Boot Record (MBR) and is by definition not operating-system specific. The conventional MBR code checks the MBR's partition table for a partition set as bootable (the one with active flag set). If an active partition is found, the MBR code loads the boot sector code from that partition, known as Volume Boot Record (VBR), and executes it. The VBR is often operating-system specific; however, in most operating systems its main function is to load and execute the operating system kernel, which continues startup.

If there is no active partition, or the active partition's boot sector is invalid, the MBR may load a secondary boot loader which will select a partition (often via user input) and load its boot sector, which usually loads the corresponding operating system kernel. In some cases, the MBR may also attempt to load secondary boot loaders before trying to boot the active partition. If all else fails, it should issue an INT 18h BIOS interrupt call (followed by an INT 19h just in case INT 18h would return) in order to give back control to the BIOS, which would then attempt to boot off other devices, attempt a remote boot via network or invoke ROM BASIC.

---

### Initrd

Linux初始化RAM磁盘（initrd）是在系统引导（boot，引导程序bootstrap）过程中挂载的一个临时根文件系统，用来支持两阶段的引导过程。initrd文件中包含了各种可执行程序和驱动程序，它们可以用来挂载实际的根文件系统，然后再将这个initrd RAM磁盘卸载，并释放内存。在很多嵌入式Linux系统中，initrd就是最终的根文件系统。

initrd是在实际根文件系统可用之前挂载到系统中的一个初始根文件系统。initrd与内核绑定在一起，并作为内核引导过程的一部分进行加载。内核然后会将这个initrd文件作为其两阶段引导过程的一部分来加载模块，这样才能稍后使用真正的文件系统，并挂载实际的根文件系统。

initrd中包含了实现这个目标所需要的目录和可执行程序的最小集合，例如将内核模块加载到内核中所使用的insmod工具。

在桌面或服务器Linux系统中，initrd是一个临时的文件系统。其生存周期很短，只会用作到真实文件系统的一个桥梁。在没有存储设备的嵌入式系统中，initrd是永久的根文件系统。

### Initramfs

Initramfs是在ramfs的cache实现上加了一层很薄的封装，其他内核开发人员编写了一个改进版tmpfs，这个文件系统上的数据可以写出到交换分区，而且可以设定一个tmpfs装载点的最大尺寸以免耗尽内存。initramfs就是tmpfs的一个应用。

最初的想法是Linus提出的：把cache当做文件系统装载。

---

power-on -> firmware in ROM (UEFI or BIOS) -> power-on self-test (POST, accessing hard disk and main memory) -> load boot manager into main memory -> load boot loader (e.g., GRUB) -> place operating system kernel into main memory -> run user space software (GUI)



    initrd和GRUB，initrd和内核，initrd和根文件系统，文件系统和操作系统

    1. GRUB中知识简单地把initrd拷贝在内存中指定的位置，没有对它做进一步的操作，由内核来初始化这段内存区域。GRUB不参与Linux内核的初始化，它是与内核独立的。
    2. 文件系统时操作系统用于明确存储设备（常见的是磁盘，也有基于NAND Flash的固态硬盘）或分区上的文件的方法和数据结构；即在存储设备上组织文件的方法。
    3. 操作系统中负责管理和存储文件信息的软件机构称为文件管理系统，简称文件系统。
    4. 根文件系统简单来说就是系统第一个mount的文件系统
    5. ramdisk是在ram上实现的块设备，initrd是启动过程中用到的一种机制。在装载linux之前，bootloader可以把一个比较小的根文件系统的映像装载在内存的某个指定位置，然后通过传递参数的方式告诉内核initrd的起始地址和大小，在启动阶段就可以暂时用initrd来mount根文件系统。initrd最初的目的是为了把kernel的启动分成两个阶段，在kernel中保留最少最基本的启动代码，然后把对各种恶样硬件设备的支持以模块的方式放在initrd中，这样就在启动过程中可以从initrd所mount的根文件系统中装载需要的模块。这样的一个好处是在保持kernel不变的情况下，通过修改initrd中的内容就可以灵活的支持不同的硬件。在启动完成的最后阶段，根文件系统可以重新mount到其他设备上，但是也可以不再重新mount（很多嵌入式系统就是这样）。
    6. initrd具体实现过程：bootloader把根文件系统映像装载到内存指定位置，把相关参数传递给内核，内核启动时把initrd中的内容复制到ramdisk中（ram0），把initrd占用的内存释放掉，在ram0上mount根文件系统。从这个过程中可以看出，内核需要同时对ramdisk和initrd的支持。
    7. Linux启动不必一定使用initrd，如果把需要的功能全部编译到内核中（非模块方式），只需要一个内核文件即可。initrd能够减小启动内核的体积并增加灵活性。
    8. GRUB是file-system sensiive的，能够识别常见的文件系统
    9. initrd是linux在系统引导过程中使用的一个临时的根文件系统，用来支持两阶段的引导过程。再白话一点，initrd就是一个带有根文件系统的虚拟RAM盘，里面包含了根目录‘/’，以及其他的目录，比如：bin，dev，proc，sbin，sys等linux启动时必须的目录，以及在bin目录下加入了一下必须的可执行命令。


