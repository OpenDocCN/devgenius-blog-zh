# 如何为多个 AWS 帐户设置 S3 接入点

> 原文：<https://blog.devgenius.io/how-to-set-up-s3-access-points-for-multiple-aws-accounts-dca3b8c81397?source=collection_archive---------1----------------------->

## 如何教程

## 再也不用浪费时间为 S3 接入点设置权限了。

![](img/612b30bd25b9f2f01cfd6770b79d2bbc.png)

气泡星云由 [NASA](http://www.nasa.gov/) 、 [ESA](http://www.spacetelescope.org/) 和哈勃 SM4 ERO 团队拍摄

最近，我接到一个客户的电话，说他们正在努力建立一个 S3 接入点。他们希望在几个公司帐户之间共享一个大型数据集，但无法获得正确工作的权限。经过一些来回，我设法清除所有的问题。然而，我们最终花费的时间比我们任何人愿意承认的都要多。

为了避免将来发生这种情况，我决定写一篇后续文章，总结关于 S3 访问点权限的要点和注意事项。

# 要求

我们有一个 S3 存储桶，它包含几个大型数据集，这些数据集被组织在不同的名称空间中。本质上，每个数据集都有自己的关键字前缀，即自己的文件夹。

我们有两个主要需求: *i)提供对其中一个数据集(bucket 的一个文件夹中的对象)的访问*和 *ii)能够列出(仅)来自同一个文件夹的所有对象。*

# 为 S3 接入点设置多帐户访问

## 方案

假设我们在`AccountA`中有一个 bucket (test-bucket ),并且我们在同一个帐户中创建了一个访问点(`test-ap)`)。在另一个账户`AccountB`，我们有一个 IAM 用户，我们称之为`UserInAccountB`。

> 注意:通常我们会有一个 IAM 角色而不是一个用户，但是为了简单起见，在本文中我们将只考虑一个 IAM 用户的情况。

在我们的测试桶中，让我们也创建两个文件夹`public`和`private`，并为它们添加一个`testObject`。最后，我们将用它来测试我们的设置。

我们的目标是使用户`UserInAccountB` 能够通过`test-ap`访问我们`test-bucket`的`public` 文件夹中的数据。现在让我们看看如何做到这一点。

## 访问策略

为了实现我们的目标，我们需要创建三个策略。首先，我们需要在`AccountB`中创建一个 **IAM 策略**。它应该类似于下面显示的策略，IAM 策略应该附加到我们的`UserInAccountB`中。

AccountB 中用户的 IAM 策略。

该策略授予对我们在`AccountA`的`test-ap`接入点的完全 S3 访问权限。请注意，在这个阶段，我们还需要提供对底层存储桶的访问(稍后我们将阻止直接存储桶)。我们可以通过只指定一些我们希望允许的动作来进一步限制访问控制，比如 *s3:GetObject* 或 *s3:ListBucket。*这样，我们就完成了`AccountB`的设置工作。

此外，我们在`AccountA`中需要两个策略:一个 S3 桶策略和一个 S3 接入点策略。我们的 **S3 存储桶策略**需要附加到我们的 S3 存储桶`test-bucket`，它看起来应该是这样的:

帐户 a 中的 S3 时段策略。

这个存储桶允许任何操作，只要它来自为该存储桶创建的访问点。但是，不允许直接访问存储桶，因此将被拒绝。实际上，通过这种模式，我们将 bucket 的权限管理委托给了接入点。否则，我们将不得不同时管理 bucket 和接入点的权限。这是因为 S3 接入点被简单地命名为网络端点，它们被附加到 S3 桶。将接入点附加到存储桶不会改变底层存储桶的任何内容。对存储桶的所有现有操作都像以前一样继续工作。接入点策略中包含的限制仅适用于通过该接入点发出的请求。这就是为什么我们需要在存储桶和接入点级别都允许期望的操作，即并行维护两个策略。

最后，我们需要创建以下 **S3 接入点策略**，并将其附加到我们的接入点。

帐户 a 中接入点的 S3 接入点策略。

这项政策是我们权限管理的核心。它有两个语句: *AllowListFolderToIamUser，*启用文件夹列表， *AllowReadAccessToIamUser，*提供对对象的访问。首先，我们注意到两个语句都将我们的`UserInAccountB` 作为主体。这是难题的第二部分，我们如何实现对 S3 接入点的跨账户访问。其次，为了满足我们只允许访问`public`文件夹的要求，我们添加了一个条件`“s3:prefix”: “public/*`来检查请求是否指定了一个允许的路径。最后，在*AllowReadAccessToIamUser*语句中，我们需要确保我们只对同一个`public`文件夹中的对象授予读权限。注意，S3 接入点语法要求我们在资源规范中添加`objects/`前缀。

# 测试设置

首先，让我们检查文件夹列表是否按预期工作。为了测试我们是否可以运行下面的命令:`aws s3 ls s3://arn:aws:s3:us-east-1:AccountA:accesspoint/test-ap/public/ --profile AccountB-Profile`我们将看到我们的`testObject`被列出。如果我们对我们的私有文件夹(`aws s3 ls s3://arn:aws:s3:us-east-1:AccountA:accesspoint/test-ap/private/ --profile AccountB-Profile`)运行相同的命令，我们将得到一个`Access Denied`。好的，太棒了！

如何访问这些对象？嗯，要在本地下载我们的`testObject`，我们可以运行:`aws s3 cp s3://arn:aws:s3:us-east-1:AccountA:accesspoint/test-ap/public/testObject . --profile AccountB-Profile`。这应该会将对象下载到我们的工作目录中。如果对`private` 文件夹运行，同样的命令会因`Access Denied`而失败。

最后，通过我们的`UserInAccountB`,我们可以尝试直接对我们的 bucket 运行任何 s3 命令，我们将看到所有这些命令都将被拒绝，因为它们不是通过我们的接入点发出的。

# 结束语

在本文中，我们研究了如何设置跨帐户权限，以便通过 S3 访问点访问数据。为了实现我们的目标，我们创建了三个策略:针对我们的用户(或角色)的 IAM 策略，以及 S3 存储桶策略和 S3 接入点策略。

最后，我们展示了一个模式，它使我们能够将访问控制和权限管理从 S3 存储桶委托给它们的访问点。这对于管理共享桶的权限特别有用，因为我们可以通过简单地添加或删除访问点来轻松扩展访问策略。

# 尾注

1.  实际上，目前 S3 接入点并不像 S3 本身那样支持所有业务。更多关于运营限制和约束的细节可以在 [*这里*](https://docs.amazonaws.cn/en_us/AmazonS3/latest/userguide/access-points-usage-examples.html#access-points-service-api-support) *找到。*