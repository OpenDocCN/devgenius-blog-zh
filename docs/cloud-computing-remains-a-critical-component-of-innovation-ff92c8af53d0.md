# 云计算仍然是创新的重要组成部分

> 原文：<https://blog.devgenius.io/cloud-computing-remains-a-critical-component-of-innovation-ff92c8af53d0?source=collection_archive---------13----------------------->

## 尽管云计算仍在不断发展并面临诸多挑战，但它仍然是许多数字化转型案例的支柱。

![](img/0c3d7f8f42f2b67f3412c73344bab15b.png)

由于从大型机到数据中心的迁移以及云计算的爆炸式发展，数字化转型正在大量进行。云计算在当今技术中无处不在，很难找到不使用、不提供或不创建云服务的企业。[云计算](https://www.codemotion.com/magazine/dev-hub/cloud-manager/cloud-computing-covid-19/)提供了快速、灵活、廉价且可大规模扩展的基础设施。它负责许多垂直领域的一系列新技术和服务创新。

对于开发者来说，随着行业从内部架构转向[云计算，这已经带来了一系列技术和服务创新。](https://www.codemotion.com/magazine/dev-hub/cloud-manager/quarkus-cloud-native-containers-based-development-java-framework/)

无论是作为独立的替代方案，还是作为虚拟化的补充，容器化都变得越来越重要。Kubernetes 是管理存储在云中的容器的领先平台之一。

# Kubernetes 推动云计算

去年在阿姆斯特丹的 Codemotion 大会上，Alain Regnier 对 Kubernetes 进行了深入的探讨，他将其描述为“最初在谷歌内部(基于 Borg)开发的开源解决方案”。它允许您创建、更新和组织容器。基本上，你定义了你想要的目标架构。你说我想要这么多份这个容器。我想要这种类型的网络等等，系统会努力维护它。”

Google Kubernetes Engine (GKE)为使用 Google 基础设施部署、管理和扩展您的容器化应用程序提供了一个托管环境。Alain 将此比作“Kubernetes 即服务”。过去，我们有基础架构即服务、容器即服务平台即服务。现在，我们正转向这种 Kubernetes 服务方式。”

Alain 是 Alto Labs 的首席技术官，他写道:“Google 已经为我们的大多数产品运行容器超过 12 年了。他们开始每周 40 亿个集装箱。因此，它让你了解他们在管理这些容器、启动它们、停止它们时可能遇到的问题，以及他们在设计第一个版本时在 Kubernetes 中投入的经验。”

# 有效使用库伯内特斯和 GKE 的最佳实践。

Alain 深入探讨了 Kubernetes 和 GKE 的最佳实践。这些包括

**1。为命令库模式定义联盟**

因为:

*   你会经常用到它们
*   有时你输入错误的命令，你将不得不再次输入
*   你并不总是记得语法。

**2。学会使用 kubectl** 的输出选项

*   作为 JSON 输出并检索特定的元素:“kubectl 会在命令行上给你一些信息，但是有时候你想要额外的信息，或者你想要它们以不同的格式，例如，用 JSON。”
*   您还可以从现有资源中检索 YAML。

**3。用谷歌云壳，用 GKE**

“这非常非常有用，尤其是当你在谷歌云上的时候。因为你可以直接从这个虚拟机上做很多事情。所以它是一个小型虚拟机，带有一个持久磁盘和一些预装的常用工具，如 G Cloud kubectl 和 git。”

他的组织技巧是:每个项目 1 个顶层文件夹/ 1 个包含“导出”的“配置”文件。

寻找更多提示？看看下面 Alain 的演示:

[https://www.youtube.com/watch?v=beI65asuvio&list = plq 2-o3pbtowe 6 moo ia 4x DMU 4 ubresijur&index = 38](https://www.youtube.com/watch?v=beI65asuvio&list=PLq2-o3pBTowe6mooia4xDMu4uBRESIJUR&index=38)

# “20 年平台”的出现以及微服务的重要性

IBM 无服务器/ FaaS & IBM 云功能首席架构师，杰出的工程师 Michael Behrent 认为，每个时代都会出现新的平台和平台架构。它的目标是始终关注核心应用程序逻辑，并支持开发人员实现他们的应用程序，而不必担心与他们的业务逻辑无关的任何事情。迈克尔认为，我们正在见证下一个主导平台的出现，即 20 年平台。

[云计算](https://www.codemotion.com/magazine/dev-hub/cloud-manager/how-covid-19-impacts-the-future-of-cloud-it-infrastructure/)已经产生了高度分布式、多组件架构。“与过去相比，我们必须为失败做更多的设计，有些只是因为统计数据表明，同时运行许多组件意味着什么，并且所有组件都必须可用。”将微服务作为一种云原生架构方法，在这种方法中，代码可以更高效地更新，团队可以为不同的组件使用不同的堆栈，而这些组件又可以彼此独立地扩展。

【https://www.youtube.com/watch?v=f71zBg0avgU】T2&list = plq 2-o3pbtowfggnxmw 5 tbwi 8 uati 3 enpd&index = 27&t = 0s

# 云计算和知识产权

这很难想象，但是你现在正在创建的云计算软件可能对其他人来说价值不菲。Kim Gagne 告诉 Codemotion，专利诉讼的激增阻碍了云计算的创新。

罪魁祸首是专利钓鱼者，他们“本质上只是购买或获得专利，他们没有兴趣使用这些专利来开发任何新技术或解决方案或其他任何东西。他们只想起诉别人，他们想说你侵犯了我们的知识产权。因此，我们将从你的产品中获得你所有收入的 40%，或者如果我们能得到的话，100%。”

在过去十年中，基于云的诉讼一直在增加。他们最常见的目标是小企业，他们也感受到最大的影响，因为“诉讼可能会让企业倒闭，因为它需要时间，需要资源，而且它可能会束缚你，突然你就失去了市场优势。所以你想尽快摆脱诉讼。这符合专利钓鱼者的计划。”

虽然知识产权风险可以通过立法和法律手段来管理，但这些方法进展缓慢，“而且在科技行业，速度是有溢价的。”

# 行业主导的私人订购项目的兴起

幸运的是，人们正在反击，如 LOT Network、Allied [Security](https://www.codemotion.com/magazine/dev-hub/security-manager/) Trust、Open Invention Network 和微软:“从本质上讲，公司将专利汇集在一起，让那些正在打官司的人可以使用它们。”一个例子是微软的 [Azure IP Advantage 计划](https://azure.microsoft.com/en-us/overview/azure-ip-advantage/)，其中他们赔偿开源和专有云软件，并分享他们的专利:“他们说，如果你因在我们平台上使用的应用程序而被起诉，我们将赔偿你。我们会支付那套衣服的费用。而且没有上限。”该公司在 2019 年将其计划扩展到初创公司，包括正在构建连接到 Azure 的物联网解决方案的客户。此外，还有一份涵盖数千项专利的详尽目录，供受访者用于辩护。

【https://www.youtube.com/watch?v=hnMMRSrXi1k】T4&list = plq 2-o3pbtowe 6 mooia 4x DMU 4 ubresijur&index = 11&t = 0s

# 保持警惕，不要惊慌

艾伦强调说，我的目的不是吓唬你，告诉你远离云，因为你最终会被起诉。那会对我的生活产生反作用。”

他的建议是:

*   评估与知识产权和运营相关的保护和业务风险。
*   确定一个计划来保护你的创新和你的知识产权，尤其是你在云计算方面的创新。“这可能只是简单地与您的云提供商交谈，并说，您有什么样的保护措施，您能为我们提供什么帮助？你为你的顾客提供什么样的保护？”
*   如果你被起诉，要注意各种各样的解决方案。

*本文原载于 7 月 31 日的* [*Codemotion 杂志*](https://www.codemotion.com/magazine/dev-hub/cloud-manager/cloud-computing-critical-component-innovation/) *。看看更多文章吧！*