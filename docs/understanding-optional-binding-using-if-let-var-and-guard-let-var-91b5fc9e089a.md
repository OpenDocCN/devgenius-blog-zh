# 使用“if let/var”和“guard let/var”理解可选绑定

> 原文：<https://blog.devgenius.io/understanding-optional-binding-using-if-let-var-and-guard-let-var-91b5fc9e089a?source=collection_archive---------3----------------------->

![](img/2d8da8ce12aa9a71163a75f94dacc647.png)

照片由 [Aaron Burden](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这个例子中，我们将看看使用 if 和 guard 语句的可选绑定(当然是在 Swift 中)。

首先，我们将尝试理解什么是可选绑定。

# 可选绑定

可选绑定是检查可选变量是否有值，然后将值存储在另一个变量中的过程。

" Swift 中的可选变量既可以存储对象，也可以存储零值."

我将会写更多关于可选的，可选的绑定，可选的链接等等。在下一篇文章中，我会更新这里的链接。

直到那时苹果[doc才是你的朋友。](https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html)

> “if let/var”和“guard let/var”之间的基本区别是通过展开可选变量创建的变量的范围(展开只是从可选变量中获取值，我将在另一篇文章中讨论更多)。).

最好的方法是在代码中看到它的运行。

假设我们必须创建一个函数，根据学生的分数百分比来检查他是通过还是失败。我们将通过学生姓名，和总百分比，并打印出如果学生身份与他的名字。(*如果学生的百分比是> =60* ，则他通过考试)

## 从上述代码中获得的关键信息:

1.  使用 guard 创建的变量的范围与声明 guard 语句的代码块的范围相同。
2.  guard 的“else”块应该包含一个“return”语句，即如果不满足 guard 语句中的条件(可选变量没有值),则该代码块中不再执行其他语句。
3.  使用“if let/var”语句创建的变量只在特定的 if 块中起作用。

如果这篇文章对你有所帮助，请考虑鼓掌。
干杯！！