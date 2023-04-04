# 什么是 Terraform，为什么你的基础设施应该写成代码

> 原文：<https://blog.devgenius.io/what-is-terraform-and-why-your-infrastructure-should-be-written-as-code-a41ddeae344b?source=collection_archive---------6----------------------->

![](img/de5f433241d8776a6f3e0b301cce4751.png)

克里斯多夫·伯恩斯在 [Unsplash](https://unsplash.com/s/photos/machinery?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

可以肯定地说，对你的职业生涯来说，更多地了解云技术是一个面向未来的举措。根据最近的一份 [Gardner 报告](https://www.gartner.com/en/newsroom/press-releases/2022-01-18-gartner-forecasts-worldwide-it-spending-to-grow-five-point-1-percent-in-2022)显示，2020 年，面向企业的云市场将首次超过内部市场，到 2025 年，其规模将是内部市场的两倍。云是基础设施，能够理解和控制它对于技术领域的每个专业人员都非常重要，尤其是在 DevOps 文化不断发展的情况下。

毫无疑问，亚马逊网络服务是云技术的领先提供商，正如亚马逊所做的一切一样，它的用户界面很糟糕(不要和 UX 混淆)。由于市场的领导者有这个问题，它给了每个人一个许可证去做不好的用户界面。很多时候，重要的东西被隐藏起来，而且比我们愿意承认的更多，我们最终会把钱花在我们甚至不知道自己在使用的组件上。

通过将所有这些东西放在一起作为代码，您将对您的环境发生的事情有更多的控制，并且还能够根据需要构建或破坏，这就是 Terraform 的亮点。

# 什么是 Terraform？

一个将基础设施写成代码的开源工具。它允许您在声明性配置文件中定义基础设施，这些文件可以用版本控制系统(如 GIT)来管理，以便对您正在做的事情有更多的控制。在代码中意味着对云环境的描述是集中的，以便于阅读，并且可以向其中添加测试和验证。

Terraform 上 AWS 的示例代码

这里就不赘述教程了，不过 Terraform 的[入门页面真的很不错(上面的例子就是从那里得到的)。当我第一次浏览这些教程时，我惊讶于创建和破坏一个完整的环境是多么容易，这让我想在我所有的项目中使用它。在我的 IDE (Vim，出于显而易见的原因，它是最好的 IDE)的舒适空间中浏览配置文件，能够了解随时间的变化，甚至在需要时回滚，这些都是 Terraform 免费提供的强大功能。重要的是开发人员比 GUI 更了解代码，因此将所有基础设施都作为代码有助于处理和理解它。](https://learn.hashicorp.com/terraform?utm_source=terraform_io)

在专业用例中，我们做的是在部署新的 Terraform 代码时运行一个管道。在这条管道上，我们有一个验证步骤，用 [Terratest](https://terratest.gruntwork.io/) 运行 Terraform 自己的验证和测试。自动化总是一件好事，有时你甚至会听说，如果某件事没有自动化，那它就做不成。

> 如果它不是自动化的，它就不会被完成——有人

Terraform 的另一个好处是它是云无关的，你可以找到 Azure、Google Cloud、AWS 等等的连接器。

# 结论

现在你知道了为什么把基础设施作为代码是如此重要，为什么我认为 Terraform 是这项工作的最佳工具。使用这种策略比通过 GUI 和疯狂的 UI 布局来控制云环境要容易得多。Terraform 以代码的形式提供了你开始使用基础设施所需的所有东西，并允许你用 GIT 控制你的基础设施，让你有机会根据变化来回移动。

这一年是 2022 年，我们需要停止手工做这么多的工作，让机器接管我们的一些职责。为什么不从让他们创造更多的机器开始呢？

> 如果你喜欢这个帖子，请给点掌声并分享它:)