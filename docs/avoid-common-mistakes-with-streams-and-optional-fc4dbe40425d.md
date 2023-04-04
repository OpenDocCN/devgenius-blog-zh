# 避免流和可选的

> 原文：<https://blog.devgenius.io/avoid-common-mistakes-with-streams-and-optional-fc4dbe40425d?source=collection_archive---------6----------------------->

## Java 中如何正确使用 Streams 和 Optional

![](img/1cd37866932956106836c2251e369198.png)

米歇尔·勒恩斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

大多数开发人员只会使用[流](https://www.baeldung.com/java-8-streams)来呈现知识。你会学会如何正确使用它。没有更深层次的知识，编码人员只会使用它。工程师会想出办法的。像工程师一样，通读本指南。

不要到处撒[随意](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)！有时[空检查](https://www.baeldung.com/java-avoid-null-check)更有益。可选的 and Streams 方法略微提高了代码的可读性。调试可能有点困难。

让我们进入代码！

# 绘图

常见的错误包括糟糕的嵌套。我们可以用贴图来弄平它。让代码的结构按顺序排列。这样我们可以避免地图嵌套。

如何不做贴图！

[方法引用](https://www.baeldung.com/java-method-references)在这个代码片段中派上了用场。你可以把它们放在周围，写得更少，但完成得更多。大家都会同意，这段代码可读性更好。在这段代码中，[λ](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)是不必要的，因为我们可以使用方法引用。

更好的，扁平的方式

# 减低

当您想要将流“收缩”为一个值时，请使用 reducing。在这种情况下，避免使用映射。For 循环不是最好的方法，因为我们在流中有 reduce 函数。

在这个函数中，我们正在获取应用于`itemPrice`的折扣。如果没有折扣，我们不会从价格中减去任何东西。

所有折扣都降价

# 零检验

空检查是每个代码不可或缺的一部分。这样做可以防止 NullPointerExceptions。检查空值的一种方法是通过可选的接口。

如果您想避免嵌套 if 块，获取其他内容是很好的。`orElseGet`和`orElse`将提供该功能。

有时我们希望返回 null，对于 POJOs 或 dto，我们不需要这种方法。

带有 orElse 的空检查

## 感谢阅读！