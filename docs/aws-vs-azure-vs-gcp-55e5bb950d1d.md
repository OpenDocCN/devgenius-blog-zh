# AWS VS AZURE VS GCP

> 原文：<https://blog.devgenius.io/aws-vs-azure-vs-gcp-55e5bb950d1d?source=collection_archive---------3----------------------->

## 云平台比较，选择符合您需求的平台

![](img/5e2a496de7a0a8e612c51e3e52785309.png)

Daria Nepriakhina 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我在硕士期间参加的网站项目使我从零开始花费了大量的努力和研究来开发网站。

但是在开发之前，我遇到的第一个问题是，我需要最合适的云平台作为托管网站的服务器，但是我应该选择哪一个呢？

我整理了比较这三个云平台的所有笔记:
**亚马逊 AWS** 、**微软 AZURE** 和**谷歌 GCP** ，将分享选择 AWS 的原因和我个人使用 AWS 平台的经验。

# 自动警报系统

[亚马逊网络服务](https://aws.amazon.com/)

作为公共汽车市场的领导者，AWS 是最成熟的企业级供应商。拥有广泛的服务，如计算服务— EC2、数据库服务— RDS、存储服务— S3、无服务器服务— Lambda 以及 DNS 服务— Route 53 等。它们可以相互集成，使功能和用途更加完整和强大。AWS 为每项服务提供详细的[教程/培训](https://aws.amazon.com/getting-started/hands-on/)，包括文档、图表和视频。

各种规模的组织、政府和个人都可以将 AWS 用于各种各样的工作负载。

缺点是价格体系难以理解。压倒性的选择和难以使用，尽管它提供教程。

# 蔚蓝的

微软 Azure

作为第二大提供商，它与自己的软件——Windows 操作系统、Office 系列等——配合得很好。支持开源，以及在混合云、[物联网服务](https://azure.microsoft.com/en-us/overview/iot/)等方面的优势。

对于已经部署了 Windows 和其他微软软件的公司来说，Azure 非常合适。不仅集成完整性提高，难度也相对降低。此外，如果你已经是微软的现有客户，你可以在服务合同上得到相当大的折扣

但是客户觉得 Azure 没有他们预期的那样适合企业，技术支持和文档也不够好。

# GCP

[谷歌云平台](https://cloud.google.com/gcp)

虽然它是最年轻的平台，但其强大的数据分析引擎、作为 Kubernetes 标准的容器中的强大产品以及强大的云基础已经在云行业中建立了卓越的影响力。

GCP 与谷歌自己的产品如谷歌搜索和 YouTube 共享相同的基础设施，专注于高计算产品，如大数据( [BigQuery](https://cloud.google.com/bigquery) )、数据分析、 [AI 和机器学习(ML)](https://cloud.google.com/products/ai) 。基于其对数据中心和无服务器数据分析解决方案的成熟知识，GCP 不仅加快了数据计算时间，还通过强大的数据洞察力转变了您的业务

缺点是它不像 AWS 和 Azure 那样提供那么多不同的服务和功能。

# 那么什么最适合你呢？

这取决于您的需求和工作量。

*   **AWS 选择:**它提供了大量的工具和服务，您可以在一个平台上使用。此外，其成熟的云服务结构和官方教程将带您更快地进入这个超大规模的平台。不选择亚马逊的唯一原因是如果你想要更私人的关系。就其规模而言，亚马逊很难与每一位客户保持密切的关系。
*   Azure Choice: 它将非常适合运行 Windows 和许多微软软件的公司。不仅可以运行您现有的。Net 代码，但你的 Windows 服务器环境也将连接到 Azure，使其易于与本地应用程序交互。此外，Azure 对混合云的深度关注将帮助您弥合快速扩展且功能丰富的微软云和传统数据中心环境之间的差距。
*   **谷歌的选择:**谷歌发展迅速，但仍在进步。Google 依靠自己的规模和机器学习优势搭建了自己的云，显然值得一看。适合基于 web 的小型创业和快速扩张。

要了解更多，这是一个信息量大、有用的网站，有很多对比图:[AWS vs Azure vs Google——详细云对比](https://intellipaat.com/blog/aws-vs-azure-vs-google-cloud/)。

# 最后，我选择 AWS 作为我网站的服务器。

首先，AWS 是最大的云平台，有足够的 AWS 免费层用于我的网站项目。它以详细的[文档](https://docs.aws.amazon.com/index.html?nc2=h_ql_doc_do)提供各种服务。由于使用这项服务的开发人员数量也很大，我不仅可以参考官方文档，还可以参考其他开发人员根据他们的开发经验分享的技巧和教程。AWS 的这个优势会大大缩短我从零开始学习和上手 AWS 的时间。

其次，由于 AWS 比其他两个平台更常用于企业，因此更大比例的公司要求/需要 AWS 相关技能。如果我能在毕业前的项目期间有 AWS 开发经验，这将使我在选择公司和工作类型时更加灵活，而且，将让我在进入公司后适应能力更强，上手更快。

如果你对我如何开发网站感兴趣，或者你也想创建自己的网站，我已经一步步记下了开发过程，请参考我的出版物 [**编码器人生**](https://medium.com/coder-life) 功能页面— [AWS 网站项目](https://medium.com/coder-life/awswebsiteproject/home)。
你将知道如何:

*   [在 EC2 上托管一个静态网站](https://medium.com/coder-life/how-to-build-a-website-by-using-aws-e3a5befac4de)
*   [将您现有的 Github repo 克隆到 AWS EC2](https://medium.com/coder-life/practice-2-host-your-website-on-github-pages-39229dc9bb1b)
*   [设置 Amazone 数据库服务 RDS](https://medium.com/coder-life/practice-3-set-up-amazon-rds-132607065091)
*   用 PHP 托管一个动态网站
*   [如何使用保护您的网站？htaccess](https://medium.com/coder-life/practice-5-password-protect-for-the-website-in-ec2-the-end-ab9e4b77a710)

*今天成为* [***媒介会员***](https://alliehsu.medium.com/membership) *支持我的写作，获得无限数量的文章。*