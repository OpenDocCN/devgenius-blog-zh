# 如何进行有价值的测试，捕获更多的 bug，做出独特的健壮代码

> 原文：<https://blog.devgenius.io/how-to-make-valuable-tests-capture-more-bugs-and-make-unique-robust-code-17b5ed9eac98?source=collection_archive---------4----------------------->

## 如何从危险的虫子中拯救你的生命

![](img/924a715e21e8afd892b8b0e7b0d42b71.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Ani Kolleshi](https://unsplash.com/@anikolleshi?utm_source=medium&utm_medium=referral) 拍照

我猜你有一些**错误**是因为缺乏测试？你知道测试是**耗时**和**无聊**。所以你尽一切可能避免它。我们都是。

没有正确的错误定义，您将花费宝贵的时间进行调试。

如果你记录了所有的逻辑，所有可能的流程，会怎么样？测试可以揭开所有的场景。**单元或集成测试**对于软件来说是必不可少的。

## 通过测试优先方法节省您的时间

开发关键场景时，测试是必不可少的。编写测试会立即产生好的文档。

当你进行重构的时候，**测试**案例作为**文档**。你的班级应该做这个，检查那个。简单而有效。

测试内容:

*   **缺少属性**
*   **抛出异常的场景**
*   **无数据，空异常**
*   **所有条件组合**

您已经消除了最常见的错误。只留给你那些和业务逻辑有关的。集成测试将涵盖其余部分。

## 你擅长编写可测试的函数吗？

传说中的开发者一定听过下面这些。

> 函数不应超过 4 行。不超过 100 行的课程。

为什么只有 4 个？人们一定会问，这怎么可能。编写小函数会产生可测试的代码。

根据定义，大功能做得更多，因此避免了单一责任。可测试的功能是小功能，因为你不会有太多的测试。

健壮的代码是我们的企业最想要的。你的函数能处理负载吗？**坏数据**？**无效参数**？涵盖一切可能和不可能，你永远不知道会发生什么。

重新思考你在编码什么。代码的正确放置是至关重要的。提防 [**特性——羡慕**](https://refactoring.guru/smells/feature-envy) 。人们一定会问，为什么会有这种方法？

## 圈复杂度

圈复杂度是软件代码线性路径的一种度量。意味着数字越大，路径越多。这可能导致函数不可测试。

尽可能降低这个数字。避免 switch，大量的 if 语句可以使其保持低位。降低这个数字后，就可以轻松测试了。

## 你会避免失败的商业场景吗？

敏捷世界呈现 [**三个朋友会议**](https://www.agilealliance.org/glossary/three-amigos/) 。你能从中学到什么？

你可以免费获得测试用例，还需要 QA 的参与。测试人员可以更好地洞察可能的流程。

使用这些来**创建更好的集成测试**。测试行为将使当前的软件对变化有弹性。软件本身可以改变，测试应该记录行为。**破坏行为**将**破坏测试**本身，从而警告错误行为。

测试揭示了所有可能的副作用。当前的副作用触发了代码的错误行为。预防未来的**副作用**，在测试中设定行为逻辑，成为更好的开发者。

## 你只是在测试快乐的心流吗？

大部分开发者做的，测试快乐路径。别忘了，bug 发生在那些**阴云密布的小路**。一个小的异常测试比一个快乐路径测试更有价值。

## 结论

如果你到了这里，你就是一个聪明的开发者。注意细节。省去难闻的代码。制作简洁的可测试函数。

参加三个朋友会议，并从中学习。用伟大的测试覆盖所有场景。抓住那些讨厌的虫子。

## 感谢您的阅读。

[](https://medium.com/dev-genius/what-ive-learned-after-merging-171-pull-requests-fae8e95100ef) [## 合并 171 个拉取请求后我学到了什么

### 揭示了我在编写了 997 行 Java 8 之后所获得的知识

medium.com](https://medium.com/dev-genius/what-ive-learned-after-merging-171-pull-requests-fae8e95100ef) [](https://medium.com/dev-genius/avoid-common-mistakes-with-streams-and-optional-fc4dbe40425d) [## 避免流和可选的

### Java 中如何正确使用 Streams 和 Optional

medium.com](https://medium.com/dev-genius/avoid-common-mistakes-with-streams-and-optional-fc4dbe40425d)