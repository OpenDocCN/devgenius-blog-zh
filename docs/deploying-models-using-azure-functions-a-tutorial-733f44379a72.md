# 使用 Azure 函数部署模型…教程

> 原文：<https://blog.devgenius.io/deploying-models-using-azure-functions-a-tutorial-733f44379a72?source=collection_archive---------7----------------------->

![](img/65d74ccb16544d9c77fd92648b763a37.png)

我们经常从事有趣的项目，我们希望将这些项目投入使用并部署到生产中，以便它们能够对人们真正有用，而不是作为一个附带项目放在我们的笔记本电脑上。但是，由于缺乏资源和关于生产部署的领域知识，我们经常无法真正做到这一点。

Azure 函数解决了这个问题。让我们看看如何…

Azure Functions 提供了一种快速简单的方法来部署本地构建的程序，无需任何额外的设置。Azure functions 是一个完全无服务器的应用程序开发工具，可以帮助我们完成从本地构建和调试到云中部署和监控的整个开发生命周期。这使我们能够通过只编写对项目真正重要的代码来节省时间。这是通过使用基于触发器和绑定的编程模型来实现的，这使得无服务器应用程序能够响应事件并无缝地连接到其他服务。

![](img/79ca0b73aa1c9d7ce6407fc17b2f401b.png)

Azure 函数的可视化

Azure 函数使我们能够用我们最熟悉的(或者最适合手头项目的)技术栈立即开始编码。这包括对以下语言的支持:

