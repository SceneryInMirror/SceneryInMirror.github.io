---
layout: post
title: "Serial Bus Protocol"
img: pins.jpg
date: 2018-04-22 19:08:00
category: blog
author: ykx
description: Recently while learning to develop RPi and C51, I met a lot of kinds of serial buses. Here is the summary of all these buses.
tag: [Learning, Computer]
---

## I2C总线协议

0. Inter-Integrated Circuit，**双向** 二进制 **同步** **串行** 总线，**2根线**。

1. 2根线：I2C总线在物理连接上非常简单，分别由SDA(串行数据线)和SCL(串行时钟线)及上拉电阻组成。通信原理是通过对SCL和SDA弦高低电平时序的控制，来产生I2C总线协议所需要的信号进行数据传递。在总线空闲时，这两根线一般被上面所接的上拉电阻拉高，保持着高电平。

2. 主从结构：I2C总线上的每一个设备都可以作为主设备或者从设备，而且每一个设备都会对应一个唯一的地址。在通常的应用中，我们把CPU带I2C总线接口的模块作为主设备，把挂载在总线上的其他设备都作为从设备。I2C总线上的主设备与从设备之间以字节(8位)为单位进行双向的数据传输。

3. 速率：I2C总线数据传输速率在标准模式下可达100kbit/s，快速模式下可达400kbit/s，高速模式下可达3.4Mbit/s。一般通过I2C总线接口可编程时钟来实现传输速率的调整，同时也跟所接的上拉电阻的阻值有关。

4. I2C协议：总线上数据的传输必须以一个起始信号作为开始条件，以一个结束信号作为传输的停止条件。起始和结束信号总是由主设备产生。总线在空闲状态时，SCL和SDA都保持着高电平，当SCL为高电平而SDA由高到低的跳变，表示产生一个起始条件；当SCL为高而SDA由低到高的跳变，表示产生一个停止条件。

   数据传输以字节为单位。主设备在SCL线上产生每个时钟脉冲的过程中将在SDA线上传输一个数据位，当一个字节按数据位从高位到低位的顺序传输完后，紧接着从设备将拉低SDA线，回传给主设备一个应答位，此时才认为一个字节真正的被传输完成。当然，并不是所有的字节传输都必须有一个应答位，比如：当从设备不能再接收主设备发送的数据时，从设备将回传一个否定应答位。

   主设备在传输有效数据之前要先指定从设备的地址，地址指定的过程和上面数据传输的过程一样，只不过大多数从设备的地址是7位的，然后协议规定再给地址添加一个最低位用来表示接下来数据传输的方向，0表示主设备向从设备写数据，1表示主设备向从设备读数据。



图1：I2C数据传输

