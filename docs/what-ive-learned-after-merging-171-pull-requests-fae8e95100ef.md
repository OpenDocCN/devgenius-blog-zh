# 997 行令人困惑的 Java 代码后的好的和坏的实践

> 原文：<https://blog.devgenius.io/what-ive-learned-after-merging-171-pull-requests-fae8e95100ef?source=collection_archive---------5----------------------->

## 揭示了我在编写了 997 行 Java 8 之后所获得的知识

![](img/e980adb39d9ac80c56d9c4ea0ac4da72.png)

由 [Ricardo Gomez Angel](https://unsplash.com/@ripato?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

做代码评审是很难的，如果你比别人有更多的经验，甚至更难。在合并了 100 多个 PRs，写了 900 多行代码之后，我知道什么是好的评审。

我将揭示一些最常见的错误，以及如何避免它们。代码审查对于避免大量错误至关重要。

重构代码会导致很多问题。这些是最优先的，因为它们可以打破现有的流动。我将分解在重构 PRs 中寻找什么。

# Java 流

[](https://www.baeldung.com/java-8-streams) [## Java 8 流 API 教程| Baeldung

### 在这篇深入的教程中，我们将介绍 Java 8 流从创建到并行执行的实际用法…

www.baeldung.com](https://www.baeldung.com/java-8-streams) 

Java 流已经成为处理数组、列表和其他可枚举的事实上的标准。使用 for 循环或大量临时变量可以使用 Java 流进行重构。流提供了一个健壮的接口，可用于映射、过滤和缩减。

# 在 Null 上使用 Optional

[](https://www.baeldung.com/java-optional-return) [## Java 可选返回类型| Baeldung

### 该类型是在 Java 8 中引入的。它提供了一种清晰明确的方式来传达这样一个信息，即可能不存在…

www.baeldung.com](https://www.baeldung.com/java-optional-return) 

Optional 防止许多空指针异常。在服务可以提供 null 结果的地方使用它是至关重要的。Optional 具有类似的映射和过滤界面。映射对于操纵类和数据来说非常方便。

Optional 不能解决所有的问题，但是可以提供可读的代码。有时你需要使用 null。optional 的一个很好的方法是`ifPresent`，它只有在 Optional 存在的情况下才会执行函数。

# 现有代码改进

改进现有代码或更改代码会给之前的功能带来很多风险。这是开发者应该记住的第一件事。改变代码需要改变测试，所以我们确信一切都按预期运行。

检查代码是否真的做了所有需要做的事情，这在评审的时候是很棒的。联系团队成员澄清公共关系也很有帮助。

# 重构

> 重构是以这样一种方式改变软件系统的过程，它不改变代码的外部行为，但改进其内部结构。马丁·福勒

通过改进和更新代码，我们可以提高质量和长期维护。重构 PRs 在团队中传播知识。这些减贫战略引发了讨论和新的想法。

重构的目标不仅仅是减少行数。它需要揭示意图。提出新的思路。人们应该想到代码复制。我们在其他地方有类似的逻辑吗？

检查代码是否属于当前的位置。我们需要在用户外观或购物车外观中重新计算购物车吗？将代码放在它该放的地方增加了代码的可维护性。

# 维护>快速修复

有人会得到我的代码，那个人可能就是我。想一想当你焊接一些代码来工作的时候。退一步想想其他的解决办法。咨询您的团队成员。可能有人在那个有问题的区域工作。

# 降低运营成本

该日志需要在数据库中吗？再查一遍。也许你可以通过直接邮寄给客户来节省一些内存。降低成本可以增加你在团队中的价值。

重复的代码也会导致运营成本。人们可能会花费大量时间来寻找分散的验证逻辑。聪明的开发人员会将它放在一个地方，并在需要的地方应用。

# 感谢您的阅读！