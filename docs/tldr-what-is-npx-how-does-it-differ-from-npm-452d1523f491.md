# TLDR:什么是 npx？和 npm 有什么区别？

> 原文：<https://blog.devgenius.io/tldr-what-is-npx-how-does-it-differ-from-npm-452d1523f491?source=collection_archive---------3----------------------->

![](img/fd844a5c10a74db85f3a65259ca4083c.png)

由[德文·艾弗里](https://unsplash.com/@devintavery?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

> 如果您目前正在或希望成为 node.js 开发人员，了解 npx 和 npm 之间的区别是非常重要的。

如果你正在阅读这篇文章，你可能已经熟悉了什么是 npm。对于那些不是这样的人来说，npm 只是一个**包管理器**——一个 CLI 工具，它允许我们安装和使用来自节点库的各种代码包。Npm 能做的远不止这些，但是为了简单起见，我们可以把它想象成允许我们在我们的项目中本地下载和运行包。

Npx 与 npm 非常相似，但有一个基本的区别-

> **Npm 是**一个用来安装节点包的工具。
> 
> **Npx 是**一个用来执行节点包的工具。

真的就这么简单。Npm 用于本地下载包，它可以在我们需要的时候执行(也是在本地)。Npx 用于执行包，而不必在本地安装和运行它们。Npx 对于减少包污染非常有用——我们不想在本地安装可能只需要执行一次的包。

在节点包`create-react-app`中有一个实际的例子，如果你用 React 开发的话，你会非常熟悉它。这个包用于建立一个新的 React 项目，所以它通常只在项目开始时执行一次。因此，我们不想在全球范围内安装它，因为我们不会使用它超过一次！(脸书自己说`create-react-app`用 npx 胜过 npm)

使用 npm:

```
npm install create-react-app //installs globally, not needed!create-react-app newApp //executing the package
```

使用 npx:

```
npx create-react-app newApp //executing the package, no install
```

当使用 npx 时，我们仍然能够向包传递命令行参数，它的功能就像在本地从 CLI 执行一样。

虽然 npm 命令仍然可能占您的节点包管理的 90%以上，但是确定您可以通过使用 npx 在哪里节省空间是很重要的。node_modules 文件夹已经足够大了，所以尽可能使用 npx！

如果您喜欢这篇文章或觉得它有用，请随意。或者，你可以在 Medium [*这里*](https://jamesmbrightman.medium.com/membership) *支持我或者给我买一杯* [*咖啡*](https://ko-fi.com/jamesbrightman) *！非常感谢所有的支持。*