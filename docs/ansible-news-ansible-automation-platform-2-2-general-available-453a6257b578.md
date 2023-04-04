# Ansible 新闻- Ansible 自动化平台 2.3 全面上市

> 原文：<https://blog.devgenius.io/ansible-news-ansible-automation-platform-2-2-general-available-453a6257b578?source=collection_archive---------13----------------------->

## Ansible Automation Platform 2.3 是 RedHat 最新发布的企业级 Ansible 平台。即使您只是一个社区用户，我也要强调三个主要改进:可信内容、企业 LDAP 和事件驱动自动化。

欢迎，Ansible 试点社区。我们今天在这里谈论新的 Ansible 自动化平台 2.3。它的最新版本是针对 Ansible 的企业使用的。让我提醒你，这种类型的内容需要 Ansible 订阅，这是一种额外的服务，为包括代码的组织提供金钱补偿，但也直接来自 Red Hat 的支持。

好吧，长话短说，新的 Ansible 自动化平台 2.3 于 2022 年 11 月 29 日发布。对于朋友来说，Ansible 自动化平台叫做 AAP。这与为期六个月的红帽发布计划一致，因为 AAP 2.2 的上一个版本是在 2022 年 5 月发布的。因此，在 11 月份，也就是节日到来之前发布它是有意义的。

一些新功能已于 10 月份在今年美国芝加哥举行的最新 Ansible Fest 2022 中展示。最后，疫情之后的一个新活动是面对面的会议。所以一些新的想法已经被宣布了，而且非常有趣。我对最新的事件驱动的 Ansible 架构特别感兴趣，但是我将在后面详细讨论。

我很喜欢 Red Hat 对可信自动化供应链的重视。这反映了太阳风的教训:我们需要验证我们的供应商。这非常有趣，因为 Ansible 自动化平台依赖于一些非常有趣的工具。例如，Ansible 可信集合:我们从可信注册中心下载的集合。

与 Ansible Galaxy registry 下载社区内容的方式一样，企业界也依赖 Ansible Automation Hub 提供 Ansible 资源和内容供您下载。Automation Hub 中的内容现在通过 GPG 密钥进行认证和签名。这是一个伟大的创新，这意味着我们在工作站上下载的所有代码实际上都是由一个密钥验证的。以前不是。因此，我们很容易受到中间人攻击，这种攻击可能会改变内容。

此外，我们创建的新内容可以使用命令行工具进行签名和验证。这被称为 ansible-sign，一个新的实用程序被添加到 ansible 工具链中。您可以将其作为开源下载，也可以在任何开源项目中使用它。

它很棒，因为它允许你签署一个项目的代码并验证它。Underneath 非常简单，因为它依赖于 GPG 密钥，您可以指定项目的哪个文件需要签名，哪个文件不需要签名；所以，使用和维护非常简单。

但是这也增加了一些额外的工作，因为您需要在执行之前验证代码。这也打开了被篡改文件或不匹配签名验证的用例场景。但是这个特性的好处超过了额外的工作开销。我对结果很满意。

长话短说，这是非常有趣的，所以在自动化中心的验证内容和签署您的代码的能力。

在 AAP 2.3 中，还有其他企业功能，如企业 LDAP，与基于角色的访问控制相匹配。Ansible Automation Platform 的企业用户一度需要这个特性。

让我提醒你，在引擎盖下，AAP 已经从以前的版本改变了很多。AAP 2.0 引入了这种新的基于容器的架构。

新架构需要一段时间才能完全运行，但现在正处于完善阶段。围绕着开发街道这一新概念的一些特征。因此，对于建议用户来说，这是一个非常有趣的特性。同样有趣的是，一个新的 ansible-builder 工具允许您创建 ansible 执行环境。容器策略这一部分的 Ansible 执行环境允许您的代码通过容器执行。

这意味着 Ansible 通过一个容器连接到目标节点，这个容器叫做 Ansible 执行环境。您可以根据工作负载所需的容器数量来扩展 Kubernetes 集群中的工作负载。所以在 Ansible 自动化平台的引擎盖下有很多东西。

我真的很喜欢这个发行说明。我认为这是非常清楚的，让我们完全了解强大的，总是新的功能。Ansible 认证内容存在于自动化中心内部，并使用 ansible-sign 工具。在博客中，有一长串不同的供应商已经采用了这个特性。

所以基本上，这些内容都用一个你可以验证的密钥来签名。而且每次下载内容，你都确定是供应商发布的。所以，这很好。就像我知道微软和其他软件生产商经常在那里。

我们在工作站上运行的应用程序的关键，并增加了一些额外的控制。

新发行说明上的最后一件事是最有趣的。这是新的事件驱动架构的预览。这是我非常想深入研究的东西，也许我会制作一个关于它的视频。

