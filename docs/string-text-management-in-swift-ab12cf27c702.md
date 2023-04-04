# Swift 中的字符串-文本管理

> 原文：<https://blog.devgenius.io/string-text-management-in-swift-ab12cf27c702?source=collection_archive---------5----------------------->

![](img/1b1f7e997ead894bddc6b90bb84e047c.png)

[万用眼](https://unsplash.com/@universaleye?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/strings?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

在软件开发中，挑战之一是软件本地化。您必须保持代码的可读性、简单性和可理解性，并为用户提供更好的体验。我不打算深究这个话题。

在我们开发的原生 iOS 应用程序中，本地化的文本管理破坏了代码的可读性。

我们可以像调用方法或属性一样调用本地化文本，而不使用任何引号。
这就是我们所需要的。

**我们开始吧。**

首先，我们需要创建一个枚举类型的文本结构，它将覆盖应用程序中的所有字符串。

那就会像下面这样。

在此之前，我们需要创建一个 Localizable.strings，如下所示。

现在，我们可以开始创建一个将调用本机 NSLocalizedString 方法的字符串扩展。

然后，我们需要添加一个属性来本地化它的原始值。每个 ExcellentName case rawValue 都必须是本地化的字符串键。

我们来到了最精彩的部分。

怎么用？

我如何调用它来增加代码的可读性？

大概就这些了。