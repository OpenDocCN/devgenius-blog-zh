# SML 函数式编程

> 原文：<https://blog.devgenius.io/functional-programming-with-sml-3ed085db1dba?source=collection_archive---------5----------------------->

探索用 SML 思考编程的新方法！

[![](img/c6d951b04d4c7b292c24f89f65999b56.png)](https://github.com/daminals/functional_programming_in_sml)

查看该项目的 GitHub！

# 为什么要麻烦 SML？

有一些特定的编程方式挑战着我们思考编写代码的方式。对我来说，SML 已经偏离了常规。标准元语言*(新泽西语法)*是一种完全函数式递归编程语言。一切都必须用函数来写，并递归定义。

您可能会想“我为什么要为这样的语言费心”，这是一个公平的观点。如果我说我从未有过这种想法，那我是在撒谎。

然而，这种范式转变允许我们考虑一种全新的编程方式，并因此扩展了我们的思维。它为递归编程打开了一个新的视角，并为我自己点燃了数学和计算之间的联系。

当我开始向你展示例子时，你可以使用在线编码平台【https://sosml.org/ 

# SML 的编码

## GCD 函数

让我们从最古老的算法和最简单的递归例子开始，最大公分母函数。

GCD 函数接受两个参数，A 和 B。如果 A=B，则返回 A，否则返回 GCD(A-B，B)。a 总是大于 b，在 SML 我们是这样写的！

```
fun gcd(a,b) = if a=b then a
  else if a<b then gcd(b,a)
  else gcd(a-b,b);
```

这是一个相当简单的例子，我们在 SML 定义了一个案例，并找到了一个使用 if 语句选择返回内容的解决方案。然而，通过使用 SML 的递归模式匹配功能，我们可以充分利用 SML 的全部能力，在这种情况下，我们定义一个与其他案例相结合的基础案例。我们来看一个例子！

## 阶乘

我敢肯定，当我们大多数人想到递归时，首先想到的是阶乘函数(当然仅次于斐波那契)

让我们使用阶乘函数来探索 SML 的模式匹配(定义多个案例)

```
fun factorial (0) = 1
    | factorial (n:int) = n * factorial (n-1);
```

这种语法一开始肯定令人困惑，所以让我们一行一行地解决它吧！

第 1 行:定义一个函数*阶乘*。这个函数使用 SML 的模式匹配功能。在函数的这一行中，我们将基本情况定义为 0，当到达基本情况时，我们应该返回 1。

第 2 行:定义阶乘中的第二种情况。如果给出一个非零整数 n，结果应该是 n 乘以阶乘(n-1)

我将用斐波那契数列来展示同样的概念！

## 斐波那契函数

```
fun fib (0) = 1 
    | fib (1) = 1
    | fib(n) = fib(n-1) + fib(n-2);
```

这里，我们定义了两种基本情况，fib(0) = 1 和 fib(1) = 1。这是因为斐波那契数列中的第 n 项等于 fib(n-1) + fib(n-2) *(数列中的第 n-1 项加上第 n-2 项)*。

## 最后一个功能

让我们定义一个返回列表最后一个元素的函数

```
val a = 1::[2,3,4]

fun last(x::nil) = x
  | last(x::xs) = last(xs);

last(a);
```

这可能需要很长时间才能理解，所以让我们一行一行地解决它吧！

第 1 行:在 SML 定义一个列表。它可以对列表使用普通的 python 语法——即方括号。然而，我们也可以将列表定义为 head::tail，其中 head 是列表中的一个元素，tail 是其余的元素。SML 中的所有列表都以 tail::nil 结尾，其中 nil 表示空列表(或者定义为[])

第 2 行:最后一个定义了一个函数*。我们将基本情况定义为当列表的尾部等于[]时，返回在此之前的元素。*

*由于 SML 列表被定义为 a::b::c::nil，这意味着 nil 的头元素将是最后一个元素。*

第 3 行:定义了*最后一个*的情况，其中头部和尾部都不为零。它将以 tail 作为新的头递归运行自己，直到 tail 为零。

第 4 行:运行我们在第 1 行创建的列表中的函数。

语法起初看起来很混乱，但是一旦你掌握了其中的窍门，你就释放了函数式编程的力量！下面再做几个例子，让你掌握其中的窍门！

## 附加功能

让我们试试另一个模式匹配的例子。这一次，我们将在列表后面追加一个值。

```
val a = 1::[2,3,4]

fun append(nil,value) = value::nil
    | append(head::tail, value) = head::append(tail, value);

append(a,56);
```

这个函数是如何工作的？

首先我们定义一个基本情况，如果是空列表，返回一个以值为头的列表。在我们的第二个例子中，我们将 head 值的 tail 设置为 append with tail 作为新 head 的结果。

在这种递归方法中，我们遍历列表中的每个元素，直到到达最后一个元素 nil。当我们到达最后一个元素时，我们返回我们想要的值作为头部，返回 nil 作为尾部。

## 反向功能

让我们试试更难的。这里我们将定义一个“反转”函数，它反转一个输入的列表。这个函数的算法将遍历原始列表，然后将当前元素追加到结果列表的前面。

```
val a = [1,2,3,4];
val b = nil;

fun reverse_helper(head::nil,new_list) = head::new_list
  | reverse_helper(head::tail,new_list) = reverse_helper(tail, head::new_list)

fun reverse(nil) = nil
  | reverse(list) = reverse_helper(list,nil);

reverse(a);
reverse(b);
```

让我们一行一行地理解它的作用。

第 2 行:定义了一个函数 *reverse_helper* 。这个函数使用 SML 的模式匹配功能。在函数的这一行中，我们定义了基本情况，如果列表的尾部等于[]，则返回新的列表，并将 head 作为其新的头部

第 3 行:为 *reverse_helper* 定义一个替代案例。如果 tail 不为 nil，递归运行以 tail 为新头的函数，以 head 为新头的新列表

第 4 行:定义函数 *reverse* 的基本情况，如果 list 为 nil，则返回 nil

第 5 行:定义了函数 *reverse* 的一般情况，它接受要反转的列表。它返回 *reverse_helper(list，[])* 的结果，其中[]将是反转列表的尾部。

第 6 行:运行我们在第 1 行创建的列表上的函数。

使用这种方法，我们遍历当前列表头部的每个值，并将其添加到新列表的尾部。这就是为什么我们的新列表是我们当前列表的反转版本！

# 结论

这是对 SML 函数式编程的一个简要介绍，展示了该语言的递归功能。它从更数学的角度提供了编程的视角，炫耀函数。

函数式编程的优势在于对任何事情都使用函数，并利用函数组合来完成更强大的任务。虽然在本文中我们没有涉及匿名函数或 SML 函数的组合，但这是扩展我们对该主题理解的合乎逻辑的下一步。

我们今天写的所有代码都在我的 github 上，链接如下:

[](https://github.com/daminals/functional_programming_in_sml) [## daminals/functional _ programming _ in _ SML

### github.com SML 的函数式编程](https://github.com/daminals/functional_programming_in_sml)