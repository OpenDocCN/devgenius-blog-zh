# 什么装修工在平原的话| Python

> 原文：<https://blog.devgenius.io/what-the-decorators-in-plain-words-python-b600623ea497?source=collection_archive---------0----------------------->

![](img/3c542ad1d9f0074d82026269a505b315.png)

装饰器是 Python 中非常强大和有用的工具，因为它允许程序员**修改函数** **或类**的行为。装饰者允许我们包装另一个函数，以便扩展包装函数的行为，**而不需要对原始函数源代码做任何修改**。

## 内容计划:

1.  [*学习装饰师的必备条件*](#37ef) *。*
2.  [*变回装修工*](#d474) *。*
3.  [*带参数*](#3b6c) *的装修功能。*
4.  [](#f275)**连锁装修工。**
5.  *将参数传递给装饰者。*
6.  *[*调试装修工*](#4806)*
7.  *[*结论*](#6720) *。**

*![](img/ab5a3a6f9331fcee1af3a7ad68983a28.png)*

# *学习装饰者的先决条件*

*要理解 decorators，你首先要了解 Python 的一些基础知识。*

*你应该对 Python 中的**一切**(甚至是类)**都是对象**这一事实感到舒服。我们定义的名称只是与这些对象相关的标识符。函数不是豁免，它们也是对象(包括属性)。同一个函数对象可以有不同的相关名称。*

***输出:***

```
*Hello
Hello*
```

*当您执行代码时，两个函数返回相同的输出。在这种情况下，`original`和`also_original`名称指的是同一个功能对象。*

*事情越来越奇怪了。*

*函数可以作为参数传递给另一个函数。*

*如果你用过 Python 中的`map()`、`filter()`、`reduce()`等函数，那么你就已经知道这个了。*

*这些以其他函数为自变量的函数也称为高阶函数。这方面的一个例子如下:*

***输出:***

```
*11
9*
```

*此外，一个函数可以返回另一个函数:*

***输出:***

```
*Nested local function*
```

*这里，`is_returned()`是一个嵌套的局部函数，它在我们每次调用`is_called()`时被定义并返回。*

*![](img/d760a3cdf0cf6521786cf2a53f5c107b.png)*

# *回到装饰者*

*函数和方法被称为**可调用**，因为它们可以被调用。*

*实际上，任何实现特殊的`__call__()`方法的对象都被称为 callable。因此，在最基本的意义上，装饰器是一个可调用的对象，并返回一个可调用的对象。*

*简而言之，装饰者接受一个函数，添加一些功能并返回它:*

***输出:***

```
*# say_hi()
Hello!# say_hi_ask_question()
I got decorated
Hello!
What`s up?*
```

*函数`say_hi()`已经被修饰，返回的函数被命名为`say_hi_ask_question()`*

*您可以看到 decorator 函数向`say_hi()`函数添加了一些新的附加特性。这类似于包装礼物。那就像包装礼物一样。装饰工作就像一个包装。被装饰的物体的性质(里面实际的礼物)保持不变。但现在，它看起来很漂亮(因为它已经被装饰)。*

*通常，我们修饰一个函数，并将其重新赋值为:*

```
*say_hi_ask_question = ask_question(say_hi)*
```

*这是一种常见的结构，因此 Python 有一种语法来简化它。*

*您可以使用带有装饰函数名称的`@`符号，并将它放在要装饰的函数的定义之上。例如:*

*相当于:*

```
*say_hi_ask_question = ask_question(say_hi)*
```

*这只是实现 decorators 的语法糖。*

# *用参数修饰函数*

*上面的装饰很简单，只适用于没有参数的函数。如果您的函数采用如下参数，会发生什么情况:*

*该功能有两个参数，`a`和`b`。我们知道如果我们将`b`作为`0`传入，它会给出一个错误。*

***输出:***

```
*0.4Traceback (most recent call last):
...
ZeroDivisionError: division by zero*
```

*让我们编写一个装饰器来检查将导致错误的情况:*

*如果出现错误情况，这个新工具将返回`None`。*

***输出:***

```
*I am going to divide 2 and 5
0.4

I am going to divide 2 and 0
Whoops! cannot divide*
```

*这允许您修饰接受参数的函数。*

*细心的观察者会注意到装饰器中嵌套的`wrapper()`函数的参数和它所装饰的函数的参数是一样的。考虑到这一点，我们现在可以编写通用的 decorators 来处理任意数量的参数。*

*在 Python 中，这种魔术是作为`function(*args, **kwargs)`来完成的。这样，`args`将是位置参数的元组，`kwargs`将是关键字参数的字典。这种装饰器的一个例子是:*

# *链接装饰者*

*在 Python 中，多个装饰者可以链接在一起。*

*这意味着一个**函数可以用不同的(或相同的)装饰者装饰几次**。只需将装饰器放在所需功能之上:*

***输出:***

```
*******************************
------------------------------
There are symbols around me
------------------------------
*******************************
```

*上面的语法为:*

```
*@star
@line
def print_msg(msg):
    print(msg)*
```

*相当于:*

```
*def print_msg(msg):
    print(msg) print_msg = star(line(print_msg))*
```

***我们链接装饰者**的顺序**很重要**。如果您将顺序颠倒为:*

```
*@line
@star
def print_msg(msg):
    print(msg)*
```

***输出将是:***

```
*------------------------------
******************************
There are symbols around me
******************************
------------------------------*
```

# *将参数传递给装饰者*

*让我们谈谈如何**将参数传递给装饰器本身**。为了实现这一点，我们定义了一个接受参数的装饰器，然后在其中定义了一个装饰器。接下来，我们像前面一样在装饰器中定义一个包装函数。*

***输出:***

```
*The wrapper can access all the variables
	- from the decorator maker: Hello this is an argument of the decorator
	- from the function call: Hi this is argument of the function
and pass them to the decorated function

This is the decorated function and it only knows about its arguments:
Hi this is argument of the function*
```

*如果去掉`@decorator_maker_with_arguments("Hello", "this is an argument of decorator")`*

***输出将是:***

```
*This is the decorated function and it only knows about its arguments:
Hi this is an argument of the function*
```

*![](img/efa860e8f529091bf2e948eaca73e376.png)*

# *调试装饰器*

*正如我们已经注意到的，装饰者包装函数。原始函数名、它的 docstring 和参数列表**都被封装器**隐藏了。例如，当我们试图访问`decorated_function_with_arguments`元数据时，我们将看到包装器关闭元数据。这给调试带来了挑战。*

```
*decorated_function_with_arguments.__name__'wrapper'decorated_function_with_arguments.__doc__'This is the wrapper function'*
```

*为了解决这个挑战，Python 提供了一个`functools.wraps`装饰器。这个装饰器将丢失的元数据从未装饰的函数复制到装饰的结束函数中。让我们展示一下如何做到这一点。*

***输出将是:***

```
*'HELLO THERE'*
```

*当我们检查`say_hi()`元数据时，我们注意到它现在引用的是函数的元数据，而不是包装器的元数据。*

```
*say_hi.__name__'say_hi'say_hi.__doc__'This will say hi'*
```

***任何时候都建议用** `functools.wraps`来定义装修工。这将有助于您在调试过程中避免头痛。*

*![](img/e7643a2f1c1d2677d33a8a33c8716572.png)*

# *结论*

*装饰器是一种优雅的方式，可以在不修改源代码的情况下扩展原始函数的功能。此外，您定义的 decorators 可以接受参数或者恢复到一组预定义的默认参数。这篇文章介绍了装饰者的基本知识，以及如何将它们整合到我们的功能设计中。*

**附言:如果你喜欢这篇文章，* [*关注我*](https://medium.com/@vlad.bashtannyk) ，*点击“拍手”几下，* *留下反馈。祝你好运，高效编程！谢谢大家！**

*[*LinkedIn*](https://www.linkedin.com/in/vladyslav-bashtannyk/)*——*[*Twitter*](https://twitter.com/VladyslavBasht2)*