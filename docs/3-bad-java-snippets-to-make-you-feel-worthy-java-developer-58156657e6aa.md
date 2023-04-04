# 3 个糟糕的 Java 片段，让你觉得自己是一个有价值的 Java 开发者

> 原文：<https://blog.devgenius.io/3-bad-java-snippets-to-make-you-feel-worthy-java-developer-58156657e6aa?source=collection_archive---------1----------------------->

## 您应该看到的 3 个糟糕的 Java 片段

![](img/3ed3a17bbb956096dc6a980b23ddcaf6.png)

照片由[亚历克斯·格林](https://www.pexels.com/@alex-green?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[佩克斯](https://www.pexels.com/photo/upset-young-black-guy-covering-face-with-hand-while-working-remotely-on-netbook-5699826/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

*   ***清理代码***
*   ***有意义的名称***
*   ***测试***

这些都是一直喊的。你知道，我知道，每个人都知道干净的代码。

*脏代码怎么办？这里有 3 个糟糕的代码片段。详细解释它们的重要性。*

让我们开始吧。

# 4 欧拉任务怎么解？

你 ***在面试一份 Java 开发人员的工作*** 。你需要解决一个编码难题。原来这是第四个欧拉任务。

下面是 ***你需要解决的实际问题*** 。

一个回文数字的两种读法是一样的。由两个 2 位数的乘积构成的最大回文是 9009 = 91 × 99。

*找出由两个 3 位数的乘积构成的最大回文。*——[来源](https://projecteuler.net/problem=4)

这里有一个解决这个问题的糟糕方法。

[来源](https://www.govnokod.ru/26836)

这就是 ***的蛮力路线*** 。开始两个循环，乘以因子，找到最大的一个。

***循环包含字符串创建。*** 没必要用 StringBuffer 或者 String。这只会增加复杂性和无意义的计算。

这是我的方法。

从 1000 开始，减少因子，找到正确的解决方案。

***用数学避免空间复杂度。*** 用数学反转数字，而不是字符串。循环中的字符串只会导致内存开销。

查看评论部分，了解如何减少无用计算的更好的解决方案。

# 你繁殖条件吗？

如果一个对象是 X do M 的实例，如果一个对象是 Y do Z 的实例。

这是 ***常见的编码问题*** 并且有自己的模式。你应该用`Strategy`来解决这个问题。

一些开发人员会求助于 if-else 语句。如果只有 2-3 个条件，这很好。

虽然这似乎是一个可行的方案， ***条件滋生。没有人会把那 2-3 个条件改写成策略。下一个 ***开发者将添加另一个*** 并继续前进。***

这里有一个条件繁殖的例子。

更好的办法是 ***使用策略*** 。您可以创建一个`Map<Condition, Strategy>`并基于它进行评估。

# 如何按字母顺序排列数据？

你需要 ***按字母顺序排序数据*** 。

您有需要按字母顺序排列的客户国家、名称或姓氏。

词汇顺序不同于字母顺序。例如:“Z”将出现在“a”之前。

下面是一个糟糕的解决方案。

下面是**的*蛮力解*的**。将它们逐一比较，并正确放置。这是对字符串进行排序的最糟糕的方法。

你不会在产品级代码中看到这一点。你可以看到初学者尝试这个解决方案。

[来源](https://www.govnokod.ru/26825)

这里有一个更优雅的方法来排序字符串。 用`Collections#sort`对字符串进行排序。提供一个定制的比较器，并根据需要对它们进行排序。

***但是你需要按字母顺序排列。*** 排序默认给出词法顺序。该订单符合 ASCII 码。词汇顺序不同于字母顺序。

```
// this won't work as expected 
list.sort(Comparator.naturalOrder());
```

***永远寻找原生解决方案。*** 这里`[Collator](https://docs.oracle.com/javase/8/docs/api/java/text/Collator.html#setStrength-int-)`帮你解围。

```
// this will sort the countries in alphabetical order
Collections.sort(countries, Collator.getInstance(new Locale(languageCode)));
```

# 偷我的电子书

我已经写了很多糟糕的 Java 片段。你可以从我的口香糖店里偷。

给一两美元小费可以帮助我免费创作更好的电子书！