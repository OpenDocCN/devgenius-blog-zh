# 有两种方法可以立即改进您的 Python 代码

> 原文：<https://blog.devgenius.io/two-ways-to-immediately-improve-your-python-code-a249b0250c22?source=collection_archive---------11----------------------->

# 即使是顶级开发人员也会犯的两个最常见的错误

在所有关于人工智能的讨论中，你有没有想过好的人工智能技术可以如何帮助你发展成为一名软件工程师？将软件分析工具应用到你的代码中看起来很棘手，但是它变得越来越容易了。

我[将 LGTM](/your-github-code-could-be-better-c77583e94e79) 应用于一些公共代码库来识别常见错误。

请看下面的两篇文章，找到立即改进代码的方法。

# 精确断言

对于测试，Python 有全系列的资产，比如 assertIn、assertGreater 和 assertIsNotNone。作为开发人员，坚持使用 assertTrue 或 assertFalse 是很常见的。然而，这些断言不会提供描述性消息。哪个对你来说更容易理解？

```
AssertionError: False is not true : Not found.
// Or...
AssertionError: 'message' not found in 'Custom string' : Not found.
```

![](img/719557909ebde739a06d2aee6a857ea2.png)

由[达里娅·内布里亚希娜](https://unsplash.com/@epicantus?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

当然，您可以添加定制的消息来提供更多的信息，但是如果内置函数已经为您完成了这些工作，那就更有帮助了。

所以当你写测试的时候，你可以在任何时候使用 assertTrue 或者 assertFalse 的时候仔细考虑。有没有一个更精确的断言可以让你省去以后调试的麻烦？

# 重点进口

看看典型的 Python 程序，你会发现**大量的导入**。我们经常添加到这个列表中，但是我们多久从这个列表中删除一次呢？我们可能删除了最后一个 NumPy 用法，但是我们删除了导入吗？

![](img/a74b59ee7ae548f14560c899a5b5b399.png)

照片由[罗斯博格رز باکس](https://unsplash.com/@rosebox?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

如果我们想避免程序变得臃肿，在更新代码时考虑什么时候可以删除导入是有意义的。这可能是一个更适合静态分析器的功能，因为即使是最勤奋的工程师也可能会错过一些未使用的导入。也就是说，关注它可以减少我们程序的大小。

同样，只进口我们需要的东西至关重要。许多 Python 程序会导入所有内容(“从<module>导入*”)。特别是如果模块没有定义“__all__”，我们的全局命名空间就可能被污染。结果是大量未使用的变量、函数等等。另外，潜在的命名冲突！这是避免的好办法。</module>

# 奖金:默认参数，变量定义

对于那些阅读到最后的人来说，这里有一些额外的奖励:

不要改变默认参数！当函数被定义时，缺省参数被求值…仅一次。假设我们有以下程序:

```
def example(inputs=[]):
    inputs.append("ran_once")
    return inputs
```

第一次运行时，它将返回["ran_once"]。第二次，["ran_once "，" ran_once"]。诸如此类。除非这是您想要的行为*，而且很少是*，这里有一个更好的方法来编写函数:

```
def example(inputs=None): inputs.append("ran_once")
    if inputs is None:
      inputs = []
    inputs.append("ran_once")
    return inputs
```

**变量定义。**变量在被使用或从未被使用之前被重定义是非常普遍的。我怀疑这发生在代码更改期间。这可能是在更新代码时需要注意的事情，但也是静态分析器可以帮助解决的事情。

同时，程序经常在函数不被使用时保存它的返回值。例如，您想调用一个函数来启动一个服务，但是您从来不检查返回值。在这种情况下，不保存返回值对于阅读您的代码的人来说更容易理解，并且减少了内存的使用。

你已经坚持到最后了！奖金什么的。向你致敬。

![](img/ea607dacdc9c67721e9bf9ed15ecb52f.png)

照片由[沃伦·王](https://unsplash.com/@wflwong?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