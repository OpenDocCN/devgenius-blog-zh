# 每个开发人员都应该知道的内置数据结构。

> 原文：<https://blog.devgenius.io/inbuilt-data-structures-every-developer-should-know-2c2cb513193a?source=collection_archive---------0----------------------->

## 易于实现的便捷数据结构

![](img/3a9a3419f423b9b499c46d9566eea5c5.png)

韦斯·希克斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

数据只不过是事实、信息和观察的任何东西。在当今时代，有效地处理数据是每个开发人员都关心的问题。**数据结构**提供了存储、检索和分析数据的有效方法。

此外，数据结构对于你在大公司找到一份好工作非常重要。数据结构在解决现实世界的问题时非常有效。

**Google Map** 用 **Graph** 数据结构告诉你两个城市之间的距离，为你找到最短的路线。**脸书**也使用简单的图形数据结构让你和朋友保持联系。

许多软件开发人员通常不了解数据结构，因为他们使用的主要技术各不相同，而且对一些开发人员来说完全脱离了上下文。

一个高级的全栈开发人员可能不需要记住数据结构，但是一个在 IT 领域开始职业生涯的人最终应该知道一些基本的数据结构，比如**数组**和**字符串**。

对于一个新手来说，实现数据结构可能是一项漫长的任务，但是在 **C++** 中有一些有用的数据结构，它们已经在内部实现了，你只需要在你的程序中调用它们，而不需要实际编写一个完整的实现。

**以下是内置的数据结构，我认为这对许多开发人员来说会很方便……**

## 矢量

参加竞技编程或者黑客马拉松的程序员都知道向量数据结构的重要性。

基本上，Vector 是 Array 的继承者。当你搜索数组的定义时，你会发现— **数组是相邻内存位置的相似元素的集合。**

向量的定义与数组的定义相同。**那么，为什么是矢量呢？**

如果您曾经使用过整数数组或任何数据类型，您一定会发现需要指定程序所需的空格数。编程时需要预测所需的空间，这难道不可怕吗？

```
Example-
**array[10];** //Array with size of 10 elements
```

每当我调用 array 时，我需要指定我需要的存储量，在上面的例子中，如果一个用户需要存储 11 个或更多的元素，而我只给我的数组分配了 10 个元素，该怎么办呢？会有巨大的混乱…

**另一方面**，Vector 的工作方式类似于数组，但是你不需要预测用户将需要的存储量。Vector 会自动创建存储相似数据的空间。

```
Example-
vector<Specify the data type> 
Let's assume we are working with integers, so we need to create a vector if integers **vector<int> a;**
Now your vector is ready to store as many elements as you need. Vector usually doubles its space when a user try to add new elements. //Insert elements in vector-**a.push_back(10);** 
//elements in vector - 10
**a.push_back(20);**
//elements in vector - 10 20
**a.push_back(11);**
//elements in vector - 10 20 11
**a.push_back(30);**
//elements in vector - 10 20 11 30
**a.push_back(40);**
//elements in vector - 10 20 11 30 40//Delete elements in vector-
**a.pop();** //elements in vector - 10 20 11 30 
**a.pop()**;
//elements in vector - 10 20 11 
```

**push Back**功能用于在向量中插入一个元素，而 **Pop** 功能用于删除向量的最后一个元素。当你使用向量时，你不需要担心大小。

此外，vector 还支持许多功能，如**查找元素**或**计算当前 vector 中所有元素的总数**。可以参考 vector 的官方 [**文档**](https://www.cplusplus.com/reference/vector/vector/) 来了解一个 vector 支持的所有功能。

我希望你喜欢这篇文章，并且你今天能够学到一些新的东西。另外，你可以阅读 c++中的 **STL** ，它提供了更多内置的有用的数据结构，比如**栈**和**队列。**

最近，我写了“ [**十大语言排名堆栈溢出**](https://javascript.plainenglish.io/top-10-programming-languages-of-2021-d2d48c634ae7)**”**看看吧，我目前正在接受**“赞助/写作项目”**，你可以通过—**writeaniketz@gmail.com**联系我

继续编程，祝你在编程之旅中好运。不要忘记每天至少花 30 分钟编码，这将帮助你记住所有你到目前为止学到的必要概念…

**不断升级，不断学习。**

[](https://aniketz.medium.com/membership) [## 通过我的推荐链接加入 Medium-Aniket

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

aniketz.medium.com](https://aniketz.medium.com/membership) [](https://javascript.plainenglish.io/3-books-every-programmer-should-read-97ac12422cfb) [## 每个程序员都应该读的 3 本书

### 帮助我理解编程基础的书籍。

javascript.plainenglish.io](https://javascript.plainenglish.io/3-books-every-programmer-should-read-97ac12422cfb) [](https://javascript.plainenglish.io/top-10-programming-languages-of-2021-d2d48c634ae7) [## 2021 年十大编程语言

### 你是使用顶级编程语言的用户之一吗？

javascript.plainenglish.io](https://javascript.plainenglish.io/top-10-programming-languages-of-2021-d2d48c634ae7)