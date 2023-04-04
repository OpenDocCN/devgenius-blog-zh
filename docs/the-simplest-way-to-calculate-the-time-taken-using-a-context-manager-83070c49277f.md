# 使用上下文管理器计算时间的最简单方法

> 原文：<https://blog.devgenius.io/the-simplest-way-to-calculate-the-time-taken-using-a-context-manager-83070c49277f?source=collection_archive---------33----------------------->

![](img/f0f4e566cbbced8cfd25d1e42388a387.png)

来源:谷歌

有时需要计算我们的代码执行需要多少时间。有许多不同的方法可以做到这一点，但我最喜欢的是使用上下文管理器。

让我告诉你上下文管理器。上下文管理器允许您在需要时精确地分配和释放资源。上下文管理器最广泛使用的例子是`with`语句。假设您有两个相关的操作，您想成对执行，中间有一段代码。上下文管理器允许您专门做这件事。

有 3 种方法，即 __init__，__enter__，__exit__，但至少上下文管理器定义了一个`__enter__`和`__exit__`方法。让我们制作自己的文件打开上下文管理器，并创建对象计时器，如下所示:

```
**class** Timer(object):
    **def** __init__(self, file_name, method):
        self.file_obj = open(file_name, method)
    **def** __enter__(self):
        **return** self.file_obj
    **def** __exit__(self, type, value, traceback):
        self.file_obj.close()
```

仅仅通过定义`__enter__`和`__exit__`方法，我们就可以在`with`语句中使用我们的新类。现在我们准备将它应用到我们的代码中。

在这段代码之后，只需调用这个函数下面的整个代码“With”语句。因此，用我们定义的语句和定时器对象启动程序，如下所示:

## 带计时器(“所用时间”):

举个例子，我写了一段简单的 python 代码来打印两个数的相加。

## num1 = 45 #第一个数
num2 = 20#第二个数
#相加两个数
sum = num1 + num2
#打印值
print({ 0 }和{1}之和为{2})。格式(num1，num2，sum))

```
Sum of 45 and 20 is 65
Time Taken: 0.00022077560424804688
```

所以我们打印了结果，并且我们的上下文管理器正在计算花费的时间。

应用上下文管理器并检查代码的执行情况。

使用上下文管理器的好处是锁定和解锁资源以及关闭打开的文件。