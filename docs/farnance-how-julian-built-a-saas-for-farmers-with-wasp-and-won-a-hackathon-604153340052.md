# 法南斯:朱利安如何用 Wasp 为农民建了一个 SaaS，并赢得了黑客马拉松！

> 原文：<https://blog.devgenius.io/farnance-how-julian-built-a-saas-for-farmers-with-wasp-and-won-a-hackathon-604153340052?source=collection_archive---------10----------------------->

![](img/7b06a5b4f581faff11c4bd4f4d63ea1c.png)

[朱利安·拉尼夫](https://jlaneve.github.io/)是一名工程师和数据科学家，目前在[天文学家. io](http://astronomer.io/) 担任产品经理。在空闲时间，他喜欢玩扑克、下棋和赢得数据科学竞赛。

他的项目, [Farnance](https://farnance.netlify.app/) ,是一个 SaaS 市场，允许农民将他们的产品转化为区块链的数字资产。朱利安和他的团队开发了 Farnance 作为伦敦商学院年度黑客马拉松 [HackLBS 2021](https://hacklbs.devpost.com/) 的一部分，并最终在 250 多名参与者中赢得了总共 6 个奖项！

请继续阅读，了解 Julian 为什么选择 Wasp 来开发和部署 Farnance，以及他最喜欢哪个部分。

# 寻找完美的 React & Node.js 黑客马拉松设置

Julian 以前有使用 React 和 Node.js 的经验，并且喜欢跨堆栈使用 JavaScript，但是建立一个新项目并确保它使用所有最新的包(然后还要弄清楚如何部署它)总是一件痛苦的事情。由于黑客马拉松只持续了两天，他需要一种快速的方法来开始，但仍然可以自由地使用他最喜欢的堆栈。

# 一行授权和无 API 方法的威力

Julian 第一次了解 Wasp 是在 Wasp 在 HN 推出的时候，他决定这将是一个完美的工具。整个应用程序设置，包括整个堆栈，都是开箱即用的，只需输入`wasp new farnance`，他就可以开始编写自己的 React & Node.js 代码了。

除了在应用程序设置上，该团队节省了大量时间，因为不需要实现身份验证和典型的 CRUD API，因为 Wasp 也涵盖了这一点。他们还可以在 Heroku 和 Netlify 上免费部署所有东西，只需几个步骤，这非常适合黑客马拉松。

![](img/e56be09d6f5fe844942ed9df01729b61.png)

Farnance 仍在运行，您可以[在此尝试一下](https://farnance.netlify.app/)！源代码也是[公开的](https://github.com/jlaneve/Farnance)，尽管注意它是在 Wasp 的旧版本上运行的，所以有些东西有点不同。

# 花更多的时间开发功能，花更少的时间重新发明轮子

Julian 惊讶于他能够如此快速地到达地面并与用户共享一个可用的 web 应用程序！他决定使用谷歌的 material-ui 作为 ui 框架，这让他的应用程序看起来很专业，尽管他们的团队中没有专门的设计师。

Wasp 开箱即用地处理了所有常见的 web 应用程序功能(设置、验证、CRUD API ),他们可以将所有节省下来的时间投入到开发和改进他们的独特功能上，最终将他们带向胜利！

> *我以前参加过很多黑客马拉松，在那里我构建了小型 SaaS 应用程序，却浪费了太多时间来设置通用工具——比如用户管理、数据库、路由等。Wasp 为我处理了这一切，并让我在创纪录的时间内完成了我们的网络应用*
> 
> *—朱利安·拉尼夫—法南斯*

![](img/0ee959012422bc283fe8dcbd93a7971b.png)

# 快速启动，同时无忧扩展

由于 Wasp compiler 在幕后生成了一个全栈 React & Node.js 应用程序，随着 Julian 应用程序的增长和未来用户的增加，扩展该应用程序没有任何技术限制。通过在项目文件夹中运行`wasp build`,开发人员可以获得前端文件和后端 docker 文件，然后可以作为任何常规的 web 应用程序部署到您选择的平台上。

Wasp 免费提供了关于如何使用 Netlify 和 Heroku 的分步说明(由于 Heroku 取消了他们的免费计划，我们将很快为其他提供商发布指南)，但我们计划在未来的版本中添加更多的示例和更多的集成部署体验！

> *部署 wasp 应用程序非常简单——我没有时间在为期两天的黑客马拉松中搭建完整的基础设施，也没有 infra/devops 背景，但我在一个小时内就在 Netlify 上运行了一些东西。黑客马拉松上的其他项目很难做到这一点，将访问权交给评委无疑有助于我们获得第一名。*
> 
> *—朱利安·拉尼夫—法南斯*