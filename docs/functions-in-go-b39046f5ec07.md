# GO 中的功能

> 原文：<https://blog.devgenius.io/functions-in-go-b39046f5ec07?source=collection_archive---------6----------------------->

![](img/5b51b9e17c4737727fc02910059f39ef.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Akhil Chandran](https://unsplash.com/@akhiltchandran?utm_source=medium&utm_medium=referral) 拍摄

我正跟着《T4》这本介绍围棋的书学习这门语言。这本书似乎具备了开始学习这种新语言的一切条件，但也有一些作者希望读者自己探索的细节。

一个这样的主题是 Go 函数。在这篇文章中，我试图解释我对这个话题的理解。

让我们从函数签名开始。函数签名包含参数和返回类型。

```
func function_name(parameter list with type)(return list with type)
```

上面一行中需要注意的几点是:

1.  函数声明总是以 **func** 关键字开始，后跟函数名
2.  函数名后面是一个类型参数列表。例如

```
func myFunc(a int, b string, c bool)
```

如果您不需要任何参数，请不要在括号中放置任何内容。

3.函数签名中的最后一对括号包含一个返回类型列表。是的，在 Go 中，我们可以返回多个值。例如

```
func myFunc(a int, b int)(int, int){
    return a + b, a - b
}
```

要返回多个值，这些值应该用逗号**分隔。如果你只有一个返回类型，你可以不用括号来说明。如果你不需要从函数中返回任何东西，你可以跳过最后一对括号。两种情况见下文。**

```
func myFunc(a int, b int) int{ // returning only int type
    return a + b
}func myFunc(a int, b int){ // returning no type
    fmt.Println(a, b)
}
```

Go 还支持命名返回类型。有趣不是吗？因此，除了多个返回值，您还可以使用函数签名提前命名您的返回值。

```
func myFunc(a int, b int) (sum int){
    sum = a + b
    return
}
```

我们来分析一下上面的代码。在最后一组括号中，我放了 **sum** ，这是函数返回值的名称。因此，加法的结果将被赋给总和**和**，当我们返回时，总和**和**的值将被返回。你也可以观察一下，我没有提到 **sum** 在 **return** 关键字之后。这是因为对于命名的返回类型，在 Go 中提及返回值名称是可选的。这种回归也被称为“裸回归”

让我用多个命名返回类型来更详细地解释这一点。

```
func myFuncVersion1(a int, b int)(c int, d int, e int){
    c = a + b
    d = a - b
    e = a * b
    return
}func myFuncVersion2(a int, b int)(c int, d int, e int){
    c = a + b
    d = a - b
    e = a * b
    return e, c, d
}
```

上面两个函数做同样的事情，但是返回值的顺序不同。在 myFuncVersion1 中，由于在 return 关键字后没有提到订单返回类型名称，所以返回值的顺序与函数签名中定义的顺序相同。

所以，如果我们用 a = 3，b = 7 调用 myFuncVersion1，我们将得到 10，-4 和 21 作为返回值。但是，对于 myFuncVersion2，返回值的顺序将遵循 return 关键字后面提到的返回类型名称的顺序。因此，返回值将是 21，10，-4。

在编写具有多个命名返回类型的函数时，还有两个要点需要注意。

1.  命名的返回类型只能出现在返回列表的末尾。不能将命名返回类型后跟匿名返回类型。
2.  不能在参数列表和返回列表中声明相同的名称。

下面提到的两个函数签名都是错误的。

```
func myFunc(a int, b int)(c int, int){ // last return type not named
    return 0, 0
}func myFunc(a int, b int)(a int){ // "a" used in both list
    return 0
}
```

希望这篇帖子能帮助你对如何在 Go 中写函数有一个正确的认识。我将用几个有效的函数签名来结束这篇文章。

```
func myFunc(){}
func myFunc(a int){}
func myFunc(a int, b int){}
func myFunc() int{}
func myFunc()(int, int){}
func myFunc(a int, b int)(int, int){}
func myFunc(a int, b int)(c int){}
func myFunc(a int, b int)(c int, d int){}
func myFunc(a int, b int)(int, c int){}
```