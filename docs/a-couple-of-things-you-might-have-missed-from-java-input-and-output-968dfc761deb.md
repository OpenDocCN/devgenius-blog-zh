# Java 输入和输出可能会遗漏一些东西。

> 原文：<https://blog.devgenius.io/a-couple-of-things-you-might-have-missed-from-java-input-and-output-968dfc761deb?source=collection_archive---------5----------------------->

## buffered reader br = new buffered reader(new InputStreamReader(system . in))和 System.out.println()是 Java 中两个常用的语句，用于接受输入和打印一些输出。

![](img/5e1674a8b27fafb13814ff39e0b48d5e.png)

托德·米滕斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

但是打住，我们有没有停下来一秒钟，想一想，一定是发生了什么 ***【幕后】*** ，在源代码里。使用了什么预编译类，调用了什么方法。

好吧，如果没有，让我们现在讨论一下。我和你，过了这个 ***中*** (挤眉弄眼)。

1.  PrintStream 类型有一个公共的 final 静态字段，在类系统中称为“***out”***。

> ***public final static PrintStream out***

变量或字段， ***out*** 全部设置为保存类 PrintStream 的实例，来自 Java 的 io 包。 ***out*** 用 PrintStream 的实例初始化，JVM 运行时调用类 System 的私有方法 ***initializeSystemClass()，运行时调用*** 。而当我们使用 System.out.println()语句时，PrintStream 类的 println()方法被调用。因为 out(类 System 的一个静态变量)持有 PrintStream 的实例。

2.BufferedReader 拥有类似***“buffered Reader(Reader in)”的构造函数。*** 所以没关系，如果我们把参数传递给所示的构造函数，比如，***" buffered reader(new InputStreamReader(system . in))。*** 到目前为止，参数 InputStreamReader 正在被传递给所示构造函数的参数读取器。没关系，因为 InputStreamReader 是 Reader 类的子类，Java 编译器支持这种类型转换。

InputStreamReader 有一个构造函数，如***“InputStreamReader(InputStream in)”所示。***

在 class System 的 中有一个 InputStream 类型的公共 final 静态字段，名为 ***。***

> *中的公共最终静态 InputStream*

*中的 ***在运行时被初始化并保存 InputStream 的实例，当 JVM 调用类 System 的私有方法 ***initializeSystemClass()时。*** 所以当 System.in 被传递给 InputStreamReader 构造函数时，它实际上保存了类 InputStream 的实例。****