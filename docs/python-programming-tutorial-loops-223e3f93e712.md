# Python 编程教程:循环

> 原文：<https://blog.devgenius.io/python-programming-tutorial-loops-223e3f93e712?source=collection_archive---------4----------------------->

![](img/cd479e107fb3d5e30d1c927440d28ec2.png)

摄影:unsplash.com 的艾蒂安·吉拉德特

有些情况下，您需要在代码中重复执行一些操作。最初的直觉是多次复制粘贴代码。虽然它在大多数情况下有效，但这不是最佳实践。尤其是当重复动作的方式需要改变时。这就是 loop 介入处理这种情况的原因。

只要满足条件，编程中的循环就会重复一个代码块。

> 这篇文章是我的 [Python 编程教程](https://arc-sosangyo.medium.com/list/introduction-to-python-programming-80e79264dcad)系列的一部分。

# For 循环

For 循环根据序列重复代码块，序列可以是列表、元组、字典、集合或字符串。

例如

```
fellowRing = [
  'Frodo',
  'Sam',
  'Merry',
  'Pippin',
  'Aragorn',
  'Boromir',
  'Legolas',
  'Gimli',
  'Gandalf'
]for member in fellowRing:
  print(*f*'{member} is a member of Fellowship of the Ring')
```

输出将是:

```
Frodo is a member of Fellowship of the Ring
Sam is a member of Fellowship of the Ring
Merry is a member of Fellowship of the Ring
Pippin is a member of Fellowship of the Ring
Aragorn is a member of Fellowship of the Ring
Boromir is a member of Fellowship of the Ring
Legolas is a member of Fellowship of the Ring
Gimli is a member of Fellowship of the Ring
Gandalf is a member of Fellowship of the Ring
```

上面的例子表明 for 循环消除了重复，而不是多次打印每个名字。

For 循环也可用于数值范围。

例如

```
for index in range(5):
  print(*f*'{index} multiplied by 10 is equal to {index * 10}')
```

输出将是:

```
0 multiplied by 10 is equal to 0
1 multiplied by 10 is equal to 10
2 multiplied by 10 is equal to 20
3 multiplied by 10 is equal to 30
4 multiplied by 10 is equal to 40
```

> **注:**关于范围的更多信息，你可以参考这个[教程](https://arc-sosangyo.medium.com/introduction-to-python-programming-sequence-list-tuples-and-range-1caa03ffd55c)。

**字符串**在循环时将被视为序列。

例如

```
for ranger in 'Aragorn':
  print(ranger)
```

输出将是:

```
A
r
a
g
o
r
n
```

你可以使用 **break** 语句来停止循环。例如，如果满足特定条件，则停止循环。

例如

```
fellowRing = [
  'Frodo',
  'Sam',
  'Merry',
  'Pippin',
  'Aragorn',
  'Boromir',
  'Legolas',
  'Gimli',
  'Gandalf'
]for member in fellowRing:
  print(*f*'{member} goes to Mount Doom') if member == 'Sam':
    break
```

输出将是:

```
Frodo goes to Mount Doom
Sam goes to Mount Doom
```

你也可以停止当前的迭代，然后**继续**下一次迭代。

例如

```
fellowRing = [
  'Frodo',
  'Sam',
  'Merry',
  'Pippin',
  'Aragorn',
  'Boromir',
  'Legolas',
  'Gimli',
  'Gandalf'
]for member in fellowRing: if member == 'Boromir':
    continue print(*f*'{member} is still alive')
```

输出将是:

```
Frodo is still alive
Sam is still alive
Merry is still alive
Pippin is still alive
Aragorn is still alive
Legolas is still alive
Gimli is still alive
Gandalf is still alive
```

注意上面的例子中没有包含 Boromir，因为这是执行停止/继续的地方。

您可以包含一个 **else** 语句，以便在条件不再为真时运行一段代码。

例如

```
for i in range(1,10 + 1):
  print(i)
else:
  print('Finish counting 1 to 10')
```

输出将是:

```
1
2
3
4
5
6
7
8
9
10
Finish counting
```

for 循环不能接受空语句。出于任何原因，您需要使它为空，您可以使用 pass 语句。

例如

```
for i in range(10):
  pass
```

# While 循环

While 循环重复一段代码，直到指定的条件变为假。

例如

```
countOneToTen = 0while countOneToTen <= 10:
  print('Number', countOneToTen)
  countOneToTen += 1
```

输出将是:

```
Number 0
Number 1
Number 2
Number 3
Number 4
Number 5
Number 6
Number 7
Number 8
Number 9
Number 10
```

> ****"+= "****是一个运算符，表示按您在右侧提供的数字递增。**

*你可以使用 **break** 语句来停止循环。例如，如果满足特定条件，则停止循环。*

*例如*

```
*countOneToTen = 0while countOneToTen <= 10:
  print('Number', countOneToTen) if countOneToTen == 5:
    break countOneToTen += 1*
```

*输出将是:*

```
*Number 0
Number 1
Number 2
Number 3
Number 4
Number 5*
```

> *在这个[环节](https://arc-sosangyo.medium.com/python-programming-tutorial-basic-operators-d34693dc4404)上讨论了 **< =** 运算符。*

*你也可以停止当前的迭代，然后**继续**下一次迭代。*

*例如*

```
*countOneToTen = 0while countOneToTen < 10: countOneToTen += 1 if countOneToTen == 5:
    continue print('Number', countOneToTen)*
```

*输出将是:*

```
*Number 1
Number 2
Number 3
Number 4
Number 6
Number 7
Number 8
Number 9
Number 10*
```

*注意**数字 5** 不包括在结果中，因为那是停止/继续执行的点。*

*您可以包含一个 **else** 语句，以便在条件不再为真时运行一段代码。*

*例如*

```
*countOneToTen = 0while countOneToTen <= 10:
  print('Number', countOneToTen)
  countOneToTen += 1
else:
  print('Finish counting 1 to 10')*
```

*输出将是:*

```
*Number 0
Number 1
Number 2
Number 3
Number 4
Number 5
Number 6
Number 7
Number 8
Number 9
Number 10
Finish counting 1 to 10*
```

*在我们的下一个教程中，我们将讨论 Python [函数](https://arc-sosangyo.medium.com/python-programming-tutorial-functions-and-lambda-1d0729097f37)。*

*愿法典与你同在，*

*   *弧*

**更多内容尽在*[*blog . dev genius . io*](http://blog.devgenius.io)*。**