# 选择包装经理:纱线与 NPM

> 原文：<https://blog.devgenius.io/choosing-package-managers-yarn-vs-npm-f05a35336f4e?source=collection_archive---------2----------------------->

## 什么是包管理器，你应该使用哪一个？

![](img/d4a7ab97fd3b50df07f8b46c4c326e1b.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Leone Venter](https://unsplash.com/@fempreneurstyledstock?utm_source=medium&utm_medium=referral) 拍摄的照片

# 目录

1.  [什么是包管理器？](#2d5a)
2.  [NPM vs 纱](#d0ec)
    2a。[异同点](#66fe)
3.  [结论](#0dcd)
4.  [想要更多？查看其他资源！](#2cc0)

# **什么是包管理器？📭**

一个重要的 web 开发工具是包管理器，它帮助您管理项目的依赖项。更具体地说，它们为您提供无缝安装、卸载和更新依赖项/包的方法，处理重复的依赖项，以及帮助配置项目的设置。摘自 [MDN](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Understanding_client-side_tools/Package_management#what_exactly_is_a_package_manager) ，如果没有它们，您必须自己处理:

*   找到所有正确的包 JavaScript 文件。
*   检查它们以确保它们没有任何已知的漏洞。
*   下载它们并将它们放在项目中的正确位置。
*   编写代码以将包包含在应用程序中(这往往使用 JavaScript 模块来完成，这是另一个值得阅读和理解的主题)。
*   对所有包的子依赖项做同样的事情，可能有几十或几百个。
*   如果要删除软件包，请再次删除所有文件。

对于软件包管理器来说，他们依赖于软件包注册中心，这是软件包发布的中心位置，允许软件包管理器知道从哪里安装软件包。但这并不意味着只有一个集中的包注册中心。事实上，您可以使用 Microsoft Azure 或 GitHub 管理自己的包注册表。

了解不同的包管理器并理解每个包管理器的优点和缺点对于帮助您决定在项目中使用哪个包管理器是非常重要的。让我们深入研究两个最流行的包管理器，npm 和 yarn！

# **NPM vs 纱线📦🧶**

NPM 代表“节点包管理器”，比 yarn 更老，最初发布于 2010 年，随 Node 自动安装。另一方面，Yarn 代表“又一个资源谈判者”，由脸书在 2016 年发布，以解决当时的性能和安全问题。要安装它，您需要在命令行中使用以下命令安装 NPM:`npm install yarn --global`

虽然 Yarn 在 NPM 之后出现，以解决其问题，但 NPM 此后一直在改进，以解决其自身的问题，从而使 Yarn 流行起来。从本质上来说，增加纱线而不是 NPM 的必要性或好处变得渺茫到零，因为纱线所做的一切 NPM 都做不了。因此，2020 年 1 月，Yarn 发布了 Yarn 2 以保持竞争力，结果却面临来自开发者社区的诸多[批评](https://njbmartin.medium.com/whats-the-problem-with-yarn-2-ca59e3fabc9f)。纱线 3 于 2021 年 7 月发布，正面评论回应了来自纱线 2 的批评，然而，为了比较 NPM 和纱线，我将重点放在纱线 1 上。

## **异同🧐**

在高层次上，除了命令之外，两者之间的主要区别是它们的流行程度和安装方式(如上所述)，管理依赖性、性能、安全性和工作空间。在这两者之间，NPM 一直是[下载量最多的](https://www.npmtrends.com/npm-vs-yarn)，然而在 GitHub 上 [Yarn](https://github.com/yarnpkg/yarn) 比 [NPM](https://github.com/npm/cli) 更活跃。

它们都用一个自动生成的锁文件来管理它们的依赖关系，这很重要，因为锁文件包含项目中使用的依赖关系的确切版本的条目，使得其他人可以更容易地在它们的本地安装软件包并保持一致。此外，它确保在所有环境中维护相同的文件结构`node_modules`。这最初是由 Yarn 引入的，然而，重要的是要注意，Yarn 2 默认不支持`node_modules`文件夹，而是支持一种新的`Plug’n’Play`方法，你可以在这里阅读更多关于[的内容。](https://yarnpkg.com/features/pnp)

在性能和安全性方面，这是引入 Yarn 的主要因素，Yarn 仍然处于领先地位。然而，两者之间的差距没有以前那么大了。为了提高性能，当需要安装一个包时，会执行一系列任务；纱并行执行这些任务，而 NPM 执行他们每个包和顺序。虽然两者相对平等，Yarn 仍然更安全，因为它只安装来自`yarn.lock`或`package.json`文件的文件，而 NPM 自动执行一个代码，允许其他包被包含进来。也就是说，两者都使用加密散列算法来确保包的完整性。

最后，再次重申一下，Yarn 带来的不仅仅是基于 NPM 的修复，两者都在互相推动，使之变得更好，现在两者都拥有的一个特性是工作区。工作空间是由 Yarn 在 2017 年推出的，通过允许您设置多个 package.json/packages.来支持 monorepo 结构，尽管 Yarn 仍然有 NPM 尚不支持的额外工具，NPM 正在赶上并支持他们自己版本的工作空间。

# **结论📬**

在本文中，我们讨论了什么是包管理器以及它们做什么。我们还仔细研究了两个最流行的包管理器，NPM 和纱，以及他们如何比较，以便我们了解使用一个比另一个的利弊。也许最重要的是他们对未来的潜力。

感谢阅读，希望你学到了新的东西！如果你喜欢这个，请看看我的其他作品。一个好的起点是“[如何用 Node.js 和 Express](https://medium.com/p/1d7c13afc7cc) 建立一个简单的 API”，“[一个用 Jest](https://medium.com/p/250d04e61117) 进行测试和单元测试的初学者指南”，或者“[所有关于渲染:客户端和服务器](https://medium.com/p/efc94b117460)”。

# **想要更多？查看其他资源！💾**

## 包管理器

📖[https://developer . Mozilla . org/en-US/docs/Learn/Tools _ and _ testing/Understanding _ client-side _ Tools/Package _ management](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Understanding_client-side_tools/Package_management)

## 纱线 vs NPM

📖[https://www . section . io/engineering-education/NPM-vs-yarn-one-to-choose/](https://www.section.io/engineering-education/npm-vs-yarn-which-one-to-choose/)
📖[https://www . white source software . com/free-developer-tools/blog/NPM-vs-yarn-white-should-you-choose/](https://www.whitesourcesoftware.com/free-developer-tools/blog/npm-vs-yarn-which-should-you-choose/)📖[https://www.sitepoint.com/yarn-vs-npm/](https://www.sitepoint.com/yarn-vs-npm/)📖[https://www . geeks forgeeks . org/difference-between-NPM-and-yarn/](https://www.geeksforgeeks.org/difference-between-npm-and-yarn/)
📖[https://www.positronx.io/yarn-vs-npm-best-package-manager/](https://www.positronx.io/yarn-vs-npm-best-package-manager/)

## 纱线工作区与 NPM 工作区

📖[https://ruanmartinelli.com/posts/npm-7-workspaces-1](https://ruanmartinelli.com/posts/npm-7-workspaces-1)