所以基本上，在我们的日常自动化中，我们按需触发剧本或一些调度工具。使用事件驱动架构，一个事件将触发一个所谓的“规则手册”，该规则手册仅在需要时执行自动化。我认为这将为自动化的基础设施即代码(IaC)用例开辟新的可能性。

# 概述

让我来介绍一下 Ansible 自动化平台 2.3。这个版本非常注重企业，我非常喜欢企业用户使用的这种特性。

Ansible Certified Contents 增加了签名和验证可信代码的能力，以及按需执行某些自动化的能力，开创了自动化的新水平。

甚至自动化，所有的东西。创造一个非常有趣的自动化工具。我希望 Red Hat 继续创新，我确信在未来的几年里，它将成为我们企业的一个非常有趣的工具。我很高兴，因为这次 AAP 发布符合 Red Har 的云计算战略。

我认为 Ansible 自动化平台将是一个非常有趣的工具，可以用来关注未来的企业用户。

非常感谢您的观看。祝您有愉快的一天，并在 Ansible Pilot 的下一次冒险中看我们。太好了，再见！

我希望你喜欢读这篇文章。如果你愿意支持我成为一名作家，考虑注册[成为一名媒体成员](https://ansiblepilot.medium.com/membership)。每月只要 5 美元，你就可以无限制地使用 Medium。

订阅 [YouTube 频道](https://www.youtube.com/channel/UC5MNbTYRHSCu9vAki3z9SmA)、 [Medium](https://ansiblepilot.medium.com/) 、 [Substack](https://ansiblepilot.substack.com/) 和[网站](https://www.ansiblepilot.com/)，以免错过 Ansible Pilot 的下一集。

# Ansible 的最佳资源

## 视频课程

*   [通过 200 多个示例学习 Ansible Automation&实践课程:通过一些真实的例子学习 Ansible，了解如何使用最常见的模块和 ansi ble 剧本](https://click.linksynergy.com/deeplink?id=euGmLrdj*Ec&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fansible-by-examples-devops%2F%3FreferralCode%3D8E065F6D6F8622A3DEC8)

## 书

*   [Ansible For VMware by Examples:自动化您的 VMware 基础架构的逐步指南](https://www.amazon.com/Ansible-VMware-Examples-Step-Step/dp/1484288785/)
*   [可通过示例回答:针对 Linux 和 Windows 系统管理员和开发人员的 200 多个自动化示例](https://leanpub.com/ansiblebyexamples)
*   [适用于 Windows 的示例:50 多个针对 Windows 系统管理员和开发人员的自动化示例](https://leanpub.com/ansibleforwindowsbyexamples)
*   [Ansible For Linux by Examples:针对 Linux 系统管理员和开发人员的 100 多个自动化示例](https://leanpub.com/ansibleforlinuxbyexamples)
*   [Ansible Linux 文件系统示例:40 多个针对现代 IT 基础设施的 Linux 文件和目录操作自动化示例](https://leanpub.com/linuxfileanddirectorybyansibleexamples)
*   [通过示例负责容器和 Kubernetes:20 多个自动化示例来自动化容器、Kubernetes 和 OpenShift](https://leanpub.com/ansible-for-kubernetes-by-examples)
*   [负责安全示例:100 多个自动化示例，用于自动化安全和验证 IT 现代化基础设施的合规性](https://leanpub.com/ansibleforsecuritybyexamples)
*   [可行的技巧和诀窍:10 多个可行的例子来节省时间和自动化更多的任务](https://leanpub.com/ansible-tips-and-tricks)
*   [Ansible Linux 用户&按示例分组:20 多个关于现代 IT 基础设施的 Linux 用户和分组操作的自动化示例](https://leanpub.com/ansiblelinuxusersandgroupsbyexamples)
*   [Ansible For PostgreSQL by Examples:10 多个自动化 PostgreSQL 数据库的示例](https://leanpub.com/ansible-for-postgresql-by-examples)
*   [ansi ble For Amazon Web Services AWS By Examples:10 多个自动化 AWS 现代基础设施的示例](https://leanpub.com/ansible-for-aws-by-examples)

# 捐赠

[](https://patreon.com/lucaberton) [## 卢卡·伯尔顿正在为 Ansible | Patreon 创建软件开源

### 今天就成为卢卡·伯尔顿的赞助人:获得世界上最大会员的独家内容和体验…

patreon.com](https://patreon.com/lucaberton) [](https://github.com/sponsors/lucab85) [## GitHub 赞助商上的赞助商@lucab85

### 我是一个活跃的开源贡献者，参与到了 Ansible 社区中，尽管我到处都是。1 个赞助商有…

github.com](https://github.com/sponsors/lucab85) ![](img/df283cbd45a4c23503115529604674c1.png)