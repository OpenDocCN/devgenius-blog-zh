# 通过编码面试的 3 种数据结构

> 原文：<https://blog.devgenius.io/3-data-structures-to-pass-coding-interview-d1942df516a0?source=collection_archive---------0----------------------->

## 掌握使用它们来解决大多数挑战

![](img/0ea3f3836f7a322eb51116be182b58f8.png)

照片由[克里斯蒂娜@ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/interview?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

在计算机科学课程中，我们学习许多数据结构。这一切都始于数据如何在机器上组织和保存。然后，我们发现更复杂的结构，如图形。一些专业甚至提供关于图论的完整课程。

数据结构的多样性向我们展示了有多少问题存在。但是大多数工程任务只能用几种类型的数据结构来解决。除非你正在为一艘宇宙飞船构建一个新的数据库引擎、导航系统或软件。

如果您有效地应用它们，您的解决方案将会达到良好的性能。根据我的经验，这在软件开发职位的技术面试中会被定期检查。

# 目录

第一个基本的数据结构是列表。有了这个列表，你将能够解决许多任务:收集条目，遍历它们，排序，作为参数传递，而不仅仅是。这种数据结构比简单的数组更高级。它为方便工作提供了额外的方法。其中一些是`remove()`、`indexOf()`、`contains()`。

在不同的语言中，列表由不同的内部实现支持。在某些情况下，它在内部用数组实现，在其他情况下用链表实现。在这里，你需要知道的区别，因为它影响效率。例如，数组列表的访问时间为 O(1)，而链表的访问时间为 O(n)。另一方面，当插入越来越多的项目时，数组列表需要更多的时间来扩展空间。因此，如果主要目的是访问项目，那么考虑一个数组列表。如果你主要是添加新的条目，那么链表是一个更好的选择。

```
val names = listOf("Anna", "Sandra", "Maria", "Laura", "Fabinenne")
val isLauraPresent = names.contains("Beatrice")
```

在上面 Kotlin 的例子中，我们有一个女性名字的列表，然后我们使用方法`contains()`检查名字 *Beatrice* 是否在列表中。这种方法没有错，但是有一点你必须记住。该方法的时间复杂度为 O(n)。在大型列表中调用方法`contains()`会导致性能问题。你会怎么做？检查下一个结构。

# 一组

集合是保证每个元素唯一性的数据结构。在内部，它使用了 Hast 表的概念。对于每个元素，都会计算一个相应的哈希值作为关键字。具体实施可能因平台而异。但是尽管如此，想法仍然是一样的:集合将只包含唯一的元素，没有特定的顺序。

因此，每当你有一个任务要忽略重复的元素时，记住关于 Set。通常，可以将列表转换为集合。让我们看看它在科特林是什么样子。

```
val names = listOf("Lara", "Ben", "Mary", "Ben", "Lucia", "Lara")
val uniqueNames = names.toSet()
```

集合`uniqueNames`将只包含`[Lara, Ben, Mary, Lucia]`。这是删除重复项的最快方法。

集合的另一个重要属性是它访问元素的方式。因为它使用散列来检索值，所以在这种情况下时间复杂度是 O(1)。这同样适用于`contains()`法。它不会遍历所有元素。这使得该方法的时间复杂度为 O(1)。如果你想进行优化，请记住这一点。

因为集合是数学中的一个概念，所以在集合上执行并、交、子集等操作更容易。

```
val classRoom1 = setOf("Anna", "Ben", "Lara")
val classroom2 = setOf("Clarice", "Ben", "Patrick")

val intersection = classRoom1.intersect(classroom2)
```

在上面 Kotlin 的例子中，我们可以看到如何使用集合交集找出两个集合中都存在的快速名称。

# 词典

字典是计算机科学中另一种强大的数据结构。字典中的每个元素都用一个键和一个值来表示。字典有多种实现方式，每个平台都有自己的特点。字典的主要特点是每个元素都有一个特定的键。

在字典中，键应该保持唯一。它充当集合中的散列。否则，就会导致[碰撞](https://en.wikipedia.org/wiki/Collision_(computer_science))。

```
val capitals = hashMapOf("Berlin" to "Germany", "London" to "UK")
```

这是一个经典的字典示例，其中城市是键，国家是值。它给出了什么是字典的概念。如果我们用相同的键添加一个新项目，它将覆盖一个现有的项目。

但是你还能在哪里使用字典呢？

一个典型的用例是计算集合中每个元素的出现次数。一个条目将是字典中的一个键，因为它应该是唯一的。该值是出现的次数。

让我们看看下面的例子，它计算字符串中每个字符出现的次数。

```
val input = "I want to be a better programmer"
val dictionary = mutableMapOf<Char, Int>()

for (character in input) {
    dictionary.putIfAbsent(character, 0)
    dictionary[character] = dictionary[character]!! + 1
}
```

键是一个字符，值是这个字符在输入字符串中出现的次数。在 Kotlin 中运行这段代码后，字典将包含以下内容:

```
{'I'=1, ' '=6, 'w'=1, 'a'=3, 'n'=1, 't'=4, 'o'=2, 'b'=2, 'e'=4, 'r'=4, 'p'=1, 'g'=1, 'm'=2}
```

这是一个字典应用程序的想法。该值表示与定义的键相关联的某个变量。

# 结论

计算机科学的世界可以引入许多数据结构。他们每个人都有自己的需求。其中一些应用广泛，另一些只用于实现一个非常具体的目标。

但是如果你花一些时间正确地使用上面的数据结构，那将足以有效地处理日常任务并进一步扩展你的知识。通过在下一次编码面试中应用它们来展示你的实力，获得这份工作的机会更大。