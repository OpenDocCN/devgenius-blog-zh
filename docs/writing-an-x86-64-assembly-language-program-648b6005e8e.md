# 编写 X86–64 汇编语言程序

> 原文：<https://blog.devgenius.io/writing-an-x86-64-assembly-language-program-648b6005e8e?source=collection_archive---------0----------------------->

## 第三部分:打印命令行参数

![](img/d75c190dee26ae1438b6185872728ab0.png)

照片由[福蒂斯·福托普洛斯](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

现在我们已经建立了一个高效的开发环境，是时候写一些实际的汇编语言代码了。在本指南中，我将分享我构建一个简单的汇编语言程序的资源和过程。

# 打印命令行参数

为了演示这些概念，让我们编写一个汇编程序，计算一个文本文件中唯一单词的数量，该文件的名称作为参数提供。

作为实现这一目标的第一步，程序需要访问命令行参数。第一个程序是关于理解如何使用 x86_64 Linux assembly 访问命令行参数。

如果你是汇编编程的新手，我建议从本指南开始学习一些基础知识。我在这里引用了一些概念，没有进一步的解释。

![](img/5f77d3cd169abf16187fd72d5db44843.png)

加布里埃尔·海因策在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 什么是命令行参数

命令行参数是程序用户在执行程序时包含在命令行中的文本输入。例如，如果有一个名为`a.out`的可执行程序，它可以在 Linux 系统上从命令行用命令运行

```
$ ./a.out
```

除此之外，“命令行参数”可以包含在可执行文件名称之后。举个例子，

```
$ ./a.out 1 2 3
```

在这种情况下，有三个参数:1、2 和 3。

# 调用堆栈

这将有助于理解调用堆栈，因为这是程序开始执行时命令行参数存储的地方。

如果您不熟悉，[堆栈](https://en.wikipedia.org/wiki/Call_stack)是驻留在计算机 RAM 中的后进先出(LIFO)数据结构。堆栈指针跟踪堆栈的“顶部”。在 x86_64 中，堆栈指针是寄存器`$rpi`。`$rpi`是指向堆栈的“最后一个”内存地址的指针。可以使用堆栈指针来访问堆栈，也可以使用两个汇编语言命令:`push`和`pop`。`push`可用于向堆栈添加一个值，并更新`$rpi`以指向这个新添加的值。`pop`可用于从堆栈中获取“最后一个”值，并更新`$rpi`以指向堆栈中的下一个值。这些命令的格式是(注意，在汇编语言中分号用于表示注释，因此编译器会忽略`;`之后的任何内容):

```
; add the value of $rcx to the stack
**push rcx**; pop the "Last In" stack value into $rbx. 
; In this case, the value of $rcx
**pop rbx**
```

# 在 X86–64 汇编语言中访问命令行参数

当程序开始执行时，任何命令行参数都存储在堆栈中。堆栈的顶部将保存参数的数量。如果您已经在`c`或`c++`中编程，这在`main()`中被称为`argc`，意思是参数计数。

堆栈上的第二个值是函数名。这被视为第一个参数，并包括在参数总数中。因此，如果在执行程序时不提供参数,`argc`将等于 1。

提供的任何参数都将是堆栈中的下一个值。例如，如果您用命令执行了一个程序

```
./a.out arg1 arg2 arg3
```

该堆栈将如下所示

## 堆栈

堆栈指针($rpi)将指向堆栈的顶部，即`argc`值 4 的存储地址。当然，这些值都是用二进制编码的。

该程序打印出`argc`和程序名，中间有换行符:

注意使用 push 和 pop 从堆栈中取出`argc`和程序名。

同样重要的是使用`call`命令。这就是如何在程序集中调用一个函数，同时保存调用该函数的行后面的行的地址。然后可以使用`ret`命令返回到该行。指向返回位置的内存位置的指针存储在堆栈上。注意，这个值被从堆栈中移除，以便能够访问“隐藏”的值，然后它被推回到堆栈中，因为那是`ret`命令期望的内存地址。

感谢阅读！查看本系列的下一部分，获得一些关于编写你的第一个汇编语言程序的提示[这里](https://tony-oreglia.medium.com/writing-an-x86-64-assembly-language-program-function-arguments-and-result-dfff25970dee)！

## 本指南是系列文章的第三部分

```
Part one: [Getting Started Writing Assembly Language](https://medium.com/@tony.oreglia/getting-started-writing-assembly-language-8ecc116f3627)
Part two: [Finding an Efficient Development Cycle for writing](https://medium.com/@tony.oreglia/finding-an-efficient-development-cycle-for-writing-assembly-language-be2092e6db6a)Part four: [Sending Function Arguments and Receiving a Return Value](https://medium.com/@tony.oreglia/writing-an-x86-64-assembly-language-program-function-arguments-and-result-dfff25970dee)
Part five: [Conditionals, Jumping, and Looping](https://tony-oreglia.medium.com/writing-an-x86-64-assembly-language-program-1aade03f3b9b)
Part six: [How to Determine String Length](https://tony-oreglia.medium.com/writing-an-x86-64-assembly-language-program-84e2432cf16b)
Part seven: [Quick Reference](https://tony-oreglia.medium.com/writing-an-x86-64-assembly-language-program-84e2432cf16b)
```