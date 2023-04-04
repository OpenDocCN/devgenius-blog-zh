# 如何用 Hugo 创建一个强大的博客(作为 Wordpress 的替代品)

> 原文：<https://blog.devgenius.io/how-to-create-an-overpowered-blog-with-hugo-as-a-wordpress-alternative-ee3771106b42?source=collection_archive---------3----------------------->

## 博客

## 用 Hugo Static Site Generator 作为 Wordpress 的替代品来创建一个博客&加上自动化和其他超级功能

![](img/74ebf2a8a6d722ee5d6d1ccc55a16cb8.png)

类固醇上的雨果静态网站发生器

# 介绍

一个没有博客的开发者就像一个没有鱼竿的渔夫。虽然只用一根线&一个鱼钩就可以捕鱼，但鱼竿让渔民的生活变得轻松多了。在这种情况下，Dan Bader 在他的文章中总结得很好— [*你为什么需要一个编程博客的 3 个理由*](https://dbader.org/blog/3-reasons-why-you-need-a-programming-blog) 。

但是，如果你读过我以前的一些文章，你就会知道选择正确的博客工具并不是一件容易的事情。我之前写了一篇关于我所处的困境的文章。下面是这篇文章——[*中型 vs 静态站点生成器——一个计算机视觉工程师的困境*](https://jarmos.netlify.app/posts/medium-vs-static-site-generators) 。自从发表了那篇文章，我有时间试验现有的博客工具。我一一回顾了。您可以在— [*查看一些最流行的静态站点生成器*](https://jarmos.netlify.app/posts/reviewing-popular-static-site-generators) 查看评论。

可以说，静态站点生成器确实把我从困境中解救了出来。

静态网站还有一个额外的好处，那就是永远不用担心服务器维护和安全更新。它们是 Wordpress 更好的替代品。此外，它们的生产成本几乎为零。你可以花一些钱来维护它，但是财务成本永远不会超过几美元。

因此，在这篇文章中，我将展示如何用 Hugo 建立一个博客。这个博客将会有一个 CMS，会自我更新&还会有更多的超能力来添加。那么，不多说了，让我们开始把它设置成 Wordpress 的替代品。

但是请注意，拥有一些编程知识可以加分，但不是先决条件。你可以按照本文中的建议去做&你会没事的。

# 必备工具

我们这篇文章的明星是[雨果](https://gohugo.io/)。这个软件毫无疑问是最容易使用的静态站点生成器之一。

除了 Hugo，我们还需要一个 GitHub 帐户来存放文章和其他工具。其中一个工具包括 [GitHub 动作](https://docs.github.com/en/actions)。在众多由社区维护的 GitHub 操作中，我们将使用其中的几个。

但是如果没有内容管理系统(CMS ),任何博客都不可能成为 Wordpress 的替代品。因此，为了满足我们的 CMS 需求，我们将使用来自[林业](https://forestry.io/)的服务。

最后，我们将使用 [Netlify](https://www.netlify.com/) 服务来交付生成的静态内容。他们使用全球内容交付网络(CDN)向我们的受众交付内容。

可选地，我们可以使用 [Google Analytics (GA)](https://analytics.google.com/analytics/web/) 来跟踪需求。但是如果你是一个关心隐私的公民，请使用 [Cloudflare Analytics](https://www.cloudflare.com/analytics/) 来代替。

总而言之，这里列出了我们将使用的所有必要工具和服务:

*   一个 Github 库，用来存放我们将要使用的工具所需的文章、主题和其他文件。
*   GitHub 的几个动作。
*   关于 CMS 的[林业](https://forestry.io/)的说明。
*   一个向我们的观众提供静态内容的网络帐户。
*   以及用于跟踪需求的可选 Google/Cloudflare 分析。

所以，也就是说，确保你已经安装了上面提到的所有工具&然后我们就可以开始了。

# 使用雨果(带着它所有的荣耀)

如前所述，Hugo 无疑是最容易使用的静态站点生成器之一。Hugo 拥有 [50K+观星者](https://github.com/gohugoio/hugo/stargazers)的事实说明了它有多有用！

因此，不再浪费时间，让我们深入了解如何使用 Hugo。

# 安装 Hugo

首先，在你的本地机器上安装 Hugo。与其他静态站点生成器不同，Hugo 没有依赖性！但是像 [Pelican](https://getpelican.com/) 、 [Gatsby](https://www.gatsbyjs.com/) 和/或 [Jekyll](https://jekyllrb.com/) 这样的生成器需要本地 Python/Ruby 运行时或 JavaScript 环境。有了雨果，你所需要的就是[雨果二进制](https://github.com/gohugoio/hugo/releases) &你就万事大吉了。

“*安装*程序也非常简单。你需要将二进制文件添加到系统`PATH` &中，从终端的任何地方调用`hugo`命令。

请注意，编辑每个操作系统的路径是完全不同的。因此，请仔细查看官方的[安装文档](https://gohugo.io/getting-started/installing/)。

现在通过在命令行上运行`hugo version`命令来确保你已经安装了 Hugo。如果一切正常，您应该会看到类似的输出。

“hugo 版本”命令的输出

# 创建您的网站

我必须说，安装 Hugo 虽然简单，但这并不是整个过程中最有趣的部分。那么，让我们从有趣的部分开始。

您可以使用`hugo new site .`命令生成一个框架站点(*，我们将很快在上面构建*)。注意到命令末尾的`.` ( *点*)了吗？它告诉 Hugo 在当前目录中生成框架站点。

该命令生成一组简单的文件和文件夹。对雨果来说，它们每个都有特定的用途。下面是生成它们后的目录结构。

Hugo 目录结构

Hugo 作为一个框架网站所产生的东西比我在一个博客中所能讨论的要多得多。要了解更多信息，请简要查看 Hugo 的[项目目录结构](https://gohugo.io/getting-started/directory-structure/)的文档。

在不深入研究目录结构细节的情况下，让我们尽量简化。这样，就有了进一步个性化的空间。您也可以尽早开始交付内容。

# 创建内容(真正有趣的东西！)

如前所述，使用 Hugo 非常简单。然而，任何给定的 Hugo 项目都有可能非常复杂。但不管怎样，你可以马上开始写内容，你现在需要的只是；

*   在`themes`目录下下载一个主题。
*   配置 Hugo，在`config.yml`文件中用一些站点元数据生成您的站点，以满足 SEO 需求。
*   在`content`目录下的 Markdown 中写入内容。

就这么简单！

Hugo 拥有大约 300 多个由社区维护的美丽主题。你可以下载一个主题到你站点的`theme`目录& Hugo 会用它来生成你站点的内容。安装主题最简单的方法是使用“ *Git 子模块*”。这样，当主题作者向其存储库提交一个提交时，您可以确保主题被更新为&。

安装主题也很容易。运行`git submodule add <DOWNLOAD-LINK> themes/<THEME-NAME> --depth=1`命令就足够了。

出于 SEO 的需要，您可能还需要站点元数据。Hugo 会开箱即用地处理它。你必须用一个`config.yml`文件来配置 Hugo。在该文件中，您包括元数据的必要细节。Hugo 使用这些值来填充站点的元数据。

为了大致了解 config.yml 文件的样子，这里有一个例子；

Hugo 配置文件示例

请注意，配置`config.yml`并不局限于我在这里提到的内容。基于你使用的主题，你可能/可能不需要扩展它。

最后，是内容目录，您可以在其中存储文章的降价文件。你可以将你的减价内容组织到子目录中，以便于维护和组织。举个例子，我的是这样的。

Hugo 目录结构示例

文档的[内容组织](https://gohugo.io/content-management/organization/)部分有关于该主题的更多细节。请务必查看以获取更多信息。

有了这些信息，您可以立即开始创建内容。

确保将所有文章以 Markdown 格式存储在内容目录下。如果你不熟悉减价，请查看[减价指南](http://markdownguide.org/)。否则请继续阅读，了解如何使用林业 CMS。

既然您已经开始创建内容，那么是时候让您的受众阅读它们了！下一部分是向我们的观众传递内容..

# 部署博客

与 Wordpress 网站不同，静态网站不需要主机提供商(*或服务器*)。这减少了潜在的安全维护事故。但是，如果没有服务器来托管内容，我们如何将它们交付给我们的观众呢？

这就是 Netlify 的服务派上用场的地方。他们将通过他们的全球 CDN 提供我们的静态内容。他们的免费服务也有一些很棒的东西。这些额外的好处包括一个域、一个构建平台&等等。但是由于后面提到的原因，我们不会使用他们的所有服务。

因此，确保你已经创建了一个[新的回购](https://repo.new)来托管你的网站内容&一个[净收益账户](https://www.netlify.com/)作为开始。您需要在 Netlify 仪表板上执行一次性操作来创建新站点。参考网络文档了解[这样做的确切流程](https://docs.netlify.com/site-deploys/create-deploys/#deploy-with-git)。

然后把你的新网站推给你自己创建的 GitHub repo。这将触发一个[网络构建操作](https://docs.netlify.com/configure-builds/get-started/)。你现在可以在 Netlify ( *提供的网址上查看你的网站，你可以通过*改变它😉).

现在，每当你写一篇新文章或者对你的网站进行一些美学上的修改，Netlify 都会触发一次构建。虽然它对大多数用户来说是可行的，但也有一些缺点。其中之一是缓慢且未优化的构建时间(*通常需要超过 2 分钟来完成构建*)。考虑到 Netlify 每月只提供 300 分钟的构建时间，用不了多久就会用完。

另外，那样你就没有充分利用雨果的潜力。这就是为什么我们将使用一些 GitHub 动作。有了它，我们将部署我们的站点&同时自动化一些单调的任务！

下一节将更深入地探讨为什么&我们如何才能实现这样的壮举。

# 设置自动化& CMS 后端

创建和部署网站既简单又有趣。但是好景不长(*从个人经历来说*)。当您将主题添加为子模块时更是如此。你可以期待它的频繁更新。

但是多亏了 GitHub 的 Dependabot，我们可以随时更新我们的主题。

还需要一个内容管理系统(CMS)。没有 CMS，Wordpress 的替代品有什么用呢？我们还应该将构建过程委托给 GitHub 操作，而不是 Netlify。这是部署和维护我们网站的更有效的方式。

也就是说，让我们列出我们需要改进的剩余特性&开始一个接一个地改进它们。

*   设置和配置 Dependabot，以保持我们的网站更新所有的依赖关系。
*   设置和使用林业作为 CMS 后端。
*   配置&将构建过程委托给 GitHub 而不是 Netlify。

GitHub 的 Dependabot 集成是天赐之物。正因为如此，保持项目依赖关系最新从未如此简单。你猜怎么着？Dependabot 也可以处理 Git 子模块！这意味着你的主题会随着作者的修改而更新。

要配置 Dependabot，您需要一个特定的配置文件。这个名为`dependabot.yml`的特定文件位于`.github`目录下(*如果它还不存在就创建它*)。

它应该是这样的:

GitHub 动作和子模块的依赖机器人配置

配置了 Dependabot 后，它将在世界协调时每天 0600 时寻找更新。如果有的话，它会打开一个 PR，为 GitHub 动作和 Git 子模块分别贴上标签。

但是这只是 Dependabot 配置的冰山一角。你可以在官方文档中找到详细的 [*依赖机器人配置选项*](https://docs.github.com/en/github/administering-a-repository/configuration-options-for-dependency-updates) 。

安装了 Dependabot 后，您再也不用担心手动更新和维护任务了。

# 将林业设置为 CMS 后端

没有 CMS，静态博客不可能成为真正的替代物。这也是 Wordpress 吸引大多数没有任何技术实力的用户的原因。因此，一些用户可能不愿意在 Markdown 中写他们的内容。对他们来说，基于网络的富文本格式环境是最好的。

因此，我们将使用林业的后端 CMS 服务。还有像 [Contentful](https://www.contentful.com/) ( *最流行的一个* ) & [Netlify CMS](https://www.netlifycms.org/) 这样的替代品。但是，从个人经验来看，我发现林业是最容易设置的。

使用 Hugo & Forestry 可以做的事情超出了本文的范围。我可以单独就这个主题写一篇完整的文章。因此，保持这篇文章相当短&作为感兴趣的读者的指南，这里是如何建立林业的要点。

1.  确保你有林业帐户&你的网站是“*增加了*”他们的服务。
2.  从管理面板配置 CMS 的设置。
3.  通过导航到`<YOUR-SITE-URL/admin>` &登录到您站点的管理面板，开始用富文本编写您的内容(*如果您喜欢的话，也可以用 Markdown 编写*)。

林业部门有一个[向导](https://forestry.io/docs/guides/developing-with-hugo/)与 Hugo 一起建立他们的 CMS。也一定要去看看！

# 自动化&将构建任务委托给 GitHub

虽然使用 Netlify 构建网站没有什么错，但是它有一些局限性。一个主要的问题是，它的构建时间很慢并且没有优化(*我的站点花了 2 分钟来构建*)。由于 Netlify 每月只有 300 分钟的构建时间，用不了多久，免费的配额就会用完。

所以，要充分发挥 Hugo 的潜力，应该使用 GitHub Actions。有了它，你可以在<1 mins ( *建立你的网站，而我的*通常平均需要 20-40 秒。

此外，与 Netlify 相比，GitHub 每月提供 2000 分钟的构建时间！因此，加上更少的构建时间和巨大的构建分钟配额，您将不必担心构建不会触发😊。

此外，将所有工具放在一个屋檐下使维护变得更加容易。因此，让我们仔细研究一下我们需要做些什么来自动化一些构建任务；

1.  首先要确保在根目录下有一个`netlify.toml`配置文件。Netlify GitHub 操作将使用它将网站部署到 Netlify 的 CDN。该文件通常如下所示:

网络配置示例

在[网络配置文件](https://docs.netlify.com/configure-builds/file-based-configuration)文档中有关于该文件的更多细节。

2.为了自动化一些更单调的任务，添加一个`build.yml`工作流文件。把它放在`.github`目录下。并将以下内容添加到`build.yml`文件中。

通过 GitHub 操作实现自动化网络构建和部署

该文件指示 GitHub Actions 使用两个工作流来完成部署任务。它将设置 Hugo，生成静态内容并将其部署到 Netlify。此外，它会在每次推送和公关事件时触发。

4.最后，自动化的最大乐趣。由 Dependabot 打开的自动 PR 合并。

还记得 Dependabot 配置文件中的`automerge`标签吗？这就是它们派上用场的地方。我们将使用`[pascalgn/automerge-action](https://github.com/pascalgn/automerge-action)`来合并任何带有标签`automerge`的拉请求。无需人工干预。

你猜怎么着？Dependabot 还被配置为更新它用该标签打开的每个 PR！😆

由于所有的自动化特性，您甚至再也不需要打开存储库了！确实是自动化 FTW！🤣

# 最后的话

唷，对于一个教程来说，这是相当长的阅读！如果你一直读到最后，那么感谢你的透彻理解，非常感谢。

你可能也注意到了，这篇文章并不全面。然而，它触及了用于维护编程博客的所有最佳技术和工具。所以，我希望我通过这篇文章分享的信息足够让你开始写博客了！如果你这样做了，向我伸出手，说“嗨！这是我在社交媒体和/或电子邮件上的博客。我可能会给你一个大喊。

另外，我的博客是[开源](https://github.com/Jarmos-san/blog)！❤:所以，如果你遇到障碍，看看我是如何保持的。或者，随意开一个[问题](https://github.com/Jarmos-san/blog/issues/new/choose) / [讨论](https://github.com/Jarmos-san/blog/discussions)的帖子。

也就是说，我期待你如何使用 Hugo 与世界其他地方分享你的精彩内容！

*原载于 2021 年 2 月 21 日*[*https://jarmos . netlify . app*](https://jarmos.netlify.app/posts/blogging-with-hugo-as-an-wordpress-alternative/)*。*