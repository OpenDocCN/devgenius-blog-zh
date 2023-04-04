# Python Up 您的代码:身份与平等

> 原文：<https://blog.devgenius.io/python-up-your-code-identity-vs-equality-ab2205a6fec8?source=collection_archive---------15----------------------->

## Python 中“==”和“is”运算符的区别

![](img/569e9fd8b44013a7098d9bec2027b602.png)

照片由[最大焦点](https://unsplash.com/@maximalfocus?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[不飞溅](https://unsplash.com/s/photos/digital-abstract?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上拍摄

这应该是一篇很短的文章，主题是我认为我们迟早都会遇到的(最好是越早越好)。尽管这个话题可能很简单，但深入探究它会让你大开眼界。所以，让我们开始吧！

在 Python 中(和其他一些语言一样，但我们在这里主要关注 Python)，在像`==`或`!=`这样的**等式**操作符和像`is`和`is not`这样的**等式**操作符之间有一些不同。区别决定了这两个类别的不同用例。但是不知何故困惑开始了。因为在一个非常特殊但又非常普遍的例子中，这些运算符实际上表现相同。

但是让我们举例说明，以便更好地理解正在发生的事情。

```
*# 2 equal integers*
a = 2
b = 2*# test for equality* **print**(a == b)*# test for identity* **print**(a **is** b)Output:
True
True
```

至少可以说，很有趣。现在考虑下一个例子:

```
*# 2 equal lists*
l1 = [1, 2, 3]
l2 = [1, 2, 3]*# test for equality* **print**(l1 == l2)*# test for identity* **print**(l1 is l2)Output:
True
False
```

这一次，我们可以确定列表是相等的，但是`is`操作符返回了`False`。

让我们再看一下第一个例子，但这一次，让我们更彻底一点:

```
*# 2 equal integers*
a = 2
b = 2*# test for equality* **print**(a == b)*# test for identity* **print**(a **is** b)*# print out the object ids* **print**(**id**(a))
**print**(**id**(b))Output:
True
True
140495050653968
140495050653968
```

这个例子中的`==`和`is`操作符都返回`True`。返回`True`的等式运算符应该不会让任何人感到惊讶，真的。但是有趣的是`is`操作符也返回`True`。注意`id()`函数调用是如何返回相同的对象 id 的？好了，现在我们可以清楚地了解情况了。

原来，`is`操作符是在测试这两个变量是否指向 Python 内存中的同一个对象。

*   在第一行之后，在`a = 2`处，Python 创建了一个整数值为 2 的对象，并将其赋给我们的变量`a`；
*   在第二行，我们有了`b = 2`。所以，这里发生的是，Python 也将变量`b`赋给了`a`正在引用的同一个对象，因为整数**是不可变的**，所以**有机会**，取决于你的 Python **实现**，将使用同一个整数对象(你可以在这里阅读关于这个微妙主题的更多内容[)；我们现在看到的叫做**共享引用**，你可以在](https://docs.python.org/3/reference/datamodel.html#objects-values-and-types)[这里](https://www.geeksforgeeks.org/shared-reference-in-python/)读到更多；

底线是，这两个变量，在这个特殊的例子中(我使用的是`CPython`实现)，最终引用了同一个 integer 对象。这就是`a is b`回归`True`的原因。我们也可以把`a is b`线看作`id(a) == id(b)`的简写。

而且，虽然我们已经谈到了 Python 实现主题，但您可能对获得 Python 实现有点好奇。方法如下:

```
**import** platform
platform.**python_implementation**()
```

现在，回到手头的任务，让我们对两个列表进行同样的尝试:

```
*# 2 equal lists*
l1 = [1, 2, 3]
l2 = [1, 2, 3]*# test for equality* **print**(l1 == l2)*# test for identity* **print**(l1 is l2)*# print out the object ids* **print**(**id**(l1))
**print**(**id**(l2))Output:
True
False
139711984251904
139711982769344
```

列表是可变的，所以即使我们创建了两个值相等的列表，Python 也创建了两个独立的、不同的对象。`is`操作符将检查两个列表的 id 是否相等，并将自然返回`False`。

不得不说，这同样适用于`!=`对`is not`的情况。它将产生非常相似的结果。

官方 Python [PEP 8 风格指南](https://peps.python.org/pep-0008/#programming-recommendations)对这个主题有一些建议。它们提出了非常好的观点，绝对不仅值得一读，而且值得实际应用，因为它们可以防止严重错误的发生:

> 应该总是使用`is`或`is not`来比较 singletons，比如 None，而不是等式运算符。
> 
> 此外，当你真正想要的是`if x is not None`时，注意不要写`if x`——例如，当测试一个缺省值为 None 的变量或参数是否被设置为其他值时。另一个值的类型(比如容器)在布尔上下文中可能为 false！
> 
> 使用`is not`操作符而不是`not ... is`。虽然这两种表达式在功能上完全相同，但前者更具可读性，也更受青睐:
> 
> #正确:如果 foo 不是 None:
> #错误:如果不是 foo 就是 None:

第一点强调，我们应该总是用`is`来比较一个变量到`None`而不是`==`。为什么？嗯，我能想到几个原因:

*   `None`是独生子。只能有一个`None`实例。将变量的值与`None`对象的值进行比较是没有意义的，因为我们真正想做的是测试变量`is``None`；
*   通过简单地覆盖`__eq__()`方法，可以在一些定制类中覆盖`==`操作符，并且`my_var == None`可能不会返回我们期望的结果。另一方面，`is`操作符不能(这么容易)被覆盖；
*   `is`通常比`==`快。它只是检查有问题的操作数的`id()`返回值。

第二个建议是我们编写类似于`if variable`的东西，并期望它像`if variable is not None`一样工作。编写更短的代码行可能会使它看起来更整洁一些，但这肯定是不正确的。这样来看:如果我们的变量是`0`会怎么样？看到问题了吗？在布尔上下文中，`None`并不是唯一计算结果为`False`的东西。还有`False`、`0`，一个空字符串等等。这些都能算到`False`而`if variable`会算到`False`，尽管都不是`None`。

第三个建议非常重视可读性，当然，当你看两行代码时，你会选择哪一行？

该结束了。我希望我设法解释了为什么了解您的操作员可以将您从无休止的寻找奇怪的错误中解救出来。我们都应该努力变得比昨天的自己更好。我们以后会感谢自己的。为此，我们在下一次的[再见！在此之前，保持安全和快乐的编码！干杯！](https://medium.com/@deck451/python-up-your-code-files-fac7e07cd34f)

*Deck 是软件工程师、导师、作家，有时甚至是老师。他拥有 12 年以上的软件工程经验，现在是 Python 编程语言的真正倡导者，同时他的热情是帮助人们提高他们的 Python(以及一般的编程)技能。你可以在* [*【领英】*](https://www.linkedin.com/in/deck451/)*[*【脸书】*](https://www.facebook.com/deck451/)*[*推特*](https://twitter.com/Deck45100)*[*不和*](https://discord.com) *: Deck451#6188，以及跟随他写在这里的* [*中*](https://medium.com/@deck451)***