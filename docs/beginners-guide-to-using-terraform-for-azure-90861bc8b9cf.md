# 使用 Terraform For Azure 的初学者指南

> 原文：<https://blog.devgenius.io/beginners-guide-to-using-terraform-for-azure-90861bc8b9cf?source=collection_archive---------4----------------------->

![](img/e34962354b5a8c22687768a3aac08ecc.png)

让我们一起来看看开始使用 Terraform 在 Microsoft Azure 中创建和管理资源所需的步骤。

本文中概述的许多步骤也适用于 AWS 用户，或者在构建 AWS 解决方案时可能非常相似。然而，下面的代码示例是特定于 Azure 的。

# 获得所需的工具

您需要在系统上安装两个不同的 CLI 工具，以便能够在本地运行本文中使用的命令:

*   你可以在这里下载[。下面的代码片段假设您将`Terraform.exe`放在了`PATH`环境变量中。如果没有，就需要通过提供相对路径来扩展代码片段。](https://www.terraform.io/downloads)
*   Azure CLI，你可以在这里下载

请注意:通常，出于开发目的，您只想在本地执行这些。最后，您会希望在构建管道中运行这些工具。幸运的是， [Terraform](https://marketplace.visualstudio.com/items?itemName=ms-devlabs.custom-terraform-tasks) 和 [Azure CLI](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/azure-cli-v2?view=azure-pipelines) 都已经作为预打包任务出现在 Azure DevOps 中。

# 使用 Terraform 为部署设置 Azure 订阅

假设您已经在 Azure Active Directory 中拥有一个 Azure 订阅，您可以在其中创建服务主体，您将需要执行一些步骤来准备您的订阅(每个步骤将在后面更详细地描述):

*   创建服务主体。Terraform 将在向您的订阅部署资源时使用此信息进行自我验证。创建服务主体时，您需要确保授予它所需的权限
*   创建 Azure 存储帐户。我们将指示 Terraform 将它的状态存储在那里。

## 创建服务主体

这一步要求您在您的订阅所分配到的 Azure AD 中拥有`Application administrator`角色。如果你没有这个角色，你可能需要让你的 Azure 广告管理员参与进来。

首先，你需要使用 Azure CLI 登录到正确的订阅。你可以通过打开一个终端并运行`az login`命令来实现。这将要求您登录，然后打印出您有权访问的所有订阅的信息。通过运行`az account set -s <your subscription id>`，选择您想要部署到的订阅。您可以通过运行`az account show`来验证您当前使用的套餐。

最后，您需要通过运行`az ad sp create-for-rbac --role="Contributor" -- scopes="/subscriptions/<your subscription id>"`来创建服务主体(或者如果您想将 Terraforms 访问限制到特定的资源组，则运行`create-for-rbac --role="Contributor" -- scopes="/subscriptions/<your subscription id>/resourceGroups/<your resource group>"`)。这个命令将打印出一些您在下一步中需要的信息，所以请记下来。

要让 Terraform 使用此服务主体进行身份验证，您需要导出一些关于服务主体、您的订阅和 Azure AD 的信息作为环境变量(代码片段假设您使用的是 PowerShell):

`$ENV:ARM_CLIENT_ID="<the app id printed above>" ; $ENV:ARM_CLIENT_SECRET="<the password printed above>" ; $ENV:ARM_SUBSCRIPTION_ID="<your subscription id>" ; $ENV:ARM_TENANT_ID="<your tenant id>"`

请注意，该命令只会为当前进程临时设置环境变量。

## 创建存储帐户

Terraform 使用状态文件来了解在对现有资源集应用更新的`.tf`文件时需要做出哪些更改。

虽然您可以将这些状态文件存储在本地计算机上，但不建议这样做——它们可能会丢失，您团队中的其他开发人员将无法访问这些文件。此外，一旦您开始自动化基础设施资源的创建，您的 CI/CD 管道将需要这些状态文件。

获取共享状态文件的一种方式是使用 Terraforms 付费服务 [Terraform Cloud](https://cloud.hashicorp.com/products/terraform) 。

在本文中，我将向您展示如何使用通过 Azure 存储帐户共享的状态文件。为了准备后面的步骤，您需要在 Azure 门户中手动或使用 Azure CLI 创建一个 Azure 存储帐户和一个容器。在这两种情况下，这都是一次性步骤:虽然 Terraform 将使用此存储帐户，但它不是 Terraform 管理的资源之一。

*   如果您还没有想要在其中拥有存储帐户的资源组，那么通过运行`az group create --name <name for your group> --location westeurope`来创建它(您可能想要根据需要调整位置)
*   然后运行`az storage account create --resource-group <name for your group> --name <name for your tf state storage account> --sku Standard_LRS --encryption-services blob`来创建存储帐户
*   最后，通过运行`az storage container create --name tfstate --account-name <name for your tf state storage account>`在这个帐户中创建一个容器

创建存储帐户后，您需要将帐户密钥作为环境变量提供给 Terraform。您可以通过运行`az storage account keys list --resource-group <name for your group> --account-name <name for your tf state storage account> --query '[0].value' -o tsv`来检索那个键，并通过运行`$ENV:ARM_ACCESS_KEY="<the access key retrieved by the last command>"`来设置环境变量。

# 开始创建 Azure 资源

现在，您已经设置了所有需要的工具，可以开始使用 Terraform 创建和管理基础设施了。为此，在存储库的文件夹中创建一个新的`main.tf`文件，并将以下内容放入其中:

这个片段将告诉 Terraform

*   使用 Azure 资源管理器提供程序插件来管理资源(第 2–7 行)
*   将状态文件存储在前面创建的存储帐户容器中(第 9–14 行)
*   依赖于特定的 Terraform 可执行版本(第 16 行)
*   配置 Azure 资源管理器提供程序插件的一些属性(第 19–27 行)

现在，您应该能够在您的`main.tf`文件所在的文件夹中运行`terraform init`。这将下载 Azure 资源管理器提供程序插件，并在您配置的存储帐户中创建状态文件的初始版本。

现在你已经准备好创建真正的 Azure 资源了。以下代码片段显示了创建资源组的示例，以及该资源组中的 Azure 容器注册表和 Azure IoT Hub:

您可以在这里找到如何定义`azurerm`提供者[所有支持的资源类型的文档。](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs)

现在，离提供`main.tf`文件中定义的资源只差最后两步了:

*   运行`terraform plan`查看 Terraform 希望应用于您的订阅的更改
*   运行`terraform apply`将更改实际应用到您的订阅

就这样，你现在可以使用🥳.平台管理 Azure 资源了如果你对更详细的教程、更深入的信息和其他例子感兴趣，可以考虑跟随 Terraform 提供的[这个教程](https://learn.hashicorp.com/collections/terraform/azure-get-started)。

觉得这篇文章有用？在 Medium 上关注我( [Christian Eder](https://medium.com/@christian.johann.eder) )并查看我下面关于 Azure、AWS、基础设施代码和物联网的文章！不要忘记👏这篇文章分享一下吧！

*   [使用 JSII 创作多语言 AWS CDK 构造](https://awstip.com/authoring-polyglot-aws-cdk-constructs-using-jsii-be0ed66e9910)
*   [在云中使用自定义 Pulumi 状态存储](https://medium.com/@christian.johann.eder/using-custom-pulumi-state-storage-in-the-cloud-e2737f2528e3)
*   [使用 CDK 将 ASP.NET 核心 API 部署到 AWS Fargate](https://awstip.com/deploying-an-asp-net-core-api-to-aws-fargate-using-cdk-dab10bef51a1)
*   [使用 Pulumi 部署 Azure 功能&。网络](https://medium.com/@christian.johann.eder/deploying-azure-functions-using-pulumi-net-bfaad97ff9dd)

另外，如果你还不是中等会员，可以考虑在这里加入。