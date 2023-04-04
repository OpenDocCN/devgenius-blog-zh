# 列表理解解决任务的有效方法？| Python

> 原文：<https://blog.devgenius.io/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa?source=collection_archive---------0----------------------->

![](img/b265132e1b8e742a97d4241245107aff.png)

Python 是一种极其多样化和强大的编程语言！做一件事有不同的方法。在文章中，我将向您展示**列表理解**。我们将讨论如何使用它，什么时候应该使用它，什么时候不应该使用它。全部读下来！

## 内容计划:

1.  [列表理解的优势](https://medium.com/@vlad.bashtannyk/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa#:~:text=Conclusion.-,Advantages%20of%20List%20Comprehension,-%C2%B7%20More%20time%2Defficient)。
2.  [如何在 Python 中创建列表](https://medium.com/@vlad.bashtannyk/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa#:~:text=into%20a%20formula.-,How%20to%20Create%20Lists%20in%20Python,-List%20comprehension%20is)。
    [循环](https://medium.com/@vlad.bashtannyk/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa#:~:text=and%20make%20findings.-,Loops,-Loops%20are%20a)。
    [映射()对象](https://medium.com/@vlad.bashtannyk/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa#:~:text=the%20append()%20method.-,map()%20Objects,-map()%20is%20an)。
    [列表理解](https://medium.com/@vlad.bashtannyk/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa#:~:text=list%20using%20list().-,List%20Comprehension,-Now%2C%20let%E2%80%99s%20take)。
3.  [哪种方式更有效率](https://medium.com/@vlad.bashtannyk/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa#:~:text=member%20in%20iterable%5D-,Which%20Approach%20is%20More%20Efficient,-Okay%2C%20we%20learned)。
4.  [提高理解水平](https://medium.com/@vlad.bashtannyk/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa#:~:text=compact%20yet%20optimized.-,Advance%20Level%20of%20a%20Comprehension,-Conditionals%20Logic)。
    条件句逻辑。
    [集合理解](https://medium.com/@vlad.bashtannyk/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa#:~:text=6%2C%208.78%2C%201%2C1%5D-,Set%20Comprehensions,-You%20can%20also)。
    [字典释义](https://medium.com/@vlad.bashtannyk/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa#:~:text=string%20is%20%E2%80%9Cx%E2%80%9D.-,Dictionary%20Comprehensions,-A%20dictionary%20comprehensions)。
    [海象算子](https://medium.com/@vlad.bashtannyk/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa#:~:text=in%20your%20expression.-,Walrus%20Operator,-Walrus%20operator%2C%20introduced)。
5.  [在 Python 中什么时候不用理解力](https://medium.com/@vlad.bashtannyk/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa#:~:text=104%2C%20109%2C%20106%5D-,When%20Not%20to%20Use%20a%20Comprehension%20in%20Python,-List%20comprehensions%20are)。
    [小心嵌套的理解](https://medium.com/@vlad.bashtannyk/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa#:~:text=an%20alternative%20way.-,Watch%20Out%20for%20Nested%20Comprehensions,-Comprehensions%20can%20be)。
    [为大型数据集使用生成器](https://medium.com/@vlad.bashtannyk/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa#:~:text=or%20hurts%20readability.-,Use%20Generators%20for%20Large%20Datasets,-A%20list%20comprehension)。
6.  [结论](https://medium.com/@vlad.bashtannyk/is-list-comprehension-the-most-effective-way-to-solve-any-tasks-python-b6bb3f5391fa#:~:text=efficient%20than%20map().-,Conclusion,-This%20article%20taught)。

# **列表理解的优势**

比循环更节省时间和空间。

需要更少的代码行。

将迭代语句转换为公式。

# 如何在 Python 中创建列表

**列表理解**是一个基于现有列表创建列表的语法结构。让我们来看看创建列表的不同实现，并得出结论。

## 环

循环是创建列表的传统方式。不管你用什么样的循环。要以这种方式进行创作，您应该:

> 1.实例化一个空列表。
> 
> 2.在可迭代的元素或元素范围内循环。
> 
> 3.将每个元素追加到列表的末尾。

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

在本例中，您实例化了一个空列表`numbers`。然后使用循环`for` 遍历`range(10)`，并使用`append()`方法将每个数字附加到列表的末尾。

## map()对象

`map()` 是创建列表的另一种方法。您需要向`map()`传递一个函数和一个 iterable，它将创建一个对象。这个对象包含使用给定函数执行每个迭代元素所得到的输出。

例如，我们将承担在一些产品的价格上增加增值税的任务。

由弗拉德·巴什坦尼克创作

**输出:**

```
<map object at 0x7f18721e7400>  # map(add_vat, prices)
[11.03, 9.46, 36.14, 45.65, 24.9]  # list(grand_prices)
```

你构建了`add_vat()` 函数并且已经有了`prices`。您将这两个参数传递给`map()`并收集一个结果 map 对象`grand_prices`，或者您可以使用`list()`轻松地将其转换成一个列表。

## 列表理解

现在，让我们来看看列表理解法！这是真正的 Pythonic 风格，也是创建列表更好的方式。为了弄清楚这种方法有多强大，我们可以用一行代码重写循环示例。

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

如你所见，这是一个不可思议的方法！列表理解看起来足够易读，你不需要写很大的代码，只需要一行。

为了更好地理解列表，请查看以下公式:

```
new_list = [expression for member in iterable]
```

# 哪种方法更有效

好的，我们学习了如何使用循环、`map()`和列表理解来创建列表，但是你可能会问“哪种方法更有效？”。我们来分析一下！

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
0.9833468980004909  # with_map
1.197223742999995  # with_comprehension
1.3564663889992516  # with_loop
```

正如我们现在看到的，创建列表的最佳方式是`map()`,第二个是列表理解，最后一个是循环。

然而，方法的选择取决于你想要达到的目标。

> 使用`map()`来优化你的代码。
> 
> 用一个循环，让它尽可能的清晰。
> 
> 如果你希望你的代码紧凑而优化，使用列表理解。这是创建列表的最佳方式，因为这种方法可读性最强。

# 理解的高级水平

## 条件逻辑

之前我向你们展示了这个公式:

```
new_list = [expression for member in iterable]
```

公式也有点不完整。对理解公式更完整的描述增加了对可选条件的支持。向列表理解添加条件逻辑的最常见方法是在表达式的末尾添加条件性:

```
new_list = [expression for member in iterable (if conditional)]
```

这里，您的条件语句位于右括号中。

条件很重要，因为它们允许列表理解过滤掉不需要的值，这通常需要调用`filter()`:

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
[0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```

正如你所看到的，这种理解收集了能被 2 整除而没有余数的数。

如果您需要一个更复杂的过滤器，那么您甚至可以将条件逻辑移到一个单独的函数中。

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
[2, 3, 5, 7, 11, 13, 17, 19]
```

构建`is_prime(number)` 来确定一个质数并返回布尔值。下一步，你应该把函数添加到理解的条件中。

该公式允许您使用条件逻辑从几个可能的输出选项中进行选择。例如，您有一份产品价格表，如果有负数，您应该将其转换为正数:

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
[1.34, 19.01, 4.2, 6, 8.78, 1,1]
```

这里，你的表达式`price`有一个条件语句，`if price > 0 else price*-1`。这告诉 Python，如果价格为正，则输出`price`的值，但是如果价格为负，则将价格转换为正。如果这显得势不可挡，那么将条件逻辑视为其自身的功能是很有用的:

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
[1.34, 19.01, 4.2, 6, 8.78, 1,1]
```

## 集合理解

你也可以创建一个集合理解！和列表理解差不多。不同的是，集合理解不包含重复。您可以通过使用花括号而不是括号来创建一个集合理解:

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
{"E", "e", "n", "t", "x", "c", "l"}
```

您的集合理解只包含唯一的字母。与列表不同，集合不保证项目将按特定顺序保存。这就是为什么集合的第二个字母是“e”，即使字符串中的第二个字母是“x”。

## 词典释义

字典中的理解是相似的，只是需要定义一个键:

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
{"Words": 1, "are": 2, "but": 3, "wind": 4}
```

为了创建`word_order`字典，在表达式中使用花括号(`{}`)和键值对(`el: ind+1`)。

## 海象操作员

Python 3.8 中引入的 Walrus 操作符允许您同时解决两个问题:为变量赋值并返回该值。

假设您需要对一个将返回温度数据的 API 应用十次。你想要的只是华氏 100 度以上的结果。假设每个请求将返回不同的数据。在这种情况下，无法使用 Python 中的列表理解来解决这个问题。iterable (if conditional)中成员的公式表达式没有为条件提供将数据分配给表达式可以访问的变量的方法。

海象运营商解决了这个问题。它允许您在将输出值赋给变量的同时执行表达式。以下示例显示了这是如何实现的，使用 get_weather_data()生成假天气数据:

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
[108, 100, 106, 103, 108, 106, 103, 104, 109, 106]
```

# 在 Python 中何时不使用理解力

列表理解真的很有用，可以帮助你写出清晰、易读、易调试的代码。但是在某些情况下，它们可能会使你的代码运行得更慢或者占用更多的内存。如果你的代码**不太有效**或者**更难**理解，那么很可能可以参考**选择一种替代方式**。

## 小心嵌套的理解

理解可以**嵌套**以在集合中创建列表、字典和集合的组合。例如，假设一家公司正在跟踪今年在五个不同城市的收入。存储这些数据的完美数据结构可以是嵌套在字典理解中的列表理解。

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
{
    "NewYork": [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    "Oklahoma": [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    "Toronto": [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    "LosAngeles": [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    "Miami": [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
}
```

您用字典理解创建外部集合`budgets`。这个表达式是一个键值对，它包含了另一种理解。这段代码将快速生成`cities`中每个城市的数据列表。

嵌套列表是创建**矩阵**的常用方法，通常用于数学目的。查看下面的代码块:

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
[
    [0, 1, 2, 3, 4, 5, 6],
    [0, 1, 2, 3, 4, 5, 6],
    [0, 1, 2, 3, 4, 5, 6],
    [0, 1, 2, 3, 4, 5, 6],
    [0, 1, 2, 3, 4, 5, 6],
    [0, 1, 2, 3, 4, 5, 6]
]
```

外部列表理解`[... for y in range(6)]`创建六行，而内部列表理解`[x for x in range(7)]`用值填充每一行。

到目前为止，每个嵌套理解的目标都是真正直观的。然而，还有其他情况，比如**展平**嵌套列表，其中的逻辑可能会使您的代码难以阅读。让我们以下面的例子为例，使用嵌套列表理解来展平矩阵:

由弗拉德·巴什坦尼克创作

**输出:**

```
[0, 1, 0, 1, 0, 1, 2, 1, 2]
```

展平矩阵的代码很简洁，但是很难，你应该花些时间弄清楚它到底是如何工作的。另一方面，如果你不得不使用`for`循环来展平同一个矩阵，那么你的代码将会更加简单易读:

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
[0, 1, 0, 1, 0, 1, 2, 1, 2]
```

现在您可以看到代码一次遍历矩阵的一行，在移动到下一行之前取出该行中的所有元素。

虽然嵌套列表理解可能看起来更 Pythonic 化，但是最重要的**是用于编写代码的**，您的团队可以毫无问题地理解和修改这些代码。当你选择你的方法时，你应该根据你认为理解是有助于还是有损于可读性来做出判断。

## 对大型数据集使用生成器

Python 中的列表理解是通过将整个输出列表上传到内存中来实现的。对于小型甚至中型的列表，这通常是好的。如果你想把前 1000 个整数加起来，那么列表理解将毫无问题地解决这个任务:

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
499500
```

但是如果你需要对前十亿个数字求和呢？您可以尝试这样做，但是您的计算机可能没有响应。这是因为计算机应该分配大量的内存。也许你的电脑没有足够的资源来生成一个庞大的列表并存储在内存中。

使用**生成器**处理大量数据！生成器不会在内存中存储单一的大型数据结构，而是返回一个 iterable。您的代码可以根据需要多次从 iterable 对象中请求下一个值，直到到达末尾，同时一次只存储一个值。

例如，你想先得到 10 亿个整数，然后让我们使用生成器！这可能需要一些时间，但计算机应该可以完成:

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
499999999500000000
```

看起来就像列表理解，但是正如你所看到的，要创建一个单行生成器，你需要使用圆括号。

上面的例子仍然占用很多资源，但是它执行操作**很慢**。一旦生成器产生一个值(例如 10)，它就可以将这个值加到当前的和中，然后丢弃那个值，生成下一个值(11)。当 sum 函数请求下一个值时，循环重新开始。它减少了内存占用。

`map()`在惰性模式下也能工作，这意味着如果您选择在这种情况下使用它，内存不会成为问题:

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
499999999500000000
```

让我们做一个调查，看看哪种方法更优化！

由[弗拉德·巴什坦尼克](https://medium.com/u/a74e486d5a33?source=post_page-----b6bb3f5391fa--------------------------------)创作

**输出:**

```
4940.844053814  # get_sum_with_map
3464.1995523349997  # get_sum_with_generator
```

如你所见，发电机**比`map()`更有效率。**

# 结论

本文向您介绍了对列表的理解，以及如何使用它来解决复杂的任务，而不会使您的代码变得太难。

现在你:

*   学会了几种创建列表的替代方法。
*   找出每种方法的优点。
*   可以简化循环和`map()`调用**列表理解**。
*   想出了一个在理解中添加条件逻辑的方法。
*   可以创建**集合**和**词典释义**。
*   学会了什么时候不使用理解。

谢谢你阅读这篇文章直到结束！**点击** **【拍手】**，**留下评论**如果这篇文章已经有所帮助，下一篇文章你想要什么主题。记得**点击** **【关注】**确保不要错过我的文章！你的活动是我的快乐！祝你好运！