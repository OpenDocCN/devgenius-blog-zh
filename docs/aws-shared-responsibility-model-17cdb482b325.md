# AWS 共同责任模型

> 原文：<https://blog.devgenius.io/aws-shared-responsibility-model-17cdb482b325?source=collection_archive---------9----------------------->

正如我们在[云计算简介](https://medium.com/@jikaramit21/introduction-to-cloud-computing-f2ff10df8b69)中了解云计算的基本概念一样，现在在我们开始深入研究 AWS 云的每个概念和强大服务之前，我们应该了解我们作为云用户的责任。

![](img/4b1bc48dcbfa5f92ed7171ca780cfe03.png)

由[拍摄的](https://unsplash.com/@stemlist?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的茎表

> “安全性和合规性是 AWS 和客户的共同责任。这种责任区分通常被称为“云”的“安全性”与“云中的安全性”。
> 
> 这种共享模式有助于减轻客户的运营负担，因为 AWS 可以运营、管理和控制从主机操作系统和虚拟化层到服务运营设施的物理安全的组件。除了配置 AWS 提供的安全组防火墙之外，客户还负责管理客户操作系统(包括更新和安全补丁)以及其他相关的应用软件。"

**云的安全——AWS 的责任**

AWS 负责保护运行 AWS 云中提供的所有服务的基础设施。这个基础设施由运行 AWS 云服务的硬件、软件、网络和设施组成。

**云中的安全性—客户责任**

客户责任将由客户选择的 AWS 云服务决定。客户应仔细考虑他们选择的服务，因为他们的职责因所使用的服务、这些服务与其 IT 环境的集成以及适用的法律法规而异。这决定了客户作为其安全责任的一部分必须执行的配置工作量。

例如，亚马逊弹性计算云(Amazon EC2)等服务被归类为基础架构即服务(IaaS ),因此需要客户执行所有必要的安全配置和管理任务。部署 Amazon EC2 实例的客户负责管理客户操作系统(包括更新和安全补丁)、客户在实例上安装的任何应用软件或实用程序，以及 AWS 在每个实例上提供的防火墙(称为安全组)的配置。

对于抽象服务，如亚马逊 S3 和亚马逊 DynamoDB，AWS 操作基础设施层、操作系统和平台，客户访问端点来存储和检索数据。客户负责管理他们的数据(包括加密选项)，对他们的资产进行分类，并使用 IAM 工具申请适当的权限。

![](img/203a0d68a42409608ce694269a81f52f.png)

欲了解更多信息，请访问 [AWS 共享责任模型](https://aws.amazon.com/compliance/shared-responsibility-model/)

*感谢阅读！如果你喜欢你所读的，按住下面的* ***拍手按钮*** *，这样其他人可能会发现这一点，跟随将会非常感谢。*

第一篇[云计算简介](https://medium.com/@jikaramit21/introduction-to-cloud-computing-f2ff10df8b69)

第三篇 [AWS DynamoDB —第一部分](https://medium.com/@jikaramit21/amazon-dynamodb-part-1-59af362bc30c)