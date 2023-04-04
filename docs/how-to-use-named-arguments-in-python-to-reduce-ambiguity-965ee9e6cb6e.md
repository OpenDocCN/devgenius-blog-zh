# 如何在 Python 中使用命名参数来减少歧义

> 原文：<https://blog.devgenius.io/how-to-use-named-arguments-in-python-to-reduce-ambiguity-965ee9e6cb6e?source=collection_archive---------13----------------------->

![](img/3844b504f9c52a0b8db9482ccf500ffb.png)

在 Python 中，假设您有一个类似如下的函数:

```
def subtract(a, b):
   return a - b
```

它的作用是接受两个参数，`a`和`b`。

然后它返回两个参数的差，因此得名`substract()`。

这很好，老实说，这是一个非常简单的函数。但烤成有可能模棱两可。听我说。让我们看看这段代码:

```
def subtract(a, b):
   return a - bprint(subtract(3, 2))
```

输出应该是什么？

我会留下这张尖叫鸵鸟的照片，让你在脑子里思考答案。

![](img/c0eb83d06092b2e34e86178665baa146.png)

好的，明白了吗？

是的，输出应该是`1`。毕竟函数只是从`a` = `3 — 2` = `1`中减去`b`。

问题是，从技术上来说，您可能会感到困惑，而应该编写这样的代码:

```
def subtract(a, b):
   return a - bprint(subtract(2, 3))
```

在这种情况下，输出将完全不同。

会是什么呢？这是另一张鸵鸟的照片——这一次，它看起来非常可疑。

![](img/a3ca15b896f36c450d79f01c0ac12412.png)

好了，你想好输出了吗？现在是`-1`。

现在我知道你在说什么了——怎么会有人把这些混在一起呢？

这恰好是一个非常简单的函数，也是一个简单的例子来说明问题。事实是，对于更复杂和抽象的函数，当您为它们提供值时，参数可能会变得不明确。

Python 对此有一个答案，它被称为**命名参数(或命名参数或关键字参数或关键字参数)**。

基本上，你可以像这样明确地标注你的论点:

```
def subtract(a, b):
   return a - bprint(subtract(a=3, b=2))
```

Python 将能够编译并返回`1`的输出。

这里的关键是命名的参数会让你很清楚函数的参数是什么。想象一下，你正在调用这样的函数，你试图输出一个篮球运动员基于某些论点进入 NBA 的可能性:

```
compute_player_likelihood_making_nba(210, 230, 240)
```

……那些`210`、`230`、`240`的论点到底是什么？除了写这本书的人谁知道呢？你肯定应该找到函数定义来查看那里的参数名，但是如果这里调用函数的代码已经有了这些参数，不是很好吗？

让我们添加命名参数，看看它是什么样子的。

```
compute_player_likelihood_making_nba(height_cm=210, wingspan_cm=230, weight_lbs=240)
```

好多了！现在，当我们看到这个函数调用时，参数是什么就更加清楚了。

命名参数是编写可读代码的有用工具，知道它是 Python 中的一个可用特性就已经成功了一半。战斗的另一半正在使用它，在这一点上我相信你。

现在，你完成这篇文章的奖励是:一只困倦的鸵鸟。

![](img/a589e769fcbb32fea1124b7627a24782.png)

如果你觉得这篇文章很有帮助或者只是喜欢阅读它，可以考虑[注册成为一名媒体会员](https://tremaineeto.medium.com/membership)。每月 5 美元，你可以无限制地阅读媒体上关于软件、技术等主题的报道。如果你[用我的链接](https://tremaineeto.medium.com/membership)注册，我会得到一小笔佣金。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入 Medium—Tremaine Eto

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)