# Java 与 Kotlin 在 Android 开发中的对比——16 种利弊

> 原文：<https://blog.devgenius.io/java-vs-kotlin-for-android-development-16-pros-and-cons-9e046f7a4908?source=collection_archive---------4----------------------->

当谷歌[在 2019 年 5 月宣布 Kotlin 作为 Android 开发](https://tproger.ru/news/first-kotlin-android/)的首选语言时，关于[语言](https://tproger.ru/news/first-kotlin-android/)的争论重新点燃。一方面，一切都很简单:你需要用对你个人来说方便的语言来写。但是，不可否认的是，有许多因素会使天平偏向 Java 或 Kotlin。

![](img/ff15d2662a306e32c9c5413314de7e8b.png)

Java 和 kot Lin——Android 开发你更喜欢哪一个？

下面是基于事实和经验丰富的开发人员的意见完成的每种语言的优缺点列表。

# 1.语言时代

# Java 语言(一种计算机语言，尤用于创建网站)

广泛的社区、许多库、解决方案和现成的模块。大致来说，如果一个开发者面临一个问题，他可以很快得到这个问题的答案。

# 科特林

但在科特林的案例中，情况正好相反。语言和相关库的青春，比如 *Kotlinx 序列化*或者*暴露*，让你花很多时间寻找解决方案。此外，语言文档通常归结为“像在 Java 中那样做；只有在这里，你需要改变一点。”

# 2.代码数量

# Java 语言(一种计算机语言，尤用于创建网站)

是的，Java 语法假设代码会比 Kotlin 更麻烦，因此会花更长的时间来编写。

> 虽然这些语言在外观上非常相似，但它们彼此之间也有很大的不同。以下是如何将文本分配给不带库的字段的示例:
> 
> **Java** `*final TextView helloTextView = (TextView) findViewById(R.id.text_view_id); helloTextView.setText("Some text");*`
> 
> **科特林** `*helloTextView.text = "Some text"*`
> 
> IDAP 的安卓开发者 Artyom Borisovskiy

# 科特林

> Kotlin 允许你用更少的代码行写和 Java 一样的东西(扩展、空安全、数据类)。UI 方面的工作值得单独一提:感谢 Android 扩展，你不再需要用`*findViewByld*`处理样板文件
> 
> Reksoft 公司软件工程师 Evgeny Pavlov

# 3.安卓开发初期选什么

# Java 语言(一种计算机语言，尤用于创建网站)

*意见 1:*

> 如今，Kotlin 是 Android 开发的最佳选择。但是如果你是一个初学程序员，那么一开始最好选择 Java:这种语言比较古老，你可以很容易地找到关于如何解决这个或那个问题的信息。不要忘记 Kotlin 是 Java 的包装器，在编译过程中它被转换成 Java 字节码。
> 
> IDAP 的安卓开发者 Artyom Borisovskiy

*意见 2:*

> 现在很难想象没有 Kotlin 的 Android，但 Kotlin 文档假设你懂 Java，所以如果你刚开始做 Android 开发者，最好先从 Java 开始学，然后再转到 Kotlin。
> 
> **Evgeny Pavlov** ，Reksoft 公司软件工程师

# 科特林

不懂 Java 也可以开始学习 Kotlin。但是 Kotlin 仍然使用 JVM，并不是一个成熟的替代品，尽管它在 Android 开发中占据了特定的位置。如果你总是专注于 Kotlin，最好开始学习 Java，或者一般同时学习两种语言。

# 4.发展环境

# Java 语言(一种计算机语言，尤用于创建网站)

Android 应用开发长期以来与 Android Studio 密切相关。这个环境最初是为了使用 Java 而改进的，因此可以一次一个字母地编写代码 IDE 将独立地获取您需要的所有内容。

# 科特林

> 由于 Android Studio 基于 Intellij Idea，而 Intellij Idea 和 Kotlin 是由 JetBrains 创建和开发的，因此，开发人员获得了一个与编程语言密切相关的最新开发环境，大大简化了应用程序开发。
> 
> Evgeny Pavlov ，Reksoft 公司的软件工程师

# 5.选择这种语言的目的是什么

# Java 语言(一种计算机语言，尤用于创建网站)

现有的大多数 Android 应用程序都是用 Java 编写的，你不应该希望它们会用 Kotlin 重写。而且由于 Android 操作系统 UI 是用 Java 开发的，这种语言有一个 SDK，还有很多现成的库，所以很多公司还是倾向于 Java。

# 科特林

> 如果应用程序的生命周期很长(例如，移动银行)，并且跨平台开发的问题甚至不值得考虑，那么我们认为 Kotlin 是最佳选择。它与 Java 完全兼容，这意味着两种语言可以在同一个项目中使用。与旧版本的 Java (7 及更早版本)相比，Kotlin 有许多新特性，使得编写代码更加容易。同时，Kotlin 不像 Java 8 那样依赖于 Android 版本。
> 
> 德米特里·内莫夫，移动开发部主管。SimbirSoft

# 6.观点

# Java 语言(一种计算机语言，尤用于创建网站)

> 现代公司越来越多地在科特林开发移动应用。但是你也可以找到一个特定的只支持 Java 的库，如果你不懂这种语言，你会很难找到。
> 
> IDAP 的安卓开发者 Artyom Borisovskiy

况且，Kotlin 是一门年轻的语言，下一步会怎么样还不知道，而 Java 的特点是跨平台:不仅移动开发基于它，还有一个带桌面的后端。

# 科特林

现在，Kotlin 的发展相当可预测，并明确专注于 Android 开发。越来越多开始创建移动应用的年轻公司选择它，现在 Kotlin 开发人员短缺。因此，这种语言需求量很大。在可预见的未来，它不太可能取代 Java，但它很可能会共存，逐渐流行起来。

# 结论

技术上的差异是显著的，但是如果你打算做 Android 开发，两者都要学。大多数流行的库都向后兼容 Kotlin，Java 代码可以在 Kotlin 中使用，反之亦然，但是要解决一个问题或者简单地理解文档的所有细微差别，您应该了解 Java。掌握了这两种编程语言，你将成为一个抢手的专家，并在开发 Android 应用程序方面为自己争取一个好的未来。