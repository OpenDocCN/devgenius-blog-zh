# 被低估的 Python 函数

> 原文：<https://blog.devgenius.io/underrated-functional-python-functions-5f0044de2f7f?source=collection_archive---------6----------------------->

![](img/48e4e5327c22bad77669321568f4e7ad.png)

Emile Perron 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Python 是目前最通用的编程语言之一。从声明式编程到面向对象编程，再到函数式编程，Python 融合了多种编程语言的方法。在本文中，我将重点介绍使用 Python 编写函数代码。

当人们想到使用 Python 进行函数式编程时，他们大多会想到三个函数:`map` `filter` `reduce`。然而，有更多的功能在雷达下，您可以在各种用例中使用。这些函数不仅使您的代码更具功能性，还能让您以最有效的方式利用和操作数据结构。

Python 的`itertools`模块公开了许多函数，您可以使用这些函数以非常高效和高性能的方式迭代和操作数据结构，如列表和元组。

下面是我将在本文中介绍的函数列表:

*   数数
*   循环
*   链子
*   压缩

让我们从`count`开始吧。

# 数数

`count`返回一个可以循环的不定迭代器。它有两个可选参数:`start`和`step`。

`start`定义开始计数的数字，而`step`定义两个生成数字之间的增量。例如，`count(5,2)`将生成一个 in 迭代器，其中生成的数字将是:5、7、9、11、13、15 等等。

在处理数据密集型应用程序时，这个函数会很方便。假设您有一组连续的数据点，您想给它们一个自动递增的单调 ID。你可以这样做:

```
data_points = [(10, 20), (5, 7), (3, 4)]
id_iterator = itertools.count(1,1)for point in data_points:
	print(f'id: {next(id_iterator)}, payload: {point}')# Output:
# id: 1, payload: (10, 20)
# id: 2, payload: (5, 7)
# id: 3, payload: (3, 4)
```

如您所见，每个有效负载都有一个自动递增的 ID。你不需要知道你有多少个数据点。假设迭代器是无限的，你可以继续前进到下一个元素并将 ID 赋给下一个有效载荷。

或者，您可以使用迭代器进行循环。假设您想要无限循环所有偶数。

```
even_number_generator = itertools.count(0,2)
for even_number in even_number_generator:
	time.sleep(2)
	print(even_number)# Output:
# 0, 2, 4, 6, 8, and so on
```

您可以使用迭代器无限地遍历偶数的“列表”,这非常酷。只要你愿意，你可以退出这个循环。

# 循环

接受一个 iterable，并返回一个迭代器，可以无限循环，每次当原来的 iterable 用完时，就从头开始。

假设您想将一组人平均分成两部分。

```
people = ["Ben", "Mike", "Jerry", "Amanda", "Jeremy"]
sections = [1, 2]section_iterator = itertools.cycle(sections)for person in people:
	print(f'person: {person} , section: {next(section_iterator)}')# Output:
# person: Ben , section: 1
# person: Mike , section: 2
# person: Jerry , section: 1
# person: Amanda , section: 2
# person: Jeremy , section: 1
```

如您所见，当您完成将前两个人分配到第 1 和第 2 部分时，您正在循环并将下一个人再次分配到第 1 部分。

另一个更复杂的例子是写入一个分片的数据库表。假设您有一个共享表，其中存放数据的碎片依赖于一个名为`partition`的字段，它可以是 1 或 2。您希望在两个分区之间平均分配写入。使用此功能，您可以轻松地在两个值之间循环，无需任何复杂的逻辑。

# 链子

`chain`为您提供了一种高效的连续循环多个序列的方式。它接受您想要迭代的多个 iterables。下面是语法:

`chain(iterable_1, iterable_2, iterable_3)`(可变的迭代次数)

让我们考虑一个用例，您有三个不同的列表，并且您想要连续地迭代它们。这里有三个列表:

```
numbers_1 = [10, 20, 30, 40]
numbers_2 = [50, 60, 70, 80]
numbers_3 = [90, 100, 110, 120, 130]
```

可以连续遍历它们的最基本的方法是使用三个循环:

```
for num in numbers_1:
	print(num)
for num in numbers_2:
	print(num)
for num in numbers_3:
	print(num)
```

这段代码极其冗长，完全不符合 DRY 原则。

这是完成同样事情的另一种方式:

```
combined = numbers_1 + numbers_2 + numbers_3 
for num in combined:
	print(num)
```

然而，这有很大的内存开销，因为你要从现有的列表中创建一个新的列表，这会使内存使用加倍。如果您的每个原始列表都是 30MB 的组合，那么您将创建另一个 30MB 的临时数据结构来进行迭代，这实际上使您的内存占用加倍。

这是使用`chain`的一个很好的用例

```
combined = itertools.chain(numbers_1, numbers_2, numbers_3)
for num in combined:
	print(num)
```

使用`chain`你可以让你的代码不那么冗长，并且以最有效的方式遍历这些列表。

当您处理非常大的数据结构，并且不希望复制它们只是为了连续迭代单独的数据结构时，这个函数非常方便。

# 压缩

`compress(data, selectors)`

*   包含主数据的主 iterable
*   `selectors`是布尔的可迭代式。它定义了是否应该过滤掉`data` iterable 中相同位置(索引)的 and 元素。

在深入细节之前，让我们先看一个例子:

```
cities = ["Atlanta", "San Francisco", "Seattle", "Houston"]
selectors = [True, True, False, False]compressed_cities = itertools.compress(cities, selectors)
for city in compressed_cities:
	print(city)# Output:
# Atlanta
# San Francisco
```

唯一保留的城市是那些在`selectors`列表中的相应索引中有`True`的城市。所以这个函数是根据您想要保留的元素来“压缩”列表，如在`selectors`列表中所定义的那样。

当数据或选择器列表用尽时，操作停止。

# 最后的想法

这些是 Python 中一些最容易被忽略的函数，它们可以帮助您编写更多的函数代码，并帮助您更有效地处理数据结构。我认为如果你开始在你的用例中加入这些功能，你会受益匪浅。

我知道人们用不同的方式学习，所以如果你更喜欢视觉学习，请随意查看我在 YouTube 上的视频，在这里你可以看到我如何使用这些功能。

[官方文件](https://docs.python.org/3.7/library/itertools.html#itertool-functions)

[YouTube 视频](https://www.youtube.com/watch?v=Vf_ynFBhLLE&t=864s)

你可以通过我的个人网站[这里](https://irtizahafiz.com)联系我。