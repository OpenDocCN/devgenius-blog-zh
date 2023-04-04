# Broccolijs 插件的测试运行程序

> 原文：<https://blog.devgenius.io/testing-broccolijs-plugins-89a500c8e83e?source=collection_archive---------41----------------------->

## /工程/ javascript

## 测试自动化的第一步

![](img/0a561cdbe030ca89bd1e6b9e6e75b191.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Louis Hansel @shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) 拍摄的照片

我们都知道维护可能是软件开发中最昂贵的部分。拥有一套可以在持续集成平台上自动执行的良好的单元和集成测试，可以减少维护成本和回归。

尽管我很真诚，但我没有在我的 BroccoliJS 插件中包含单元测试，而是在发布新版本之前手工测试了它们。没什么大不了的，因为我不需要经常发布它们，这只是私人时间。我想我们都经历过。有时候编写测试感觉像是一种负担。有时候你想把事情做好。

几周前，在更新我的 npm 包的依赖项时，我没有心情进行手工测试。我想是时候增加一些测试了。

所以，我们走吧。

![](img/b4449de9053a786c59b551621c8d46a9.png)

照片由[戴恩·托普金](https://unsplash.com/@dtopkin1?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 要求

西兰花插件通常使用一个目录树和这个目录树的内容。插件可以操作给定树中的文件，然后必须将转换后的内容写入给定的输出目录。一个很好的例子就是[图像缩小](https://github.com/stfsy/broccoli-image-min)。

西兰花提供了两个命令:**构建**和**服务**。 **build** 命令将运行所有插件一次，并将结果放入 **dist** 目录，而 **serve** 命令将从项目文件夹运行本地 web 服务器。插件配置和顺序在[文件](https://broccoli.build/#example-pipeline)中定义。

为了测试一个插件，我们需要(1)能够在测试中运行这两个命令，并且(2)能够断言我们的插件已经按照预期修改了项目文件。因为我们在 NodeJS 上运行，所以我们应该——按照惯例——异步运行 I/O。(4)为了支持不同的测试场景，我们需要使用具有不同插件配置的多个 Brocfiles。

## 总结:

*   支持测试和服务命令
*   基于承诺的 API 允许异步处理
*   允许使用不同的 Brocfiles

# 履行

定义了上面的需求后，我们知道我们必须在给定的目录中用上面提到的命令运行花椰菜。NodeJS 允许使用[子流程模块](https://nodejs.org/api/child_process.html)生成带有附加参数的流程。因此，我们可以将花椰菜作为一个子流程来运行，并捕获它的输出。我们可以使用承诺让调用实现现在了解进程的状态。已启动进程的所有输出都可以写入标准输出。要停止进程，我们可以使用 kill 命令并传递进程 id。

最终的实现如下所示:

为了在 Windows 上正确地停止进程，我们必须使用 **broccoli.cmd** 作为目标命令，并使用 **taskkill** 来停止。

# 介绍西兰花-试跑者

一个简化 BroccoliJS 插件测试的 npm 模块。

测试运行程序将在后台给定的目录下运行一个花椰菜实例。它支持 **serve** 和 **build** 命令。

提供来自测试/装置的内容

测试/固定装置中的构建内容

看看这个:

```
npm install broccoli**-**test**-**runner **--**save**-**dev
```

感谢来到这里，也感谢你的阅读，
Stefan👋