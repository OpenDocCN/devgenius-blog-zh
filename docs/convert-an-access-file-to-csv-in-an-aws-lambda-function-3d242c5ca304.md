# 在 AWS Lambda 函数中将 Access 文件转换为 CSV

> 原文：<https://blog.devgenius.io/convert-an-access-file-to-csv-in-an-aws-lambda-function-3d242c5ca304?source=collection_archive---------1----------------------->

或者如何在 AWS Lambda 函数中安装和使用 Linux 包

![](img/1ba8ac328701481cf3e0838f9f980bc1.png)

自 1.0 版以来，Access 在视觉上取得了长足的进步，但它仍然是我们生存的祸根。资料来源:winworldpc.com

转换 Access 文件。mdb 或者。accdb，是一个虽然不流行，但有很好文档记录的过程。在 Windows 和 Linux 上，使用 Python、C#、Perl 以及世界上几乎所有其他语言(我想可能是像 Java 这样的语言)将 Access 数据库和表转换成 CSV 和其他格式的指南和 StackOverflow 文章已经有了..明白了吗？Java，Sun…)。

但这是 2021 年，我们在*云*里做事，用*无服务器代码*！当然，我们可以只用几行代码在几分钟内创建一个简单的 Lambda 函数…

实际上这并不那么简单，这都是因为 [mdbtools](https://github.com/mdbtools/mdbtools) ，Linux 中处理 Access 文件的事实上的库/驱动程序。Lambda 的“层”特性是向我们的 Lambda 函数添加 *Python* 依赖项的一种便捷方式，但是如果我们的应用程序依赖于像 mdbtools 这样的 **Linux** 包，它对我们没有任何帮助。谢天谢地，AWS 的人想到了这一点，并给了我们将定制的 Docker 容器作为 Lambda 函数运行的能力。

> Lambda 的“层”特性是向我们的 Lambda 函数添加 *Python* 依赖项的一种便捷方式，但是如果我们的应用程序依赖于像 mdbtools 这样的 **Linux** 包，它对我们没有任何帮助。

我们可以构建一个预先安装了 mdbtools 及其依赖项的 [Lambda 容器映像](https://docs.aws.amazon.com/lambda/latest/dg/images-create.html)(又名 Docker 映像)。然后，我们可以在本地机器上的模拟环境中测试它。最后，我们可以使用 AWS CLI 将我们定制的 Lambda 容器映像上传到 AWS ECR Registry，这将允许我们直接在 AWS Lambda 中使用它。

# 先决条件

*   [Python 3 和 pip3](https://www.python.org/downloads/)
*   码头工人
*   [AWS CLI，已通过您的 AWS 帐户认证](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)

就是这样！我将使用 [Visual Studio 代码](https://code.visualstudio.com/download)，但是请随意使用您喜欢的 IDE 或文本编辑器。我喜欢创建一个新文件夹来存储我的 app.py、Dockerfile 和 requirements.txt 文件，并在 Visual Studio 代码中将其作为工作区打开。

![](img/ce670c12fe62fb43f76a168806e74981.png)

这是我们构建自定义 Lambda 映像所需的三个文件。

# Python 应用程序

互联网上有一些使用 Python 中的 mdbtools 的例子。您可能希望在某种程度上定制我的代码，以满足您的需求(并且可能使代码不那么难看)。

大部分完整的 Lambda 函数代码，使用 mdbtools 将 accdb 文件转换为 CSV 文件。

一旦你完成了 app.py 代码并做了你需要做的事情，用`python freeze > requirements.txt`生成一个 requirements.txt 文件，在终端窗口中与`app.py`在同一个目录下运行。您应该会得到这样的结果:

python“冻结”命令的示例输出，该命令自动在同一目录中查找 python 文件的所有依赖项，以便在新的计算机/服务器上方便地恢复 python 依赖项。

# 码头工人形象

AWS 提供了 docker 容器，这些容器反映了为每一个流行的 Lambda 运行时运行的 Linux 环境。我们将使用 Python 3 的版本。

定义我们的自定义容器的 Dockerfile，与 AWS Lambda 兼容，因为它是从官方 Python 3.8 Lambda 基础映像派生的。

首先，我们将 EPEL 存储库添加到默认的包管理器 yum 中。这将允许我们通过 yum 安装 mdbtools，这比从 mdbtools Github 页面下载一个 tarball 并手动安装要容易得多。

Amazon 记录了如何[将 EPEL 添加到 Amazon Linux 和 Amazon Linux 2](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-enable-epel/) ，但是我没有找到将 EPEL 添加到 Lambda 运行时映像的文档。经过一番痛苦的反复试验，我终于使用了 EPEL 的直接网址。

接下来，我们安装 mdbtools 及其依赖项。然后，我们将 Python 代码复制到`/var/task`目录中(这是自动设置为环境变量`LAMBDA_TASK_ROOT`)。最后，我们使用 requirements.txt 文件来安装所有 python 依赖项，并将它们复制到`/var/task`目录中。

## 创建 Docker 图像

现在我们可以构建我们定制的 docker 映像，它将成为我们的 Lambda 函数(确保`cd`到我们三个文件所在的文件夹中):

`docker build -t our-custom-lambda-function .`

如果您没有看到任何错误，我们的图像已经成功创建，您应该能够在 Docker 桌面应用程序的图像选项卡上的图像列表中看到它。如果您确实看到了错误，请在下面贴出它们，我们会解决它们！

## 测试 Docker 映像

现在我们可以测试 Docker 映像，就像它在 AWS Lambda 中运行一样(有一些限制)。

`docker run -p 9000:8080 our-custom-lambda-function`

随着 Docker 容器的运行，我们可以通过`curl`触发 Lambda 函数，即我们的 Python 代码(如果你在 Windows 上，确保你在 bash shell 中，否则`curl`命令可能无法工作)。

Windows (PowerShell): `curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d "{}"`

macOS/Linux(或带有 bash shell 的 Windows):`curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}'`

*有一个问题是 Windows 会抱怨* `*{}*` *两边的单引号，所以我们用双引号代替。*

使用这个命令，我们向 Docker 容器发送一个空请求，触发 Lambda 函数并执行我们的`handler()` Python 函数中的代码。

我们可以从启动 Docker 容器(`docker run -p....`)的终端窗口中观察 Python 代码的输出(例如打印语句)。

# 上传到 AWS ECS 注册表并在 AWS Lambda 中运行

既然我们已经验证了我们的容器如预期运行，下一步也是最后一步就是将它上传到 AWS 弹性容器注册中心(ECS ),这样我们就可以将它作为 Lambda 函数运行。

## 通过 Docker CLI 验证您的 Amazon ECR 存储库

`aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin {your ECR URL here e.g. 123456789012.dkr.ecr.us-east-2.amazonaws.com}`

![](img/436ce895e2680b10c57d8a2f4e03ba05.png)

AWS 控制台 GUI 中 ECR 内的“创建存储库”页面，显示您的 ECR 存储库的潜在 URL。这是我的。希望这是可以公开的…

使用此命令要检查两件事:1)两个点的区域都正确；2)ECR URL 的 ID/整数值正确。您的答案不会是“123456789012……”。您可以通过 AWS 控制台(Web GUI)转到弹性容器注册页面找到此信息。如果您还没有一个存储库，您会看到“创建存储库”表单，但是请注意，在我们创建第一个存储库之前，就已经显示了 URL 的 ID/整数部分(用红色下划线标出)。这个值在我们所有的存储库中是恒定的，因为它是特定于 ECR 实例的，而不是 ECR 实例中的存储库*。您可以创建您的第一个存储库，或者只需获取该 URL，因为下一步将自动为我们创建存储库(如果它还不存在的话)。*

*如果这不是您的第一个存储库，而不是“创建存储库”表单，您会看到您的私有和公共存储库的列表。由于您的 ECR 实例中的所有存储库的 ID 值都相同，所以您可以从现有的存储库中获取一个 URIs，并用您想要用于存储库的名称替换最后一部分(在“/”之后)。*

## *在 ECR 中创建存储库*

*如前所述，您可以使用 AWS 控制台创建存储库，也可以使用 AWS CLI:*

*`aws ecr create-repository --repository-name my-repo-name --image-scanning-configuration scanOnPush=true --image-tag-mutability MUTABLE`*

*您的存储库名称不一定要与您的 Docker 映像的名称匹配，但是让它们匹配以最大程度地减少潜在的混淆也不是一个坏主意。*

*请注意，AWS CLI 命令将`scanOnPush`设置为`true`并允许[图像标签可变性](https://docs.aws.amazon.com/AmazonECR/latest/userguide/image-tag-mutability.html)。可变性很好，因为这意味着当我们将容器图像推送到 ECR 存储库时，我们可以为容器图像的未来版本重用“最新”标签。*

*启用`scanOnPush`意味着 AWS 将[自动扫描我们上传到该存储库中的容器图像以查找漏洞](https://docs.aws.amazon.com/AmazonECR/latest/userguide/image-scanning.html)。对于特定 ECR 存储库中的特定图像，可通过图像详细信息页面“扫描”部分的 AWS 控制台获得扫描结果。*

## *将码头工人图像标记(“映射”)到 ECR*

*接下来，我们调用 Docker 的[标记命令](https://docs.docker.com/engine/reference/commandline/tag/)来告诉 Docker 将我们的图像推送到哪里。*

*`docker tag our-custom-lambda-function:latest {your ECR repository URL here e.g. 123456789012.dkr.ecr.us-east-2.amazonaws.com/ECR-repo-name:latest}`*

*现在我们正在使用 ECR 存储库的完整 URI，它包括`/our-repo-name:latest`。*

## *将 Docker 映像推送到 ECR 存储库*

*现在我们让 Docker 将我们的图像推送到 ECR:*

*`docker push {your ECR repository URL here e.g. 123456789012.dkr.ecr.us-east-2.amazonaws.com/ECR-repo-name:latest}`*

*您应该会在您的终端窗口中看到几个进度条，显示 Docker 图像每一层的上传进度。*

*如果您得到一个关于权限或授权的错误，那么您用来向 AWS 认证 Docker 的命令可能有问题——可能是区域不正确，或者是 ECR URL 的 ID 部分(或区域部分)。只需使用正确的参数重新运行该命令，就可以重新尝试`push`命令了。*

## *从图像创建一个 Lambda 函数*

*最难的部分已经过去了！现在我们的容器映像在 AWS 中，我们可以用它来创建一个新的 Lambda 函数。这是一个非常简单的过程:我们只需进入 AWS 控制台的 Lambdas 页面，点击页面右上角的“创建函数”，然后在**创建函数**页面上选择“容器图像”。完成后，我们可以粘贴 ECR 图像 URI，或者点击“浏览图像”按钮选择它。*

*![](img/e4086ef8bf880d232f75a1fcdd28c1fb.png)*

*AWS 控制台中的“创建函数”页面允许我们选择 ECR 图像作为 Lambda 函数运行。*

*就是这样！根据需要配置触发器、超时和任何其他设置，然后开始。*

# *笔记*

*   ***使用相同标签(例如“latest”)推送更新的图像不会自动将该更新的图像部署到 Lambda 函数**。您必须通过 AWS 控制台中 Lambda 函数页面上的“部署新映像”来手动部署更新的映像(在“映像”选项卡下)。这是因为 Lambda 不是通过标签引用 ECR 映像，而是通过“摘要”(又名 sha256 hash)。*
*   *使用相同的标签推送更新的图像将从图像的现有版本中移除该标签，并且旧图像将不再具有标签。*

# *来源*

*   *[创建 Lambda 容器图像，AWS](https://docs.aws.amazon.com/lambda/latest/dg/images-create.html)*
*   *[EPEL](https://fedoraproject.org/wiki/EPEL)*