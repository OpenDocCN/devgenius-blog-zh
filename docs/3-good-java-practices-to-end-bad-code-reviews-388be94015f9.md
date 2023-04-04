# 3 个良好的 Java 实践来结束糟糕的代码审查

> 原文：<https://blog.devgenius.io/3-good-java-practices-to-end-bad-code-reviews-388be94015f9?source=collection_archive---------3----------------------->

## 以下是获得更好的代码审查的实践

![](img/bd6df2097cf786bfc427acb976fefab3.png)

照片由[马体·米罗什尼琴科](https://www.pexels.com/@tima-miroshnichenko?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[佩克斯](https://www.pexels.com/photo/man-person-people-woman-6914348/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄——作者编辑

***你的团队只和你最弱的评审员一样好。——***[***乔尔肯普***](https://medium.com/u/4262f02e25a6?source=post_page-----388be94015f9--------------------------------)

大多数开发人员讨厌代码评审。我们不喜欢进行可怕的讨论，也不喜欢在评论上浪费时间。

我们需要改进我们的代码以获得更好的评论。*开发人员每小时审查超过 200 行代码，导致源代码质量降低。*

一般来说，频繁的代码审查者比其他人写出更好的代码。他们看到更多的代码——他们知道更多。改进您的代码，并鼓励其他人这样做。

这里有一些你可以用来改进你的代码以获得更好的评论的东西。

# 1.使用更多的泛型

泛型让你的代码更简洁，更易读，更易维护。简洁的代码作为类属只显示接口。更容易阅读，因为`Collection<T>`比`Collection<MyCustomType>`更好。随着您变得更加冗长，创建更好的接口，以及创建特定的实现，它是可维护的。

Joshua 认为泛型会导致编译时问题。像这样使用泛型:

*   使用`Set<String>`而不是`Set`
*   使用`Set<E>`超过`Set<Object>`
*   使用有界通配符，`Set<? extends E>`
*   `lvalue`应该比`rvalue`更通用，例如 `Set<YourObject> objectsSet = new HashSet<>();`

*你不该对泛型做什么？避免同时使用数组和泛型。*那些对`Lists`胜过`Arrays`的好处一无所知的人，把两者混为一谈。比起数组，我更喜欢列表，因为它们更容易操作。Joshua 建议，当你发现它们在一起时，选择列表而不是数组。

这里还有一些关于为什么你应该使用泛型的理由:

*   编译时更强的类型检查。

Java 编译器对泛型代码应用强类型检查，如果代码违反了类型安全，就会发出错误。修复编译时错误比修复运行时错误更容易，运行时错误很难发现。

*   消除管型。
*   使程序员能够实现通用算法。

通过使用泛型，程序员可以实现泛型算法，这些算法可以在不同类型的集合上工作，可以自定义，并且是类型安全的，更易于阅读。

# 偷我的电子书

我发现了很多高质量评论的技巧。你可以在这里偷到它们[。](https://zivce.gumroad.com/l/become-high-quality-code-reviewer)

# 2.使用`varargs for array-like arguments`

当你需要传递一个参数数组时，使用`varargs`。Varargs 接受零个或多个参数。瓦拉格斯有一个`variable arity`。`variable arity`表示参数的数量是可变的。

*`*varargs*`*为什么好？*使用`varargs`传递一组参数轻而易举。如果没有`varargs`,你需要创建一个数组并将其作为参数传递。用`varargs`、*减少代码增加了未来代码评审的质量。**

**你不该拿* `*varargs*` *做什么？*不要把泛型和`varargs`混在一起。如果您将两者混合使用，就需要解决未检查的警告。Joshua Bloch 和 Maurice 深入探讨了这一主题。要点是数组创建受到未检查的强制转换的影响。当混合使用`varargs`和泛型时，你需要确保类型安全。混合泛型和`varargs`增加了更多的编译时问题，给评审者带来了困难。*

**什么时候不该用* `*varargs*` *？* Joshua 建议为`varargs`创建数组会降低性能。需要性能的时候不要滥用`varargs`。*代码评审可以吃亏，性能不能吃亏。**

*[*如何将一个数组传递给变量 arguments 方法*](https://stackoverflow.com/questions/2925153/can-i-pass-an-array-as-arguments-to-a-method-with-variable-arguments-in-java) *？*你可以使用`Object…`和`Object[]`，因为它们在幕后是一样的。你不应该去掉`varargs`，来满足这个方法的参数。可变参数使我们的代码更加甜美，因此更容易理解。*

*[工作示例](https://stackoverflow.com/questions/2925153/can-i-pass-an-array-as-arguments-to-a-method-with-variable-arguments-in-java/2926653#2926653)*

# *3.改进您的错误处理*

**对可恢复条件使用检查异常，对编程错误使用运行时异常。**

*使用检查异常意味着您的系统可以恢复。一个例子是`RetryLaterException`。你需要重新安排你的任务，并继续执行。使用这些异常进行更可靠的错误处理。*

**“未检查的异常对您的开发人员体验有什么影响？查明您需要解决的故障点。有助于调试，因为您获得了堆栈跟踪。通过适当的日志记录查明持续的故障。”——*[*来源*](https://medium.com/javarevisited/5-advantages-of-java-exceptions-java-developers-should-know-29412f2fd330)*

*运行时异常应该指示不可恢复的错误。最臭名昭著的是`NullPointerException`。创建自己的运行时异常，记录环境，并停止执行。运行时错误可能是来自客户端的错误请求。由于关键部分通常会引起注意，所以审阅者会格外注意错误处理。*

**什么时候不该使用异常？当您可以使用简单的错误处理实践时。使用自定义错误代码、空集合、可选值或布尔值。当这些足以表明错误时，使用它们。比起异常，更喜欢简单的错误处理。**

**“当通常疯狂的事情发生时，软件幸存下来。”——詹姆斯·高斯林对* [*异常*](https://web.archive.org/web/20060407124657/http://www.artima.com/intv/solid2.html)*

# *外卖食品*

*好的评审导致更少的错误、更多的专业知识和知识转移。改进您的代码以结束糟糕的代码审查。*

*使用更通用的解决方案。Java 中的泛型实现了所有这些功能，而且没有任何成本。泛型简化了您的代码，使其可读性更好。*

*尽可能使用`varargs`。可变参数有助于减少样板文件。减少代码行数可以改进代码审查。*

*修改您的错误处理。争取更简单的错误处理，但也要改进您的异常处理。错误处理吸引了很多评论，你应该尽力让它更简单。*

# *参考*

*[1] [代码评审研究](https://sail.cs.queensu.ca/Downloads/EMSE_AnEmpiricalStudyOfTheImpactOfModernCodeReviewPracticesOnSoftwareQuality.pdf)*

*[2]Joshua Bloch 的《有效的 Java》*

*[3] Java 泛型和集合，作者 Maurice Naftalin 和 Philip Wadler*

*[4]麦金托什，谢恩等人，“现代代码评审实践对软件质量影响的实证研究。”*