# 有经验的开发人员喜欢的 5 个习惯

> 原文：<https://blog.devgenius.io/5-habits-experienced-developers-like-for-smooth-java-code-review-c5e5ae683df7?source=collection_archive---------0----------------------->

## 【Java 开发人员应该知道的加快代码审查的 5 个技巧

![](img/b0e6016908b12edafab5ac9faa04e2ce.png)

由[马体·米罗什尼琴科](https://www.pexels.com/@tima-miroshnichenko?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄，来自[佩克斯](https://www.pexels.com/photo/man-person-people-woman-6914069/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)——由作者编辑

大多数经验丰富的 Java 开发人员喜欢在你的简历上看到以下内容。

Java 开发人员会在 PR 上盖上橡皮图章，然后继续前进。橡皮图章忽略了代码审查的要点。 ***重点是得到一个内聚的代码库。***

如果我们对代码有一个共同的理解，我们将来会更容易地修改它。

***你能在代码评审中发现什么？为什么这些实践首先对您的代码库有好处？什么可以提高可读性，让代码评审更快？***

让我们深入了解一下经验丰富的开发人员喜欢在你的公关中看到的 5 个习惯。

# 1.喜欢唱片胜过龙目岛

***为什么沟龙目为首辅？***

不需要额外的依赖。可以用记录代替`@Value`。有了 Java 17，这将使你的代码更简洁，更适合重构。

龙目岛做了很多见不得人的背景材料。如`SneakyThrows`实现哪个 [***【停止 javac 的牢骚】***](https://github.com/projectlombok/lombok/blob/master/src/core/lombok/Lombok.java#L30) ***。*** 去龙目岛后头脑中如此光秃秃。

不要误解我的意思，如果你使用的是较低版本的 Java，Lombok 是你的朋友。而且事半功倍。 ***但是如果你有记录，就用它们代替龙目。***

***龙目岛拿不到你的福利记录却可以。***

***举个例子，一个*** [***穷尽将来的开关***](https://openjdk.org/jeps/405#Record-patterns-and-exhaustive-switch) ***。有了记录，这将是本机的，而对于 Lombok 注释的类，它们不会马上出现。***

`@Data`或`@Value`哪一个把我们带到*不可变的*对象？

对于 Lombok，您有两种选择。一个生成不可变的对象，另一个生成可变的对象。我说的不可变是指浅不变性，引用字段仍然是可变的。默认情况下，记录是不可变的，所以你不必为此争论。

# 2.根据需要使用`Optional`

大多数人会弄错可选用法。

并且可能有可选的或其他的控制流。该类旨在显示潜在的值缺失，而不是空检查。有些人甚至完全不喜欢 Optional，不在他们的代码库中使用它。

***大多数会滥用可选，或者给它赋值 null。***

更加注意`Optional`的用法，质疑不寻常的用法。如果你包装了很多选项或者包装了列表，这是应该避免的。

知道在哪里应用`Optional`有助于提高代码的可读性。

正确使用静态`Optional`方法很有帮助。

*   `Optional#of` —告知包装的对象必须为非空
*   `Optional#ofNullable` —告知包装的对象可为空
*   `Optional#empty` —告诉我们被包装的对象不存在。缺失不同于可空性

***亦作*** `***Optional***` ***将来会享受瓦尔哈拉的改进。***

你不会使用`OptionalInt`，而是使用`Optional<int>`。此外，Valhalla 将把这些转化为值类型，这将改善内存占用。所以多了解一些可选的正确用法对以后会有帮助。

同样重要的一点是，即使在瓦尔哈拉之后，可选也是一样的。

是的，我们知道。向后兼容，咄。 ***是的，所以可空可选大概是留到以后吧。*** 不是我们喜欢的东西，而是为了源码兼容需要的东西。

# 3.不要硬编码实例

大多数人会瞬间瞥见。

他们会使用它并继续前进。

尽管这使得代码难以测试。您可以模仿静态方法，但是有更好的方法来测试时间。

你可以使用`InstantSource`，传入不同的时钟。这样你就可以在测试中控制时间，而不是模仿静态的`Instant#now`方法。

这是相关的测试。我们可以传入时钟，改变时区，在一个单元测试中做所有的事情。

# 4.您应该在 API 中使用枚举

例如， [Spring Boot 遇到了 HttpStatus 枚举的问题。](https://github.com/spring-projects/spring-framework/issues/26842)

他们使用了一个枚举作为响应，并陷入了内存问题。这种整数状态码的解析给`HttpStatus`分配了大量的内存。

这是用那些枚举值的缓存[解决的。](https://github.com/spring-projects/spring-framework/commit/7f1062159ee9926d5abed7cadc2b36b6b7fc242e)

你应该在 API 响应中避免枚举。

对于响应，如果表示的值有限，则使用枚举。比如常数，或者很少变化的东西。这里有一个[错误枚举](https://developers.google.com/people/api/rest/v1/people#agerange)的例子，它因为某种原因而被弃用。

***在代码内使用枚举是可以的，也可以作为 API 输入。***

例如，您可以将 enum 与实体连接，并存储一些配置属性。此外，您可以在数据库中存储枚举，因为也有对它的支持。

另一种响应方法是使用预定义的值。类似于[地址响应是如何表示的](https://developers.google.com/people/api/rest/v1/people#address)。这使得在消费端处理值变得更加容易。

# 5.你如何在函数接口中定义默认方法？

假设您需要以下内容:

这是不可能的，因为`FunctionalInterface`期望单个方法声明。所以这留给我们以下的解决方案。不太好，但这个也可以。

你需要随时创建这个`DefaultMyInterfaceImplementation`来使用它。如果这是默认实现，我们能不能把它移到接口上？毕竟我们可以在需要的时候参考使用。

下面是[霍尔格为此提出的](https://stackoverflow.com/a/30165602/5999670)。

这样我们得到了 lambda 的默认实现，所以没有类实例化。此外，我们可以为特定的用例覆盖`authorize`。更少的代码和更简洁的解决方案。

所以即使我们在`FunctionalInterface`中没有默认的方法，我们也不会放弃这个想法。只需寻找一条不同的道路来完成你所需要的。