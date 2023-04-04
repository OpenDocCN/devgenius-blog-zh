# Java 17:你必须知道的 5 大特性

> 原文：<https://blog.devgenius.io/java-17-top-5-features-you-must-know-bbed2afaea3d?source=collection_archive---------2----------------------->

## 软件工程之旅

## Java 17:作为软件工程师你必须知道的 5 大特性

![](img/4a241f9470d439dde2cba805c62fd8f2.png)

马克西米利安·魏斯贝克尔在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

# **概述**

在本教程中，我将分享关于 Java 平台的新闻， [Java SE 17](https://jdk.java.net/17/release-notes) ，包括新特性和变化。Java 17 LTS 是 Java SE 平台的长期支持版本。JDK 17 二进制文件可在生产中免费使用，并可根据甲骨文免费条款和条件许可协议免费再分发，LTS 代表长期支持。2021 年 9 月 15 日上映**。**

首先，Java 17 中有许多 JDK 增强建议(jep)。我把它们按照 JEP 号码的顺序排列，你可以看到下面的列表:

*   JEP 306:恢复总是严格的浮点语义
*   JEP 356:增强型伪随机数发生器
*   JEP 382:新的 macOS 渲染管道
*   JEP 391: macOS/AArch64 端口
*   JEP 398:反对删除小应用程序 API
*   JEP 403:强封装 JDK 内部
*   JEP 406:开关的模式匹配(预览)
*   JEP 407:删除 RMI 激活
*   JEP 409:密封类
*   JEP 410:删除实验性的 AOT 和 JIT 编译器
*   JEP 411:反对删除安全管理器
*   JEP 412:外部函数和内存 API(孵化器)
*   JEP 414: Vector API(第二个孵化器)
*   JEP 415:特定于上下文的反序列化过滤器

虽然我们在 Java 17 中有很多特性，但我将分享这个 Java 17 版本中任何软件工程师都必须知道的顶级特性。我们开始吧！

# JAVA 17 中你必须知道的 5 大特性

## #1:增强型伪随机数发生器(PRNG)

对于没有 PRNG 背景的人来说，这是一个概念:

> 伪随机数发生器(PRNG)，也称为确定性随机位发生器(DRBG ),是一种用于生成数字序列的算法，该数字序列的属性近似于随机数序列的属性。PRNG 生成的序列并不是真正随机的，因为它完全是由一个初始值决定的，这个初始值叫做 PRNG 种子(它可能包含真正随机的值)。虽然使用硬件随机数发生器可以产生更接近真正随机的序列，但伪随机数发生器在实践中因其产生数的速度和可再现性而很重要。

从 Java 17 开始，我们有了一个名为 RandomGenerator 的新接口。这个接口使得 PRNG 的实现和使用更加简单。

```
package java.util.random;  
public interface RandomGenerator { }
```

这是一个使用`RandomGeneratorFactory`让`Xoshiro256PlusPlus` PRNG 算法生成从 0 到 10 的特定范围内的随机整数的例子。

```
**import** java.util.random.RandomGenerator;
**import** java.util.random.RandomGeneratorFactory;

**public class** TechIsBeautiful {

  **public static void** main(String[] args) {
    **final** String randomAlgorithms = **"Xoshiro256PlusPlus"**;
    **final int** seed = 999;
    RandomGenerator randomGenerator = RandomGeneratorFactory.of(randomAlgorithms).create(seed);
    **for** (**int** i =0;i<=10;i++){
      **int** output = randomGenerator.nextInt(11);
      System.***out***.println(output);
    }
  }
}
```

这样，输出将在 0 到 10 的范围内随机变化:

```
2
3
0
5
2
1
10
6
5
8
7
```

您可以通过下面的示例代码看到所有的 PRNG 算法:

```
RandomGeneratorFactory.all()
    .map(x -> x.name())
    .sorted()
    .forEach(System.***out***::println);
```

这是 Java 17 中的 PRNG 算法列表:

```
LXM : L128X1024MixRandom
LXM : L128X128MixRandom
LXM : L128X256MixRandom
LXM : L32X64MixRandom
LXM : L64X1024MixRandom
LXM : L64X128MixRandom
LXM : L64X128StarStarRandom
LXM : L64X256MixRandom
Legacy : Random
Legacy : SecureRandom
Legacy : SplittableRandom
Xoroshiro : Xoroshiro128PlusPlus
Xoshiro : Xoshiro256PlusPlus
```

此外，在遗留随机类中有一些增强，如`java.util.Random`、`SplittableRandom`和`SecureRandom`，也扩展了新的`RandomGenerator`接口。

## #2:开关的模式匹配(预览)

这是增强开关表达式和语句的模式匹配的另一个特性。不过请注意，这个功能还在“预览”中。由于这是一个预览功能，您需要使用`--enable-preview`选项在您的 JDK 17 环境中启用它。

这个例子展示了在 Java 17 中使用 switch-case 是多么方便。

```
**public** String checkValidShape(Shape shape) {
  **return switch** (shape) {
    **case** Rectangle r && (r.getNumOfSides() != 4) -> **"Sorry! Not valid Rectangle"**;
    **case** Line l && (l.getNumOfSides() != 1) -> **"Sorry! Not valid Line"**;
    **default** -> **"Great! This may a good shape"**;
  };
}
```

## #3:密封类

这用密封的类和接口增强了 Java 编程语言。密封的类和接口限制了哪些其他类或接口可以扩展或实现它们。

```
public abstract sealed class Shape
permits Circle, Rectangle, Triangle {
   ...
}
```

实际上，这个特性是从 Java 15 和 Java 16 中引入的，但是它仍然是一个预览特性。从 Java 17 开始，这个特性最终将密封类作为 Java 17 中的标准特性。

## #4: Vector API(第二个孵化器)

在 Java 17 中，它利用专门的 CPU 硬件来支持向量指令，并允许像管道一样执行这些指令。这提高了 Vector API 的性能，还增加了其他增强功能，如支持字符操作、将字节向量转换为布尔数组以及从布尔数组转换字节向量等。

## #5:特定于上下文的反序列化过滤器

众所周知，从不受信任的数据中反序列化数据是危险的，不应该是最佳实践。在 Java 9 中，它使软件工程师能够验证从不可信来源传入的序列化数据，以防止安全问题。在 Java 17 中，它允许应用程序配置在 JVM 级别定义的特定于上下文和动态选择的反序列化过滤器。

# 摘要

在这篇文章中，我用实例分享了 top 5 必知 Java 17 的新特性。本文中分享的信息是探索和学习 Java 17 其他特性的一个很好的切入点。

*喜欢这篇文章？给我拿个*[*Ko-fi*](https://ko-fi.com/techisbeautiful)*。*

*爱我的文字？加入我的* [*邮件列表*](https://medium.com/subscribe/@techisbeautiful) *。*

*爱读书？加入* [*中等*](https://medium.com/@techisbeautiful/membership) *(如果你用这个链接，也是支持我，因为我有来自中等的小提成)。*

*如果你喜欢这个故事，你也会喜欢:*

*   [你必须知道的 Java 18: 4 特性](/java-18-top-4-features-you-must-know-1f36ee23e2ab)
*   [你必须知道的 Java 11: 8 特性](https://medium.com/@techisbeautiful/new-features-you-must-know-in-java-11-and-examples-3fda2ad26def)
*   [你必须知道的 Java 8 : 7 特性](/java-8-seven-features-you-must-know-and-examples-1c3964ae7fe8)