# Golang 中注释代码的语法是什么？

> 原文：<https://blog.devgenius.io/what-is-the-syntax-to-comment-code-in-golang-53087d7e45d7?source=collection_archive---------5----------------------->

![](img/6a626d1e3aa7ad22c389aab78d06836a.png)

在 [Unsplash](https://unsplash.com/photos/vdXMSiX-n6M) 上[咪咪 Thian](https://unsplash.com/@mimithian) 拍照。

注释在任何编程语言中都非常有用，可以帮助你的代码的读者——无论是队友、同学、教授，甚至是你自己——能够更深入地了解代码的实际含义。

在 Golang 中，有很多语法上与你可能习惯的其他语言略有不同。评论是其中之一吗？在这篇简短的文章中，我们将讨论如何在 Golang 中发表评论。

# 如何在线评论

如果你想注释内联代码，你可以用`//`来完成。

```
var a = 5 // default value 
```

您可能会注意到，这在 Java 和 C++等其他语言中很常见。

虽然您可以使用这些双斜线来注释行内代码，但是没有什么可以阻止您在 Go 中将注释放在自己的行上:

```
// default value
var a = 5
```

# 如何注释一个块

有时候你想注释整个代码块。一种方法是使用`//`注释语法:

```
// type user struct {
// name  string
// id    int
// }
```

在上面的例子中，我们有一个被注释掉的结构。我们不得不去每一行手动输入`//`，这有点乏味，尤其是当你有更大的代码块需要注释掉的时候。

另一种方法是使用`/* */`语法。那是什么意思？这意味着你把`/*`放在你想注释的代码前面，把`*/`放在后面。你实际上是在用这两个代码做书夹:

```
/*
type user struct {
 name  string
 id    int
}
*/
```

这段代码现在被注释掉了，您可以开始了！这种情况的一种变体如下所示:

```
/* type user struct {
 name  string
 id    int
} */
```

这是如何在 Golang 中评论的超级快速入门。还有一些其他的技巧可以让你的评论变得更有力量，我们将在以后的文章中讨论。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)