# 如何自动保护 Azure 应用服务

> 原文：<https://blog.devgenius.io/how-to-automatically-protect-azure-app-services-1e925484663f?source=collection_archive---------4----------------------->

## 创建 PowerShell 脚本来管理对应用服务的访问限制

![](img/cb057451cc3afe1037264573b7194d3b.png)

[Pixabay 上的数字摄影师](https://pixabay.com/users/thedigitalartist-202249/)

当我说保护您的应用程序非常困难时，我想您会同意我的看法。

或者是？

事实证明确实如此，但是我们可以通过自动化更耗时和容易出错的部分来使我们的生活变得更容易。

在我们公司，我们在微软 Azure 中使用了很多应用服务。应用服务是微软提供的平台即服务(PaaS)产品。我们用它来托管几个通过 REST API 公开的服务。

这些服务及其 API 不应该公开。您可以通过指定允许的 IP 地址来限制应用服务访问。由于我们使用许多应用服务，我们希望自动管理这些访问限制。

本文描述了我们通过 PowerShell 脚本管理访问限制的解决方案。你可以在 GitHub 库中找到脚本和文档。

# 脚本组织和先决条件

该解决方案由一个 PowerShell 模块`Manage-AccessRestrictions.psm1`和两个 PowerShell 脚本`Remove-AccessRestrictions.ps1`和`Add-AccessRestrictions.ps1`组成。该模块包含两个脚本使用的函数。这两个脚本都可以从命令行调用。

要使用这些脚本，您需要安装 [PowerShell 7.1.3](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.2) 或更高版本。你还需要安装 Azure Az PowerShell 模块，你可以在这里找到。

脚本使用`Write-Information`记录信息。当脚本被调用时，您不会看到日志记录。你必须添加`-Verbose`选项使其可见。

# 取消所有现有的访问限制

虽然我们不想公开应用程序服务，但有时开发者直接访问服务是必要的。例如，在故障排除期间。

发生的情况是，开发者添加额外的访问限制来直接访问应用服务。他们通过 Azure 门户手动添加他们的 IP 地址。完成故障排除后，他们会删除自己的 IP 地址。

然而，有时他们会忘记这最后一步，删除他们的 IP 地址。

因此，我们有一个 PowerShell 脚本来删除所有现有的访问限制，并添加默认的访问限制。该脚本每天晚上运行一次，以重置所有访问限制。

首先，我们将看看删除所有现有访问限制的函数。如下图所示，函数`Remove-AllAccessRestrictionsFromAppServices`需要一个参数`$ResourceGroupName`。此参数标识包含应用程序服务的资源组。Azure 中的资源组是资源的容器。

该函数首先通过`Get-AzWebApp`检索资源组中的所有应用服务。这是来自 [Azure Az PowerShell 模块](https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-7.1.0)的一个函数。然后我们迭代所有的应用服务并调用`Remove-AccessRestrictionsFromAppService`函数。

移除所有应用服务的所有访问限制

你在下面看到的`Remove-AccessRestrictionsFromAppService`功能移除了单一应用服务的所有访问限制。该函数做的第一件事是通过`Get-AzWebAppAccessRestrictionConfig`检索现有的访问限制。结果包含应用服务上存在的所有规则。该函数然后使用`Remove-AzWebAppAccessRestrictionRule`删除规则。

由于应用服务可以包含部署插槽，我们还需要删除对部署插槽的访问限制。这发生在第 12–22 行。

移除应用服务的所有访问限制

这些函数由一个名为`Remove-AccessRestrictions.ps1`的脚本使用，您可以从命令行启动它。

## 调用删除访问限制脚本

您可以通过 PowerShell 命令行调用`Remove-AccessRestrictions.ps1`脚本来使用`Remove-AccessRestrictionsFromAppService`。

该脚本有一个名为`ResourceGroupName`的必需参数。它首先导入`Manage-AccessRestrictions`模块，然后调用`LoginToAzure`。这将显示 Microsoft 登录屏幕以输入您的凭据。登录后，调用函数`Remove-AccessRestrictionsFromAppServices`。

请看下面`Remove-AccessRestrictions.ps1`脚本的实现。

Remove-access restrictions . PS1 PowerShell 脚本

# 添加访问限制

添加访问限制的脚本使用一个输入文件，该文件在 JSON 中指定将哪些 IP 地址添加到访问限制中。请参见下面的文件示例。

规则名称是规则的标识。你应该把它设置成你能识别的东西。IP 地址是可以访问应用服务的地址。最新的字段 priority 表示 Azure 评估规则的顺序。

指定需要访问的 IP 地址的输入文件

当脚本完成时，只有该文件中的 IP 地址可以访问特定资源组中的应用服务。

## 添加访问限制脚本

添加访问限制脚本由两部分组成，第一部分从给定的资源组中检索所有应用服务，读取 JSON 文件，并验证排除列表。参见下面的第一部分脚本。

在第 7 行，使用`Get-AzWebApp`函数从给定的资源组中检索所有的应用服务。然后在第 9 行，调用一个特殊的函数来验证要排除的应用服务是否确实存在于资源组中。当您在想要排除的某个应用程序服务中出现拼写错误时，这可以防止运行脚本。

在最后一行，18，JSON 文件被读取并存储在变量`$ipRules`中。

添加访问限制 PowerShell 脚本的第 1 部分

脚本的第二部分遍历资源组中的应用服务，并设置访问限制。下面是脚本的第二部分。

第二部分从标签`:nextAppService`开始。当在排除列表中找到应用服务时，会使用此标签。在第 10 行，我们直接跳到下一个应用服务。

我们检查`$RemoveExistingRules`标志是否为真。如果是这样，我们通过调用`Remove-AccessRestrictionsFromAppService`来删除现有的规则。该函数与删除访问限制脚本使用的函数相同。

最后，脚本遍历 JSON 文件中的所有记录，并为每条记录调用`Add-AccessRestrictionToAppService`函数。

添加访问限制 PowerShell 脚本的第 2 部分

`Add-AccessRestrictionToAppService`函数使用`Add-AzWebAppAccessRestrictionRule`向应用服务添加新的限制规则，见下文。它还使用参数`**-**ScmSiteUseMainSiteRestrictionConfig`调用`Update-AzWebAppRestrictionConfig`。这意味着您的应用服务的 SCM(源代码管理管理器)站点将使用相同的访问限制。

该函数还将对它通过应用服务找到的所有暂存槽设置相同的访问限制。

向单个应用服务添加访问限制的功能

您可以从命令行使用`Add-AccessRestrictions.ps1`来执行`Add-AccessRestrictionsToAppServices`。

## 调用添加访问限制脚本

您可以通过 PowerShell 命令行调用`Add-AccessRestrictions.ps1`脚本来使用`Add-AccessRestrictionsToAppService`功能。

该脚本有两个必需参数和两个可选参数。所需的参数称为`JsonFilename`和`ResourceGroupName`。这两个可选参数可用于移除现有的访问限制和排除一些应用服务。

它首先导入`Manage-AccessRestrictions`模块，然后调用`LoginToAzure`。这将显示 Microsoft 登录以输入您的凭据。然后调用函数`Add-AccessRestrictionsToAppServices`。

请看下面`Add-AccessRestrictions.ps1`脚本的实现。

Add-access restrictions . PS1 PowerShell 脚本

# 结论

安保很难！但是您可以通过自动化更耗时和容易出错的部分来使您的生活变得更容易。我们每天晚上使用本文中的 PowerShell 脚本来重置应用服务的访问限制。

我们增加的一件事是将 Azure DevOps 管道也添加到访问限制中。否则，我们无法从 Azure DevOps 管道部署到我们的应用服务。我们对 Azure DevOps 管道使用了访问限制的服务标签。

假设您的基础设施需要更多或不同的规则。请务必查看 [Azure Az PowerShell 模块](https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-7.1.0)的功能和文档。我几乎可以肯定，这个模块中有一些功能可以提供帮助。

现在，这当然不是保护你的 Azure 应用服务的唯一方法。还有其他选择，比如将你的应用服务放在虚拟网络中。这确实会影响您可以使用的应用服务类型，并可能增加成本。

到目前为止，将访问应用服务的资源列入白名单对我们来说是有效的。你可以在 GitHub 库中找到这些脚本和它们的文档。

感谢阅读，记住永远不要停止学习。