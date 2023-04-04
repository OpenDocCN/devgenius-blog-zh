# 经验丰富的 Java 开发人员使用的 7 个良好的代码审查实践

> 原文：<https://blog.devgenius.io/7-good-code-review-practices-seasoned-java-developers-use-6cf83d3deb68?source=collection_archive---------2----------------------->

## 如何执行高质量的 Java 代码评审

![](img/774cdf1f270ae230fe2ac08cb14d22ab.png)

来自[派克斯](https://www.pexels.com/photo/photo-of-man-sitting-in-front-of-people-3184299/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[派克斯](https://www.pexels.com/@fauxels?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的照片

*代码评审对于分散知识至关重要。*

一些开发人员不知道在代码审查中要寻找什么。他们会抱怨风格问题，而代码质量会受到影响。

您应该知道如何执行高质量的 Java 代码评审。这将使你与众不同，并教会你更多。

这里有 7 个好的 Java 代码审查实践。

# 使用方法引用

***尽可能使用方法引用。比起绑定的方法引用，更喜欢未绑定的。***

我会建议使用未绑定的(`Class::method`)，因为它们不会有很大的捕获成本。绑定的`this::method`会有额外的捕获开销。他们需要捕获比未绑定方法引用更大的词法范围。

***方法引用是一种 lambda 表达式。*** 正因如此，它们的编译方式不同。这里有一个例子。

***Lambda 运行时不失败，方法引用失败。***

答案就在这个[规格](https://docs.oracle.com/javase/specs/jls/se13/html/jls-15.html#jls-15.13.3)中:

*首先，如果方法引用表达式以 ExpressionName 或 Primary 开头，则评估该子表达式。* ***如果子表达式的计算结果为*** `***null***` ***，则抛出一个*** `***NullPointerException***` ***，方法引用表达式突然完成。*** *如果子表达式突然完成，方法引用表达式也会因为同样的原因突然完成。*

并且 [lambda 在编译阶段](https://cr.openjdk.java.net/~briangoetz/lambda/lambda-translation.html)早期失败。

*当编译器遇到 lambda 表达式时，它首先将 lambda 主体降级(反糖)到一个方法中，该方法的参数列表和返回类型与 lambda 表达式的匹配，可能带有一些附加参数(如果有的话，用于从词法范围捕获的值)。)*

***同时使用 lambdas 和方法引用，但要注意有副作用的方法(线程，或者异步函数)。***

# 首选功能`for`循环

***比起传统的 for 循环，更喜欢*** `***IntStream***` ***。***

`IntStream`降低复杂性，隐藏样板文件，产生更好的代码。这里有一个来自文卡特的例子。

`IntStream`解决的另一个问题是突变。索引变量的突变，如果使用`IntStream`就不会发生。

有了`IntStream`，我们不再需要临时变量。传统的 for 循环需要它来确保索引变量实际上是最终的。默认情况下，这实际上是最终版本。

使用非传统的`for`方法，你确实会损失一些性能。即便如此，您也可以通过并行执行来解决这个问题。使用流更容易实现并行化。

 [## JMH 观察仪

### JMH 可视化工具可以帮助你可视化你的 JMH 基准测试结果。只要上传你的 JSON 报告，你就会得到它…

jmh.morethan.io](https://jmh.morethan.io/?gist=0dac70e0966679e160fdfbbf44ade61e) 

# 首选功能界面

***函数接口是具有单一方法的接口。*** 这些也被称为 SAMs 或者单一抽象方法。

功能接口与流混合得很好。您也可以创建自己的，或重用现有的。现有的 Sam 有`Function<T,R>`、`Predicate<T>`、`Consumer<T>`等。您也可以创建带有`@FunctionalInterface`注释的自定义模板。所有这些都比创造传统方法更有效。

有时使用现有的 SAM 更好。它们通过设计具有描述性的名称。此外，您不必创建新的接口，这将增加代码的可读性。

这里有一个例子。当我们使用 SAM 时，我们可以传入匿名类或 lambda 表达式。后者当然更好。

# 测试预期异常

大多数钻杆排放系统没有这些测试。

开发人员也应该测试模糊案例。这就是为什么您应该检查预期的异常。您可以在 JUnit 中使用`@Test`中的`expected`值来这样做。

有了更新的 JUnit，JUnit 5，[可以用](https://github.com/junit-team/junit5/blob/main/junit-jupiter-api/src/main/java/org/junit/jupiter/api/AssertThrows.java#L41) `[assertThrows](https://github.com/junit-team/junit5/blob/main/junit-jupiter-api/src/main/java/org/junit/jupiter/api/AssertThrows.java#L41)` [代替](https://github.com/junit-team/junit5/blob/main/junit-jupiter-api/src/main/java/org/junit/jupiter/api/AssertThrows.java#L41)。这是比`expected`更好的选择。为什么？因为您可以检查异常。使用`expected`可以验证类型，使用`assertThrows`也可以验证状态。

# 如何使用断言？

这里有一个很好的关于异常和断言区别的备忘单。

[来源](https://github.com/google/guava/wiki/ConditionalFailuresExplained#summary)

断言错误表示程序中的错误。例外显示例外情况。这两个不一样，你应该知道什么时候用。

断言错误可能位于静态类的私有构造函数中。这是一个编程错误的例子。那是不应该发生的事情。

# 使用静态实例初始值设定项

您可能会看到填充单例成员`HashMap`的方法。

你可以把它移到一个静态初始化器中。 这防止了每次调用该方法时的地图填充。还有，这样读更清楚。

您只能从静态初始化器中引用类的静态成员。这是初始化实例的好方法，不需要通过构造函数。

您可以将它与多个构造函数一起使用，以防止重复初始化。 ***一次静态初始化器胜过多次初始化。***

# 强制枚举

***比起字符串常量，更喜欢枚举唯一接口，或者一般的常量。***

在以前的企业项目中，我们会有这些。即便如此，这个接口也是一个反模式。本质上不是反模式，[而是对接口的糟糕使用](https://stackoverflow.com/a/2659740/5999670)。

现在使用`EnumSet`来代替只有常量的接口。

*为什么要用* `*EnumSet*` *而不是常量专用接口？* `EnumSet`最多可以定义 64 个常量，在大多数场合足够了。由于枚举集后面的位集，您还可以获得更快的访问速度。`EnumSet`实现了`Set`接口，所以你也将得到`Set`操作。

***枚举节拍字符串常量。***

`Enum`会给你比`String`更大的灵活性。你可以用`Enum#values`得到所有的枚举并和它们一起工作。如果您想将枚举映射到其他结构，可以将枚举放在`EnumMap`中。或者放在`EnumSet`里，在测试或者其他地方使用。

***代码评审是一个重要的过程。***

尽管如此，大多数开发人员不知道如何去做。我为你准备了一个不同的代码审查视角。 [查看这里。](https://zivce.gumroad.com/l/become-high-quality-code-reviewer)