# JavaScript 中的闭包:来自技术面试的一课

> 原文：<https://blog.devgenius.io/closure-in-javascript-a-lesson-from-a-tech-interview-a7c80e8b43f4?source=collection_archive---------8----------------------->

我选择熨斗学校的原因是我看到他们有职业支持。这种支持的一部分包括与 [SkilledInc](https://www.skilledinc.com/app/home) 的模拟技术面试。

我和大多数其他人在采访中强调的部分是现场编码挑战。

没问题！我完成了他们的数据结构和算法部分，在 Leetcode 上做了一些问题，甚至在 Udemy 上开了一个[课程](https://www.udemy.com/course/js-algorithms-and-data-structures-masterclass/)来帮我准备。我明白了。

现场编码部分之前是琐事/概念检查部分。这包括关于语言如何工作的信息，比如提升，var，let 和 const，scope 之间的区别，等等。

我不太担心这一部分，因为我已经用 JavaScript 构建了有框架和没有框架的应用程序。

我的面试官问我能否解释一下 JavaScript 中的闭包是什么。

我记得在面试中要始终保持诚实和冷静，所以我说“嗯…我不认为我能做到，但我想知道它是什么！”

我的面试官做了很好的解释，我明白了这个想法的要点，但从那以后我读了更多的文章，做了更多的谷歌搜索，所以我现在要尽我所能解释它。

当用 let 或 const 声明变量时，该变量是块范围的。

这是什么意思？这意味着当你在一个函数或代码块中使用 const 或 let 声明一个变量时，这个变量在这个块之外是不可用的。

这和结案有什么关系？

考虑下面的例子。

```
function functionFactory(){
    const a = 1
    return function(){
        console.log(a)
    }
}const functionInstance = functionFactory()
```

我尽力使函数名和变量具有描述性。

这是怎么回事？我们的函数 functionFactory 返回一个引用变量的匿名函数。该匿名函数被返回并保存在一个名为 functionInstance 的变量中，functionFactory 完成执行。

那么当 functionFactory 执行完之后，变量 a 会发生什么呢？变量 a 会消失吗？你可能会这样认为，因为它是在一个函数块中声明的，该变量不应该在该块之外可用。

如果我们调用匿名函数的实例 functionInstance，会发生什么？ReferenceError: a 未定义？

我们将有 1 登录到控制台，我们有关闭感谢这一点。

当我们将函数返回值保存到变量 functionInstance 时，我们不仅保存了函数，还保存了函数的*词法范围*，对该函数周围环境的引用*、*。

这意味着返回值中函数可用的任何内容都可以用于我们调用 functionFactory 时创建的该函数的任何实例。

![](img/ffbceafc91519231a8f9594f9e109d99.png)

令人震惊

说到这个话题，这只是冰山一角。这是对那些不熟悉 closure 的人的一个快速、基本的介绍，即使他们以前使用过它而没有意识到！

给我的关键外卖？不要低估技术面试中的概念检查部分！这很容易做到，尤其是当编码挑战对大多数申请者来说是令人生畏的部分。

另一个是当你不知道某事时，要诚实并承认。我做到了，我不仅学到了一些东西，我也给了自己机会证明我可以很快地学习一个新概念。

最后，一旦你学会了一个新概念，写下来或者向别人解释。我听说如果你不能有效地向某人解释某事，你自己也不能真正理解它。这篇博文就是这么写的！它帮助我有效地理解了结尾，我希望它也能帮助读者。

我用来写这篇文章的参考资料包括 [freecodecamp](https://www.freecodecamp.org/news/closures-in-javascript/) 、 [medium](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36) 和 [JS docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) 上的一个帖子。

请随意查看我的模拟面试反馈[这里](https://api.skilledinc.com/report-card/9749)！我的面试官给了我大量的资源。