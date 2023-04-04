# Java 中的 Try-with-resources 可以有多个资源

> 原文：<https://blog.devgenius.io/try-with-resources-in-java-can-have-more-than-one-resource-5f832de4e9a6?source=collection_archive---------3----------------------->

![](img/c4e91c3e0b6408202ee4e0c57c887c22.png)

照片由[周隼](https://unsplash.com/@chris_chow?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

它就在名字里:resources，复数，不是单数。这种罕见的 Java 分号推理导致人们忘记或者没有意识到，Java 中的 Try-with-resources 语句可以有多个资源。

让我们回顾一下更常用的单一资源语句，以及它现在已经基本上取代的旧用法。旧用法大致如下:

```
SomeResource resource;
try {
    resource = new SomeResource(param);
    // TODO: Use the resource
} catch (Exception e) {
    // TODO: Replace with more specific exception
} finally {
    resource.close();
}
```

现在我们可以把它改写成

```
try (SomeResource resource = new SomeResource(param)) {
    // TODO: Use the resource
} catch (Exception e) {
    // TODO: Replace with more specific exception
}
```

如果实例化或使用资源会导致检查异常，那么 Catch 块可能仍然是必要的，否则您需要更早地放入 Throws 子句。如果最终只关闭了一个资源而没有做任何其他事情，那么它现在肯定是不必要的。

一般来说，从控制台获取用户输入的`Scanner`资源，正如它更常用的那样，不会抛出异常，不管是检查的还是未检查的。

```
Scanner input = new Scanner(System.in);
// TODO: Get user input on something or other
input.close();
```

您的集成开发环境(IDE)可能会为您提供改变资源的方法。如果您接受 IDE 的建议，IDE 会将上述内容更改为以下内容:

```
try (Scanner input = new Scanner(System.in)) {
    // TODO: Get user input on something or other
}
```

不用写`input.close()`。编译器会为你进行调用。最终，您可能会养成立即自己编写单一资源语句的习惯。

感觉没什么大不了的。在这个`Scanner`的用例中，它没有为我们节省任何行。在最好的情况下，它会使 IDE 少发出一个警告。

但是，如果您需要两个或更多相关的资源，并且这些资源会抛出异常，那么多资源语句可以帮助您用更少的行更清晰地完成相同的目标。

我正在为一个文件选择器编写测试。我想在每次测试运行后看到测试文件的内容，我认为我需要一个`FileReader`和一个`Scanner`来完成这个任务。所以我在测试课上写了这个:

根据 Joost Visser 等人在*构建可维护软件:Java 版*中的指导方针，我们应该努力将`printFile()`减少到 15 行或更少。我们应该通过真正的削减来做到这一点，而不是把我们通常放在多行上的东西放在一行上。

我最近才意识到一个`Scanner`可以带一个`File`。这意味着我可以将其重构为单资源语句，然后我可以删除`IOException`的 Catch 子句，因为只有它的子类`FileNotFoundException`被那个特定的`Scanner`构造函数声明为抛出。

只要那个特定的`Scanner`构造函数能够定位指定的文件，`Scanner`实例大概会吸收读取文件时出现的任何异常。那可能会让我们降到 15 线以下。

但是为了这个例子，让我们继续两个资源的陈述。如果您的 IDE 提供转换为 Try-with-resources，请接受该提议，并让它为您完成。

您可能需要做两次，第一次是针对`FileReader`，第二次是针对`Scanner`。之后，你可能需要清理缩进，或者调用你的 IDE 的自动格式化清理。

你可能在之前的版本中注意到了，我先写了`scanner`的关闭，再写了`reader`的关闭。我认为先进后出在这里是有意义的。而且除此之外，`scanner`还要靠`reader`。

多资源语句的结束顺序也是这样吗？我不知道，但是我可以通过几个玩具示例资源轻松找到答案。

为了这个目的，一个类被认为是一个资源所需要的就是让它实现来自`java.lang`的`AutoCloseable`。所以，这个演示:

运行`main()`，输出应该是这样的:

> 运行:
> 该资源现已打开
> 该资源现已打开
> 正在启动该资源
> 正在启动该资源
> 该 com . example . resources demo＄inner resource @ 15db 9742 现已关闭
> 该 com . example . resources demo＄outer resource @ 6d 06d 69 c 现已关闭
> 构建成功(总时间:0 秒)

散列码可能不同，第一行和最后一行可能不同，这取决于您是使用 NetBeans 之类的 IDE 还是从命令行运行它。

重要的是，这表明资源关闭的顺序与它们打开的顺序相反。

# 概括起来

尽管 Try-with-resources 几乎总是与单个资源一起使用，但是请记住，您可以将 Try-with-resources 与多个资源一起使用。这些资源用分号分隔，就像同一范围内的任何其他语句组一样。

对于多个资源，假设正常执行，第一个声明的资源将最后关闭，最后一个声明的资源将首先关闭。

顺便说一句，如果你愿意，可以在单资源语句中放一个分号，但你不必这样做，因为 Java 编译器会推断出来。

至少还有一种情况，Java 编译器可以推断出分号。你知道这是什么吗？