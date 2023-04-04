# Python 编程教程:函数和 Lambda

> 原文：<https://blog.devgenius.io/python-programming-tutorial-functions-and-lambda-1d0729097f37?source=collection_archive---------7----------------------->

![](img/5c63acf00277c662fb96b2ff7fafea0e.png)

摄影:克里斯·里德在 unsplash.com

> 这篇文章是我的 [Python 编程教程](https://arc-sosangyo.medium.com/list/introduction-to-python-programming-80e79264dcad)系列的一部分。

函数是一个可重用的代码，包含一组执行特定结果的指令。使用函数的主要目的是避免出于相同目的多次编写相同的代码块。

在 Python 中，我们通过使用 **def** 来声明函数。

例如

```
*def* raceHobbit():
  print('The Hobbits are peaceful')raceHobbit()
```

输出将是:

```
The Hobbits are peaceful
```

您可以使用参数(也称为自变量)自定义函数。

例如

```
*def* raceHobbit(*hobbitName*):
  print("The Hobbits are peaceful.")
  print(*hobbitName*, "is a Hobbit")raceHobbit("Bilbo")
```

输出将是:

```
The Hobbits are peaceful.
Bilbo is a Hobbit
```

有些情况下，可读性更好、更清晰的参数会更好。这在处理大量数据时非常有用。下面的示例产生了与上面所示相同的输出。

例如

```
*def* raceHobbit(*hobbitName*):
  print("The Hobbits are peaceful.")
  print(*hobbitName*, "is a Hobbit")raceHobbit(*hobbitName*="Bilbo")
```

您也可以使用多个变量和参数。

例如

```
*def* raceHobbit(*race*, *hobbitName*):
  print(*f*"The {*race*} are peaceful.")
  print(*hobbitName*, "is a Hobbit")raceHobbit(*race*="Hobbits", *hobbitName*="Bilbo")
```

输出将是:

```
The Hobbits are peaceful.
Bilbo is a Hobbit
```

函数也可以有强制返回值。在下面的例子中，我们将创建一个简单的磅到千克的转换器。

例如

```
*def* lbsToKilogram(*lbs*):
  kg = *lbs* * 0.453592
  print(*f*"{kg}kg")
  return kglbsToKilogram(200)
```

输出将是:

```
90.7184kg
```

函数不能为空，但是如果需要，可以使用 pass 语句。

例如

```
*def* emptyFunctions():
  pass
```

# λ表达式

在其他编程语言中也称为匿名函数，因为它没有任何声明的名称。是一个单行函数，通常用于减少代码行数。

在下面的例子中，我们将创建一个 lambda 版本的简单的磅千克转换器。

例如

```
lbsToKilogram = *lambda* *lbs*: *lbs* * 0.453592print(lbsToKilogram(200), 'kg')
```

输出将是:

```
90.7184 kg
```

在我们的下一个教程中，我们将用 python 来处理[面向对象编程](https://arc-sosangyo.medium.com/python-programming-tutorial-object-oriented-programming-using-classes-1de04f027611)的世界。

愿法典与你同在，

*   弧

*更多内容尽在*[*blog . devgenius . io*](http://blog.devgenius.io)*。*