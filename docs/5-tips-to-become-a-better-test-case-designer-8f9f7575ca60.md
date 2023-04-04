# 成为更好的测试用例设计者的 5 个技巧

> 原文：<https://blog.devgenius.io/5-tips-to-become-a-better-test-case-designer-8f9f7575ca60?source=collection_archive---------2----------------------->

## 创建测试用例可能很难。下面是怎么做的。

![](img/5e4adb755aa9de87fb96d3379f5d489e.png)

[绿色变色龙](https://unsplash.com/@craftedbygc?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我们都经历过。用所有可能的选项测试软件。测试软件，同时遗漏一些案例。那些被遗漏的通常是边缘情况。

我们能做的是改进测试用例的设计。[等价划分方法](https://www.satisfice.com/blog/archives/1669)帮助我们设计测试用例。

等价划分创建你的程序可以处理的输入类。这样我们可以减少测试用例的数量。

边界值是任何程序成败的关键。让我们回忆一下臭名昭著的“一个接一个”错误。[边界值分析](https://www.guru99.com/equivalence-partitioning-boundary-value-analysis.html#1)也帮助我们设计更好的边缘测试用例。

说了这么多，让我们继续实际的技巧。

# 各个击破

选择正确的测试数据类至关重要。

首先，您可以将域分为有效输入和无效输入。如果一个字段接受整数，你有两个等价类。一个用于正数，一个用于非正数。

前面的例子是二进制的，但是我们可以有一个需要的数字范围。对于 1 之间的范围..100，我们会有 3 个班。一个是正在讨论的范围， *C1 = {x | 1 ≤ x ≤ 100 }* 。第二个是小于 1 的数字， *C2 = { x | x < 1}。*第三个包含超过 100 的数字， *C3 = { x | x > 100}。*

让我们有一个需要测试的客户模型。客户可以在个人环境中，也可以在组织环境中。测试这个域需要 3 个类。每个上下文一个，没有上下文的客户一个。

如果出于某种原因，我们有客户可以在这两种情况下，这将创建另一个类。目前的课程没有涵盖这方面的内容，因为我们没有一个好的代表。

# 挑选一个进行测试

用来自一个类的一个输入数据进行测试就足够了。这证明了我们的程序可以处理等价类中的其他数据。

有了正确的等价类划分，我们就可以放心使用我们的测试用例了。如果划分不正确，我们需要回到第一步。当有特殊情况时，增加一个额外的等价类。

举个例子，我们来看看整数的除法。假设我们有两个类，作为类创建的一部分。一个用于正整数，一个用于负整数。

选择零作为测试用例，我们得到异常的行为。考虑到这一点，zero 需要一个额外的等价类。

# 选择正确的等价类

按照之前的想法，我们从课堂上选一个，可能是错的。我们假设来自一个类的所有输入都足够了，但这种假设有时并不成立。

当我们遇到这种情况时，需要给域增加额外的分区。我们应该记住，一个代表应该代表整个班级。如果这个假设失败了，我们需要更多的类。

# 选择边界值进行测试

[](https://stackoverflow.com/questions/2939869/what-is-an-off-by-one-error-and-how-do-i-fix-it) [## 什么是差一位错误，我如何修复它？

### 假设您有以下代码，其中包含一个数组和一个 for 循环:char exampleArray[] = { 'H '，' e '，' l '，' l '，'…

stackoverflow.com](https://stackoverflow.com/questions/2939869/what-is-an-off-by-one-error-and-how-do-i-fix-it) 

我们都犯过“一个接一个”的错误。让我们通过测试边界值来修复它。

边界值分析是找到好的边界候选者的可靠方法。

边界值的简单列表:

*   选取一个范围内的最小值和最大值
*   选择比最小值小，比最大值高一点的
*   从等价类的边界选择(0 表示测试划分)

测试二进制值的边界是不可能的。测试大范围的边界也是不值得的。例如，检查购物车中 INTEGER_MAX 的产品价值。即使生意兴隆，这种情况发生的可能性也很低。

# 从依赖类中挑选配对

假设你有测试数据的依赖类。一个例子是在不同浏览器和不同视窗宽度下的文本预览。

*   *浏览器= {Chrome，Opera，Firefox}*
*   *Viewport = {手机、桌面、平板}*
*   *语言= {英语、芬兰语、塞尔维亚语、德语}*

有了这个环境，您将有 36 个测试组合。添加更多的依赖类，测试用例的数量就会激增。

您需要创建测试对。以一对 *{Chrome，mobile，English}* 为例。这不仅要测试这对，还要测试 *{Chrome，mobile}* 和 *{mobile，English}* 对。

感谢阅读！

 [## 重新思考等价类划分，第 1 部分。

### 维基百科上关于等价类划分(ECP)的文章是糟糕的思维和教学的一个很好的例子

www.satisfice.com](https://www.satisfice.com/blog/archives/1669) [](https://www.guru99.com/equivalence-partitioning-boundary-value-analysis.html#1) [## 边界值分析和等价划分测试

### 本教程用一个简单的例子演示了等价划分和边界值分析的使用。

www.guru99.com](https://www.guru99.com/equivalence-partitioning-boundary-value-analysis.html#1)  [## 等价划分方法- GeeksforGeeks

### 等价类划分方法也称为等价类划分(ECP)。这是一个软件测试…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/equivalence-partitioning-method/)