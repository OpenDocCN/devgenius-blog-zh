# 使用 ValueTuple 在 C#方法中返回多个属性

> 原文：<https://blog.devgenius.io/returning-multiple-properties-in-a-c-method-with-valuetuple-3493b441ede7?source=collection_archive---------3----------------------->

![](img/9d8d57e0ab7f02dadd7a6fc4675971ed.png)

来自 [Pexels](https://www.pexels.com/photo/working-woman-person-technology-7375/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的[创业股票照片](https://www.pexels.com/@startup-stock-photos?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

元组在 C#世界中很少被实现，这让他们很讨厌，部分原因是因为在某些编程语言(如 C#)中“元组”的晦涩，还因为传统 C#元组中的对象是作为“Item1”、“Item2”等来访问的。

说到如何从一个方法返回多个属性，下面是我的层次结构:

1.  返回一个元组(值元组)
2.  将方法分解成更小的方法，每个方法只返回一个对象
3.  最后:创建并使用 POCO (plain ol' C# object)来保存对象

对我来说，仅仅为了存储方法的返回值而创建一个 POCO 是最坏的也是最后的选择，然而这可能是大多数 C#开发人员在这种情况下的首选。使用定制的 POCO 为我们提供了智能感知，并允许我们通过属性名来访问方法的结果。有什么不喜欢的？

不喜欢的是，我们最终得到的 POCOs 在应用程序代码之外没有任何意义或价值。我们把这个 POCO 放在哪里？这不是模型。它肯定不是视图模型或 DB 实体。我们把它放在一个叫“内部”的文件夹里吗？也许是它被使用的班级？

还要考虑到，当一个人阅读代码时，他们必须导航到这个 POCO 的定义，以便理解这个方法到底返回什么。

这是我拒绝添加到应用程序中的额外负担，尤其是当我们有 ValueTuple 时。

## 值元组

在 C# 7.0 中，微软给了我们 ValueTuple。ValueTuple 应该能够解决开发人员对 C# Tuple 的所有疑虑。那么 Tuple 和 ValueTuple 之间有什么变化呢？简而言之，ValueTuple 是一种值类型，因此存储在堆栈中而不是堆中。这在理论上给了我们一个性能增益，因为在堆中少了一件垃圾收集要处理的事情，但更重要的是，它允许我们用元组得到*花式*。

怎么想象？我们得到了一个干净的语法糖，并且能够以命名 POCO 属性的相同方式来命名元组的成员。

考虑下面的例子，其中 ValueTuple(由 **< (…) >** 定义)被用于有效且清楚地返回 API 令牌及其到期日期:

当然，可以显式定义值元组，例如`return new ValueTuple<string, DateTime>(token, expirationDate);`，但是这样做会删除定义成员名称的能力(“token”和“expirationDate”)。

## 摘要

使用 ValueTuple，我们避免了 POCO 的额外干扰，同时通过 value tuple 成员的命名保持了清晰性。更重要的是，我们可以通过查看方法签名来判断`GetToken()`是否返回了一个令牌及其到期日期。太美了。

[关于元组和值元组之间差异的更多信息](https://stackoverflow.com/questions/41084411/whats-the-difference-between-system-valuetuple-and-system-tuple)