# Python Lambda

> 原文：<https://blog.devgenius.io/python-lambda-ef9cb7f24dbd?source=collection_archive---------6----------------------->

# 概观

Lambda 是一个没有名字的单行函数，它可以有多个参数，但只有一个表达式。这对于编写只运行一次的函数很有用。

![](img/6143bd1cb9f37c40b364ffacf6d3cf64.png)

[https://devclass.com/wp-content/uploads/2018/10/lambda.jpeg](https://devclass.com/wp-content/uploads/2018/10/lambda.jpeg)

# 履行

## 正常实施

```
def add(x, y):
    return x + y
```

## Lambda 实现

```
lambda a, b : a +b
```

如你所见，为了将一个函数声明为 lambda 函数，你需要使用 lambda 关键字。lambda 关键字后面是函数的参数。然后冒号符号将参数与定义分开。

# 分配

您可以通过设置一个等于 lambda 函数的变量来保存 lambda 函数。

```
adder = lambda a, b : a +b
print(adder(1+2))>> 3
```

# λ内部的λ

可以声明一个 lambda 函数，同时在其内部声明另一个 lambda 函数。

```
square_double = lambda x: (lambda y: 2 * y)( x*x )
print(square_double(3))>> 18
```

如你所见，首先执行外部 lambda 函数，然后将结果作为参数传递给作为结果的内部 lambda 函数。

# 结论

当我们只需要 a 函数一次时，我们使用 lambda 函数。这通常发生在我们使用高阶函数时，这些函数将函数作为参数，如 filter()和 map()。例如，filter()函数接受一个函数来确定要过滤的内容，然后接受一个要过滤的序列。

```
my_list = [1, 2, 3, 4, 5, 6, 7, 8]
new_list = list(filter(lambda x: ( x %2 == 0) , my_list))
print(new_list)>> [2, 4, 6, 8]
```

正如你所看到的，使用 lambda 方法允许我们只在一行代码中使用 filter 方法，而不必单独定义如何过滤列表。