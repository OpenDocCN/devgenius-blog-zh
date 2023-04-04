# # 9 for loops——Python 适合初学者

> 原文：<https://blog.devgenius.io/9-for-loops-python-for-beginners-cb0bbe8f5f8b?source=collection_archive---------12----------------------->

## 尽可能频繁地运行循环(x 次)

酷，我们现在可以检查一个条件并运行一次代码块，或者检查其他条件，如果没有条件被评估为真，则设置“默认”代码块(if-elif-else-statement)，并且只要给定的条件为真，我们就可以运行代码块(while-loop)。现在我们开始学习如何在给定的回合数内运行代码块。我们通过使用一个 *for 循环*来做到这一点。

![](img/2bf2529bece3b439a17e10d2de3276ff.png)

照片由[简·kopřiva](https://www.pexels.com/@koprivakart?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/photo-of-a-red-snake-3280908/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

## 什么是 for 循环？

**就像引言中提到的，for 循环给了我们运行代码块给定次数的机会(我们可以设置)。这通过使用 *range()* 函数来实现。在下一部分，我将向你解释 for 循环的语法和用法。**

## for 循环语法

for-loop 的语法是这样的:关键字 *for* ，一个*变量* (name)，关键字 in*，函数 *range()* 的调用(可以给这个函数 3 个整数)，一个*冒号*，一个*缩进代码块*。像这样:*

```
for x in range(10):
  print(x)

# OUTPUT: 
#0
#1
#2
#3
#4
#5
#6
#7
#8
#9
```

老实说，你也可以用一个简单的 while 循环来解决这个问题。看看这个例子:

```
x = 0
while x < 10:
    print(x)
    x += 1

for y in range(10):
    print(y)

# Gives us both the same output (numbers from 0 to 9)
```

但是就像你在上面的例子中看到的，for 循环**比 while 循环**短，并且**更容易使用**。此外，**我们稍后将使用 for-loop for*list****(一个我们稍后会了解的数据类型)。*

## *关键字范围()*

*我们可以用**一个**、**两个**或者**三个自变量**来调用 *range()* 函数。在上面的例子中，我们使用了带有一个参数的调用。*

***一个参数:**如果我们使用一个参数，它将设置 for 循环代码块将停止的数字**(不将给定的数字带入迭代)。所以下面的例子给了我们 0，1，2，3，4。***

```
*for x in range(5):
    print(x)*
```

***两个参数:**如果我们使用两个参数，**第一个数字会设置条目编号**(默认编号为 0)(“起始编号”)。**第二个数字设置 for 循环代码块将停止**的数字(不考虑迭代中给定的数字)。所以下面的例子给了我们 3，4。*

```
*for x in range(3, 5):
    print(x)*
```

***三个自变量:** **如果我们用三个自变量，前两个数做的事情就像用两个自变量一样。第三个数字设置 for 循环的步长。**因此，例 2 让 for 循环每隔一次迭代就跳过一次。所以下面的例子给了我们 2，4，6。*

```
*for x in range(2, 7, 2):
    print(x)*
```

*完美！您现在知道了如何对给定迭代次数使用 for 循环。但是在我们结束这节课之前，让我给你一些其他有用的东西。*

## *在 for 循环中使用 continue 和 break*

*在 for 循环中使用 continue 使程序跳转到 for 循环的下一次迭代。break 使程序跳出循环。很简单，对吧？*

> *要点:只能在 while 循环或 for 循环中使用 continue 和 break。否则 Python 会给你一个错误。*

*for 循环可能很复杂，所以要花时间去理解它们。有了 for 循环，我们可以在以后做一些疯狂而有用的事情，所以要真正学习和理解它们。*

*和往常一样，如果你有任何关于 Python 或编码的问题，请在下面的评论中提出。*

***直到那时！***

**l0ckD2wN**