# 提高单元测试水平的 3 个技巧

> 原文：<https://blog.devgenius.io/3-tips-to-become-better-at-unit-testing-a2833cc4827b?source=collection_archive---------4----------------------->

## 如何提高你的单元测试技能

![](img/b739f8e84c0e5d93271dcbeb97c9b880.png)

[国家癌症研究所](https://unsplash.com/@nci?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

测试对于任何软件都是至关重要的。尤其是单元测试和集成测试。

你写了一个算法，做了一些手工测试，它工作了。你错了。个别组件可能会断裂，或者出现边缘情况。

你是一个好的开发者，你不会让失败发生。创建单元测试和集成测试降低了失败的风险。

记住这一点，让我们继续学习 3 个技巧来改进你的单元测试。

这篇文章的灵感来自于阅读*“单元测试的艺术”。*

[](https://www.artofunittesting.com/) [## 单元测试的艺术

### 这个扩展版以函数式、模块化和面向对象的风格教授模拟、存根和依赖注入…

www.artofunittesting.com](https://www.artofunittesting.com/) 

# 你创建可维护的测试吗？

没有人会改变难以维护的测试。没人。您需要通过编写干净、易于使用的测试来铺平道路。

你应该总是测试类的契约。内部的私有方法需要改变。公共方法应该向类显示接口，并且不应该改变行为。

测试私有方法容易出错。这就是为什么你应该避免这些测试。

> "测试私有方法的功能可能会导致测试中断，即使整体功能是正确的."
> 
> —罗伊·奥舍洛夫

将所有方法都公开并不是正确的做法。拥有大量公共方法违反了单一责任原则。

> 不应该强迫客户端依赖他们不使用的接口
> 
> 罗伯特·马丁

记住这一点，隔离你的界面。遵循[这篇关于接口分离的好文章](https://stackify.com/interface-segregation-principle/)。

要点是避免用不属于你的方法填充你的接口。分离导致更好的单元测试，因为你有小块的代码要测试。

# 你隔离你的测试吗？

单元测试应遵循[白盒测试](https://www.geeksforgeeks.org/software-engineering-white-box-testing/)原则。覆盖正反向流，覆盖所有决策分支。

不良的测试隔离会导致不稳定的、易变的测试。创建易变测试的技巧:

> **流程测试** —开发人员编写必须以特定顺序运行的测试，以便他们可以测试流程执行、由许多操作组成的大用例，或者每个测试都是完整测试中的一个步骤的完整集成测试。
> 
> 清理中的懒惰 —开发人员很懒惰，不会返回任何她测试可能已经改变回原始形式的状态。
> 
> —摘自单元测试的艺术

我们都对上述情况感到内疚。单元测试中的测试流程，而不是使用集成测试。整理单元测试的状态，并安排测试通过。

通过改变你的习惯来对抗这些。完成测试后，清理数据库。测试后立即初始化。将流程测试转移到集成测试。

# 你测试什么会出错吗？

我们是乐观的开发者，从不认为任何事情会出错。这本身就是错误的。我们需要首先想到最坏的情况。

写了很多行代码后，我发现我们错过了负面场景测试。这会导致错误和不良功能。从概述负面场景的测试用例开始编码。

例如，您必须创建一个将两个数相除的方法。从负面测试案例开始。

```
@Test(expected = IllegalArgumentException.class)
public void divide_OneNumberIsNull_ThrowException(){...}@Test(expected = IllegalArgumentException.class)
public void divide_BothNumbersAreNull_ThrowException(){...}@Test(expected = IllegalArgumentException.class)
public void divide_DivideByZero_ThrowException(){...}
```

先做消极的场景会解开新的想法。你会看到一些可能发生的新情况。用测试驱动开发总是有好的回报。

感谢阅读！

[](https://www.artofunittesting.com/) [## 单元测试的艺术

### 这个扩展版以函数式、模块化和面向对象的风格教授模拟、存根和依赖注入…

www.artofunittesting.com](https://www.artofunittesting.com/)