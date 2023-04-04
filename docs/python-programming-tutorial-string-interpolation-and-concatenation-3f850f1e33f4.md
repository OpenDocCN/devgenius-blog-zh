# Python 编程教程:字符串插值和连接

> 原文：<https://blog.devgenius.io/python-programming-tutorial-string-interpolation-and-concatenation-3f850f1e33f4?source=collection_archive---------3----------------------->

![](img/1fbf9924cc80ecc9690b616c47c6d826.png)

摄影:塔拉·伊万斯在 unsplash.com

每个编写代码的人都与字符串打交道，不管他们是资深程序员还是新程序员。

> 本文是我的 [**Python 编程教程**](https://arc-sosangyo.medium.com/list/introduction-to-python-programming-80e79264dcad) 系列的一部分。

# 什么是字符串插值

如果你不熟悉字符串插值，这个主题可能听起来很华丽。不过不用担心，没你想的那么复杂。字符串插值就是在字符串内部插入变量的过程。如果需要用相应的值替换占位符，这很有用。

基本上，python 支持 3 种字符串插值方法。

1.  **%操作员**
2.  **f 弦**
3.  **str.format()**

## 1.%运算符

进行字符串插值的默认方式是使用 python 中已经内置的 **%操作符**。这是在 python 中插入字符串的一种老方法。

例如

```
capital = 'Manila'print('The capital of Philippines is %s' % capital)
```

输出将是:

```
The capital of Philippines is Manila
```

你也可以像这样插入多个插值。

```
country = 'Philippines'
capital = 'Manila'print('The capital of %s is %s' % (country, capital))
```

输出将是:

```
The capital of Philippines is Manila
```

## 2.f 弦

如果您使用的是 python 3.6，您可以使用文字字符串插值来代替。f 字符串可以通过在你的字符串的开头写 **f** 来声明。使用此方法使您的代码更具可读性。

例如

```
country = 'Philippines'
capital = 'Manila'print(*f*'The capital of {country} is {capital}')
```

输出将是:

```
The capital of Philippines is Manila
```

## 3.str.format()

该方法是更为复杂代码定制的字符串插值方式。

例如

```
sampleCountry = 'The capital of {country} is {capital}'.format(*country* = 'Philippines', *capital* = 'Manila')print(sampleCountry)
```

输出将是:

```
The capital of Philippines is Manila
```

这是另一个使用 str.format 的语法示例，其输出与上面的相同。

```
sampleCountry = 'The capital of {country} is {capital}'print(sampleCountry.format(*country* = 'Philippines', *capital* = 'Manila'))
```

# 什么是字符串连接

字符串连接是将字符串连接在一起。

例如

```
hello = "Hello"
world = "World"
conStr = hello + worldprint(conStr)
```

输出将是:

```
HelloWorld
```

下面是连接字符串的另一个示例:

```
hello = "Hello"
world = "World"print(hello + " " + world)
```

输出将是:

```
Hello World
```

**+=** 操作符也可用于将字符串添加到现有的字符串变量中。

```
conStr = "Hello"
conStr += "World"print(conStr)
```

输出将是:

```
HelloWorld
```

在我们的下一个教程中，我们将处理大约[个基本操作符](https://arc-sosangyo.medium.com/python-programming-tutorial-basic-operators-d34693dc4404)。

愿法典与你同在，

*   弧

*更多内容请看*[*blog . dev genius . io*](http://blog.devgenius.io)*。*