# 编写 X86–64 汇编语言程序

> 原文：<https://blog.devgenius.io/writing-an-x86-64-assembly-language-program-function-arguments-and-result-dfff25970dee?source=collection_archive---------3----------------------->

![](img/db5d0eb5909d19cd90eb44426cc39891.png)

由 [Pawel Czerwinski](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

## 第四部分:发送函数参数并接收返回值

## 本指南是系列的第四部分

*   第一部分:[开始编写汇编语言](https://medium.com/@tony.oreglia/getting-started-writing-assembly-language-8ecc116f3627)
*   第二部分:[寻找编写汇编语言的高效开发周期](https://medium.com/@tony.oreglia/finding-an-efficient-development-cycle-for-writing-assembly-language-be2092e6db6a)
*   第三部分:[打印命令行参数](https://medium.com/@tony.oreglia/writing-an-x86-64-assembly-language-program-648b6005e8e)

## 本指南后面是

*   第五部分:[条件句、跳转和循环](https://tony-oreglia.medium.com/writing-an-x86-64-assembly-language-program-1aade03f3b9b)
*   第六部分:[如何确定字符串长度](https://tony-oreglia.medium.com/writing-an-x86-64-assembly-language-program-84e2432cf16b)
*   第七部分:[快速参考](https://tony-oreglia.medium.com/writing-an-x86-64-assembly-language-program-f847d4edf577)

## 发送函数参数并返回结果

在本系列的上一篇文章中，我们看到了如何在 x86–64 汇编中定义和调用函数。

现在我想知道如何给函数提供参数并返回值。换句话说，当这是用高级语言完成的时候，在汇编代码级别是如何翻译的呢？

答案很简单。在 x86–64 汇编中，有一个约定定义了哪些寄存器应该用于第一个参数、第二个参数等等。

所有进一步的参数都被推送到堆栈上。

返回值将存储在寄存器`$rax`中。

下面是一个代码示例，它利用传统的寄存器来传递参数和结果，以便打印出命令行参数

还有一些定义明确的调用约定，遵循这些约定会很有帮助。见[此处](https://en.wikipedia.org/wiki/X86_calling_conventions)。

我希望你喜欢这篇文章。要了解更多，请在这里找到本系列的下一部分:[条件、跳转和循环](https://tony-oreglia.medium.com/writing-an-x86-64-assembly-language-program-1aade03f3b9b)。