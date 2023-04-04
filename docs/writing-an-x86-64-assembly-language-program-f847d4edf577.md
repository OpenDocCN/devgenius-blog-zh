# 编写 X86–64 汇编语言程序

> 原文：<https://blog.devgenius.io/writing-an-x86-64-assembly-language-program-f847d4edf577?source=collection_archive---------3----------------------->

## 第七部分:快速参考

![](img/00f866da2990a84720f3e3e242c55786.png)

Alexander Schimmeck 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

本文更像是学习 x86–64 汇编语言的参考指南。我希望底部的信息和链接对您有所帮助。

# 本参考指南是系列的第七部分

*   第一部分:[开始编写汇编语言](https://medium.com/@tony.oreglia/getting-started-writing-assembly-language-8ecc116f3627)
*   第二部分:[寻找编写汇编语言的高效开发周期](https://medium.com/@tony.oreglia/finding-an-efficient-development-cycle-for-writing-assembly-language-be2092e6db6a)
*   第三部分:[编写 X86–64 汇编语言程序:打印命令行参数](https://medium.com/@tony.oreglia/writing-an-x86-64-assembly-language-program-648b6005e8e)
*   第四部分:编写 X86–64 汇编语言程序:[发送函数参数，接收返回值](https://medium.com/@tony.oreglia/writing-an-x86-64-assembly-language-program-function-arguments-and-result-dfff25970dee)
*   第五部分:编写 X86–64 汇编语言程序:[条件、跳转和循环](https://medium.com/@tony.oreglia/writing-an-x86-64-assembly-language-program-1aade03f3b9b)
*   第六部分:编写 X86–64 汇编语言程序:[如何确定字符串长度](https://tony-oreglia.medium.com/writing-an-x86-64-assembly-language-program-84e2432cf16b)

# 登记

通用寄存器—共有 16 个通用寄存器— `rax`、`rbx`、`rcx`、`rdx`、`rbp`、`rsp`、`rsi`、`rdi`、`r8`、`r9`、`r10`、`r11`、`r12`、`r13`、`r14`、`r15`

*   `data` -该部分用于声明初始化数据或常量
*   `bss` -该部分用于声明未初始化的变量
*   `text` -部分用于代码
*   `rdi` -第一个参数
*   `rsi` -第二个参数
*   `rdx`——第三个论点
*   `rcx` -第四个参数
*   `r8`——第五个论点
*   `r9` -第六名

前六个整数或指针参数在寄存器`rdi`、`rsi`、`rdx`、`rcx`、`r8`、`r9`中传递。`r10`在嵌套函数的情况下用作静态链指针。

`rax`用作函数的返回值。

寄存器`rbx`、`rbp`和`r12`–`r15`是保留寄存器。

如果调用者希望保留其值，则必须保存所有其他寄存器。

# 操作

*   `ADD` -整数相加
*   `SUB` -减去
*   `MUL` -无符号乘法
*   `IMUL` -带符号乘法
*   `DIV` -无符号除
*   `IDIV` -带符号的除
*   `INC` -增量
*   `DEC` -减量
*   `NEG` -全盘否定

初始数字必须存储在`rax`中。`rax`可以乘以任何其他寄存器中的值。结果将存储在`rax`中。

# 控制流

`JE` -如果相等则跳转

`JZ` -如果为零则跳转

`JNE` -如果不相等则跳转

`JNZ` -如果不为零则跳转

`JG` -如果第一个操作数大于第二个，则跳转

`JGE` -如果第一个操作数大于或等于第二个，则跳转

`JA` -与 JG 相同，但执行无符号比较

`JAE` -与 JGE 相同，但执行无符号比较

# 数据类型

基本数据类型是字节、字、双字、四字和双四字。

*   `byte`是八位
*   `word`是两个字节
*   `doubleword`是四个字节
*   `quadword`是八个字节
*   `double quadword`是十六个字节(128 位)。

# 。数据指令

指令是汇编语法的一部分，但与 x86 处理器指令集无关。所有汇编指令都以句点(.).

`.DATA`指令用于设置存储器中的值。

## 句法

`.DATA`指令中的语法是

【T2`define-directive``initial-value`

define 指令有五种基本形式

例如:

# 基因组数据库

显示`ecx`寄存器的值，它是一个字符指针(换句话说，打印参考的字符串):

```
display (char *) $ecx
```

注意，这将在程序执行的每个中断处显示值，如果您正在单步执行，则包括每个步骤。要停止这种行为:

```
undisplay 1
```

请注意，如果显示模式中有多个变量，该数字可以是两个、三个或其他数字。

# 更多资源

## 教程链接

**NASM**

*   [NASM 教程](http://cs.lmu.edu/~ray/notes/nasmtutorial/)
*   [NASM 教程](http://cs.lmu.edu/~ray/notes/nasmtutorial/)
*   [在 OS X 上运行组件](https://lord.io/blog/2014/assembly-on-osx/)

**GDB**

*   [GDB 在 OS X](https://lord.io/blog/2014/gdb-on-osx/)

**装配**

*   [向汇编问好(Linux x64)](https://github.com/0xAX/asm)
*   [汇编—基本语法](https://www.tutorialspoint.com/assembly_programming/assembly_basic_syntax.htm)
*   [x86–64 汇编](http://ian.seyler.me/easy_x86-64/)
*   [英特尔对 x64 的介绍](https://software.intel.com/en-us/articles/introduction-to-x64-assembly)
*   [x86 _ 64 Linux 汇编上的视频系列](https://www.youtube.com/watch?v=VQAKkuLL31g)

**码头工人**

*   [为什么以及如何使用 Docker 进行开发](https://medium.com/travis-on-docker/why-and-how-to-use-docker-for-development-a156c1de3b24)

## 快速参考链接

**NASM**

*   [x86_64 NASM 汇编快速参考](https://www.cs.uaf.edu/2009/fall/cs301/support/x86_64/index.html)

**LD 连接器**

*   [ld 链接器手册页](https://github.com/kellyi/nasm-gcc-container)

**组装**

*   [X86–64 汇编编程](https://www.engr.mun.ca/~anderson/teaching/8894/reference/x86-assembly/)
*   [X86–64 w/Ubuntu](http://www.egr.unlv.edu/~ed/assembly64.pdf)
*   英特尔 64 和 IA-32 架构软件开发人员手册[第 1 部分](https://www.cs.uaf.edu/2009/fall/cs301/support/x86_64/instructionsAM.pdf)、[第 2 部分](https://www.cs.uaf.edu/2009/fall/cs301/support/x86_64/instructionsNZ.pdf)
*   [调用约定](https://en.wikipedia.org/wiki/X86_calling_conventions)
*   [Linux 系统调用表](http://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/)

**调用堆栈**

*   [维基百科](https://en.wikipedia.org/wiki/Call_stack)

## 公用事业

**码头工人**

*   [为编写 NASM x86 汇编而配置的 Docker 容器& C](https://github.com/kellyi/nasm-gcc-container)