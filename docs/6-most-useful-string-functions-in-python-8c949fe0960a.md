# Python 中最有用的 6 个字符串函数！

> 原文：<https://blog.devgenius.io/6-most-useful-string-functions-in-python-8c949fe0960a?source=collection_archive---------19----------------------->

## 如果你是一个初学者，你需要知道这些字符串函数

![](img/543a03db0cdea3c3545dd7de599b0960.png)

由[凯利·西克玛](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/working-women?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

Python 是一种强大的编程语言。语言本身内置了许多辅助函数。

今天我将写一些字符串函数，它们对初学者来说非常有用。

我们今天要看的函数是

*   len()
*   上部()
*   下部()
*   查找()
*   替换()
*   在()

# #1:获得长度

假设你想计算一个字符串中的字符数。意味着你得到了一个数值作为答案。这个函数就是这样做的

```
name = 'My name is Akhi'result = name.len()print(result)# result will be 15
```

可以手动计数，检查数值。需要记住的一点是，空格在这里也算作一个字符。

# #2:转换成大写

当你想将所有字符都大写时，你需要使用这个`upper()`函数。请记住，写完大写字母后，请将左右括号放在一起。

```
name = 'My name is Akhi'result = name.upper()print(result)# result will be -> "MY NAME IS AKHI"
```

# #3:做相反的事情！

这个`lower()`函数看起来很像大写的。如果你想让所有的字符都是小写，就用同样的方法。

```
name = 'My name is Akhi'result = name.lower()print(result)# result will be -> "my name is akhi"
```

# #4:在字符串中查找字符

如果你想知道字符串中任意字符的位置，函数`find()`非常有用。

但是作为初学者，你必须记住，弦上的位置是从零开始的

```
name = 'My name is Akhi'
print(name.find('A'))
```

我跑去找上面的字‘A’。结果是 11。

另一件事是，如果在你的台词中多次出现同一个角色。find 函数将只从第一个开始计数。

假设“我的名字是 Akhi ”,其中有两个较低的“I”字符。一个在第 8 位，另一个在第 14 位。

如果您在这种情况下使用 find 函数，您将得到 8。这意味着你将从第一个开始。

# #5:替换功能

如果我们需要改变字符串中的一个字符。替换功能非常有用。当我们需要纠正任何单词或值或任何字符时，它可以节省我们的时间。这里给出了语法；

```
name = 'My name is Akhi'result = name.replace('My','Her')print(result)# result will be -> ‘Her name is Akhi’
```

# #6:在功能上

如果你想知道一个特定的字符串是否存在于字符串中，那么`in()`函数将为你保存这一天！

您可以简单地使用 in 函数。它会给出真值或假值。语法如下所示:

```
name = 'My name is Akhi'
print('akhi' in name)# it will print -> False
```

我的结果是“假的”。因为在我的句子中我有 akhi 而不是 Akhi。所以要小心这里的字母大小写。

如果你像我一样是数据爱好者，希望有一天成为数据分析师，那么这里有另一个适合你的。

[](/beginner-roadmap-to-become-a-successful-data-analyst-f864703844f5) [## 成为成功数据分析师的初学者路线图

### 探索这个非常需要的工作领域

blog.devgenius.io](/beginner-roadmap-to-become-a-successful-data-analyst-f864703844f5)