# 如何用三行代码在 Java 中按字母顺序对字符串进行排序

> 原文：<https://blog.devgenius.io/how-to-alphabetically-sort-a-string-in-java-with-just-three-lines-of-code-2d2778dafecf?source=collection_archive---------1----------------------->

![](img/7caa00b1627b4e3939b03046080d2ddf.png)

照片由[特雷西·亚当斯](https://unsplash.com/@tracycodes)在 [Unsplash](https://unsplash.com/photos/TEemXOpR3cQ) 上拍摄

如果你需要在 Java 中按字母顺序对一个`String`进行排序，那么你应该继续阅读并记住这个操作顺序。

首先，Java 中没有原生函数可以只取一个`String`然后排序。

# 台阶

1.  将`String`变成一个字符数组(`char`)
2.  对字符数组进行排序
3.  使用字符数组创建一个新的`String`

# 排序字符串的示例

1.  “dcba”→“ABCD”
2.  “gggggc”→“cggggg”
3.  “za”→“az”

# 代码

所以重申一下，首先我们定义一个字符数组，然后用我们的输入字符串`str`初始化它，这个字符串应用了`toCharArray()`函数。`toCharArray()`函数恰好返回一个字符数组，因此类型匹配。

然后，我们使用`Arrays.sort()`方法，它是`Java.util.Arrays`的一个类方法。这为我们做了排序工作。

但是，排序后的数组仍然是字符数组的形式。为了回到一个`String`，我们创建一个新的`String`，然后将字符数组传递给它的类构造函数(基本上，我们只是将它作为唯一的参数提供)。

这样，我们就有了一个按字母顺序排序的`String`，我们可以返回它。

当你想对一个`String`进行排序时，这可以派上用场，虽然它不是立即显而易见的，但它是你可以记住并在下次需要时快速应用的东西。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)