# Python 编程教程:序列-列表、元组和范围

> 原文：<https://blog.devgenius.io/introduction-to-python-programming-sequence-list-tuples-and-range-1caa03ffd55c?source=collection_archive---------2----------------------->

![](img/4497771cc2dc2d15940290554da6428b.png)

摄影:格伦·卡斯滕斯·彼得斯论 unsplash.com

Sequence 是一种包含有序值集合的数据类型。它的工作方式与变量相同，但包含多个值。每个值都是有序的，从 0 开始。Python 编程提供了三种基本序列:列表、元组和范围。

> 这篇文章是我的 [Python 编程教程](https://arc-sosangyo.medium.com/list/introduction-to-python-programming-80e79264dcad)系列的一部分。

# Python 列表

列表是包含可变值的有序集合的序列数据类型。列表是通过将值括在方括号 **[]** 中来声明的。

例如

```
numbers = [1,2,5,9]
progLang = ["Python", "Swift", "C++", "Javascript"]print(numbers)
print(progLang)
```

输出将是:

```
[1, 2, 5, 9]
['Python', 'Swift', 'C++', 'Javascript']
```

列表也可以用这种方式声明，因为有些程序员觉得这样更有条理。

例如

```
progLang = [
  "Python",
  "Swift",
  "C++",
  "Javascript"
]
```

提取列表中的特定值可以这样执行:

例如

```
progLang = ["Python", "Swift", "C++", "Javascript"]print(progLang[1])
print(progLang[2])
print(progLang[3])
print(progLang[0])
```

输出将是:

```
Swift
C++
Javascript
Python
```

列表中的值从 0 开始。所以上面例子中的第一个值，Python 的索引是 0，Swift 是 1，C++是 2，JavaScript 是 3。

因为列表是可变。您可以向已经声明的列表中添加值。

例如

```
progLang = ["Python", "Swift", "C++", "Javascript"]progLang.append("Kotlin")
print(progLang)
```

输出将是:

```
['Python', 'Swift', 'C++', 'Javascript', 'Kotlin']
```

# Python 元组

元组是包含不可变的有序值集合的序列数据类型。这意味着与 list 不同，您不能编辑 tuple 中的值。元组是通过将值括在圆括号 **()** 中来声明的。

例如

```
numbers = (1,2,5,9)
progLang = ("Python", "Swift", "C++", "Javascript")print(numbers)
print(progLang)
```

输出将是:

```
(1, 2, 5, 9)
('Python', 'Swift', 'C++', 'Javascript')
```

Tuple 也可以用这种方式声明，因为一些程序员发现它更有组织性。

例如

```
progLang = (
  "Python",
  "Swift",
  "C++",
  "Javascript"
)print(progLang)
```

提取元组中的特定值可以这样完成:

例如

```
progLang = ("Python", "Swift", "C++", "Javascript")print(progLang[1])
print(progLang[2])
print(progLang[3])
print(progLang[0])
```

输出将是:

```
Swift
C++
Javascript
Python
```

看上面的例子，提取一个值的工作方式和列表一样。索引从 0 开始。

# Python 系列

范围是以特定顺序返回数字的序列。

例如

```
r = range(5)for n in r:
  print(n)
```

输出将是:

```
0
1
2
3
4
```

默认情况下，范围从 0 开始。在上面的例子中，声明 5 将给出 5 个数字，以 0 开始，以 4 结束(0，1，2，3，4)。

范围的起点和终点也可以自定义。

例如

```
r = range(6, 10)for n in r:
  print(n)
```

输出将是:

```
6
7
8
9
```

它将从字面上得到起始数字 6，但不是最终数字。在上面的例子中，最后的数字是 10，但它只显示输出到 9。因此，请注意范围是如何工作的。

如果您的程序也需要在输出中输入 10，这是它的变通方法。

例如

```
r = range(6, 10 + 1)for n in r:
  print(n)
```

输出将是:

```
6
7
8
9
10
```

数字增加的方式也可以调整。

例如

```
r = range(1, 10, 2)for n in r:
  print(n)
```

输出将是:

```
1
3
5
7
9
```

如代码中所声明的，该数字从 1 开始，增加 2。

> 在我们关于范围的例子中，我们使用称为 for loop 的循环语句。这将在单独的教程中解决。

在下一篇教程中，我们将处理另一种包含值集合的数据类型，称为[字典](https://arc-sosangyo.medium.com/introduction-to-python-programming-mapping-dictionary-4d63c5b4db2e)。

愿法典与你同在，

-电弧