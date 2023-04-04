# 你应该看到的 5 个可怕的 Java 片段

> 原文：<https://blog.devgenius.io/5-hideous-java-snippets-you-should-see-6ecaa8995e72?source=collection_archive---------0----------------------->

## 从 5 个糟糕的 Java 片段中学习好的东西

![](img/9553d15ddfb39b279617ab829d019145.png)

照片由 [Darcy Lawrey](https://www.pexels.com/@d123x?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 从 [Pexels](https://www.pexels.com/photo/opened-mouth-black-haired-boy-in-gray-full-zip-jacket-standing-on-grass-field-taking-selfie-848740/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

*你用随机吗？你的工作有例外吗？你搜索空值吗？*

如果你看到了，你应该会看到这些可怕的片段。你会看到愚蠢能走多远。

以下是 5 个可怕的 Java 片段，以及它们的好的替代品。

# 1.你会产生随机数吗？

你看得越久，情况就越糟。在一个无限循环中有一个范围。代码循环，直到生成正确的数字。

*范围是如何生成的？*范围可以是`100`，也可以是`1_000_000`，甚至更多。该范围不会存在为负数的`CODE_LENGTH`。无限循环可能会无限期运行并停止执行。

[来源](https://www.govnokod.ru/15009)

你可以使用`[java.util.Random#nextInt(bound)](https://docs.oracle.com/javase/8/docs/api/java/util/Random.html#nextInt-int-)`。您指定上限。上限是唯一的。你加上最小值，得到你想要的随机数。

[来源](https://www.baeldung.com/java-generating-random-numbers-in-range)

# 2.你知道浮子是如何工作的吗？

正如 [OP](https://stackoverflow.com/questions/9921690/java-increment-through-float-floattointbits) 所说:*“我想要 2.25，却得到了 1.2500001。”不了解 IEEE 标准会回来困扰你。*

[来源](https://stackoverflow.com/questions/9921690/java-increment-through-float-floattointbits)

如果你想要`2.25`，加上`1`。不需要显式转换。Java 知道你想要什么。

*是否要将* `*1*` *添加到最小精度的单位？使用以下:*

`current + [Math.ulp(current)](http://docs.oracle.com/javase/6/docs/api/java/lang/Math.html#ulp%28float%29);`

也可以用`[Math#nextUp](https://docs.oracle.com/javase/6/docs/api/java/lang/Math.html#nextUp%28float%29)`。`nextUp`为您处理边缘案例。

学习 IEEE 标准，即我们如何存储浮点数。这些知识消除了新手的数字错误。

# 3.你知道什么是例外异端吗？

*“那是异端！”*我的一位学长正在研究最糟糕的实践。称之为不好的做法是不正确的。

*异常自行处理。*你永远不会想到这一点，然而有这样的代码。

[来源](https://www.govnokod.ru/14514)

我会继续讲其他异端的例子。以下示例可以编译。我会把这些留着*不予置评。*

[来源](https://www.govnokod.ru/14514)

[来源](https://www.govnokod.ru/14514)

[来源](https://www.govnokod.ru/14514)

# 4.如何找到第一个非空值？

有几种方法可以找到第一个非空值。以下是不要这样做的方法。

[来源](https://www.govnokod.ru/)

当你只需要检查两个值时，使用`Optional#ofNullable`。当第一个值为空时，使用`orElse`返回第二个值。您可以将`Optional#ofNullable`与自定义`orElseGet`结合，提供一个`Supplier`。

如何从集合中检索第一个非空值？不要这样做。有了泛型，就没必要引入`Object`。

[来源](https://stackoverflow.com/a/2768080/5999670)

组合流以获得惰性计算，组合泛型以移除转换。你会得到下面的解决方案。

[来源](https://stackoverflow.com/a/48192967/5999670)

您也可以使用 Apache Commons lib 中的`[firstNonNull](http://commons.apache.org/proper/commons-lang/javadocs/api-3.1/src-html/org/apache/commons/lang3/ObjectUtils.html#line.119)`。

# 5.你如何创建目录？

令我惊讶的是，这是 Java 7 的一个方法。简单，易懂，难调试。

[来源](https://docs.oracle.com/javase/7/docs/api/java/io/File.html#mkdir%28%29)

当且仅当目录被创建时，**返回:**`true`；`false`否则。

所以`false`表示缺少父目录、没有权限和其他 IO 异常。*还有哪些 IO 异常？这是完整的名单:*

*   *该目录的上级目录可能不存在*
*   *父目录的所有权和权限可能有误*
*   *可能不允许应用程序在此文件系统中写入。*
*   *设备可能是只读的*
*   *设备可能损坏或遇到硬件错误*
*   文件系统可能已满，以至于无法创建目录。

从`false`找不到错误。这种代码现在已经废弃了。

今天我们有 [Java 8 方法](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createDirectory-java.nio.file.Path-java.nio.file.attribute.FileAttribute...-)。该方法抛出各种`IOExceptions`，返回`Path`，并接受文件属性。

```
public static [Path](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html) createDirectory([Path](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html) dir,
                                   [FileAttribute](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/FileAttribute.html)<?>... attrs)
                            throws [IOException](https://docs.oracle.com/javase/8/docs/api/java/io/IOException.html)
```

# 额外收获:你写 java 文档吗？

我将再次把这个问题留给*无评论区*。双关语。

[来源](https://www.govnokod.ru/15777)

学习 float 如何工作在今天可能没有帮助。即便如此，对开发人员来说，了解其中的区别也是有益的。浮点数和双精度数的区别及其内存表示。

随机值有其使用案例。总是质疑自己的随机生成。内置的随机生成将解决你的大部分问题。

了解有关泛型的更多信息。避免使用 Object 而不是泛型。更多阅读本书:[《Java 泛型和集合》](https://www.oreilly.com/library/view/java-generics-and/0596527756/)。

不要用布尔值来表示错误。它们是二进制的，不能表示很多错误。使用异常来显示错误。

# *今天就加入 Medium！*

***你为什么要*** [***订阅***](https://zivce.medium.com/membership?source=responses-----f49b64432202---------------------respond_sidebar-----------) ***？*** 率先抛弃微服 Chrome 模式。其次，你会接触到很多精彩的故事。你可以从实用程序员的书架上读到大约 [100 本书。你可以看到障碍、非常有用的提示和来自 Pinterest 团队的伟大建议。你可以阅读谷歌云的最新发展。](https://medium.com/pragmatic-programmers/directory-of-pragmatic-programmer-books-on-medium-6a5cbadbd4b4?source=responses-----f49b64432202---------------------respond_sidebar-----------)

**这就是你每月**[**【5 美元(两杯咖啡)**](https://zivce.medium.com/membership?source=responses-----f49b64432202---------------------respond_sidebar-----------) **所得到的。你可以花 5 美元阅读整个实用程序员库。**

免责声明:$2 出 [$5](https://zivce.medium.com/membership?source=responses-----f49b64432202---------------------respond_sidebar-----------) 将直接支持我，为你传递精彩话题。

# 偷我的电子书

我已经写了很多糟糕的 Java 片段。你可以从我的口香糖店里偷。

给一两美元小费可以帮助我免费创作更好的电子书！