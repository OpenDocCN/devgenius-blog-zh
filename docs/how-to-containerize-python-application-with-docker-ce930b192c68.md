# 如何用 Docker 容器化 Python 应用

> 原文：<https://blog.devgenius.io/how-to-containerize-python-application-with-docker-ce930b192c68?source=collection_archive---------13----------------------->

![](img/656b26dd42a963865578405165e19a20.png)

**目录**

1.  为什么要用 dockers？
2.  创建自动化 Python 脚本
3.  为 Python 脚本创建 Docker 文件
4.  为 Python 脚本生成 Docker 图像
5.  运行需要人工输入 docker 图像

**1。为什么要用 dockers？**

Docker 使开发者能够通过使用容器来开发和部署应用程序。容器是一个软件包，它允许开发人员将应用程序与其可执行文件、需求、配置、库打包在一起；简而言之，运行该特定应用程序所需的一切。我们收到的最终产品是 Docker 图像。通过创建 Docker 映像，开发人员不必担心应用程序能否在任何给定的系统上正确执行。这个过程减少了应用程序的部署时间，并使用了最少的客户端系统资源。现在让我们向码头工人的世界迈出第一步，创造我们自己的码头工人形象。对于本教程，我假设你已经安装了 Docker 和 Python。

**2。创建自动化 Python 脚本**

你曾经觉得你不知道该看什么，只是想感到惊讶，那么我给你掩护，我的朋友。让我们编写一个快速的 python 脚本，从 IMDB 中抓取 250 部收视率最高的电影，并向您推荐一部随机电影。因为这是一篇与 Docker 相关的文章，所以我们不会深入 python 脚本的细节。

**3。为 Python 脚本创建 Docker 文件**

一旦我们有了 python 脚本，我们需要创建一个 docker 文件。docker 文件基本上是构建图像的蓝图。Docker 映像是运行容器(即您的应用程序)的模板。现在在同一个目录中创建一个名为 Dockerfile 的文件。我建议使用 visual studio 代码，因为它有微软认证的 Docker 扩展。

编写 Docker 文件的第一步是指定基本映像。因为我们正在使用 python，所以我们将使用 python 图像。

`FROM python:3.8`

接下来，我们需要在 Docker 文件中添加可执行代码，在本例中是我们的。py 文件。指定的格式是:

1.  “添加”命令
2.  后跟可执行文件的名称“file.py”
3.  然后是我们的 exe 文件的目的地，在这个例子中是“.”因为我们的代码在同一个目录中

`ADD main.py .`

之后，我们需要安装第三方库。其格式为:

1.  命令“运行”
2.  接下来是“pip 安装”
3.  后跟所需库的名称

`RUN pip install beautifulsoup4 requests`

最后是容器运行时执行的命令语句。

`CMD ["python" , "./main.py"]`

您的 docker 文件应该如下所示:

瞧啊。你的文件准备好了。

**4。为 Python 脚本生成 Docker 图像**

现在让我们建立一个 docker 图像。在终端中执行命令

`docker build -t movie-randomizer .`

此命令中的“.”最后指定 docker 文件的目的地。执行该命令后，您最终得到了 docker 图像。

接下来，我们需要运行 docker 映像，为此我们将使用命令

`docker run movie-randomizer`

这应该会给你这样的输出

`Good Will Hunting (1997) Rating: 8.3 Starring: Gus Van Sant (dir.), Robin Williams, Matt Damon`

恭喜你！！！你终于编写了你的第一个 python 脚本并成功执行了它。

**5。运行需要人工输入的 docker 映像**

现在，让我们说，我们需要运行一个脚本，其中包括某种人类交互。让我们修改一下我们的 python 脚本，以防我们已经看过了建议的电影并想跳过它。在代码中实现以下更改。

现在构建 docker 映像并执行`docker run movie-randomizer`

这将导致:

`tmp = input(“Would you like another suggestion? kindly type [y]es or [n]o. “)
EOFError: EOF when reading a line`

为了避免这种错误，我们需要在运行映像时进行一些更改。您需要使用的命令是:

`docker run -i -t movie-randomizer`

这里的“-i”代表用户输入的交互模式，而“-t”代表 sudo 终端，即用户可以书写他/她的输入的终端。执行该命令后，您已经学会了如何运行交互式 docker 图像。

这是所有的乡亲。愿原力与你同在！