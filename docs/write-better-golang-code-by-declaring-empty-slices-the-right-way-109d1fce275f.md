# 通过正确地声明空切片来编写更好的 Golang 代码

> 原文：<https://blog.devgenius.io/write-better-golang-code-by-declaring-empty-slices-the-right-way-109d1fce275f?source=collection_archive---------1----------------------->

![](img/300d5c528a10b1ba6c5c2f9eb0e4f023.png)

由[杰克·威瑞克](https://unsplash.com/@weirick)在 [Unsplash](https://unsplash.com/photos/hieOkhzGyCE) 上拍摄的原始照片

如果您对编写更好的 Go 代码感兴趣，那么您可能已经注意到许多建议都围绕着约定。

多读一些 Github 周围的 Go 代码，你会开始看到一些模式。这些模式中有很多来自 Golang 自己编写有效 Go 的风格指南，其中之一叫做 *CodeReviewComments* 。你可以在这里找到文件。

在其中的一节中，它讨论了空切片的声明，我今天将简要介绍一下。

在 Golang 中，你可能知道也可能不知道一个片是像数组一样的*类*，这在其他语言中你可能很熟悉。事实上，数组的大小是固定的，而切片的大小是动态变化的。[围棋之旅](https://tour.golang.org/moretypes/7)是这样说的:

> 数组有固定的大小。另一方面，切片是动态调整大小的、灵活的数组元素视图。实际上，切片比数组更常见。
> 
> 类型`[]T`是包含类型`T`元素的切片。

顺便说一句，我确实推荐*一次围棋之旅*,只是为了玩一玩，熟悉一下围棋的基础知识。

不管怎样，有时候你会想要明确地声明一个空的片。所以你不会像这样声明它(同样来自于*一次围棋比赛的例子*):

```
primes := [6]int{2, 3, 5, 7, 11, 13}
```

相反，在这种情况下，您将声明一个空切片。也许这是因为你正在做一个空的片来存储一个质数列表，但是你将在你的逻辑中填充片，而不是在初始化变量时预先知道它们。

声明空切片的一种方法是:

```
p := []int{}
```

旁注:这是非零和零长度。

然而，在 Golang 中有一种不同的方式来声明一个空切片，根据 *CodeReviewComments* 的说法，这实际上是首选的方式。事情是这样的:

```
var p []int
```

所以基本上，你使用`var`关键字，然后忘记`:=`操作符。你也失去了括号。总的来说，尤其是当你开始更多地看到它时，它会看起来更干净一些。

但是不要只相信我的话:这里是 CodeReviewComments 谈论它的地方。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership) 

# 编写更好 Go 代码的更多方法

[](/write-better-golang-code-by-grouping-similar-declarations-2dc6145f9277) [## 通过将相似的声明分组来编写更好的 Golang 代码

### 在本文中，我们将学习一个简单的技巧来编写更好的 go 代码。

blog.devgenius.io](/write-better-golang-code-by-grouping-similar-declarations-2dc6145f9277) [](/write-better-go-code-by-keeping-your-variable-names-short-13f404ac515c) [## 通过保持变量名简短来编写更好的 Go 代码

### Go 中的变量名通常有一两个字符长。这是为什么呢？

blog.devgenius.io](/write-better-go-code-by-keeping-your-variable-names-short-13f404ac515c) [](/how-should-you-name-your-receivers-in-go-aec60abd7f67) [## 在围棋中，你应该如何命名你的接球手？

### Golang 中接收器的最佳命名约定。

blog.devgenius.io](/how-should-you-name-your-receivers-in-go-aec60abd7f67)