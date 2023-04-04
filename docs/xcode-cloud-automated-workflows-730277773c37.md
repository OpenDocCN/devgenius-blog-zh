# Xcode 云:自动化工作流程

> 原文：<https://blog.devgenius.io/xcode-cloud-automated-workflows-730277773c37?source=collection_archive---------0----------------------->

使用 Xcode Cloud 构建、测试和分发您的应用程序。集所有功能于一身。

![](img/bc3c068830abc2344eef37a69e53ee6d.png)

目录

*   Xcode 云是什么？
*   使用 Xcode 云有什么要求？
*   Xcode Cloud 支持哪种源代码管理(SCM )?
*   如何为我的项目设置 Xcode 云？
*   我可以在其他平台上交付工件吗？
*   我可以在 Xcode Cloud 上使用自定义脚本吗？
*   我们需要 Xcode 云来持续集成和持续交付吗？

**Xcode 云是什么？**

苹果最近宣布 Xcode 云是 2021 年 WWDC Xcode 13 的一部分。 [WWDC21:遇见 Xcode 云](https://developer.apple.com/wwdc21/10267)，

这意味着苹果推出了自己的 CI/CD 工具，与市场上的一些其他工具竞争，包括 Jenkins、TeamCity、CircleCI、Azure DevOps 和 Github Actions。

Xcode Cloud 是一种基于云的持续集成和交付服务，构建到 Xcode 中，专为 Apple 开发人员设计。使用 Xcode Cloud 时，您的首要目标是继续 CI/CD 服务的循环。

![](img/689853543cb5d490832f36b632f401c1.png)

苹果

Xcode Cloud 允许您利用*持续集成和交付* (CI/CD)，这是一种标准的软件开发实践，可以帮助您开发和维护代码，并将应用交付给测试人员、团队和用户。

Xcode Cloud 是一个 CI/CD 系统，连接你用来为苹果平台创建软件和框架的工具: [Xcode](https://developer.apple.com/xcode/) 、 [TestFlight](https://developer.apple.com/testflight/) 和 [App Store Connect](https://appstoreconnect.apple.com/) 。

使用 Xcode Cloud，您可以:

*   构建您的项目
*   运行测试
*   向测试人员交付构建

成功完成上述所有过程后，您可以立即发布您的应用程序。

![](img/88afd792398798fb83bb63de4ea401f9.png)

苹果

**使用 Xcode 云有什么要求？**

Xcode Cloud 附带 Xcode 13，这意味着 Xcode Cloud 是 Xcode 13 的一部分，目前处于测试阶段。

在配置您的项目或工作空间以使用 Xcode Cloud 之前，您需要一个帐户、项目和源代码控制要求。

您必须使用 Xcode 的新构建系统，请参见 Xcode 10 的[构建系统发行说明。](https://developer.apple.com/documentation/Xcode-Release-Notes/build-system-release-notes-for-xcode-10)

您必须使用自动代码签名，参见[代码签名](https://developer.apple.com/support/code-signing/)。

如果您有苹果开发者计划，您需要访问 [Xcode Cloud Beta](https://developer.apple.com/xcode-cloud/beta/) 来提交访问请求。

当您的帐户符合条件时，您可以开始使用 Xcode Cloud。

【Xcode Cloud 支持哪种源代码管理(SCM)？

要使用 Xcode Cloud，您需要使用 Git 进行源代码控制。如你所知，Xcode 不支持另一个 VCS，比如 Subversion(SVN)。

要了解更多关于在 Xcode 中使用 Git 的源代码管理，请参见[协调和同步代码更改](https://developer.apple.com/documentation/Xcode/coordinating-and-syncing-code-changes)。

Xcode Cloud 支持以下*源代码管理* (SCM):

*   Github 和 Github 企业
*   比特桶云和比特桶服务器
*   Gitlab 和自我管理的 Gitlab 实例

**如何为我的项目设置 Xcode 云？**

您可以通过两种方式访问 Xcode Cloud 菜单:
产品- > Xcode Cloud 如下图截图所示。

![](img/42a08ab2f56a1ef8a5ee3760fe83a024.png)

第二种方法是沿着左侧栏->屏幕截图下方显示的报告导航器。

![](img/a90ef7ebbb002b5f03ea4b74a3f8febc.png)

现在，我将向您展示如何逐步配置工作流的屏幕截图。

Xcode Cloud ->创建工作流，然后您会看到选择产品，然后选择产品并单击下一步按钮。
选择下一步按钮后，您将看到审核工作流程框。您可以使用“编辑工作流”按钮编辑工作流，然后选择“下一步”按钮，然后您将看到“授予对源项目的访问权限”对话框。这一点对于继续基于云的 CI/CD 服务非常重要。您需要单击“授予访问权限”按钮，并允许对存储库进行访问。

Apple 将访问您的代码来构建您的项目，存储最近构建的工件、相关的 SCM 元数据，并在 Xcode 和 App Store Connect 中显示这些信息。您可以随时撤销访问权限。

Xcode 会将您重定向到 App Store Connect 帐户，使用帐户凭据登录后，您需要完成 Github 授权 Xcode Cloud 的步骤。当你在 Github 中点击完成第 1 步，你会被重定向到 Github 授权 Xcode 云，然后点击授权 Xcode 云。在所有的过程之后，你会看到下面的截图。

![](img/3effd5faeb2873ddb6fa372496c5376b.png)

返回 Xcode，然后点按“授予访问权限”对话框中的“下一步”按钮。完成此操作后，如果您尚未在 App Store 中设置，您将在 App Store 连接对话框中看到创建应用程序。设置 SKU 和主要语言，并完成工作流程。

您可以使用产品-> Xcode Cloud ->管理工作流来编辑工作流设置。将列出工作流，然后选择一个工作流，然后单击对话框底部的设置按钮。

将打开所选工作流程的“常规”部分。

![](img/ad54859a0a7ae6d6199ad189fba047a6.png)

您可以使用“名称”文本字段来命名工作流，并且您还可以使用“描述”字段中的一个字段来进行附加描述。

通过监听分支变化，可以自动开始构建。
在类型字段，您可以根据需要配置分支上的变更。在类型下面你会看到源分支字段:你可以设置自定义分支。

![](img/7bff26ffba03e3555000e5e53127017a.png)

在左侧，您可以使用环境菜单设置环境。Xcode 版本，macOS 版本等。

此外，在左侧，你会看到行动和行动后菜单。
Actions 菜单提供了添加构建、测试、分析和归档选项。要启动工作流，您必须至少添加一个。

此外，事后措施提供了一些好的措施，如通知、试飞外部测试和试飞内部测试。

完成所有这些配置后，您可以通过分支侦听开始构建，或者通过右键单击任何工作流然后选择 Start Build 来手动启动。

![](img/0ebd3fffa15aa6f25031ed1f9daebf23.png)

**我可以在其他平台上交付工件吗？**

Xcode Cloud 有严格的规则，只在 Testflight 中交付工件。

**我们需要 Xcode 云来持续集成和持续交付吗？**

苹果通过提供与 Xcode、TestFlight 和 App Store Connect 捆绑的持续集成，加入了这一趋势。这种方式提供了一个不使用 3 的本地解决方案。派对工具。从时间和资源的角度来看，它可以节省您处理其他工具的时间。

还有，Xcode Cloud 还在测试阶段，我们也不能说一定用得上。费用还不知道。据苹果公司称:Xcode Cloud 的定价和可用性的更多细节将于今年秋天公布。

感谢阅读。