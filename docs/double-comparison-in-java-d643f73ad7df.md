# Java 中的双重比较

> 原文：<https://blog.devgenius.io/double-comparison-in-java-d643f73ad7df?source=collection_archive---------4----------------------->

![](img/2ac05dae5c7d0c5b9aa42d6e4328951d.png)

迪特尔马·贝克尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

最近我在解决一个有趣的 bug，这个 bug 归结为用`equals`方法比较两个`Double`变量。看起来很无辜，像`firstDouble.equals(secondDouble)`这样的东西能有什么错？这里的问题是如何储存替身。为了使它们适合 64 字节(通常)，它们被四舍五入。

请参见下面的示例:

```
Double firstDouble = 0d;
for (int i = 1; i <= 42; i++) {
 firstDouble += 0.1;
}Double secondDouble = 0.1 * 42;System.out.println(firstDouble); // 4.200000000000001
System.out.println(secondDouble); // 4.2
System.out.println(firstDouble.equals(secondDouble)); // false
```

这种不准确性是由舍入误差引起的。
我们需要用不同的方法来比较那些替身。

# 阈值方法

如果我们无法访问任何库，而只想用 Java 来解决这个问题，我们可以使用一种叫做阈值法的方法。
简单来说，我们会减去那些 doubles，做绝对值，比较结果是否小于某个很小的数。

```
double epsilon = 0.000001d;
boolean result = Math.abs(firstDouble - secondDouble) < epsilon;
System.out.println(result); // true
```

这个小数字被称为ε，它越小，结果的准确度越高。大多数情况下，5 位小数就足够了。

# 阿帕奇公共数学

在 JDK 没有实用的方法。幸运的是，Apache Commons 数学库涵盖了我们。有了它，我们可以像这样比较那些双精度:

```
double epsilon = 0.000001d;
boolean result = Precision.equals(firstDouble, secondDouble, epsilon)
System.out.println(result);
```

ε和上面例子中的含义相同。

番石榴和其他图书馆也有类似的方法。

你可以在推特[上关注我，以获得更多类似的提示。](https://twitter.com/pavel_polivka)

*原载于 2021 年 5 月 12 日*[https://ppolivka.com/](https://ppolivka.com/double-comparison-in-java)*。*