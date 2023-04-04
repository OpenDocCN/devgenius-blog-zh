# 谷歌云 IAM 和 AWS IAM

> 原文：<https://blog.devgenius.io/google-cloud-iam-and-aws-iam-similarities-and-differences-442f2e13835?source=collection_archive---------0----------------------->

## 异同

现在有许多云提供商，提供类似的产品和服务，有时甚至为他们的产品使用类似的名称。但是这些看似相似的名字背后的概念并不总是相同的。在这篇短文中，我们将探讨来自不同供应商的两款名为 IAM 的产品——Google Cloud IAM 和 AWS IAM——的异同。

![](img/86e7080496752b560c8f9e4583bd7278.png)

照片由[凯特在](https://unsplash.com/@katetrysh?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上与幽会

IAM 代表*身份和访问管理*，通常是指一套工具、方法和流程，用于识别和管理用户，并仅向期望的用户提供对特定资源的访问。

云环境中的一个误解是 IAM 用于管理最终用户对应用程序本身的访问。GC IAM 和 AWS IAM 用于管理对云资源的访问，而不是对在云产品上实现的应用程序的访问。对于后者，你可能应该检查一下 [AWS Cognito](https://aws.amazon.com/cognito/) 和 [Google Firebase](https://firebase.google.com/) 。

# 术语

AWS 和 GC 都在使用*角色*和*策略*，但是用法不同，这甚至会让有经验的用户感到困惑。

简而言之: **GC 策略将角色链接到身份，而 AWS 策略是可以附加到身份的权限集合。**这个简短的解释可能会令人困惑，所以这里有更多的细节来帮助你理解其中的区别:

在 Google Cloud 中，您有预定义的*角色*列表，例如查看者、所有者或发布者，您可以将这些角色分配给身份(用户、组或服务帐户)——角色与身份的这种绑定被称为*策略*，然后它被绑定到资源。

在 AWS 中，您可以通过指定效果、动作和资源来创建*策略*——您可以指定自己的策略，或者从 AWS 管理的策略列表中找到合适的策略。然后，您可以将所需的策略附加到身份(如用户、组或角色)或资源。关于 AWS 政策和权限的完整信息可以在[这里](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)找到。

例子可能会帮助你抓住不同之处。以下是 GC 策略示例:

```
{
    "bindings": [
    {
        "members": [
            "group:my-viewers@mydomain.com"
        ],
        "role": "roles/viewer"
    },
    ],
    "etag": "BwUjMhCsNvY=",
    "version": 1
}
```

AWS 政策是这样的:

```
{
  "Version": "2012-10-17",
  "Statement": {
  "Effect": "Allow",
  "Action": "s3:ListAllMyBuckets",
  "Resource": "*"
  }
}
```

# 接近

AWS 和 GC 都允许两种类型的访问:用户访问和编程访问。这里的用户访问是指您或任何其他人可以通过键入密码手动登录到 web 控制台，并通过单击、拖放等手动执行一些操作。编程式访问是指使用键(直接或间接)来识别运行在其他地方的代码片段——或者在云中的另一个实例上，或者根本不在云中。

## 用户访问

AWS 和 GC 都支持来自外部提供商的帐户联盟，或者可以自己管理帐户，这对于 Google Cloud 来说不是真的，因为 Google 有自己的帐户和组系统，与 GC 无关，他们明智地决定在他们的云上重用它。因此，您只需在登录 GC web 控制台时使用 Google 帐户。

这在 AWS 中是不一样的，用户账号是 AWS 自己管理的。这意味着您拥有所谓的 root 帐户，它充当管理员帐户，可以完全访问您的所有云资源。使用这个 root 帐户，您可以创建其他 IAM 用户，然后通过提供用户名、密码和帐户 ID 登录 AWS 控制台。

因此，这里的主要区别是:AWS IAM 用于管理帐户和授予访问权限，而 GC IAM 仅用于授予通过其他方式管理的帐户的访问权限。

## 程序化访问

通过使用**服务帐户**可以对谷歌云中的资源进行编程访问，这是一种由应用程序而不是个人使用的特殊类型的帐户。没有密码，你可以输入登录到这个帐户，你只有访问密钥，你可以下载和管理自己或让 GC 为你管理它。与用户帐户相反，服务帐户本身既是身份也是资源，这意味着您可以授予其他用户访问给定服务帐户的权限。

AWS 方法与此类似:您应该创建新的 IAM 用户，该用户将由密钥而不是密码来标识。这足以识别外部运行的代码，但是如果您想要在 EC2 实例(AWS 虚拟机)上使用编程访问，那么您需要创建一个链接到给定实例的实例概要文件，并为这个概要文件定义 IAM 角色。实例概要文件只不过是 IAM 角色的容器。

# 策略评估逻辑

GC IAM 和 AWS IAM 之间的另一个区别是如何确定策略的有效性，或者换句话说，当某个用户将访问某个资源时，IAM 将如何确定它是否被允许？

在 Google Cloud 中，策略可以附加到以下层级的任何级别:

1.  组织
2.  文件夹
3.  项目
4.  资源

**Google Cloud 将有效策略计算为在该资源上设置的策略和从其父节点**继承的策略的并集，这意味着:

1.  如果您在更高的层次结构级别上授予用户权限，则该权限将在所有后续级别上继承。即在项目级别授予的权限实际上是授予该项目的所有资源的权限。
2.  更广泛的角色包括有限角色的权限，更广泛的角色将对给定资源的给定用户有效。

与 AWS 的逻辑不同。在评估策略时，明确的拒绝可以覆盖更广泛的允许策略。以下是 AWS 的[策略评估逻辑文档](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)中的 AWS IAM 逻辑:

*   默认情况下，所有请求都会被拒绝，唯一的例外是拥有完全访问权限的帐户 root 用户。
*   基于身份或基于资源的策略中的显式允许会覆盖默认的拒绝。
*   如果存在权限边界、组织 SCP 或会话策略，它可能会用隐式拒绝覆盖允许。
*   任何策略中的明确拒绝都会覆盖任何允许。

# 结论

对我来说，谷歌提供的 IAM 模型看起来更优雅，更容易理解，如果与 AWS IAM 相比，但我想这可能只是一个个人喜好的问题。

AWS 是公共云计算的先驱，作为先驱，他们有时会面临一些障碍和隐藏的设计问题，其他云提供商可以考虑这些问题。

# 链接

*   谷歌云 IAM 文档可以在[这里](https://cloud.google.com/iam/docs)找到
*   AWS IAM 文档可以在[这里](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)找到
*   [AWS IAM 最佳实践](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
*   [谷歌云与其他平台的比较](https://cloud.google.com/docs/compare)