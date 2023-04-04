# 清晰理解大型企业代码库的 3 个技巧

> 原文：<https://blog.devgenius.io/3-tips-to-clearly-understand-big-enterprise-code-base-13996d68c84?source=collection_archive---------3----------------------->

## 使用企业代码库是困难的。以下是更简单的方法。

![](img/9b38e0d386ee8bbdfbe25141ea6dd146.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Karsten Winegeart](https://unsplash.com/@karsten116?utm_source=medium&utm_medium=referral) 拍摄

图片为企业公司工作。您将有一个很大的代码库需要维护。

大项目让初级开发人员不知所措。你不能把所有的功能，模块都放在脑子里。你可以理解正在发生的事情。

我从事企业代码库工作已经有几年了。我会分享我犯过的大错误，以及如何自己改正。

让我们继续小费。

# 为什么需要了解商业模式？

[](https://www.altexsoft.com/blog/business/software-business-models-examples-revenue-streams-and-characteristics-for-products-services-and-platforms/) [## 软件商业模式、范例、收入流以及产品、服务的特征…

### 业务模型(BM)描述了一个组织如何为客户创造和交付价值。它描述了产品的特征…

www.altexsoft.com](https://www.altexsoft.com/blog/business/software-business-models-examples-revenue-streams-and-characteristics-for-products-services-and-platforms/) 

首先，试着理解商业模式。项目怎么赚钱？什么是收入流？

这将有助于你了解业务需求。一旦你了解了现金流，你就可以浏览代码库了。与商业模型相一致的好的软件架构帮助很大。

假设你需要关于顾客的信息。你应该有一个独立的模块，与客户逻辑相关。有了这种区分，你很快就能找到你需要的东西。

电子商务就是一个很好的例子。任何电子商务的核心都是订单处理。保存订单处理逻辑的模块应该是核心模块。

> 不要做假设！

在几次冲刺中，你会陷入不知道企业想要什么的境地。边缘情况场景。假设实现导致错误的结论。

> 为边缘案例安排电话，在优化会议上提出问题。

通过弄清楚业务是如何工作的，你可以毫无问题地遍历代码库。

# 你需要四处玩耍

您在企业环境中的第一张入场券是关于购物车逻辑的。您需要更改购物车的重新计算。

您需要导航到购物车页面并进行操作。查看该页面触发了哪些端点。找到负责服务页面的控制器。进行更改并测试。

从相关的地方开始，到后端的相关逻辑。你不需要知道整个代码库。从一个相关的页面开始会帮助你。

> 不要添加不必要的改动！

代码评审员大多数时候不会检查所有的代码。当你在周围玩耍时，一定要清理干净。不要让你的注释或未使用的代码挂起。移除死代码或代码中未使用的路径。

> 想想你是团队中唯一的编码员。

一旦您进行了更改，请检查其他团队成员最近的更改。完全确定你没有破坏他们的改变。运行测试，并创建您自己的测试。阅读代码，而不是编写代码。

[](https://medium.com/better-programming/3-tips-to-become-a-better-code-reader-c84fcb4cedc3) [## 成为更好的代码阅读者的 3 个技巧

### 阅读代码和编写代码一样重要。以下是如何成为一个更好的读者

medium.com](https://medium.com/better-programming/3-tips-to-become-a-better-code-reader-c84fcb4cedc3) 

> 寻找共同的逻辑！

您需要根据订单的属性获得不同的订单。在网上搜索，你会找到一个有用的片段。假设这个片段就是你需要的。

```
public static <T> Predicate<T> **distinctByKey**(Function<? super T, ?> keyExtractor) 
{
        Map<Object, Boolean> seen = new ConcurrentHashMap<>();                   return t -> 
seen.putIfAbsent(keyExtractor.apply(t), Boolean.TRUE) == null;  
} // [code taken from here](https://www.baeldung.com/java-streams-distinct-by).
```

作为一名优秀的开发人员，您应该知道这是一种常见的逻辑。您搜索代码库，发现公共模块包含这个代码片段。重用它将使项目免于重复代码库。

> 询问团队成员共同的逻辑。

# 与团队成员交谈

说话可以减少时间投入。有一个团队成员更了解这个领域。安排一次通话，并确保准备好问题。

你需要自己做一些代码介绍。找出痛点，并向您的团队成员展示。这将作为橡皮鸭编程，但也提出了其他问题。

> 不要无礼！

尊重其他团队成员的时间。询问他们的时间，如果他们在打电话，不要打扰他们。你想被打扰吗？编码需要大量的注意力，如果团队成员给你时间，珍惜他们的努力。

> 珍惜他人的时间。

希望这些技巧能帮助您浏览企业代码库。感谢阅读！