# 代码气味 108 —浮点断言

> 原文：<https://blog.devgenius.io/code-smell-108-float-assertions-ce806da449bb?source=collection_archive---------8----------------------->

## 断言两个浮点数是相同的是一个非常困难的问题

![](img/188e54a69d2943052f459671005b40e0.png)

米卡·鲍梅斯特在 [Unsplash](https://unsplash.com/s/photos/numbers) 上的照片

> TL；博士:不要比较浮动

# 问题

*   错误的测试结果
*   脆弱的测试
*   失败快速原则违反

# 解决方法

1.避免浮动，除非你有真正的性能问题

2.使用任意精度的数字

3.如果你需要比较浮动和公差。

# 语境

比较浮点数是一个古老的计算机科学问题。

通常的解决方案是使用阈值比较。

我们建议完全避免使用浮点数，尽量使用无限精度的数字。

# 示例代码

## 错误的

```
Assert.assertEquals(0.0012f, 0.0012f); // Deprecated
Assert.assertTrue(0.0012f == 0.0012f); // Not JUnit — Smell
```

## 对吧

```
Assert.assertEquals(0.0012f, 0.0014f, 0.0002); // true
Assert.assertEquals(0.0012f, 0.0014f, 0.0001); // false
// last parameter is the delta thresholdAssert.assertEquals(12 / 10000, 12 / 10000); // true
Assert.assertEquals(12 / 10000, 14 / 10000); // false
```

# 侦查

[X]自动

我们可以在测试框架上添加一个检查图标 *assertEquals()* 来避免检查浮动。

# 标签

*   测试气味

# 结论

我们应该总是避免比较浮点数。

# 关系

[](/code-smell-71-magic-floats-disguised-as-decimals-31271b957ac5) [## 代码气味 71 —伪装成小数的魔法浮动

blog.devgenius.io](/code-smell-71-magic-floats-disguised-as-decimals-31271b957ac5) 

# 更多信息

[](https://codeburst.io/fail-fast-3f3f036032b0) [## 快速失败

### 失败是时尚。制造比思考容易得多，失败不是耻辱，让我们把这个想法带到我们的…

codeburst.io](https://codeburst.io/fail-fast-3f3f036032b0) 

> 上帝创造了自然数；其他一切都是人类的工作。

*利奥波德·克罗内克*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) 

*更多内容尽在*[*blog . dev genius . io*](http://blog.devgenius.io)*。*