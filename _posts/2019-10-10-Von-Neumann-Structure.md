---
layout:     post
title:      冯诺依曼体系结构
subtitle:   Von Neumann Structure
date:       2019-10-10
author:     BY
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - java
---
  

## 前言
    冯·诺伊曼结构（英語：Von Neumann architecture），也称馮·紐曼模型（Von Neumann model）或普林斯顿结构
    （Princeton architecture），是一种将程序指令存储器和数据存储器合并在一起的電腦設計概念结构。本詞描述
    的是一種實作通用圖靈機的計算裝置，以及一種相對於平行計算的序列式架構參考模型（referential model）。

![](https://img-blog.csdn.net/20171023175841366?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmthaWJzdw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

> 其主要特点有：
    
    1.以运算单元为中心
    2.采用存储程序原理
    3.存储器是按地址进行地址的划分
    4.控制流由指令流产生
    5.指令由操作码和地址码组成
    6.数据以二进制编码

![](https://img-blog.csdn.net/20171023180044611?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmthaWJzdw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

  例如：存储器中一条命令，这个命令是ADD 123 321,ADD表示要做的事情（相加），后面是参数相加动作的两个数。
    
    （1）通过命令记录员找到当前执行到的命令，并将命令提取出来放到命令控制器中的指令暂存处

    （2）接着控制器中的命令解释器对命令进行解释，并将解释结果传给控制信号产生器产生相应的控制信号

    （3）在控制器的控制下，将两个数再从存储器中提取出来，分别放到运算器的两个数据缓存区中（数据暂存）

    （4）接着控制器产生一个控制信号告诉电路做这两个数的加法，相加得到运算结果.

     总结：控制器从存储器取出一条命令，然后对命令进行解析，按照命令的要求把相应参与运算的数据取出，放到运算器，运算器计算获得结果,最后输出到输出设备上执行。
     完成这个命令之后再转到其他命令执行

## 存储器的结构和特点

### 衡量存储空间大小的计量单位

    在计算机中能存储的最小的数是1或0，我们讲存储一个1或0的控件称为位（Bit），通常为了使用和程序编写，将8个位称为一个字节（BYTE）.
    
### 存储器的种类

![](https://img-blog.csdn.net/20171023180229574?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmthaWJzdw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

## 存储设备

> 寄存器：
    
    （1）处于CPU内部和算术逻辑单元直接相连，算数逻辑单元直接对寄存器进行读写操作
    （2）一次存取数据花费0.X纳秒的时间，非常接近算术逻辑单元的 计算速度
    （3）很昂贵，容量很小
    
> 高速缓存Cache

     (1）通常由静态随机存储器(SRAM)制成，比寄存器便宜
    （2）高速缓存分为内部高速缓存和外部高速缓存（内部高速缓存在CPU中，外部高速缓存在主板上）
    （3）通常可以分为1到3级，不同级的工作频率不同
    （4）不需要刷新电路即能保存内部存储的数据
    
> 内存

    （1）通常由动态随机存储器 (DRAM)制成
    （2）程序普通执行时的程序指令和很多用到的数据都放在内存中
    （3）临时存放，断电丢失

> 外存

    （1）最常用的就是磁盘和闪存(Flash EPROM)
    （2）数据断电后不丢失
    （3）现在常用的外存有磁盘和固态硬盘
    （4）容量大，速度慢（相对上面提到的）

> 显存

    （1）全称是显示存储器
    （2）专门存储要显示的图像数据
    （3）一般和内存一样，由DRAM构成
    
## 程序运行原理

![](https://img-blog.csdn.net/20171023180852891?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmthaWJzdw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

> 执行的指令

    红色是指令码，黑色是操作数

    指令包含两部分：
    一部分时指令码，一部分时操作数，这里的操作数是一个地址00011110，在这个地址里面放了数据01001011,该指令要完成的事情是对数据进行加1的操作
    
> 详细执行过程
    
    （1） 程序计数器PC将指令的地址发送给地址寄存器AR
    （2）地址寄存器到响应的存储单元中将指令取出放入指令寄存器
    （3） 指令寄存器将指令交给指令译码器ID进行译码，经过分析这条指令的操作数是一个地址
    （4）控制器将指令中的地址传回地址寄存器AR
    （5）在控制器的协调下，到相应的存储器中取出数据，将其送到运算器的缓冲寄存器DR
    （6）缓冲寄存器将数据送到算数逻辑单元ALU
    （7）操作控制器发送一个加一操作的信号给算数逻辑单元ALU
    （8）ALU完成运算，并将运算结果放回到累加器中

**程序的运行**

![](https://img-blog.csdn.net/20171023181008876?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmthaWJzdw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

> 执行如下代码

```java
a = 4;
b = 3;
c = a + b;
```

_程序执行过程_

    （1）对于某个程序而言，在内存中会开辟两个区域，一个是代码区，一个是数据区，首先程序计数器指向待执行的第一条程序mov a,4 
    
![](https://img-blog.csdn.net/20171023181140737?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmthaWJzdw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

     (2)执行mov a,4,将数据区相应区域给变量a赋值为4，然后程序计数器指向下一条语句，mov b,3
     
 ![](https://img-blog.csdn.net/20171023181303267?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmthaWJzdw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

    （3）执行mov b,3,将数据区中变量b相应的位置被赋值为3，然后程序计数器指向下一条语句
    
![](https://img-blog.csdn.net/20171023181347974?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmthaWJzdw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

    （4）执行mov eax,a (eax代表运算器中的累加寄存器),控制器先去访问数据区中a的值，将值取出送到累加器中,
         然后程序计数器指向下一条语句

![](https://img-blog.csdn.net/20171023181512712?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmthaWJzdw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

    （5）执行add eax,b,即将累加器中值与b的值相加，控制器先取出数据区中b的值3，送到运算器的缓冲寄存器，
         然后通过ALU(算术逻辑单元（Arithmetic&logical Unit）)完成相加的运算，结果存放在累加器中，
         然后程序计数器指向下一条语句

![](https://img-blog.csdn.net/20171023183106247?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmthaWJzdw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

    （6）执行mov c,eax  ,含义是将累加器的值传递给变量c中,程序执行完毕

![](https://img-blog.csdn.net/20171023183224799?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmthaWJzdw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

> 小结

    程序必须通过编译才能转换为CPU可接收的指令（java通过JVM来进行编译）
    一句程序有可能转换为多句指令
    程序执行过程中是在内存中完成的
    程序在执行过程中，在内存中的不同区域来存放代码和相关数据
    










