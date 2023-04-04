# 有效的 Java:为所有公开的 API 编写文档注释

> 原文：<https://blog.devgenius.io/effective-java-write-doc-comments-for-all-exposed-apis-21c2450fa822?source=collection_archive---------1----------------------->

![](img/aa7953d41c231c19bf48a5055ab8c58b.png)

照片由[西格蒙德](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

本节*有效 Java* 的最后一章把我们带到了大家最喜欢的话题，文档。本章关注的特定类型的文档是关于 JavaDocs 的。正如本系列文章之前所讨论的那样， *Effective Java* 似乎主要面向 Java 库的作者。这个焦点在本章中被看得最多。虽然我仍然认为这对“普通”Java 开发人员来说是很好的学习资料，但它的适用性可能不如本书的其他章节。

由于这一章的适用性对我来说似乎较低，我想我将只涵盖重点，并鼓励那些有兴趣深入了解这本书的人学习其中的细节。

*   JavaDocs 应该关注方法的契约、应该给出什么、这些变量的前提条件、将返回什么、出错时会发生什么以及副作用。
*   一个类中不能有两个构造函数成员共享同一个 JavaDoc
*   JavaDoc 经常被转换成 HTML，这可以让你更好地格式化，或者在你不期望的时候意外地遇到解析，从而导致错误。
*   测试 JavaDoc 通常只是看一看它，并确保它看起来是正确的。

通过 JavaDoc 工具记录代码的能力很强。如果你不怕麻烦地编写这个文档，如果你的代码的主要目的是供其他 Java 开发人员使用，你就应该这样做，花时间把它做好。这意味着使它有用，完整地记录，将 JavaDoc 托管在某个可访问的地方，等等。半途而废的文档工作可能比什么都没有更糟糕。同样值得注意的是，JavaDoc 可能不是对您的特定代码最有用的文档类型，只有当您看到它将如何被使用时，您才会知道这一点。