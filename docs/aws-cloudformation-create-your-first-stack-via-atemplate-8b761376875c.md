# AWS CloudFormation 通过模板创建您的第一个堆栈

> 原文：<https://blog.devgenius.io/aws-cloudformation-create-your-first-stack-via-atemplate-8b761376875c?source=collection_archive---------8----------------------->

**使用自动气象站云形成**

![](img/4555762a22a778994d9c4dbaf72698af.png)

AWS CloudFormation 模板

AWS CloudFormation service 通过将基础设施视为代码(IAC ),让我们能够对 AWS 资源进行建模、供应和管理。我们将所需的服务组合成一个 CloudFormation 模板。这种分组被称为堆栈。堆栈是 AWS 资源的集合，我们可以将它作为一个单元来管理。换句话说，我们可以通过创建、更新或删除堆栈来创建、更新或删除资源集合。堆栈中的所有资源都由堆栈的 AWS CloudFormation 模板定义。在本课程中，我们将创建一个虚拟私有云(VPC)及其所有组件，如子网、路由表、规则和安全组。我们将创建一个红移星团。该群集包括一个红移数据库、IAM 角色和群集子网组。红移集群子网组与用于公共连接的 VPC 公共子网相关联。所有资源都在一个 yaml 文件中定义为 IAC。

在[之前的会话](/setup-aws-redshift-cluster-with-external-connectivity-6d3a04d60399)中，我们创建了一个红移集群，并在 AWS 控制台中手动创建了 VPC 组件。

如果你喜欢视觉效果，那么我在 [YouTube](https://www.youtube.com/watch?v=3nsLNAZ9Zok&t) 上有一个附带的视频，里面有完整设置的演示。

今天我们将:

*   创建一个 AWS CloudFormation 模板作为 yaml 文件
*   在 CloudFormation 模板中将所有资源定义为一个堆栈
*   创建一个具有公共连接的红移星团
*   从查询编辑器和 DBeaver 查询红移

让我们启动 AWS CloudFormation 服务，并单击 Create Stack 按钮开始这个过程。

![](img/c39045c48170572a059a5a1473aabc7a.png)

AWS CloudFormation 堆栈创建

这将把我们带到下一页，在这里我们选择“在设计器中创建模板”选项，并单击“在设计器中创建模板”选项。这将启动 AWS CloudFormation 用户界面。

![](img/18b831d9613b2ab3423e01a78aa941cf.png)

准备 AWS 云信息模板

在 AWS CloudFormation UI 中，我们将模板语言设置为 yaml。另外，请确保单击左下方的模板选项卡。一旦我们切换到模板选项卡，删除现有的默认代码。

![](img/a6371e495d99f7a8045e98bf7b4a183d.png)

现在我们可以粘贴我们的 yaml 模板。我们需要刷新用户界面(UI)来选择模板资源。我们看到一个图表，其中包含了 yaml 模板中定义的所有资源。我们还可以看到资源是如何相互关联的。我们点击 C *创建堆栈*按钮来模拟堆栈的创建。

![](img/5614f0a18114db599c1b1e8e440f96c5.png)

AWS 云信息用户界面

在下一页，我们将看到模板先决条件。我们看到了 S3 的模板位置。我们接受默认设置，点击*下一步*继续。

![](img/1c14ce7b5a3e38b1dedc10af0e3ff705.png)

模板位置

在下一页，我们提供了堆栈名称。如果我们的环境中有多个堆栈，这将有助于识别堆栈。我们将 EC2 助手实例保留为默认实例并继续。

![](img/56effa54d65482c1ce7323148e980370.png)

堆栈名称

我们继续堆栈配置。在这里，我们提供了堆栈的标签。如果在堆栈创建过程中出现故障，我们将设置回滚选项，并继续堆栈配置。

![](img/a1076ca4b6d6d2375e22b2522b808164.png)

堆栈标签

我们已经完成了堆栈配置。我们确认并点击*创建堆栈*按钮。

![](img/908ad0cde4d0074fef73d57af22e7431.png)

查看堆栈配置

![](img/04afd8c3d6bf5fca92b985a5be54f97d.png)

确认并创建堆栈

这将我们带到一个可以监控堆栈进度的页面。这可能需要一段时间，具体取决于您堆栈中的资源。我们可以单击刷新按钮查看已完成的任务和仍在进行中的资源。一旦完成，堆栈将具有 *CREATE_COMPLETE* 状态。

![](img/9467207fe0e3809ae2e28b91fb7aebc3.png)

堆栈创建状态

我们可以启动 AWS 红移服务来观察星团。我们单击集群名称查看其属性。我们需要确保*公众可访问的*选项被启用。此页面包含使用 SQL 客户端(如 db 屋檐)连接此数据库的数据库详细信息。

![](img/fabb4575f330da8f070e968c15167d67.png)

红移数据库详细信息

我们需要一条信息来连接到这个集群*端点*。

![](img/7f559e575c1981b74ddd98e6b058fa8f.png)

红移终点

我们启动数据库客户端并创建一个新的连接。让我们选择红移数据库。

![](img/7ca99b28ae83495f31ab092ab5f8fcd6.png)

数据库创建连接

我们将在下一页提供连接详细信息。我们从红移集群的属性页面复制了这些细节。我们测试，如果成功，请单击“完成”保存此连接。

![](img/1c3163de318dbd58904590af19d4c3e2.png)

数据库连接详细信息

**结论:**

*   我们已经创建了 AWS 云信息模板
*   我们已经成功配置了 AWS 云信息模板
*   我们已经创建了一个堆栈，其中包含创建具有外部连接的红移集群所需的所有资源。
*   我们通过一台带有 SQL 客户端的本地电脑与红移数据库建立了连接。
*   在 [GitHub](https://github.com/hnawaz007/pythondataanalysis/tree/main/AWS%20CloudFormation) 上完成 AWS 云信息模板。