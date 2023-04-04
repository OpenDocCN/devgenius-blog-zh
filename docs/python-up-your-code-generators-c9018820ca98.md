# Python Up 您的代码:生成器

> 原文：<https://blog.devgenius.io/python-up-your-code-generators-c9018820ca98?source=collection_archive---------6----------------------->

## Python 中的生成器代表什么，它们的用法和最佳实践

![](img/55b8fc9dd6a97c63d9cbb0c0c532d49e.png)

Jr Korpa 在 [Unsplash](https://unsplash.com/s/photos/digital-abstract?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

作为自然的下一步，正如我们稍后将看到的，在上一篇[文章中我们发现并讨论了迭代器的主题，今天我们将讨论另一个与 Python 相关的非常重要的主题:生成器。](https://medium.com/@deck451/python-up-your-code-iterators-af5331f68ccd)

我们已经看到用 Python 构建迭代器并不是最简单的事情。我们看到了我们如何需要创建和使用一个实现了`__iter__()`和`__next__()`方法的类，此外，我们还必须密切关注元素耗尽的情况，并引发一个`StopIteration`异常作为直接后果。

现在让我们指出，通过这些生成器，所有这些功能都可以用我们这边更少的努力来实现。我想我描述生成器的最简单的方式是:一个返回生成器对象的函数，我们可以在迭代过程中使用它。

现在，生成器只不过是一个函数，创建它相当容易。但是它确实使用了`yield`语句，与常规函数相反，常规函数通常使用`return`语句。

就返回值而言，`yield`和`return`的作用相同。它们返回值。不同之处在于这两者对函数执行流程的影响:

*   `return`终止函数的执行；
*   `yield`相当于音乐播放器上的**暂停**按钮。它维护函数的状态，以便以后再次调用时可以从该点恢复。

生成器函数与常规函数有一些基本区别:

*   生成器函数包含一个或多个`yield`语句。常规函数不包含这样的语句。如果有，它将立即成为一个生成器函数；
*   作为一个生成器函数，它返回一个生成器对象。但是，调用该函数不会开始执行。我们可以使用`next()`函数或`for` / `while`循环对其进行迭代，然后生成器函数才开始执行；
*   像`__iter__()`和`__next__()`这样的方法是自动实现的。因此，我们可以使用`next()`遍历元素；
*   如前所述，`yield`只是暂停函数，控制转移到调用块；这意味着局部变量及其值在连续调用之间得到维护；
*   当函数终止时，`StopIteration`会在进一步调用时自动引发。这也意味着迭代器只能迭代一次。要重启这个过程，我们需要创建另一个生成器对象；

让我们举例说明所有这些特征:

```
*# define a generator function to generate 1 number* **def** my_generator():
    **print**("Generator function running")
    n = 1
    **yield** n *# run the function to return a generator object* my_gen = my_generator()
**print**(f"{**type**(my_gen)}: {my_gen}") *# only when we iterate over it,
# does the generator function execute
# ask for two values, but generator can only give 1* **print**(**next**(my_gen))
**print**(**next**(my_gen))Output:
<class 'generator'>: <generator object my_generator at 0x7fbed6d56490>
Generator function running
1
Traceback (most recent call last):
  ...
    print(next(my_gen))
StopIteration
```

如我们所见，在这种情况下，生成器函数只包含一个`yield`语句。

我们还看到，通过使用`next()`函数，我们可以对生成器函数返回的迭代器进行迭代。这意味着迭代器协议方法(`__iter__()`和`__next__()`)都已经实现；

输出行非常清楚地表明，在被我们对`next()`函数的两次调用触发之前，生成器函数不会开始实际执行。另外，注意对`next()`的第二次调用是如何触发`StopIteration`异常的。

让我们举一个更现实的例子。如果生成器只生成一个值，就没什么用了:

```
*# define a generator function* **def** my_generator():
    **print**("Generator function called")
    **for** element **in** **range**(10):
        **yield** element *# run the function to return a generator object* my_gen = my_generator()*# use the generator object*
**for** number **in** my_gen:
    **print**(number)Output:
0
1
2
3
4
5
6
7
8
9
```

生成器的另一个非常有趣的特性是，我们不仅可以从它们那里获取数据，还可以向它们提供数据，潜在地动态定制它们的行为。这里有一个简单的使用案例。假设我们需要一个生成器函数来返回一个数字的三倍值:

```
*# define a generator function* **def** my_generator():
    **while** **True**:
        *# get the value*
        value = **yield** *# triple it and return it* **yield** value * 3 *# now use the generator*
my_gen = my_generator()**for** number **in** **range**(10):
    **next**(my_gen)
    **print**(my_gen.**send**(number))Output:
0
3
6
9
12
15
18
21
24
27
```

看看我们使用`yield`关键字让生成器函数获取我们通过`send()`发送给它的值有多容易？这是`yield`关键字的另一种用法。可能会有点混乱，因为我们已经习惯了`yield` — `return`的类比，而`return`绝不是一个与输出相关的关键字，然而，值得注意的是，我刚刚介绍的`yield`关键字的两种用法都只与生成器相关，所以也有这种情况。

官方文档谈到了这个`yield` **语句**(也就是通常的`yield some_value`)与`yield` **表达式**(它表示在赋值的右边使用的`yield`关键字)[这里是](https://peps.python.org/pep-0342/#specification-sending-values-into-generators)。

另外，请注意，不允许试图在尚未执行到到达`yield`语句的点的生成器上立即调用`send()`。它会抛出一个`TypeError`:

```
Traceback (most recent call last):
  ...
    my_gen.send(123)
TypeError: can't send non-None value to a just-started generator
```

让我们深入了解一下`for`循环实现了什么，以及它实际上是如何工作的:

1.  对`next()`的调用使生成器执行，直到到达第一个`yield`语句(对`next()`的调用相当于一个`send(None)`，所以我们可能没有调用`send(None)`而是调用了生成器上的`next()`)；
2.  此时，生成器正在等待发送给它的某个值；
3.  我们通过`send()`方法发送该值，然后生成器恢复执行，消耗发送的值，然后`yield`处理三倍的值，一旦生成新元素就再次暂停执行；
4.  循环重新开始，对`next()`的另一个调用将看到发生器再次恢复执行，直到发生器函数的第一个`yield`语句，它将耐心等待下一个`send()`调用，循环继续进行。

上面的代码可以重写为只使用一个`yield`表达式，如下所示:

```
*# define a generator function* **def** my_generator():
    value = -1
    **while** **True**:
        *# get the value and yield its triple*
        value = **yield** value * 3 *# create the generator
# start its execution up to the yield statement*
my_gen = my_generator()
**next**(my_gen)**for** number **in** **range**(10):
    **print**(my_gen.**send**(number))Output:
0
3
6
9
12
15
18
21
24
27
```

1.  首先，我们调用生成器函数。它返回一个生成器对象，我们将把它赋给`my_gen`；然后，我们对它调用`next()`，生成器函数开始执行，直到它到达`yield`表达式；
2.  在生成器对象上调用了`send()`方法。生成器函数获取这个值，然后继续执行，直到再次到达`yield`语句；此时，它返回三倍的值，并再次暂停执行，等待下一个`send()`方法。

## 生成器表达式

如果我们的目标是构建相对简单的生成器，我们甚至不需要单独的生成器函数。我们可以使用生成器表达式。从语法上来说，它看起来非常类似于[列表理解](https://medium.com/@deck451/python-up-your-code-list-comprehensions-bc34ac300b48)。然而，有两个基本方面将它们区分开来:

1.  与列表理解不同，列表理解可以通过包围理解的方括号来识别，生成器表达式具有常规(圆)括号或圆括号的特征；
2.  列表理解生成整个列表，而生成器表达式一个接一个地生成元素。

让我们举例说明:

```
*# define a list*
my_list = [0, 1, 2, 3]*# list comprehension: triple the value for each element* my_list_comprehension = [i * 3 **for** i **in** my_list]*# generator expression: same functionality* my_generator = (i * 3 **for** i **in** my_list)**print**(my_list_comprehension)
**print**(my_generator)Output:
[0, 3, 6, 9]
<generator object <genexpr> at 0x7f8a4446e340>
```

正如预期的那样，生成器表达式生成了一个生成器对象，它将根据需要一个接一个地生成结果，就像我们前面看到的生成器函数一样。

## 发电机的优势

与其他 iterables 相比，生成器的一个很大的优势是，不像 list 那样必须在内存中存储所有元素，生成器只需要存储当前元素以及如何到达下一个元素。这使得生成器在内存消耗方面非常高效，并且它还为生成器提供了独特的优势，能够表示真正无限的数据流，特别是因为它们一次生成一个项目，这意味着在任何给定时间只有一个元素会存储在内存中。

生成器的另一个优点是，在我们开始在代码中进一步使用它们之前，我们不需要等待所有的元素都被生成(在列表的情况下，我们需要等待)。

我们应该注意到，只有当我们不打算多次使用生成的数字时，这两个优点才是显著的和真正有用的。

我们还发现它们非常容易实现，不像[迭代器](https://medium.com/@deck451/python-up-your-code-iterators-af5331f68ccd)，它们有相当多的样板代码来实现这两个`__iter__()`和`__next__()`方法。

总结这篇文章，我希望它有助于积累一些(更多)关于发电机的知识。这篇文章决不是关于这个话题的详尽的信息来源。互联网上充满了关于 Python 生成器的令人难以置信的全面教程和文档。写这篇文章时，我让自己从中获得灵感的一个这样的网页可以在这里找到。

话虽如此，请注意安全，我们在下一站[见！编码快乐！](https://medium.com/@deck451/python-up-your-code-chained-comparisons-a949778d166d)

*Deck 是软件工程师、导师、作家，有时甚至是老师。他拥有 12 年以上的软件工程经验，现在是 Python 编程语言的真正倡导者，同时他的热情是帮助人们提高他们的 Python(以及一般的编程)技能。你可以在*[*Linkedin*](https://www.linkedin.com/in/deck451/)*，* [*脸书*](https://www.facebook.com/deck451/) *，*[*Twitter*](https://twitter.com/Deck45100)*，*[*Discord*](https://discord.com)*:Deck 451 # 6188，以及跟随他写在这里的* [*Medium*](https://medium.com/@deck451)