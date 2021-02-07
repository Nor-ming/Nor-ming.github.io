---
title: MIPS汇编入门基础知识笔记
categories: 逆向学习笔记
tags:
- 逆向
- 汇编
---
<escape><!-- more --></escape> 

学习资料：[计算机组成与设计：硬件、软件接口(第4版)]
###### 1.
每行最多只有一条指令，注释总在一行之尾结束。
add a,b,c      #表示把b,c相加放入a中，#后是表示注释。

###### 2.
每条MIPS算术指令只执行一个操作，并且有且仅有三个变量。

###### 3．
MIPS中，只能对存放在寄存器中的数执行算术操作。

###### 4．
减法 sub

###### 5.
MIPS体系中寄存器大小为32位，故在MIPS体系中将其称为”字“。
字和寄存器的大小相同。

###### 6.
一般的，用$s0~$s31表示变量所对应的寄存器，用$t0之类的表示所需的临时寄存器。

###### 7.
数据传送指令：在存储器和寄存器之间传送数据的指令，因为MIPS的算术运算只对寄存器进行操作；为了访问存储器中的一个字，指令必须给出存储器地址。存储器就是一个很大的下标从0开始的一堆数组，地址就相当于数组的下标。

###### 8.
取数指令：lw 示例  A[8]，指令为 lw   $t0,32($s3)   $3是数组A的起始地址，又叫基址。数据传送指令中的常量（本例中的32）称作偏移量，存放基址的寄存器（本例中的$s3)称为基址寄存器。

###### 9.
常数和寄存器中的值相加即得存储器地址。

###### 10.
在MIPS中，字的起始地址必须是4的倍数，这叫对齐限制,MIPS实际上是按字节编址的，一个字有4个字节，字的地址是4的倍数。

###### 11.
存数指令：  sw  示例：变量h存放在寄存器$s2中，数组A的基址放在$s3中，编译A[12]=h+A[8];
                 lw  $t0,32($s3)
                 add   $t0,$s2,$t0
                 sw       $t0,48($s3)   

###### 12.
使用常数： 
法1：lw  $t0,AddrConstant4($s1)
         add  $s3,$s3,$t0
(假设$s1+AddrConstant4 是常量4 的存储器地址）
法2：用立即数       addi    $s3,$s3,4            # $s3=$s3+4    （而且支持副常数，不需要设置减立即数的指令)

###### 13.
数据传送指令可以被视作一个操作数为0的加法，MIPS将寄存器$zero恒置为0。
最低有效位：在MIPS字中最右边的一位。
最高有效位：在MIPS字中最左边的一位。
C和JAVA中用符号0xnnnn表示十六进制数。

###### 14.
类似if和go to语句功能的指令：
beq register1,register2,L1   表示如果register1,register2中的数值相等，则转移到标签L1的语句执行，beq代表”如果相等则分支“。
bne register1,register2,L1 表示如果两者值不相等，转到标签L1的语句执行，bne代表“如果不相等则分支“
这两条传统上称为条件分支。
条件最后要有Exit退出。
 
###### 15.
无条件分支指令：jump,简写为j,在if语句的结尾部分，需要引入另一种指令。

###### 16.
循环:
Loop:
……
……
j   Loop        (跳转到循环开始Loop标签处）
Exit:

###### 17.
$ra:用于返回起始点的返回地址寄存器
$a0~$a3:用于传递参数的四个参数寄存器
$v0~$v1:用于返回值的两个值寄存器

###### 18.
栈指针：$sp（第29号寄存器）  栈指针指示栈中最近分配的地址的值，它指示寄存器被换出的位置，或寄存器旧值的存放位置。
栈指针以字为单位进行调整

###### 19.
逻辑左移：sll   例：sll      $t1,$s3,2    左移两位（4）
逻辑右移：srl

###### 20.
压栈：将数据放入栈中
出栈：从栈中移除数据

###### 附：MIPS常用指令一览表
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200323152756435.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200323152812179.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FsaWVuRW93eW5XYW4=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200323152831498.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FsaWVuRW93eW5XYW4=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200323152843303.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FsaWVuRW93eW5XYW4=,size_16,color_FFFFFF,t_70)
