# 编写 X86–64 汇编语言程序

> 原文：<https://blog.devgenius.io/writing-an-x86-64-assembly-language-program-84e2432cf16b?source=collection_archive---------2----------------------->

## 第六部分:如何确定字符串长度

![](img/da7b6482f2d7ae4677c5313dcd217732.png)

照片由[布雷特·乔丹](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 本指南是系列的第六部分

*   第一部分:[开始编写汇编语言](https://medium.com/@tony.oreglia/getting-started-writing-assembly-language-8ecc116f3627)
*   第二部分:[寻找编写汇编语言的高效开发周期](https://medium.com/@tony.oreglia/finding-an-efficient-development-cycle-for-writing-assembly-language-be2092e6db6a)
*   第三部分:[编写 X86–64 汇编语言程序:打印命令行参数](https://medium.com/@tony.oreglia/writing-an-x86-64-assembly-language-program-648b6005e8e)
*   第四部分:编写 X86–64 汇编语言程序:[发送函数参数并接收返回值](https://medium.com/@tony.oreglia/writing-an-x86-64-assembly-language-program-function-arguments-and-result-dfff25970dee)
*   第五部分:编写 X86–64 汇编语言程序:[条件、跳转和循环](https://medium.com/@tony.oreglia/writing-an-x86-64-assembly-language-program-1aade03f3b9b)
*   第七部分:[快速参考](https://tony-oreglia.medium.com/writing-an-x86-64-assembly-language-program-f847d4edf577)

# 如何计算字符串长度

为了计算字符串的长度，我们首先需要知道是什么决定了给定字符串的结尾。

内存中的字符串表示为指针。指向的位置是一个字节的数据，代表一个字符，后跟内存中连续的其他字符。重要的一点是，这个字节序列由字节`0x00`终止。这被称为零终止字符，这是一种终止字符串的方法(例如，C 使用这种方法)。有一些对字符串进行编码的方法，我们在这里不做介绍。

例如，字符串“HELLO WORLD！”将在内存中保存以下值(以十六进制表示的二进制值):

因此，可以通过遍历字符串中的每个内存字节直到到达零终止字符来确定长度。这是如何实现的一个初步实现，但是如果你仔细观察，会发现有几个错误，看看你是否能发现这个代码片段的问题。

如果你发现了问题，干得好。如果你只是想学习，请继续阅读。问题是[rdi]可能指向一个空字符串，所以在这种情况下[rdi]处的字符是 0x00。这是一个在编写解决方案时不考虑边角情况的典型例子。

现在[rax] = 1，即使字符串的长度是 0。

现在程序指向内存中字符串的第一个字符。

现在，[rax]可能等于 2，即使字符串长度是 0。

[rdi]处的字节不是 0x00，因为程序直接跳过了该字节。这个循环什么时候结束？程序不知道，因为它在看它不应该看的内存。

第二个 bug 是终值长了一个。长度不需要包括终止空字符。

该解决方案的修订版可能如下所示

当然，有一点重复的代码。也许您可以优化这个解决方案以避免重复。如果你有，请在评论中让人们知道！

我希望你喜欢这个系列。要了解更多，请查看我在制作这个系列时收集的资源[这里](https://tony-oreglia.medium.com/writing-an-x86-64-assembly-language-program-f847d4edf577)。