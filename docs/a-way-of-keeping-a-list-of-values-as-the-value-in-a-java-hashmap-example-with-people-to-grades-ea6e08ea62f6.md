# 一种将值列表作为值保存在 Java HashMap 中的方式(例如人员评分)

> 原文：<https://blog.devgenius.io/a-way-of-keeping-a-list-of-values-as-the-value-in-a-java-hashmap-example-with-people-to-grades-ea6e08ea62f6?source=collection_archive---------1----------------------->

![](img/9f6690a529d69ec218561d0b6933d02b.png)

照片由[海拉戈斯蒂奇](https://unsplash.com/@heylagostechie)在 [Unsplash](https://unsplash.com/photos/IgUR1iX0mqM) 上拍摄

如果您想知道如何在 Java 的 HashMap 数据结构中将值列表类型保存为键、值对，那么您就找对地方了。

本文将介绍一种实现这一点的方法。

假设您有以下代码:

基本上，对于二维数组`peopleToGrades`中列出的每个人，我想存储他们所有的相关分数。我们如何做到这一点？

嗯，我们可以这样做的一个方法是将数据存储在一个`HashMap`数据结构中，该数据结构将一个`String`(人名)映射到一个`String`值的`ArrayList`(该人的分数)。该代码将如下所示:

所以基本上，我们在第 12 行声明一个空的`peopleToGradesMap`。

在第 14 行，我们循环遍历第 1 行到第 10 行给出的`peopleToGrades`的长度。

在第 15 行，我们检查`peopleToGradesMap`是否包含了我们正在查看的内部数组的名称。

如果地图上还没有这个名字，我们就在第 16 行创建一个新的`ArrayList<String>`，在第 17 行我们在地图上获得这个名字，然后添加`peopleToGrades[i][1]`，这基本上意味着我们将这个等级添加到与地图上的那个人相关联的列表中。

如果地图*已经包含了*的名称，那么我们跳过那里的第一个街区，直接进入`else`。我们只是把等级加到值上，这个值就是与那个人相关联的`String`值(等级)的列表。

在这个程序中，一切都很顺利，在执行结束时，映射看起来是这样的:

```
{Jeremy=[A-, B-], Yuta=[B], Tremaine=[A], Damian=[A], Klay=[B+, A], Steph=[A+]}
```

记住:`HashMap`数据结构不保证顺序，因此不保证内容的顺序。

然而，我们的算法奏效了。我会注意到您可以像这样稍微减少代码:

你注意到不同了吗？

本质上，如果你回到我们第一次实现它的时候，第 17 行和第 19 行是重复的。它们实际上同时出现在`if`和`else`块中，所以实际上我们可以把它放在`if`块的外面，不管怎样，那条线都会出现。

还有其他方法可以实现这一点，但是我想快速地指出这种方法，因为它是一种有用的数据存储方法，在实际的 Java 代码应用程序中肯定会出现。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)