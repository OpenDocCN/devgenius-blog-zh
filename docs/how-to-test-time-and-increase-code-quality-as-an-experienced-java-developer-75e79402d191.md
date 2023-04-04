# 作为一名有经验的 Java 开发人员，如何测试时间和提高代码质量

> 原文：<https://blog.devgenius.io/how-to-test-time-and-increase-code-quality-as-an-experienced-java-developer-75e79402d191?source=collection_archive---------7----------------------->

## *这里有一个关于使用 InstantSource 进行时间旅行测试的深入教程*

![](img/26a9265fe4949634329b45379d26678e.png)

图片由 [RF 提供。_.来自](https://www.pexels.com/@rethaferguson?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)[像素](https://www.pexels.com/photo/crop-focused-repairman-fixing-graphics-card-on-computer-3825582/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的工作室

***大部分 Java 开发者都不测试时间。尽管它可以清除很多 bug。***

他们避免时间测试。或者他们硬编码日期。反过来时间虫穿越。

***不要硬编码日期。***

有些人甚至在测试中使用硬编码日期。这在某些情况下可能行得通。尽管如此，你还是会得到很多时间常数和难以维护的测试。

硬编码的日期会产生*测试定时炸弹。测试定时炸弹在硬编码日期后爆炸。更多的测试依赖于同一天，更多的维护麻烦。*

***此外，你对硬编码日期的控制也很差。***

你需要改变很多常量。或者您需要为不同的日期创建新的。所以你不能动态控制日期。

你能做些什么来代替呢？如何控制考试时间？如何创建质量更好的时间测试？

让我们试着解决这个问题。

假设我们需要测试下面的代码。这个代码来源于 [*这个例子*](https://mateuszjarzyna.github.io/posts/how-to-test-time-improve-code-quality-pt-1/) ，重点是`Instant`时间。

***进行测试的一种方法是使用*** `***@VisibleForTesting***` ***。***

将`instantSource.instant()`提取到一个单独的方法中。这个方法将是包私有的，并且对于测试是可见的。然后在测试中提供测试数据。

```
@VisibleForTesting  
Instant getInstant() {  
    return Instant.now();  
}// Test.javawhen(service.getInstant()).thenReturn(Instant.now().plus(20, ChronoUnit.DAYS));// or create the test InstanceResetPasswordService r = new ResetPasswordService() {  
    Instant getInstant() {  
        return Instant.now();  
    }  
};
```

***你不会得到所需的时间控制。***

由于你除了`getInstant`之外没有其他方法，所以你受到了限制。您可以偏移时间和其他时间操作。即便如此，测试中还是会加入很多逻辑。

测试会变得比需要的更加冗长。

另一个问题是集成测试。您需要再次为此方法提供响应。

你可以提取代`TokenEntity`来分离类。那么`@VisibleForTesting`将会工作，你将不需要额外的
依赖。

另一个解决方案是分离的构造函数。

我们会忽略一个事实，即`Token`一代并不适合`ResetPasswordService`。我们将添加一些新的构造函数，以支持测试。

我们需要为 DI 标记构造函数，并为测试目的创建一个单独的。我们添加了`@Autowired`注释，这样 Spring 就知道对 DI 使用哪个了。`@VisibleForTesting`是我们将在测试中使用的。

这样我们可以避免额外的依赖。

即便如此，我们也会失去时间控制。我们不能在时间中前进或后退。所以如果这是你需要的，那么这就是解决方案。

如果您使用的是较低版本的 Java，以前的解决方案是可行的。或者您不想添加额外的依赖项。

你不能用这种方法进行时间旅行。您将供应商硬编码到一个`Instant`供应商。虽然您可以为供应商创建一个 setter，但是有一个更好的方法可以做到这一点。

有了 Java 17，我们现在有了`InstantSource`。有了这个类，我们不再需要创建工作区，我们也获得了`Clock`类的好处。

`[*InstantSource*](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/time/InstantSource.html)`是瞬间工厂。此外，您可以提供存根`Clocks`作为`InstantSource`。

为什么这是一个更好的解决方案？想进`Clocks`就进。`OffsetClock`、`TickClock`或其他。或者您可以切换时区并进行测试。这样你就能得到所需的时间控制。

这是测试。

我已经添加了 InstantSource 的一个单独的实例，`TestInstantSource`。这是因为`Clock`中的每个方法都会创建一个新的`Clock`实例。所以你需要一个包装器来存放新的时钟。

使用`InstantSource`使生活变得更容易，因为你没有任何变通办法。`Clock`实例可以放成`InstantSource`，这样包装器，比如`TestInstantSource`，这样就可以控制时间。

`InstantSource`提供一个轻量级的即时发生器。因为大多数时候，你只需要一个`Instant`的供应商。相反，使用`Clock`会增加更多的开销，而且大多数时候是没用的。