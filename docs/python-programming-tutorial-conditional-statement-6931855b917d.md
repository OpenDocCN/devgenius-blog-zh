# Python 编程教程:条件语句

> 原文：<https://blog.devgenius.io/python-programming-tutorial-conditional-statement-6931855b917d?source=collection_archive---------9----------------------->

![](img/851b4bef692d272414525395dedfd12d.png)

摄影:unsplash.com 的马库斯·温克勒

> 这篇文章是我的 [Python 编程教程](https://arc-sosangyo.medium.com/list/introduction-to-python-programming-80e79264dcad)系列的一部分。

编程中的 Conditional 处理决策并执行特定的操作，前提是满足指定的条件。

以下是 Python 编程中条件的形式:

*   **if 语句**
*   **if … else 语句**
*   **if … else if … else 语句**
*   **嵌套语句**
*   **通过语句**

# 如果语句

if 语句是条件语句的基本形式。

例如

```
gameScore = 100if gameScore == 100:
  print('Perfect')
```

输出将是:

```
Perfect
```

将上述代码翻译成人类语言。意思是如果 gameScore 等于 100，那么打印完美。

一个简单的 if 语句可以用下面的速记方法写成一行:

例如

```
gameScore = 100if gameScore == 100: print('Perfect')
```

# if … else 语句

有些情况下，如果值不符合编程条件，您还想执行不同的操作。这就是你必须在 if 语句中包含 **else** 的地方。在某些情况下，这也是避免不必要错误的简单方法。

例如

```
gameScore = 99if gameScore == 100:
  print('Perfect')
else:
  print('You did not get the perfect score')
```

输出将是:

```
You did not get the perfect score
```

上面的例子显示 else 下的动作被执行，因为 gameScore 不是 100。

下面是上面例子的简写版本。这种语法也被称为**三元运算符**

例如

```
gameScore = 99print('Perfect') if gameScore == 100 else print('You did not get the perfect score')
```

# if … else if … else 语句

在更复杂的代码中，您将需要在条件语句中有多种选择。如果可以通过在语法中使用 elif 添加更多选项，则使用 **else。**

例如

```
gameScore = 50if gameScore == 100:
  print('Perfect')
elif gameScore >= 80:
  print('Average')
elif gameScore >= 50:
  print('Below Average')
elif gameScore >= 20:
  print('Beginner')
else:
  print('Not in range')
```

输出将是:

```
Below Average
```

> **注意:**如果需要了解本教程中使用的运算符如 **> =** ，可以参考这个[链接](https://arc-sosangyo.medium.com/python-programming-tutorial-basic-operators-d34693dc4404)。

# 嵌套语句

也可以将一个 if 语句放在另一个 if 语句中。这种做法称为**嵌套**，用于更复杂的多分支条件语句。

例如

```
gameScore = 84if gameScore == 100:
  print('Perfect')elif gameScore >= 80:
  print('Average') if gameScore < 85:
    print('Next level not qualified')elif gameScore >= 50:
  print('Below Average')elif gameScore >= 20:
  print('Beginner') if gameScore < 10:
    print('Game Over')else:
  print('Not in range')
```

输出将是:

```
Average
Next level not qualified
```

# Pass 语句

Python 的条件语句不接受空语句。如果因为某些原因需要放一个空语句，使用 pass 来避免得到错误。

例如

```
gameScore = 50if gameScore != 100:
  pass
```

# 组合条件语句

逻辑运算符**或**以及逻辑运算符**和**可以用来组合条件表达式。

以下是使用**和**运算符的条件语句示例。

例如

```
level1Score = 100
level2Score = 50if level1Score == 100 and level2Score == 100:
  print('You got the perfect score')
else:
  print('You did not get the perfect score')
```

输出将是:

```
You did not get the perfect score
```

以下是使用**或**运算符的条件语句示例。

例如

```
level1Score = 50
level2Score = 100if level1Score == 100 or level2Score == 100:
  print('You got the perfect score')
else:
  print('You did not get the perfect score')
```

输出将是:

```
You got the perfect score
```

在我们的下一个教程中，我们将讨论 python 循环。

愿法典与你同在，

*   弧

*更多内容尽在*[*blog . dev genius . io*](http://blog.devgenius.io)*。*