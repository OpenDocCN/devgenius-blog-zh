# 在 C#中添加整数数组的 12 种方法—第 2 部分:自由形式迭代

> 原文：<https://blog.devgenius.io/12-ways-to-add-an-array-of-integers-in-c-part-2-freeform-iteration-2f5c810a8e7b?source=collection_archive---------17----------------------->

![](img/67424f6e47db6624217990eeb857ba8b.png)

照片由 [C 在](https://unsplash.com/@cdrying?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [Unsplash](https://unsplash.com/s/photos/yield?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上烘干

在[第 1 部分，](https://medium.com/dev-genius/12-ways-to-add-an-array-of-integers-in-c-part-1-34753ff7f17a?source=friends_link&sk=ffb9131efdf8777a4d01208093229f3d)中，我谈到了在 C#中添加整数数组的 12 种方法中的前 3 种，我发现这是面试开发人员候选人的有用的白板编码练习。所有这三种方式实际上只是一个主题的变体:执行一个代码块固定的次数( *n* )，其中 *n* 是数组中元素的数量。在每次迭代中，将数组中第 *n* 项的值添加到一个输出变量中。迭代了 n 次*后，返回输出变量，就完成了。*

我暗示我们的下一个解决方案将利用 *foreach* 循环结构。如我所承诺的，在这里。

# 为每一个

看起来非常类似于循环版本的*。您可能会认为，在内部，这些代码被编译成一个 *for* 循环。但是这里有一些更有趣的事情。*

在 C#编程中需要理解的最重要的概念之一是 IEnumerable 接口和它的泛型对应物 IEnumerable <t>。一般来说(但技术上不总是)，foreach 循环在实现 IEnumerable 的类上操作。</t>

顺便说一下，这是一个**很多**的类。C#中的大多数数据结构(或者更恰当地说是。NET framework)实现了 IEnumerable，它包含不止一个相同的东西——数组、列表、集合、队列、哈希表，甚至字典。甚至 *string* 也实现了 IEnumerable，因为，当然，string 只是一个包含多个 chars 的数据结构。

那么实现 IEnumerable 是什么意思呢？简单。您只需要有一个返回 IEnumerator 的 GetEnumerator 方法。

啊，那么 IEnumerator 是做什么的呢？也很简单。只需要一个属性和一个方法。该方法是 MoveNext()，它返回一个指示是否有当前值的布尔值。当前属性只返回那个值。

方法 5 展示了这是如何工作的。

# 人口普查员

Array 类实现 IEnumerable <t>，因此它包含一个 GetEnumerator()方法，该方法返回 IEnumerator <t>。该枚举器知道数组的长度，并在内部保存对“当前”实例的引用。只要当前实例的索引小于数组的长度，MoveNext()就迭代该指针并返回 true。当无处可去时，MoveNext()返回 false，迭代结束。</t></t>

换句话说，这两个例子本质上是一样的。foreach 版本基本上是枚举器版本的语法糖。

当然，它们最终也和其他例子一样。IEnumerator <t>正在做*与*完全相同的事情，就像我们在 for 循环、指针和 goto 中做的一样。但事情本不应该是这样的。这就是 IEnumerable 真正有趣的地方:迭代不一定是有限的！</t>

C#有一个非常简洁有用的“迭代器方法”的概念(JavaScript 中有一个类似的、更新的概念，现在被称为“生成器函数”。)如果您有一个返回 IEnumerable 的 C#方法，您可以使用特殊的“yield return”和“yield break”关键字从该方法中退出。这里有一个非常简单(而且很傻，如果你不熟悉美国东南部的口语，可能会很荒谬)的例子:

在内部。NET 在这个 IEnumerable <string>上生成一个 IEnumerator <string>。每个“yield”语句表示对该枚举器上的 MoveNext()方法的调用。对于“yield return”语句，MoveNext()返回 true，Current 属性设置为产生的值。在调用“yield break”时，MoveNext()返回 false，这样就结束了。</string></string>

当然，你也可以用它做一些有用的事情！关键是，迭代是自由的，不是固定的。C#中的 IEnumerator 只要有 MoveNext()和 Current，就可以任意枚举。

# 包扎

因此，C#中的的*只要满足某些条件就会执行特定的代码块(通常自动递增指针的值小于预定的限制，但并非总是如此)。并且 *foreach* 执行一个特定的代码块，只要它的枚举器告诉它有一个值来执行它。我们还有其他选择吗？*

我之前提到过 IEnumerable 是 C#编程中最重要的概念之一。我在这里谈到的枚举器只是冰山一角。下一次，我将讨论 IEnumerable 可能带来的其他一些非常好的东西，因为我们将介绍在 C#中添加整数数组的两种方法。