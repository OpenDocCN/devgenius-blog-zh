# Python 中的 __str__()与 __repr__()的比较

> 原文：<https://blog.devgenius.io/str-vs-repr-in-python-63bf2923bea8?source=collection_archive---------12----------------------->

![](img/237a9b2a52dab166927e28d6bc2e55ea.png)

**_ _ str _ _()**python 中的方法返回字符串。当在对象上调用 str()或 print()方法时，会调用这个函数。

**_ _ repr _ _()**python 中的方法可以基于我们的实现返回列表、字典、元组、集合、字符串，相比之下 __str__()方法应该总是返回字符串，否则会抛出这样的错误:

> TypeError: __str__ 返回了非字符串(int 类型)

对对象调用 repr()时，会调用 __repr__()方法

## **如果我们没有显式声明这两个方法会怎么样？**

默认情况下，您创建的每个类都将实现这些方法。那为什么我们需要明确地定义它们呢？让我们看看下面的示例代码

正如你所看到的，我们没有显式定义 __str__()和 __repr__()方法，但我们仍然可以看到一切都很好，因为它们是为每个类预定义的。我猜你在想有线输出，这些是我们默认得到的方法，让我们把它们改成我们想要的

现在我们可以看到我们得到了我们需要的东西。

**注意**:如果我们没有在类中定义 __str__()方法，那么 __repr__()方法会被自动调用。让我们看看这个代码片段

# 摘要

1.  __str__ 必须返回字符串对象，而 __repr__ 可以返回任何 python 表达式。
2.  如果 __str__ 实现丢失，则 __repr__ 函数用作后备。如果 __repr__ 函数实现丢失，则没有回退。
3.  如果 __repr__ 函数返回对象的字符串表示，我们可以跳过 __str__ 函数的实现。

[](https://www.linkedin.com/in/durgaprasadmamidi/) [## durga prasad mamidi - Jr .软件开发实习生- Fountane，LLC | LinkedIn

### 查看 durga prasad mamidi 在世界上最大的职业社区 LinkedIn 上的个人资料。杜尔迦有一份工作列在…

www.linkedin.com](https://www.linkedin.com/in/durgaprasadmamidi/) [](https://github.com/durgaprasadmamidi) [## durgaprasadmamidi -概述

### 在 GitHub 上注册你自己的个人资料，这是托管代码、管理项目和构建软件的最佳地方…

github.com](https://github.com/durgaprasadmamidi)