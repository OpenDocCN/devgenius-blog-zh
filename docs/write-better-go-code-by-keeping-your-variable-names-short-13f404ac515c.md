# 通过保持变量名简短来编写更好的 Go 代码

> 原文：<https://blog.devgenius.io/write-better-go-code-by-keeping-your-variable-names-short-13f404ac515c?source=collection_archive---------1----------------------->

![](img/5e698cbbd916a97eeca790ead8b3c788.png)

如果您开始阅读来自变量名很长且非常具有描述性的语言的 Go 代码，您可能会遇到一点麻烦，因为 Go 可能有一些非常短的变量名。我们经常谈论像`c`或`r`这样的单个字母。

现在，它不像 Go 语言从根本上不能支持长变量名。很有可能。你可以给变量起任何你想要的名字，但是这篇文章更多的是谈论标准的约定或者 Go 开发者通常编写产品 Go 代码的方式。

Golang 的代码评审评论文档是这样说的:

> Go 中的变量名应该短而不是长。对于范围有限的局部变量来说尤其如此。更喜欢`c`而不是`lineCount`。更喜欢`i`而不是`sliceIndex`。

具体例子，好看！

`lineCount`和`sliceIndex`是非常具体和描述性的，但有趣的是，Go 对于短变量名放弃了这一点。

原因是多方面的，这是一个之前已经讨论过多次的话题；看看这个由 unix15e8 在回答问题“[为什么 Go 鼓励短变量名”时发表的](https://www.reddit.com/r/golang/comments/8wxwgv/why_does_go_encourage_short_variable_names/) [Reddit 评论。](https://www.reddit.com/r/golang/comments/8wxwgv/why_does_go_encourage_short_variable_names/e1zahlm/)”:

> 拉斯·考克斯在这里解释了原因——http://research.swtch.com/names
> 
> 这里还有一个介绍——[https://talks.golang.org/2014/names.slide](https://talks.golang.org/2014/names.slide)
> 
> 网上还有一些文章非常广泛地涉及了这个话题:
> 
> http://wordaligned.org/articles/go-for-short-variable-names[https://groups.google.com/forum/#!](http://wordaligned.org/articles/go-for-short-variable-names) [topic/Golang-nuts/j 9 qeizedpui](https://groups.google.com/forum/#!topic/golang-nuts/J9QeizedpuI)
> [https://software engineering . stack exchange . com/questions/176582/a](https://softwareengineering.stackexchange.com/questions/176582/a)
> [https://www . quora . com/Why-Golang-promote-short-and-kind-of-design-names-for-variables](https://www.quora.com/Why-does-Golang-promote-short-and-kind-of-meaningless-names-for-variables)
> [https://blog . learn programming . com/Golang-short-variable-variable](https://blog.learngoprogramming.com/golang-short-variable-declaration-rules-6df88c881ee)

应该注意的是 [Golang 的代码评审注释文档](https://github.com/golang/go/wiki/CodeReviewComments#variable-names)指出你不会总是使用短变量名:

> 基本规则是:离名字的声明越远，名字就越具有描述性。对于方法接收者，一两个字母就足够了。循环索引和读取器等常见变量可以是单个字母(`i`、`r`)。更不寻常的事物和全局变量需要更具描述性的名称。

因此，虽然这是一个普遍的约定，但这并不意味着它不能被打破。

感受变量的最佳方式是阅读大量开源的 Go 代码，很快，你可能会被这些非常短的变量名所吸引，这些变量名起初看起来毫无意义，但最终会产生更多类似 Go 的代码。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)