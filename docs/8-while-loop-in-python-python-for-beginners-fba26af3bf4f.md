# # 8 Python 中的 while-loop——面向初学者的 Python

> 原文：<https://blog.devgenius.io/8-while-loop-in-python-python-for-beginners-fba26af3bf4f?source=collection_archive---------14----------------------->

## 只要条件为真，就运行代码

在这篇文章中，我将向你展示 if-elif-else-statement 之后的下一步。 **While-loops。**走吧！

![](img/eedc961f44e8e74e65cff44a15143ac1.png)

照片由[简·kopřiva](https://www.pexels.com/@koprivakart?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/photo-of-a-red-snake-3280908/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

## 什么是 while 循环？

While 循环没有那么复杂。I**n 代替运行一次的 if 语句(其中的一段代码),只要条件被评估为真，它就会运行。**它的结构和 if 语句一样:**关键字 while，条件(评估为 True 或 False)，冒号和缩进代码块**。

```
while condition == True:
  # run code

# Short from
while condition: # We do not need the == True, just do it like this
  # run code

# This code is not able to run, it is just an explanation
```

**检查条件后，如果条件评估为真，则运行预期代码。运行代码后，它再次跳转并再次检查条件，如果条件被评估为真，则运行预期的代码。只要给定的条件为真，我们就有机会运行代码。**

## if 语句与 while 循环

```
# if-statement
x = 0
if x < 5:
  print('x is smaller than 5.') # Gets printed out 1 time
  x += 1 # Short form for x = x + 1 (so 1 gets added to x (x=5, x+1=6))

# while-loop
y = 0
while y < 5:
  print('y is smaller than 5.') # Gets printed out 5 times
  y += 1 # Short form again
```

你看出区别了，对吧？

## while 循环的问题(一个大警告)

**如果 while 循环中的某个条件总是被赋值为 True，那么就会出现一个不定式循环，这是不好的。所以请记住，在整个 while 循环中，始终要有一个变化的条件，这样 while 循环才能中断。**

```
while True:
  print('Hello World')
# This would give us an infinitive Hello World output - do not do that!
```

这可能看起来像很多事情，但回过头来，我们已经知道什么条件是如此，而循环并不复杂。与只运行一次的 if 语句(其中的一段代码)不同，只要条件的计算结果为 True，就会运行 if 语句。

和往常一样，如果你有任何关于 Python 或编码的问题，请在下面的评论中提出。

直到那时！

*l0ckD2wN*