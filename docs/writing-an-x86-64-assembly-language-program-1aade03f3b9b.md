# 编写 X86–64 汇编语言程序

> 原文：<https://blog.devgenius.io/writing-an-x86-64-assembly-language-program-1aade03f3b9b?source=collection_archive---------4----------------------->

## 第五部分:条件、跳转和循环

![](img/5abd4b947720433ab522900ba6ecf9c6.png)

[Clem Onojeghuo](https://unsplash.com/@clemono?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 本指南是系列的第五部分

*   第一部分:[开始编写汇编语言](https://medium.com/@tony.oreglia/getting-started-writing-assembly-language-8ecc116f3627)
*   第二部分:[寻找编写汇编语言的高效开发周期](https://medium.com/@tony.oreglia/finding-an-efficient-development-cycle-for-writing-assembly-language-be2092e6db6a)
*   第三部分:[打印命令行参数](https://medium.com/@tony.oreglia/writing-an-x86-64-assembly-language-program-648b6005e8e)
*   第四部分:[发送函数参数并接收返回值](https://medium.com/@tony.oreglia/writing-an-x86-64-assembly-language-program-function-arguments-and-result-dfff25970dee)

## 本指南后面是

*   第六部分:[如何确定字符串长度](https://tony-oreglia.medium.com/writing-an-x86-64-assembly-language-program-84e2432cf16b)
*   第七部分:[快速参考](https://tony-oreglia.medium.com/writing-an-x86-64-assembly-language-program-f847d4edf577)

## 条件和循环

汇编语言支持有条件地跳转到代码的特定行。循环实际上只是跳转的一个特殊实现。这是跳转到“循环”的开始，直到最终满足某个条件。

本质上，这篇文章是关于写条件语句，根据某些条件分支两种方法中的一种。

核心条件运算符是`cmp`，是“比较”的缩写。`cmp`操作符比较两个值，然后设置一些标志来指示这两个值的关系。可以使用其他一些运算符来检查标志，即:

*   `JE`意思是如果相等就跳
*   `JZ`表示如果为零则跳转
*   `JNE`意为不等则跳
*   `JNZ`表示不为零时跳转
*   `JG`表示如果第一个操作数大于第二个，则跳转
*   `JGE`表示如果第一个操作数大于或等于第二个，则跳转
*   `JA`与`JG`相同，但执行无符号比较
*   `JAE`与`JGE`相同，但执行无符号比较

要循环，函数只需根据条件语句的结果调用自己。当然，在某些时候，条件必须打破循环，以避免无限循环。

下面的例子演示了一个循环函数，用于打印堆栈中的参数列表

我希望你喜欢这个。要了解更多，请在这里找到本系列的下一部分: [*如何确定字符串长度*](https://tony-oreglia.medium.com/writing-an-x86-64-assembly-language-program-84e2432cf16b) *。*