![img](http://www.embeddedlinux.org.cn/uploads/allimg/130317/1219452.png)

图2：I2C地址指定

![img](http://www.embeddedlinux.org.cn/uploads/allimg/130317/1219453.png)

图3：I2C主设备写入从设备

![img](http://www.embeddedlinux.org.cn/uploads/allimg/130317/1219454.png)

图4：I2C主设备读取从设备

![img](http://www.embeddedlinux.org.cn/uploads/allimg/130317/1219455.png)

图5：I2C主设备读写从设备![img](http://www.embeddedlinux.org.cn/uploads/allimg/130317/1219456.png)


参考文献：[https://blog.csdn.net/w89436838/article/details/38660631](https://blog.csdn.net/w89436838/article/details/38660631)





## SPI总线协议

0. Serial Peripheral Interface，高速 **全双工** **同步** **串行** 通信总线，**4根线**。

1. 4条线：3条总线(SCK, MOSI, MISO)和1条片选(NSS)；SPI通信以NSS线电平置低为起始信号，以NSS线电平被拉高为停止信号。

   ![spi](https://github.com/SceneryInMirror/SceneryInMirror.github.io/blob/master/assets/img/bus_protocol/spi.jpg?raw=true)

2. 通信模式：MOSI和MISO线在SCK的每个时钟周期传输一位数据，开发者可以自行设置MSB或LSB先行，不过需要保证两个通讯设备都使用同样的协定。

   SPI有四种通讯模式，在SCK上升沿触发，下降沿采样只是其中一种模式。四种模式的主要区别便是总线空闲时SCK的状态及数据采样时刻。这涉及到“时钟极性CPOL”和“时钟相位CPHA”，由CPOL和CPHA的组合而产生了四种的通讯模式。

   - CPOL：即在没有数据传输时，时钟的空闲状态的电平。

   - CPHA：即数据的采样时刻。

   如果CPOL被清0，则SCK在空闲状态保持低电平，反之被置1则保持高电平；如果CPHA位被清0，则在SCK每个时钟周期的第1个边沿（奇数边沿）进行数据位采样，反之被置1则在SCK每个时钟周期的第2个边沿（偶数边沿）采样。

   ![CPHA](https://github.com/SceneryInMirror/SceneryInMirror.github.io/blob/master/assets/img/bus_protocol/CPHA.jpg?raw=true)

   ![mode](https://github.com/SceneryInMirror/SceneryInMirror.github.io/blob/master/assets/img/bus_protocol/mode.jpg?raw=true)

3. 通信过程：通过控制选定的NSS信号线的电平高低决定通信的起始与否。

> 查阅stm32引脚原理图可知，芯片有提供一个连接到从设备的NSS引脚。但实际中很多常常会在初始化外设时将NSS引脚配置为软件控制，换言之，就是将NSS引脚当做是一个普通GPIO引脚，而通过控制该引脚的电平状态来产生起始/停止信号。

  * 把需要发送的数据写入数据寄存器SPI_DR，该数据会被写入到发送缓冲区；
  
  * SCK开始运行，MOSI线通过移位寄存器把发送缓冲区的数据逐位传输。全双工模式下，发送和接收是同时进行的，此时MISO则把数据一位一位存进接收缓冲区；
  
  * 传输完成一帧数据后，状态寄存器SPI_SR的TXE位会被置1，表示发送缓冲区为空，可以再次往SPI_DR写入数据进行发送；RXNE位也被置1，表示接收缓冲区非空，可以读取缓冲区数据获取内容。

   ![main_mode](https://github.com/SceneryInMirror/SceneryInMirror.github.io/blob/master/assets/img/bus_protocol/main_mode.jpg?raw=true)

   全双工模式下，接收和发送是同时进行的，即使是只接收数据，也是需要向从设备发送数据，以触发SCK时钟的运行，这样从设备才能向主机发送数据。例如常常通过读取从设备的ID，以此识别设备，我们就需要向设备发送相应指令以获取设备ID（可以通过阅读设备手册获得具体指令）。


参考文献：

[https://zhuanlan.zhihu.com/p/27376153](https://zhuanlan.zhihu.com/p/27376153)

[https://zhuanlan.zhihu.com/p/27462822](https://zhuanlan.zhihu.com/p/27462822)





## UART总线协议

0. Universal Asynchronous Receiver/Transmitter，一种异步收发传输器，它将要传输的资料在串行通信与并行通信之间加以转换。作为把并行输入信号转成串行输出信号的芯片，UART通常被集成于其他通讯接口的连结上，一般是RS-232C规格的。在UART上追加同步方式的序列信号变换电路的产品，被称为USART(Universal Synchronous Asynchronous Receiver Transmitter)。

    UART是一种通用 **串行** 数据总线，**双向** **异步** 通信，可实现 **全双工** 传输和接收，**1根线**。

1. 数据收发：发送时，数据被写入发送FIFO。如果UART 被使能，则会按照预先设置好的参数（波特率、数据位、停止位、校验位等）开始发送数据，一直到发送FIFO 中没有数据。一旦向发送FIFO 写数据（如果FIFO 未空），UART 的忙标志位BUSY 就有效，并且在发送数据期间一直保持有效。BUSY 位仅在发送FIFO 为空，且已从移位寄存器发送最后一个字符，包括停止位时才变无效。即 UART 不再使能，它也可以指示忙状态。

   在UART 接收器空闲时，如果数据输入变成“低电平”，即接收到了起始位，则接收计数器开始运行，并且数据在Baud16 的第8 个周期被采样。如果Rx 在Baud16 的第8 周期仍然为低电平，则起始位有效，否则会被认为是错误的起始位并将其忽略。如果起始位有效，则根据数据字符被编程的长度，在 Baud16 的每第 16 个周期（即一个位周期之后）对连续的数据位进行采样。如果奇偶校验模式使能，则还会检测奇偶校验位。最后，如果Rx 为高电平，则有效的停止位被确认，否则发生帧错误。当接收到一个完整的字符时，将数据存放在接收FIFO 中。

2. 帧结构

<img src="https://github.com/SceneryInMirror/SceneryInMirror.github.io/blob/master/assets/img/bus_protocol/frame_structure.png?raw=true" width = "700" height = "250" alt="frame_structure" />

参考文献：

[https://baike.baidu.com/item/UART](https://baike.baidu.com/item/UART)

[https://www.cnblogs.com/lulipro/p/5994368.html](https://www.cnblogs.com/lulipro/p/5994368.html)


---


图片来源：[http://xilinx.eetrend.com/article/12037(SPI、UART、I2C三种串行总线简介)](http://xilinx.eetrend.com/article/12037   SPI、UART、I2C三种串行总线简介)

推荐阅读：[https://blog.csdn.net/ivy_reny](https://blog.csdn.net/ivy_reny)
