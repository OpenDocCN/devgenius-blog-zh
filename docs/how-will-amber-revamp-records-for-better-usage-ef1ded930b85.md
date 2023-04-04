# 琥珀将如何修复记录以更好地使用

> 原文：<https://blog.devgenius.io/how-will-amber-revamp-records-for-better-usage-ef1ded930b85?source=collection_archive---------7----------------------->

## 5 Java 记录更改，这将使您的开发更容易

![](img/a793823ae4962f53cb13463b09d41bbe.png)

照片由来自[佩克斯](https://www.pexels.com/photo/man-person-people-woman-6914058/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[马体·米罗什尼琴科](https://www.pexels.com/@tima-miroshnichenko?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

***爪哇的记录将来会改变。为了更好。***

如果你跟上了这些变化，你会得到很多好处。更快的重构、简洁的代码和更好的性能。

原始类型[会带来布局优化、编译器优化等等](https://www.youtube.com/watch?v=1H4vmT-Va4o&t=915s&ab_channel=Java)。如果我们能把这些和记录结合起来，那将会发生什么是显而易见的。

***哪 5 个记录变化会让你的代码更好？***

# 1.你能在 API 中使用记录吗？

***你可以使用记录作为 API 的有效载荷。*** 这很好，直到[你放弃某些字段](https://mail.openjdk.org/pipermail/amber-dev/2022-June/007373.html)。

您可以弃用该字段，但要确保有一个向后兼容的构造函数。在未来，也需要解构者。

即使这样，源代码和二进制代码的兼容性也将得到满足。

***那么你需要使用类型模式和访问器来确保兼容性吗？或者一个记录模式就足够了？***

只要我们小心使用构造函数和解构函数，这应该是可行的。

例如，如果我们有以下内容。

```
record Point(int x, int y, int z) {}
// new version removes the z
record Point(int x, int y) {
    @Deprecated
    Point(int x, int y, int z) { this(x,y); } //ignore the Z
}
```

下面的每一个都有一个解构器。意思是如果我们想忽略 z，我们可以忽略它。

```
if(o instanceof Point(int x, int y, int z)) {// z will be 0 if this is new deconstructor}
```

# 2.您不会在注释中使用记录

注释只允许在声明中包含常量。并且记录在注释中不可用。

***反之亦然记录上的注释即使在今天也是可能的。***

现在就可以添加 [@Builder](https://github.com/projectlombok/lombok/issues/2356) 为例。不过，如果我们在记录中得到萎顿，这就没有必要了。

# 3.你需要原始记录吗？

记录是身份类。但是也有创造原始记录的可能性。那么这些记录是否有匹配的模式呢？

***对于当前记录，将内置解构。对于其他特定的记录，比如原始记录，您需要定义显式的解构器。***

我们可以将这与任意的类进行比较。这意味着这些新定义的(原始)记录模仿任意的类。

***了解原始物体有什么重要的？***

***原始物体被剥夺了身份。*** 这种特质使我们能够在记忆中展平物体。

当我们剥离身份时，我们可以按需解构和构建对象。 ***当被这样询问时，对象表现得像一个对象。*** 在幕后，我们将这些存储为原语。

因为没有身份，我们可以构造和解构物体。这些将是新的值相等的原语。他们没有任何身份。

# 4.详尽无遗将使你的记录易于维护

随着记录的改变，所有相关的方法也会改变。将这与密封类型结合起来，你在重构方面会快很多。

如果你添加了一个新的允许类型，就需要更新所有的开关表达式。这实现了更快的重构和类型安全。

让我们回顾一下你能做什么和不能做什么。

***你现在哪里得不到穷尽？***

[混合枚举和记录，并期望穷尽工作。由于枚举没有解构，你会遇到编译问题。](https://stackoverflow.com/questions/73787918/java-19-compiler-issues-when-trying-record-patterns-in-switch-expressions)

这里 enum 和 record 都实现了接口`Opt`。在这种情况下穷尽是行不通的。即便如此，这一场景的详尽性在未来很可能还会存在。

如果我们定义了以下接口，这将会起作用:

然而，如果我们添加枚举，它将失败，因为解构无法对枚举起作用。

上面的错误意味着你想要解构一个任意的类。在这种情况下，是一个枚举，但也可以是您的自定义类。

您还可以将开关与记录及其类似元组的特性结合起来，以获得简洁的代码。

您可以更改此代码:

到这段代码中:

# 5.威瑟斯会移除建筑者

之前我们提到过`@Builder`。您可以添加这个并获取记录的构建器。

*建造者解决大建造者的问题。*大型构造函数的问题应该由 Java 来解决，而不是第三方。[威瑟斯](https://mail.openjdk.org/pipermail/amber-spec-experts/2022-June/003461.html)以后解决大施工员的麻烦。

我们会得到一个设置了 x 值的记录。否则，我们需要记录中的 setters。

此外，威瑟斯应该删除建设者。

[信号源](https://mail.openjdk.org/pipermail/amber-spec-experts/2022-June/003461.html)

如果需要，您可以根据其他字段计算某些字段。

这是目前的方法:

我们不再需要为每个自定义构造函数调用规范构造函数。

# 这些改进将全面改进我们的工作

琥珀项目还能实现什么？他们会试着把一些好的东西移到任意的类中。

***未来的一个特点将是对任意类的解构*** 。解构现在只适用于唱片。

尝试解构枚举时的当前错误如下。未来任意的阶级解构应该也是有的。

```
Exception in thread "main" java.lang.InternalError: Exception during analyze - java.lang.NullPointerException: Cannot invoke "com.sun.tools.javac.code.Symbol$ClassSymbol.getRecordComponents()" because "deconstructionPatterns.head.record" is null
```