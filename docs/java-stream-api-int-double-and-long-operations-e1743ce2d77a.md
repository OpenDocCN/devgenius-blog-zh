# Java 流 API — int、long 和 double 操作

> 原文：<https://blog.devgenius.io/java-stream-api-int-double-and-long-operations-e1743ce2d77a?source=collection_archive---------7----------------------->

![](img/2c821e35cf19d5d499f4a638b088d227.png)

照片由[艺术工作室](https://www.pexels.com/@nick-bondarev?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[像素](https://www.pexels.com/photo/rocky-shore-of-mountain-river-in-fog-4337894/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

如果您必须对一个对象流中的整数值求和，您会考虑一些选项，比如利用`reduce`流函数。或者…最广为人知的一个:遍历所有对象，用值的运行总和保存一个变量

`Stream` API 提供了一种非常简单的方法来计算总和，寻找最小值或最大值，并计算对象流的平均值。

根据您的数字数据类型(`int`、`double`或`long`)，您可能需要使用`IntStream`、`DoubleStream`或`LongStream`。鉴于三种不同数据类型的一切都非常相似，我将只关注`IntStream`类。

根据 Java 的文档，一个`IntStream`被定义为

> 支持顺序和并行聚合操作的原始整数值元素序列。这是 Stream 的 int 原语专门化。

## 如何构建 IntStream？

你有两种方法来构建一个`IntStream`。您可以使用 IntStream 中的 builder 方法构建一个`IntStream`

```
IntStream
    .builder()
    .add(1)
    .add(3)
    .build();
```

您还可以通过使用一个现有的流来构建一个新的`IntStream`，并将其映射到一个`IntStream`。您可以通过调用`mapToInt`函数来实现这一点。

```
IntStream intStream = cars
  .stream()
  .mapToInt(car -> car.getNumberOfSeats());
```

如果您要使用`DoubleStream`或`LongStream`，方法将是`mapToDouble`和`mapToLong`。

## 我能用 IntStream 做什么？

一旦你构建了一个`IntStream`，你可以选择调用这些方法中的一个:
1-sum
2-min
3-max
4-average
5-count

这些名字不言自明。这些方法允许您添加、获取最小值、获取最大值、计算平均值或计算流中元素的数量。

假设您需要获得一个流中所有整数的平均值，您可以很容易地这样做:

```
cars
    .stream()
    .mapToInt(car -> car.getNumberOfSeats())
    .average();
```

这样做的一个很大的好处是，它现在简化了您执行此操作的编码，并且在幕后，它处理了您很容易忽略的一些小事，例如在计算空流的平均值时处理被零除的情况。

需要指出的是，`average`方法不会返回一个`double`，它会返回一个`OptionalDouble`类型的对象。`OptionalDouble`类提供了一个类似于`Optional`类的接口。

如果要提取平均值结果的值，需要调用`getAsDouble()`方法。唯一的问题是，如果数据流是空的，没有平均值，会发生什么？您的代码可能会抛出异常。

一个简单的替代方法可能是在可选对象上调用`isPresent()`方法，如果返回 true，则调用`getAsDouble()`。

```
OptionalDouble optionalAverage = Stream
        .*of*(1, 2, 3)
        .mapToInt(car -> car)
        .average();if (optionalAverage.isPresent()) {
    double average = optionalAverage.getAsDouble();
}
```

现在您可能已经发现，并不是所有的其他方法都会返回一个`int`。让我们快速回顾一下他们所有的返回类型

1 - sum —返回一个`int`。如果没有元素将返回零。
2 分钟—返回一个`OptionalInt`。首先使用`isPresent()`验证，然后调用`getAsInt()`。
3 分钟—返回一个`OptionalInt`。首先使用`isPresent()`验证，然后调用`getAsInt()`。
4 -平均值—返回一个`OptionalDouble`。首先使用`isPresent()`验证，然后调用`getAsDouble()`。
5 -计数—返回一个`long`。

## 汇总统计数据

一旦你构建了一个`IntStream`，你现在可以获取一个`IntSummaryStatistics`类型的对象。这个对象可以让你访问总和、最小值、最大值、计数和平均值。

您可能会问，当您可以通过`IntStream`类直接获取所有这些信息时，这样做有什么好处。当将`IntSummaryStatistics`类与其他数据的汇总统计数据相结合时，它的一个好处就体现出来了。

让我们通过这个例子来评估一下，我们最初有两个列表，一个叫做`suvs`，另一个叫做`sedans`。我们将基于这些对象的属性`numberOfSeats`的值将二者映射到一个`IntStream`。

```
IntSummaryStatistics statisticsForSUVs = suvs.stream()
    .mapToInt(x -> x.getNumberOfSeats())
    .summaryStatistics();IntSummaryStatistics statisticsForSedans = sedans.stream()
    .mapToInt(x -> x.getNumberOfSeats())
    .summaryStatistics();
```

在我们完成这些之后，你现在可以用最少的努力组合这两个`IntSummaryStatistics`的结果，并得到这些结果的组合统计数据。

通过现在调用`combine`方法，它会将两个统计数据合并成一个，并自动为您计算所有这些统计数据。您现在的平均值是综合统计值，最小值、最大值、总和以及计数也是如此。

```
statisticsForSedans.combine(statisticsForSUVs);
```

我希望在阅读完这篇文章之后，您能够更好地理解如何利用 Stream API 对数字流执行数学运算。

尽情享受吧！