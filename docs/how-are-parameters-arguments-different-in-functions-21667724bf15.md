# 函数中的形参和实参有什么不同？

> 原文：<https://blog.devgenius.io/how-are-parameters-arguments-different-in-functions-21667724bf15?source=collection_archive---------18----------------------->

## 提示:一个是合同，另一个是符合合同的数据

![](img/36b541381e92062f187ba99b556cd4fe.png)

照片由 [**Ajegbile**](https://www.pexels.com/@diimejii?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 发自 [**Pexels**](https://www.pexels.com/photo/man-working-using-a-laptop-2696299/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

你有多少次听到软件开发人员在谈论函数时交替使用术语“参数”和“自变量”?

就在几天前，我和一位进入软件开发领域才一年多的学员开发人员进行了一次代码审查会议。这个年轻人聪明、有智慧，他在掌握与编程相关的概念和原则方面取得了显著的进步。

我很羡慕他快节奏的学习经历。我还记得当时我对软件开发的入门是多么具有挑战性和令人沮丧。

通过 Skype，我听着看着他向我介绍他为正在开发的应用程序实现的功能。他一行一行地解释了他的实现，解释了他的思维过程和对他所写代码的决策。

他实施的大部分措施看起来还不错。但是在他的演讲中，我注意到他经常混淆“参数”和“论证”这两个词。很多次我希望他使用“参数”,结果是“参数”,反之亦然。

他发表声明说

*“我有一个参数为 A1 和 A2 的函数 A”*

*“我调用了传递参数 A1 和 A2 的函数 A”*

就我所关心的而言，这并不完全让我感到惊讶，因为即使是拥有多年软件开发经验的开发人员也同样被这两个编程术语的混淆所困扰。

为了我的学员的利益，希望也是为了你的利益，在本文的剩余部分，我将解释函数中两个术语“参数”和“自变量”的区别。

# 什么是函数参数？

函数参数是函数签名的一部分。在另一篇[文章](https://python.plainenglish.io/a-noob-programmers-guide-to-functions-a69e3b44fad)中，我阐述了什么是函数签名。查看[文章](https://python.plainenglish.io/a-noob-programmers-guide-to-functions-a69e3b44fad)中关于 noob 程序员必须知道的函数的信息。

函数参数定义了函数的输入和输入的性质。它们为函数接受的输入定义了一个契约。参数还描述了函数接收输入的方式。

总之，函数参数:

*   定义函数接收的外部数据的输入。
*   定义这些输入的排列和顺序。
*   定义默认值，允许省略一些预期的功能输入。

# 什么是函数自变量？

自变量与函数的定义无关。它不构成函数签名的一部分。如果你不知道函数签名，不要担心。我解释了一下，包括其他需要了解功能[的重要事情在这里](https://python.plainenglish.io/a-noob-programmers-guide-to-functions-a69e3b44fad)。

参数仅在函数调用期间相关。我们只在函数调用期间向函数传递参数。

参数是传递给函数并影响函数的操作和输出的外部状态或数据。

参数必须符合协定或其参数的约束。我们将该契约指定为函数定义的一部分，也称为函数签名。

函数定义中的参数和函数调用期间传递的实参之间总是存在一对一的映射。

用顺序为`p1: integer, p2: string, p3: string`的三个参数定义的函数将期望在函数调用期间传递顺序相同的三个参数:`a1: integer, a2: string, a3: string`。

参数的顺序与其定义的类型不匹配会引发错误。

在像 Java 这样的静态语言中，函数参数定义了它们期望的数据类型。在函数调用期间，有一个严格的检查来确保匹配的参数符合预期的数据类型。参数不符合其参数定义的类型会导致函数调用错误。

像 JavaScript 和 PHP 这样的弱类型语言不会对参数进行类型检查。

# 结束语

希望这篇文章能澄清“参数”和“自变量”的混淆。你现在知道两者的区别了。是啊！没有人会因为交换这些编程术语而丢掉工作，但是在与其他开发人员交流时恰当地使用它们会非常好。

*如果你一路走到这里，那么谢谢你。*

我希望这篇文章对你有所帮助。祝你一切顺利，下次再见。

[](https://ofelix03.medium.com/membership) [## 通过我的推荐链接加入媒体-费利克斯·奥托

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

ofelix03.medium.com](https://ofelix03.medium.com/membership)