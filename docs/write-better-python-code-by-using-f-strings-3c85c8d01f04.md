# 使用 f 字符串编写更好的 Python 代码

> 原文：<https://blog.devgenius.io/write-better-python-code-by-using-f-strings-3c85c8d01f04?source=collection_archive---------6----------------------->

![](img/ebe6b5c4f31d47943ff72bfefd33ca7d.png)

照片由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary) 在 [Unsplash](https://unsplash.com/photos/D9Zow2REm8U) 上拍摄。

在您了解 Python 中的 f 字符串之前，它们看起来可能有点奇怪。

```
print(f"Hello world, my name is {name}.")
```

这看起来有点像是打印错误之类的。那是什么`f`？它应该在那里吗？

在 Python 中，它应该在那里。

不过，上面的代码片段并不是全部。它需要更多一点:

```
name = "Trey"
print(f"Hello world, my name is {name}.")
```

啊，好吧。现在有点说得通了。`{name}`将被替换为变量`name`的值，在本例中为`Trey`。因此，输出将是`Hello world, my name is Trey.`

f 字符串是格式化文本字符串的一种方式，它代表“格式化字符串”。顾名思义，它是 Eric V. Smith 在 2015 年推出的名为文字字符串插值的字符串格式化机制的代表。

在出版的 PEP 498 中，史密斯写道:

> string 提供了一种在字符串中嵌入表达式的方法，使用了最少的语法。应该注意，f 字符串实际上是一个在运行时计算的表达式，而不是一个常数值。在 Python 源代码中，f-string 是一个文字字符串，以' f '为前缀，在大括号内包含表达式。表达式被替换为它们的值。

现在，Python 中还有一些其他的格式化方法，比如%-formatting(比如用字符串值替换`%s`)和`str.format()`，但是限制(%-formatting 只能处理 int、str 和 doubles)以及冗长等问题使得 f-string 在许多情况下更有吸引力。

所以如果你想格式化字符串，写一些更好的 Python 代码，那么我高度鼓励 f 字符串。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)