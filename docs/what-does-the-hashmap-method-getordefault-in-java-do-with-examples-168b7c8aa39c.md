# Java 中的 HashMap 方法 getOrDefault 是做什么的？(有例子)

> 原文：<https://blog.devgenius.io/what-does-the-hashmap-method-getordefault-in-java-do-with-examples-168b7c8aa39c?source=collection_archive---------3----------------------->

![](img/502d4e93e723c6b643debaba90276f78.png)

如果您经常使用 Java 的 HashMap 数据结构——您可能应该使用( *hello，O(1)time complexity for data adds and retrievals*)——那么您有时会想要利用`getOrDefault()`方法。

# 从高层次理解方法

在我们讨论代码本身和一些例子之前，我认为用简单的英语来理解这个函数真正要实现的功能是有帮助的。

基本上，您试图通过指定键来获得映射中的某个值(记住`HashMap`是键到值的映射)。然而，如果键是*而不是*出现在映射中，那么您还需要在函数中提供一个将被返回的**默认值**。

假设您的地图在较高层次上有以下键-值对，这些键-值对表示咖啡馆的菜单项及其价格:

```
("hokkaido milk tea", "4.99")
("espresso", "3.99")
("bacon tater tots", "5.99")
("earl grey tea", "2.99")
("matcha milk tea", "4.99")
("chocolate brick toast", "7.99")
("ube cream puffs", "6.99")
("jasmine milk tea", "4.59")
```

假设我们想从地图上检索`bacon tater tots`的价格。幸运的是，它在那里，所以我们得到了它的值:`5.99`。

然而，假设我们想要检索地图中*不在*的商品的价格。说我们要找回`takoyaki`。`getOrDefault()`的想法是，如果键不在映射中，您提供一个返回的后备选项。所以我们可以说“我想要 takoyaki 的价格，但如果它不在菜单上，那么我们可以只说它是 8.99 美元)。因此，当一切都结束时，我们得到一个值`8.99`。

# 从代码层面理解方法

如果这没有意义，那么看看真正的 Java 代码可能会有意义。

从[甲骨文的 Java 文档](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html#getOrDefault-java.lang.Object-V-)，`getOrDefault()`描述如下:

> 返回指定键映射到的值，如果此映射不包含键的映射，则返回`defaultValue`。
> 
> **参数:** `key` -返回关联值的键`defaultValue` -键的默认映射
> 
> **返回:**指定键映射到的值，如果此映射不包含键的映射，则返回`defaultValue`

首先，让我们创建一个映射`String`到`Integer`的新地图。

然后，我们插入一堆值。

现在，我们试着在地图上调用`getOrDefault()`，并提供两个参数；按顺序，我们将尝试获取`b`，如果它不在地图中，我们将只提供`99`作为默认值。我们将调用的返回值存储在一个名为`b`的整数中。

`b`的值将会是`2`。原因是因为`b`确实是 map 中的一个键，它的值是`2`，所以被检索然后返回。在这种特殊情况下没有使用`99`。

然而，让我们试着在哈希表中获取`e`，然后我们也将提供`99`作为默认值。我们将把该调用的返回值保存在一个名为`e`的整数中。

在我们继续之前，问问你自己:`e`的值会是多少？

在这种情况下，`e`不在哈希映射中。因此，不会返回任何值。`getOrDefault()` as called 使得`99`作为默认值返回。因此，`e`的值为`99`。

总而言之，一旦你弄清楚它在做什么，`getOrDefault()`真的没那么糟糕。在另一篇文章中，我将回顾这个特殊函数可以派上用场的可能用例。出来的时候我一定会链接到这里。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)