# 使用 Synology 活动备份保护 Microsoft 数据

> 原文：<https://blog.devgenius.io/protect-microsoft-data-with-synology-active-backup-60610c81bfe1?source=collection_archive---------19----------------------->

Synology 凭借其用户友好的解决方案，在面向家庭用户和小型企业的网络连接存储(NAS)市场中一直是一个强有力的参与者。作为一个成熟的平台，Synology NAS 的功能可以通过 Package Center 得到极大的扩展。其中一个流行的软件包是微软 365 的主动备份。在本文中，我将介绍在 DiskStation Manager (DSM) 7 上创建您的 Microsoft 365 云服务的第一个备份的必要步骤。

本文原载:[如何用 Synology 主动备份备份微软 365？(hungvu.tech)](https://hungvu.tech/how-to-back-up-microsoft-365-with-synology-active-backup)

> 针对 Microsoft 365 的活动备份提供了一个集中式界面，该界面附带了用于高效数据备份和管理的自动发现服务、用于最大限度降低数据丢失风险的连续备份模式、用于轻松恢复的 Microsoft 365 门户活动备份等等。
> 
> Synology 公司。

# 开始之前

*   截至撰写本文时，仅支持以下 Microsoft 365 计划:商业(基本、标准、高级)、企业(F3、E3、E5)、教育(A1、A3)和 Exchange Online(计划 1、计划 2)。
*   请确保您的 NAS 设备与 Microsoft 365 的活动备份兼容。兼容性列表可以在这里找到[。](https://www.synology.com/en-us/dsm/packages/ActiveBackup-Office365)
*   本教程假设你事先已经有了一些 DSM 的经验。Microsoft 365 的活动备份版本为 2.4.1，其他主要版本可能需要不同的步骤。

# 步骤 1:安装 Microsoft 365 活动备份

Active Backup for Microsoft 365 是一个免费的附加包，因此它不会默认安装在新安装的 DSM 上。下载并安装软件包。需要一个在线 Synology 帐户来激活该软件包。

![](img/3abdb5bd6242e092878fc6f381478bfd.png)

# 步骤 2:创建备份目标

*   转到控制面板>共享文件夹。
*   按创建，然后输入文件夹名称。根据您的喜好设置其他复选框，然后按下一步。
*   选择加密此共享文件夹并提供您的首选密钥。这是敏感信息，因此请安全地存储它，并且您将需要密钥来解密和装载共享文件夹以供将来使用。可以使用密码生成器或其他密钥生成器库来生成密钥。

![](img/25cf6ad524791faa3a310f1b1f1860c0.png)

*   按下一步并启用数据校验和以实现高级数据完整性。不要启用文件压缩，否则，加密将被禁用。

![](img/0a3b62085864025ea2c3fe55fbdca67e.png)

*   按下一步下载加密密钥文件(`.key`)，并完成创建共享文件夹。
*   根据您的偏好设置用户对共享文件夹的权限。

# 文件级加密怎么样？

敏感数据必须始终加密，这样才能更好地防范恶意行为者(如小偷、黑客等)。)然而，由于缺乏 Synology 的官方文档，为 Microsoft 365 加密活动备份(也适用于 Google Workspace)可能会令人困惑。文件级加密是在 Active Backup for Business 的最新版本中引入的(一个不同的包，不适用于 Microsoft 365 和 Google Workspace)。因此，加密发生在备份任务中，而不是共享文件夹级别。

![](img/b0d7302ef361a1938e6a350c0193b035.png)

但是无论是微软 365 的主动备份还是谷歌 Workspace 的主动备份，仍然没有呈现文件级加密，所以我们只能依靠共享文件夹加密。

# 为什么一开始就要创建加密共享文件夹？

这里有一个有趣的点，Synology 使用 eCryptFS 作为其共享文件夹的加密机制，因此一个文件或文件夹名称有 143 个英文字符或 47 个亚洲(CJK)字符的限制(*不是完整路径长度*)。因此，对包含长文件名的非加密共享文件夹启用加密将会导致错误。

![](img/06c2ba6855dc5a772c57ed58abe08930.png)

但是，如果共享文件夹从一开始就被加密，备份任务仍然可以正常运行并将文件存储到目标。但是，长文件名的末尾会追加`(name too long)`。这不仅仅是 Synology portal 中的一个可视化展示，因为 downloaded-from-NAS 文件的末尾也有`(name too long)`。可以毫无困难地卸载(加密)和装载(解密)共享文件夹。我联系了 Synology support 询问这种行为，他们确认这是正常的。

![](img/08da68ef10412e385875d8d78ac9b1df.png)

# 步骤 3:为 Microsoft 365 服务创建备份任务

![](img/5735e2afc924d589be5a2719479fe6b3.png)

*   打开 Microsoft 365 包的活动备份并转到任务列表。
*   选择创建备份任务，然后按下一步。
*   选择您首选的 Microsoft 365 端点。如果你的订阅直接来自微软网站，那么选择是*微软 365* 。
*   设置证书密码，将其视为在步骤 2 中创建加密密钥，然后按下一步。
*   Synology 将要求您以 Microsoft 租户管理员的身份登录，并拥有足够的权限代表您的组织提供许可。下图是授予 Synology 在 Azure 上创建的应用程序执行备份任务的权限。

![](img/0801a82cfdb53e12c27af34026d14b74.png)

*   您将被要求确认 NAS IP 地址。作为一个 NAS，它应该有一个静态的 IP 地址，但是如果以后发生了某种变化。Active Backup for Business 允许连接编辑，但 Microsoft 365 和 Google Workspace 似乎不允许，因此您需要重新创建一个任务。
*   填写任务名称、备份目标(您刚刚创建的加密共享文件夹)。其他设置根据你的喜好。针对 Microsoft 365 的活动备份允许对租户用户、组、站点、团队、自动发现等进行备份。您可以选择性地选择要备份的内容。
*   Microsoft Teams backup 需要访问受保护的 API，因此您必须请求 Microsoft 进行访问。请求说明可在[这里](https://learn.microsoft.com/en-us/graph/teams-protected-apis)找到。如果不满足这一要求，微软团队的备份将会失败。也就是说，活动备份任务可以部分失败，而不会对流程造成任何阻塞，这很好。

![](img/243d73a4f28e50818d5d8763f23706c6.png)

*   按“下一步”选择备份和保留策略。就我个人而言，我更喜欢有一个物理空隙系统，所以我的选择是手动备份任务并保留所有版本的策略。
*   确认摘要，然后按 Apply 以完成任务创建。
*   既然创建了备份任务，它应该出现在任务列表中，您可以监视它的状态或重新配置某些设置。

![](img/dbe11331282097fa8d8ba6b0da2aba66.png)

# 包裹

在本文中，我们了解了如何在 DSM 7 中为 Microsoft 365 (v2.4.1)设置活动备份。请记住，不同的包版本可能会改变过程的方式，但一般来说，它应该保持相对相同。如果你有任何问题，请在下面的评论中告诉我！

> 对 web 开发、GitHub Actions 等感兴趣？我的其他文章可能对你有帮助！
> 
> [高级 GitHub 操作—条件工作流❓](https://hungvu.tech/advanced-github-actions-conditional-workflow)
> 
> [如何为你的博客选择最好的无头 CMS？](https://hungvu.tech/headless-cms-for-portfolio-and-blogs)
> 
> [Strapi vs Directus vs Payload，Headless CMS 比较](https://hungvu.tech/strapi-vs-directus-vs-payload-headless-cms-comparison)
> 
> 在[Dev Report](https://hungvu.tech/series/the-dev-report)找到更多开发者新闻，在 [my blog](https://hungvu.tech/) 找到技术文章。