*   [C#](https://docs.microsoft.com/en-us/azure/azure-functions/functions-reference-csharp)
*   [Java](https://docs.microsoft.com/en-us/azure/azure-functions/functions-reference-java)
*   [JavaScript](https://docs.microsoft.com/en-us/azure/azure-functions/functions-reference-node)
*   [PowerShell](https://docs.microsoft.com/en-us/azure/azure-functions/functions-reference-powershell)
*   [Python](https://docs.microsoft.com/en-us/azure/azure-functions/functions-reference-python)
*   [打字稿](https://docs.microsoft.com/en-us/azure/azure-functions/functions-reference-node#typescript)

在这篇博客中，我们通过构建一个电影推荐系统并使用 azure 的云将其部署到生产中来探索如何使用 Azure 的功能。推荐系统是使用 Python 和惊奇库构建的。我们将在教程中使用 Visual Studio Code 的 Azure Functions 扩展。这有助于我们在本地机器上方便地开发和调试。

# 入门指南

我们需要以下要求，使我们的 Azure 功能项目:

*   具有有效订阅的 Azure 帐户。Azure 还为学生提供免费订阅。
*   Azure Functions Core Tools 版本 3.x .要安装核心工具包，请使用以下命令(macOS):

```
brew tap azure/functions
brew install azure-functions-core-tools@4
# if upgrading on a machine that has 2.x or 3.x installed:
brew link --overwrite azure-functions-core-tools@4
```

*   Windows 用户可以下载并运行核心工具安装程序，基于 Windows 版本
    - [v4.x — Windows 64 位](https://go.microsoft.com/fwlink/?linkid=2174087)(这是推荐版本，因为 Visual Studio 代码调试需要 64 位。)
    [- v4.x — Windows 32 位](https://go.microsoft.com/fwlink/?linkid=2174159)
*   Python 版本(3.7–3.9)
*   [Visual Studio 代码](https://code.visualstudio.com/)在[支持的平台](https://code.visualstudio.com/docs/supporting/requirements#_platforms)之一上。
*   Visual Studio 代码的 [Python 扩展](https://marketplace.visualstudio.com/items?itemName=ms-python.python)。
*   Visual Studio 代码的 [Azure 函数扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)。

# 开发本地项目

一旦我们有了所有的要求，我们就可以开始我们的项目了。我们首先在本地机器上创建我们的项目。我们将在教程的后面学习如何将我们的代码发布到 Azure。

要创建新项目，请单击活动栏中的 azure 图标(一旦安装了 Azure 扩展，该图标就会出现)。

![](img/b89522ac1e6228b5b2e05b3a1ac21039.png)

创建新项目

1.  根据提示提供以下信息:

*   为您的功能项目选择一种语言:选择`Python`。
*   选择 Python 别名来创建虚拟环境:选择 Python 解释器的位置。
    如果没有显示位置，输入 Python 二进制文件的完整路径。
*   为项目的第一个功能选择一个模板:选择`HTTP trigger`。
*   提供一个函数名:例如`A_Movie_Recommender`。
*   授权级别:选择`Anonymous`，使任何人都可以调用你的函数端点。要了解授权级别，请参见[授权密钥](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook-trigger#authorization-keys)。
*   选择您希望如何打开您的项目:选择`Add to workspace`。

一旦创建了项目，我们就可以在该文件夹中开发功能。该函数的启动页面是 __init__。巴拉圭

![](img/48fa238dd4a463b5a24e3de7cebf15f3.png)

启动页面

一旦开发了您的功能，我们就可以从 __init__.py 中调用该函数。在我们的例子中，我们从文件 model_SVD 中调用“预测”函数。

我们可以在本地运行该功能，以确保它按预期工作，并调试可能出现的任何问题。要在本地运行该函数，请从“运行”下拉列表中单击“运行而不调试”或“开始调试”按钮。

![](img/8eedfef3b771515c2fbb93889e6c4d8e.png)

您的应用程序在终端面板中启动。您可以看到本地运行的 HTTP 触发函数的 URL 端点。

![](img/492160b2607a46dff96208f8d4bea690.png)

**重要提示:**为了下载项目所需的任何库，将其添加到 requirements.txt 文件中。

在我们的项目中，我们使用预测函数来提供由用户 ID 提供的电影推荐。我们可以使用该 URL 端点来访问结果。

![](img/6724dad257e241bd48d4c1e4bc0b4527.png)

一旦在本地机器上开发并调试了项目，现在是时候通过将项目发布到 Azure 来部署它了。

在发布之前，请确保您已在 VS 代码上登录 Azure。

![](img/2478f08e9a2930e3eec523ccd88db1ee.png)

登录 Azure

若要部署到 Azure，请选择“部署到功能应用程序”按钮。

![](img/3a4b37babcceab95cf80e8a9b124f986.png)

部署按钮

按下按钮后，会提示输入 python 版本和函数 app 名称等信息。

部署完成后，我们可以看到下面的通知。选择查看输出以获取部署的端点。

![](img/04c9cdde69255f73bd8ad0e2f306b12f.png)

我们现在可以使用这个 URL 作为我们的电影推荐器的 API，我们使用这个 URL 和一个 userID 在需要的时候获得电影推荐。

# 优势和局限性

# 清理资源

一旦您在部署中使用完该函数并希望释放 Azure 上利用的资源，请使用以下步骤来清理资源。

1.  在 Visual Studio 代码中，按 F1 打开命令选项板。在命令面板中，搜索并选择`Azure Functions: Open in portal`。
2.  选择您的功能应用程序，然后按 Enter 键。功能应用页面在 Azure 门户中打开
3.  在“概述”选项卡中，选择资源组旁边的命名链接。
4.  在“资源组”页面上，查看包含的资源列表，并确认它们是您要删除的资源。

![](img/8e98e3c4b7b495e7db746ecb4021c477.png)

1.  选择删除资源组，并按照说明进行操作。
2.  删除可能需要几分钟时间。完成后，会出现几秒钟的通知。您也可以选择页面顶部的钟形图标来查看通知。

现在我们已经看到了 azure functions 是如何工作的，让我们看看这个有趣工具的一些优点和缺点。

**优势:**

*   函数扩展可以与 VS 代码一起使用，以便在本地机器上进行快速有效的开发。
*   节省了编码时间，因为它创建了一个基于触发器和绑定的编程模型，使无服务器应用程序能够响应事件并无缝连接到其他服务。
*   有多个托管方案，可以选择符合业务需求的方案。
*   支持包括 Python、Java、nodejs 等多种语言。

**限制:**

*   它有一个非常复杂的定价模型。它有 3 种类型的计划:消费，保费，和专用。如果打开了永远在线实例，后 2 种方法可能有点昂贵。消费计划是一个低端计划，在其支持的功能方面有一些严重的限制。
*   添加存储帐户的相关定价非常模糊。这取决于服务的使用方式，最终可能会花很多钱。
*   消费和保费计划的扩展存在限制。高级计划，即使有点高端，也有 100 个应用程序功能的上限。

现在你已经看到了如何使用 Azure 函数，并且知道了它的优点和局限性，祝开发愉快！！

— — —(阿达尔什·普拉蒂克和伊西塔·戈亚尔的博客)

([https://medium.com/@ishitagoyal](https://medium.com/@ishitagoyalhttps://medium.com/@ishitagoyal)