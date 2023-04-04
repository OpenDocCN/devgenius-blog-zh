# 鲜为人知的 Java 8 特性

> 原文：<https://blog.devgenius.io/the-lesser-known-java-8-features-49625a89424b?source=collection_archive---------5----------------------->

![](img/0cf733a4bc0c83cb36c5e027bed9b095.png)

由 [Unsplash](https://unsplash.com/s/photos/secret?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 [krakenimages](https://unsplash.com/@krakenimages?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## 介绍

Java 8 发布了一系列新特性，Streams、Functional Interface 和 Lambda 成为了焦点。

直到昨天，我还认为功能界面只能是这样的，

一个接口，有一个抽象方法。

但是，功能界面也可以是这样的，

Java 8 引入了一个新概念，除了通常的公共抽象方法之外，接口还可以有一个*默认*和*静态*方法。

根据这个定义， *MyInterface* 仍然是一个函数接口，因为它只有一个抽象方法，其他两个方法都有一个主体。

在本文中，我们将详细分析这两种方法。

## 默认方法的角色

*案例 1:* 它为实现它的所有类提供了一个默认功能。

假设类 A 和类 B 实现了 MyInterface，并且它们都在 myMethod1()中执行相同的任务。传统的方法是——类 A 和类 B 都通过重写 myMethod1()来实现它们自己的方法，或者为了避免重复代码，可以创建一个 Helper 类来实现这个方法。

默认方法将所有的公共功能放在一个地方，避免了创建新类或复制代码的负担。

*案例 2:* 新的功能可以添加到接口中，而不会影响已经实现它的类。

假设 A 类和 B 类实现了 MyInterface。现在，作者决定向该接口添加新功能，该功能对其所有实例都可用。此时，如果我们向接口添加一个抽象方法，我们将不得不通过覆盖这个新方法来修改现有的类。因此，添加一个默认方法是有意义的，该方法可用于具有默认行为的现有类，并且新类可以根据需要覆盖该功能。

现有的类可以显示 myMethod1()的默认行为，

新类可以根据需要覆盖 myMethod1()。

## 静态方法的作用

在上面的例子中，如果作者决定向 MyInterface 添加新功能，并且不希望该功能被覆盖，那么我们可以将该方法设为静态方法。

我能想到的另一个用例是在给定的接口中使用特定的功能，而不用实例化它。

比如，在下面的例子中，

类可以使用 myMethod2()中提供的功能，而无需实现该接口

执行上述类的输出是

```
From Interface
```

## 结论

如果您能想到这两种方法类型在接口中的任何其他可能的用例，请留下评论。

*更多内容尽在*[*blog . dev genius . io*](http://blog.devgenius.io)*。*