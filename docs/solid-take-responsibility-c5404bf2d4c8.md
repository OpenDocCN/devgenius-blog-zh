# 坚实的原则——承担责任

> 原文：<https://blog.devgenius.io/solid-take-responsibility-c5404bf2d4c8?source=collection_archive---------0----------------------->

![](img/eaf7f0c05daed6e5874cbbb1df4ab7ae.png)

学习与工作相关的新知识的最糟糕的时候是在面试的时候。几年前我被灌输了坚实的原则。感觉好像我在面试教化学。没有办法，我可以透露事情发生的年份，因为我的经理可能正在读这篇文章，而且我喜欢我目前的薪水。哦，对不起，我是说我的工作。

坚实的原则是由美国软件工程师和讲师[罗伯特·C·马丁](https://en.wikipedia.org/wiki/Robert_C._Martin)推广的许多原则的子集，非正式地称为[罗伯特叔叔](https://en.wikipedia.org/wiki/Robert_C._Martin)。在这篇文章中，我将谈论[坚实的](https://en.wikipedia.org/wiki/SOLID)原则之一，即 [**(S)单一责任原则(又名 SRP)**](https://en.wikipedia.org/wiki/Single_responsibility_principle) 。

> 另一篇关于单一责任的文章。为什么？？？？？

写作是提高知识的最好方法。在过去的几天里，我广泛阅读了这个话题。阅读带来了新的视角。它帮助我在当前和以前的项目中识别违反 SRP 的代码片段。希望这篇文章能对那些想改进编码风格的人有所帮助。

**什么是“单一责任原则”？**

> 每个软件模块应该有一个且只有一个责任，因此只有一个改变的理由

![](img/56256be9dcc5761d98cd820a2db18b03.png)

**SRP 提供了什么价值？**

单一责任原则提供了两个价值:

1.  主要价值:易于改变
2.  次要价值:满足用户需求

让我们通过一个例子来看看这是如何实现的。

假设悉尼有一个鞋子仓库，经理(Joe)希望 IT 团队创建一份仓库库存报告。

**类报告生成器**

```
public class ReportGenerator
{
 public void PrintReport(ProductionInventory pi)
 {
// Prints report
 }

 public void EmailReport(ProductionInventory pi, string emailTo)
 {
 // Email Report
 }

 public void PublishToWeb(ProductionInventory pi)
 {
// The method displays the report on the UI
 }

 public void SaveReport()
 {
// Saves / Export the report
 }
}
```

上面的类可以自给自足地创建可通过 4 种不同方式访问的报告:

1.  打印
2.  电子邮件
3.  在线的
4.  出口

这堂课是否违反了单一责任原则？这完全取决于这个应用程序的用户。让我们来看一个变更请求:

1.  Joe 希望将一些附加字段添加到悉尼仓库的电子邮件报告中。
2.  高级经理 Andrew 想使用该应用程序查看澳大利亚所有仓库的在线报告。

IT 团队分配了两个不同的开发人员来分别处理 Joe 和 Andrew 的需求。问题是两个开发人员必须在同一个类上工作。

![](img/15d779c52d1a8de55edc9f451b0b0782.png)

当开发人员努力同步他们的工作时，一个新的需求来自高级管理层。

*   高级管理层希望向外部零售店发送一份电子邮件报告，以便他们在下订单之前可以了解库存情况。此外，生成该报告的逻辑很复杂，并且与 Joe 期望的报告不同。

在仔细回顾课程后，IT 开发团队得出了以下结论:

1.  当前代码违反了 SRP。任何逻辑上的变化都会影响 Joe、Andrew 和外部零售点(紧密耦合)。
2.  ReportGenerator 类处理 4 个不同的职责，**因此它们应该被分成 4 个不同的类**。

**总结**

*   SRP 带来的主要价值是变更的简易性，计算责任的最佳方式是识别对变更负责的各种来源。
*   变化是由用户驱动的，因此识别用户对于实现尽可能多的功能(即 SRP 的第二个价值)是非常关键的

**参考文献**

1.  [https://medium . com/@ severinperez/writing-flexible-code-with-single-respons ibility-principle-b 71 C4 F3 f 883 f](https://medium.com/@severinperez/writing-flexible-code-with-the-single-responsibility-principle-b71c4f3f883f)
2.  [https://blog . clean coder . com/uncle-bob/2014/05/08/singlereponsibility principle . html](https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html)
3.  [https://code . tuts plus . com/tutorials/solid-part-1-the-single-respons ibility-principle-net-36074](https://code.tutsplus.com/tutorials/solid-part-1-the-single-responsibility-principle--net-36074)
4.  [https://en.wikipedia.org/wiki/SOLID](https://en.wikipedia.org/wiki/SOLID)
5.  [https://en.wikipedia.org/wiki/Robert_C._Martin](https://en.wikipedia.org/wiki/Robert_C._Martin)
6.  [https://en . Wikipedia . org/wiki/Single _ respons ibility _ principle](https://en.wikipedia.org/wiki/Single_responsibility_principle)

问候塔伦

页（page 的缩写）s-Medium 是一个阅读、写作和向其他作者学习的绝佳平台。如果你想加入我的旅程，今天就加入 [medium](https://tarunbhatt9784.medium.com/membership) 。