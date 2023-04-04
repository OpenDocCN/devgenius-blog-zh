# 每个程序员都应该知道的 Python DSA 技巧

> 原文：<https://blog.devgenius.io/python-dsa-tricks-every-programmer-should-know-7e6bba402445?source=collection_archive---------10----------------------->

![](img/565c7e3405347849fb423b43653aff13.png)

[利特·约翰斯顿](https://unsplash.com/@natejohnston?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/photos/PcbJL9CYSXs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

您是否正在准备一次编码测试，并且被所有需要学习的信息淹没？不要害怕，因为 DSA 的技巧是来拯救世界的！

DSA，即数据结构和算法，是任何编码测试的关键部分。这是你建立技术技能的基础，也是大多数编码考试中的常见话题。

但是让我们现实一点——DSA 可能会令人生畏，尤其是如果你是编程新手的话。这就是 DSA 技巧的用武之地。这些方便的技巧可以帮助你快速解决问题，给面试官留下深刻印象，并在编码测试中胜出。

那么，为什么 DSA 技巧对通过编码测试很重要呢？首先，它们可以帮助你节省时间，避免陷入棘手的问题。它们也展示了你解决问题的技巧，并表明你在 DSA 概念上有很强的基础。

此外，学习和掌握 DSA 技巧是非常有趣的！找到一个解决难题的巧妙方法会让人兴奋不已。

既然我们已经确定了 DSA 技巧的重要性，那么让我们深入了解其中的一些技巧。为了方便广大读者，我将使用 Python 作为我们的主要编程语言。但是，如果您更熟悉 C++或 Java，请不要担心——如果适用的话，我还会为我介绍的每个技巧提供这些语言的语法。因此，无论您喜欢哪种编程语言，您都可以跟随学习一些有价值的 DSA 技巧。

**平分模块**

Python 中的`bisect`模块是一个内置的库，它为有序列表中的高效搜索和插入操作提供支持。它是处理有序列表和维护大型数据集排序顺序的有用工具。

`bisect`模块提供了两个主要功能:`bisect_left`和`bisect_right`。这两个函数都将一个排序列表和一个搜索值作为输入，并返回索引，该值将被插入到列表中以保持其排序顺序。这两个函数的主要区别是当搜索值已经存在于列表中时的行为。

`bisect_left`函数返回列表中最左边的搜索值的索引。例如，给定列表`[1, 2, 2, 3, 4]`和搜索值`2`，`bisect_left`将返回`1`。

另一方面，`bisect_right`函数返回列表中最右边的搜索值的索引。使用与之前相同的列表和搜索值，`bisect_right`将返回`2`。

除了`bisect_left`和`bisect_right`函数之外，`bisect`模块还提供了`insort_left`和`insort_right`函数，用于将值插入到排序列表中，同时保持其排序顺序。`insort_left`函数在`bisect_left`返回的索引处插入值，而`insort_right`在`bisect_right`返回的索引处插入值。

从头开始编写二分搜索法算法需要大量的工作，包括实现分治法和处理边缘情况。使用`bisect`模块，您可以跳过所有这些工作，使用`bisect_left`或`bisect_right`函数，只用几行代码就可以对一个排序列表执行二分搜索法。

下面是一个如何使用`bisect`模块对排序列表执行二分搜索法的例子:

```
import bisect

def binary_search(sorted_list, value):
  index = bisect.bisect_left(sorted_list, value)
  if index < len(sorted_list) and sorted_list[index] == value:
    return index
  return -1

sorted_list = [1, 2, 3, 4, 5]
value = 3
index = binary_search(sorted_list, value)
if index >= 0:
  print(f"{value} found at index {index}")
else:
  print(f"{value} not found in list")
```

这段代码使用`bisect_left`函数对排序列表`[1, 2, 3, 4, 5]`中的值`3`执行二分搜索法。如果找到该值，它的索引将被打印到控制台。否则，将打印一条消息，指示找不到该值。

C++ — `upper_bound, lower_bound`

Java — `array.binarySearch()`

**已分类的容器**

`sortedcontainers`是一个第三方 Python 库，提供了高效的排序列表、排序字典和排序集合数据类型。它建立在内置的`list`、`dict`和`set`类型之上，提供了许多附加功能和性能改进。

`sortedcontainers`的一个主要好处是，它允许您以更高效的方式处理大型排序数据集。例如，`SortedList`类型提供了快速的插入、查找和删除，同时保持了排序的顺序。这在处理需要一直保持排序的大型数据集时特别有用。

`SortedList`的一个主要好处就是它在 O(log n)时间内执行这些操作，这比内置`list`类型所需的 O(n)时间要快得多。

随着数据集大小的增加，`SortedList`和`list`之间的性能差异变得更加明显。例如，如果您有一个包含 10，000 个元素的列表，并且您想要在特定位置插入一个新元素，那么`list`类型将花费 O(n)时间来执行插入。这意味着随着列表大小的增加，插入会花费更长的时间。

另一方面，`SortedList`在 O(log n)时间内执行插入，这意味着插入一个元素所需的时间保持不变，即使列表的大小增加了。当处理需要一直保持排序的大型数据集时，这是一个显著的优势。

总的来说，`sortedcontainers`库提供的`SortedList`类型是在 Python 中处理大型排序数据集的有用工具。与使用内置的`list`类型相比，它高效的插入、查找和删除可以节省大量时间和计算资源。

下面是如何使用`SortedList`类型执行高效插入和删除的示例:

```
from sortedcontainers import SortedList

# Create a new sorted list
sorted_list = SortedList()

# Add some elements to the list
sorted_list.add(3)
sorted_list.add(1)
sorted_list.add(2)

# The list is now sorted
print(sorted_list)  # [1, 2, 3]

# You can also add elements using the `+=` operator
sorted_list += [4, 5, 6]
print(sorted_list)  # [1, 2, 3, 4, 5, 6]

# You can delete elements using the `remove` method
sorted_list.remove(4)
print(sorted_list)  # [1, 2, 3, 5, 6]
```

C++ — `ordered_set`

Java — `TreeMap`

**lru_cache**

`lru_cache` decorator 是 Python 中的`functools`模块提供的一个函数，允许您缓存函数的结果并提高其性能。这对于计算量很大的函数或者用相同的输入值重复调用的函数特别有用。

`lru_cache`装饰器的工作原理是将函数的结果存储在缓存中，输入值作为键，输出值作为值。当用相同的输入值调用函数时，`lru_cache`装饰器从缓存中检索输出值，而不是重新执行函数。这可以显著减少函数的执行时间，并提高其整体性能。

`lru_cache`装饰器有许多可选参数，允许您定制它的行为。例如，您可以使用`maxsize`参数指定缓存的最大大小。当缓存达到最大值时，`lru_cache`装饰器会丢弃最近最少使用的条目，为新条目腾出空间。

当使用记忆技术解决动态规划问题时，它特别有用。记忆化是一种技术，涉及到将昂贵的或频繁调用的函数的结果存储在缓存中，并从缓存中检索它们，而不是重新执行函数。这可以显著提高函数的性能，尤其是当使用相同的输入值重复调用该函数时。

使用`lru_cache`装饰器进行内存化的一个主要好处是它节省了大量代码。您可以简单地使用`lru_cache`装饰器来缓存函数的结果，而不用手动实现缓存和实现从缓存中存储和检索值的逻辑。这可以极大地简化您的代码，使其更易于阅读和维护。

下面是一个如何使用`lru_cache`装饰器通过记忆化来解决动态编程问题的例子:

```
from functools import lru_cache

@lru_cache(maxsize=None)
def fibonacci(n):
  if n == 0:
    return 0
  elif n == 1:
    return 1
  else:
    return fibonacci(n-1) + fibonacci(n-2)

# Calculate the nth Fibonacci number
print(fibonacci(40))  # 102334155
```

在这个例子中，`fibonacci`函数用`lru_cache`装饰器装饰，并且`maxsize`参数被设置为`None`，这意味着缓存没有最大大小，可以存储无限数量的值。`fibonacci`函数是一个递归函数，使用记忆技术计算第 n 个斐波那契数。`lru_cache`装饰器将函数的结果存储在缓存中，并从缓存中检索它们，而不是重新执行函数，这大大提高了它的性能。

总的来说，`lru_cache` decorator 是一个有用的工具，通过使用记忆化技术来提高 Python 中动态编程算法的性能。它节省了大量代码，并使在程序中实现记忆化变得更加容易，从而导致更快、更有效的解决方案。

**默认字典**

`defaultdict`是 Python 中内置的`dict`类型的一个子类，它提供了一种便捷的方式来使用字典，这些字典对于字典中不存在的键有一个默认值。这对于使用需要用新的键和值更新的字典特别有用，默认值可以用作占位符，直到有实际值可用。

`defaultdict`的一个主要好处是，它消除了在访问或更新值之前检查字典中是否存在键的需要。对于常规字典，在访问或更新它的值之前，您需要使用`in`操作符或`.get()`方法来检查字典中是否存在一个键。这可能是乏味且容易出错的，尤其是在使用大型词典时。

要使用`defaultdict`，您需要为字典中不存在的键指定默认值。这可以通过将默认值作为参数传递给`defaultdict`构造函数来实现。默认值可以是任何类型，如整数、字符串或列表。

下面是一个如何使用`defaultdict`来计算字符串列表中单词的频率的例子:

```
from collections import defaultdict

# Create a defaultdict with a default value of 0
word_count = defaultdict(int)

# Read a list of strings
strings = ["apple", "banana", "apple", "cherry", "banana"]

# Count the frequency of each word
for word in strings:
  word_count[word] += 1

# Print the word count
print(word_count)  # defaultdict(int, {'apple': 2, 'banana': 2, 'cherry': 1})
```

在这个例子中，`defaultdict`被初始化为默认值 0。`for`循环遍历字符串列表，并递增`word_count`字典中每个单词的值。如果字典中不存在某个单词，则使用默认值 0 作为占位符，直到有实际值为止。

总的来说，`defaultdict`是使用字典的一种方便有效的方式。

**setrecursionlimit**

`setrecursionlimit`函数是 Python 中的内置函数，允许您为 Python 解释器设置最大递归深度。递归是一种编程技术，涉及从函数内部调用函数，递归深度是指函数调用自身的次数。

Python 中默认的最大递归深度通常是 1000 或 10000，这取决于 Python 的版本。这意味着，如果你试图递归调用一个函数超过 1000 或 10000 次，Python 解释器将引发一个`RecursionError`并停止执行。

`setrecursionlimit`函数允许您根据需要增加或减少最大递归深度。这在需要执行大量递归调用的情况下非常有用，例如在解决动态编程问题或处理树或链表等递归数据结构时。

然而，使用`setrecursionlimit`函数时一定要小心，因为增加最大递归深度会导致内存和性能问题。一般来说，将最大递归深度保持得尽可能低是一个好主意，只要有可能，就使用其他技术，如迭代解决方案或记忆化。

下面是一个如何使用`setrecursionlimit`函数增加最大递归深度的例子:

```
import sys

# Increase the maximum recursion depth to 5000
sys.setrecursionlimit(5000)

def recursive_function(n):
  if n > 0:
    return recursive_function(n-1)
  else:
    return 0

# Call the recursive function 5000 times
print(recursive_function(5000))  # 0
```

在这个例子中，`setrecursionlimit`函数用于将最大递归深度增加到 5000。然后调用【5000 次，由于默认的最大递归深度为 1000 或 10000，这通常会引发一个`RecursionError`。但是，最大递归深度已经增加到 5000，因此该函数能够成功执行。

总的来说，`setrecursionlimit`函数在某些需要执行大量递归调用的情况下非常有用。但是，谨慎使用它并尽可能考虑替代方法以避免内存和性能问题是很重要的。

谢谢你看我的文章！
与我互动，加入我的 [discord 服务器](https://discord.gg/UUXs24qtRz)。

*如果你觉得这篇文章很有帮助，记得去*👉 ***跟着*******拍手👏*******合用*** 👐*和你的朋友一起吧！***