# 在 C#中添加整数数组的 12 种方法—第 4 部分:一次又一次

> 原文：<https://blog.devgenius.io/12-ways-to-add-an-array-of-integers-in-c-part-4-again-and-again-and-again-dddd86ec8ee?source=collection_archive---------1----------------------->

![](img/ebd9ee9c4285ca3827934bfbe0333f8e.png)

照片由 [𝓴𝓘𝓡𝓚 𝕝𝔸𝕀](https://unsplash.com/@laimannung?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/vortex?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

当我要求开发人员向我展示如何在 C#中添加整数数组时，可能 95%的人都想到了一个简单的`for`循环([第 1 部分](https://medium.com/dev-genius/12-ways-to-add-an-array-of-integers-in-c-part-1-34753ff7f17a?source=friends_link&sk=ffb9131efdf8777a4d01208093229f3d))，这很棒。有几个想出了一个`foreach`循环( [Part 2](https://medium.com/dev-genius/12-ways-to-add-an-array-of-integers-in-c-part-2-freeform-iteration-2f5c810a8e7b?source=friends_link&sk=ae3d73998d2e57811d3bea1374f7ddc7) )，也很棒。对于极少数建议使用我个人最喜欢的`Enumerable.Sum` ( [第三部](https://medium.com/dev-genius/12-ways-to-add-an-array-of-integers-in-c-part-3-thinking-in-sets-e2f456454f88?source=friends_link&sk=4dff1c44cc446516161d65fe6e0fba26))的人来说，这是额外的奖励。

有一种方法我*从未*有过候选建议:递归函数，也就是说，一个函数无限地调用自己，直到它满足某种停止条件。实际上，如果一个程序员真的建议在 C#中使用递归来添加整数数组，我会感到震惊，更不用说沮丧了。

但是如果我们谈论的是，比如说，F#，而不是 C#，那会怎么样呢？那会有什么不同吗？这里有一个非常合理的方法来解决 F#中的问题。

这其实是一个很好的方法。在 C#中，**而不是**是一个好方法。先说为什么。

# 递归

这是我们 12 种方法中的第 8 种，基本上与 F#版本相同:

所有这些代码做同样的事情？是的，恐怕是的。嗯，算是吧。F#有一个`::`语法，将数组分成头部和尾部。C#没有这样的语法，所以我们必须自己写一个`GetHeadAndTail`方法。F#还有一个很好的、简单的表达式语法，用于将输入与一系列模式进行匹配，并返回第一个匹配的内容。C#正在实现它的版本`match`(被称为`switch`)，但它似乎是一项正在进行的工作。

更糟糕的是，代码甚至不工作。我的意思是，它实际上在小数组上工作得很好。在大网站上，你听说过由杰夫·阿特伍德和乔尔·斯波尔斯基创建的广受欢迎的问答网站吗？它是以将要发生的事情命名的。是的，你会得到一个堆栈溢出。

为什么在 C#中会出现堆栈溢出，而在 F#中不会？答案很简单:尾部递归。F#有，C#没有。

我们大多数人都非常熟悉调用栈的概念。如果您曾经在。NET 中，您已经看到了一个方法如何调用另一个方法，而另一个方法又调用另一个方法，等等。运行库跟踪该堆栈。但是堆栈空间是有限的资源。你只能有这么多的一个东西调用另一个东西调用另一个。做太多次，你会得到，嗯，我们已经说过了，一个 StackOverflowException。

用尾部递归可以解释很多东西。我不会做得很好，而且在这里也没什么关系。这里有一个[好的总结](https://cs.stackexchange.com/a/7814)。长话短说:如果递归调用是方法中的最后一项，编译器可以优化掉调用堆栈问题。在 F#中。但在 C#中没有。(为什么不呢？这里也不相关。[看这个](https://stackoverflow.com/q/491376/44586)。)

# 极限递归

所以在 C#中这通常不是一个好方法。

但是对于小数据集，从性能上来说，这实际上并不可怕。方法 9 是方法 8 的一个稍微不那么丑陋的版本，具有硬编码(可以参数化)的最大长度。

注意来自`Enumerable`的`Any`、`First`和`Skip`方法。我告诉过你这是一个有用的课程。

# 递归+指针

但是等等！还有呢！如果……不，它永远不会工作……嗯，也许，只是也许……当然，YOLO……我们能不能把两个糟糕的想法合并成一个稍微不那么糟糕的想法？为什么是的，是的，我们可以。

我希望我可以向您报告，这以某种方式解决了堆栈溢出异常。看起来应该是这样的。但事实并非如此，因为尽管它看起来不像 C#，但它确实像 c#，因此没有尾部调用优化。

但是还记得我们在 F#中可悲的缺少一个`::`操作符来返回一个头和一个尾吗？好吧，有了不安全的 C#，我们有了一个很好的选择。因为我们已经固定了内存地址，所以我们可以取消对指针的引用来获得头部，然后增加它来获得尾部(减少剩余的长度来保持直线)。

随便你怎么说，但是对于小数据集来说，这是一种非常快的方法。

我们能让它变得更奇怪吗？老实说，没有。我想不出来。不过，我很想听听你的想法。

但是我们能让它变得更好吗？最后两种方法将探索一些想法，这些想法可能会也可能不会改进我们到目前为止所做的工作。我们下次会谈到这些。