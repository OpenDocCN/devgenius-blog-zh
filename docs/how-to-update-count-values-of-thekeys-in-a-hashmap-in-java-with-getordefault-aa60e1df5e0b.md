# 如何用 getOrDefault()在 Java 中更新 HashMap 中键的计数值

> 原文：<https://blog.devgenius.io/how-to-update-count-values-of-thekeys-in-a-hashmap-in-java-with-getordefault-aa60e1df5e0b?source=collection_archive---------2----------------------->

![](img/17464cd0af18dcdf3d2164dc7c597cd9.png)

照片由 [Timo Wielink](https://unsplash.com/@timowielink) 在 [Unsplash](https://unsplash.com/photos/4Zk45jNyQS4) 上拍摄

在 Java 中，`HashMap`数据结构的用处有很多:常量访问、常量存储，以及以逻辑和内置的方式将键和值绑定在一起。

# 一些背景

在之前的一篇文章中，我写过一个有用的函数`getOrDefault()`，它是 Java 中的`HashMap`的一个函数。如果您不太了解这个函数，或者想要复习一下，那么我建议您快速阅读一下:

[](https://medium.com/dev-genius/what-does-the-hashmap-method-getordefault-in-java-do-with-examples-168b7c8aa39c) [## Java 中的 HashMap 方法 getOrDefault 是做什么的？(有例子)

### 返回一个值，不管该键是否存在于 Java 映射中。

medium.com](https://medium.com/dev-genius/what-does-the-hashmap-method-getordefault-in-java-do-with-examples-168b7c8aa39c) 

简而言之，`getOrDefault()`基本上试图获取`HashMap`中特定键的值(通过参数提供)。如果它找到了那个键，那么它将像调用`get()`一样返回值。

然而，它真正与众不同的地方在于它是否在地图中找到了键*而不是*。然后，它返回一个默认值，程序员将它指定为另一个参数。例如，对`getOrDefault()`的调用可能如下所示:

在那个例子中，我创建了一个名为`hm`的`HashMap`，并添加了绑定`a, 1`、`b, 2`、`c, 3`和`d, 4`。

然后我调用`getOrDefault("b", 99)`，然后将返回值存储在一个名为`b`的整数值中。

因为`"b"`确实是映射中的一个键，所以`b`的值将是`2`，因为这是与`"b"`相关联的值。

如果我调用`hm.getOrDefault("z", 44);`，那么`b`的值就是 44。原因是因为地图中没有键`"z"`，所以返回默认值`44`(这是第二个参数)。

# 在散列表中跟踪计数

`HashMap`数据结构的一个用例是跟踪某些键的计数。

假设你正在遍历一个`String`，你想要跟踪所有的字符。字符串是`ABCDA`。你希望`HashMap`以某种方式表示有两个`A`角色，分别是`B`、`C`和`D`。

我们能做的如下:

我们可以一行一行地分析。

在第 4 行，我们声明了一个名为`map`的新`HashMap`，它将`Character`键绑定到`Integer`值。

在第 6 行，我们使用了一个`for`循环，一个字符一个字符地遍历我们的字符串`str`。

在第 7 行，我们有一个检查，用简单的英语说，“如果地图还没有包含字符串中我们所在的字符…”

如果我们满足这个条件，程序进入第 8 行。然后，代码会将它所在字符的绑定以及计数值`1`放入映射中，因为根据定义，这是映射中该特定字符的第一个也是唯一一个字符。

如果我们不满足这个条件，那就意味着这个角色实际上已经在地图上了。然后我们进入第 9 行的`else`子句。然后，我们在第 10 行检索当前计数值，并将其存储在一个我们称为`currentCount`的`int`值中。

然后我们在第 11 行将`currentCount`增加 1，然后用新的和更新的字符替换特定字符和计数的整个映射。

这段代码应该可以工作，但是如果你聪明地使用`getOrDefault()`，那么你实际上可以减少代码。在这里忍耐一下。

# 使用 getOrDefault()跟踪计数

让我们直接进入代码，好吗？

当我第一次看到这个的时候，我承认我很困惑。那里发生了很多事。

不过，我们把它分解一下，就不会那么吓人了。

我们有相同的设置:我们在第 3 行声明了一个新的`HashMap`,我们一次一个字符地遍历映射。

不同之处在于，在循环中，我们已经将行数减少到了一行。

在这一行中，我们在地图中放置了一个新的值，不管这个字符是什么键。

然而，我们输入的值使用了`getOrDefault()`,这样它就试图检索所讨论的字符的值。如果它找不到它，那么默认情况下它将返回的值是`0`，这是有意义的，因为在地图中没有字符有任何其他计数。最后，也是最关键的，虽然，有一些算术逻辑，增加了 1。

这一步是至关重要的，因为我之前说过:无论如何，我们都要在地图中加入新的值。因此，如果角色不在地图上，我们就输入角色和`1`的值。

这一行的美妙之处在于，如果字符*已经是 map 中的*，那么我们能够检索它，然后`0`几乎被忽略(因为我们不需要返回默认值),然后计数仍然增加 1，因为我们需要更新那个特定字符的计数。

我知道这可能会令人困惑，但是希望这篇文章提供一些背景知识，说明这个方法对于维护`HashMap`中的计数并不是 100%必要的，但是却是一个很好的捷径。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)