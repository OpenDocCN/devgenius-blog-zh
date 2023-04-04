# 创建 Python 包|诗歌快速入门指南

> 原文：<https://blog.devgenius.io/creating-a-python-package-poetry-quickstart-guide-dea4ac685007?source=collection_archive---------1----------------------->

![](img/db5ffbc060d46396d5d84c0c30f1a017.png)

来源:[https://dotmethod.me/posts/poetry-python-package-manager/](https://dotmethod.me/posts/poetry-python-package-manager/)

# 什么是诗歌？

[poems](https://python-poetry.org/)是一个 Python 打包和依赖管理工具。它使管理依赖关系变得容易，并提供了一个`poetry.lock`文件，确保用户环境的一致性和可再现性。

使用`poetry`的一些额外好处:

*   `poetry`自动为你的项目生成和管理虚拟环境。只要运行`poetry shell`就能进入一个隔离的环境。
*   `poetry`可以自动更新项目/包的依赖关系。`poetry update`会将您的所有依赖项更新到最新版本。
*   `poetry`可以轻松构建 python 包并发布到 PyPI。
*   `poetry`也可以将它的依赖树导出为`requirements.txt`，这样它就可以在没有其他包管理器的情况下兼容。

关于`poetry`特性的完整列表，请查看官方[诗歌文档](https://python-poetry.org/docs/cli/)。

# 装置诗歌

在 Linux/macOS/WSL 上

```
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
```

在 Windows 上

```
(Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python -
```

如果您在使用上述方法时遇到任何问题，请尝试

```
pip install poetry
```

它应该可以在大多数机器上工作。

要确认您的安装，请运行

```
poetry --version
```

如果你看到类似`Poetry version 1.1.x`的东西，我们就可以走了！

如果你面临任何挑战，你可以参考[安装指南](https://python-poetry.org/docs/#installation)。

# 创建新的诗歌项目

要启动一个新的 Python 项目，运行

```
poetry new <project_name>
```

这将创建一个具有以下结构的新项目:

```
SERVUS
│ pyproject.toml
│ README.rst
│
├───servus
│         __init__.py
│
└───tests
        test_servus.py
        __init__.py
```

我使用`servus`作为我的项目名称。我将在以后的文章中写更多关于这方面的内容。

> *潜行峰:* `*servus*` *是一个对人友好的包装器，用于* `*aiohttp*` *库，用于发出异步 web 请求。*

每个文件是做什么的？

*   *pyproject.toml* :不是针对`poetry`的。它的作用类似于`setup.py`，指定包的依赖项和属性。你可以在这里阅读更多关于那个[的内容](https://rotadev.com/what-is-pyproject-toml-file-for-dev/)
*   *README.rst* :重构包文档的文本。我更喜欢使用降价的方式，所以我会将其更改为`README.md`，但这是基于偏好的。
*   *servus* :包源文件夹。这是您为包放置所有代码和逻辑的地方。
*   *检测*:保存我们包裹的所有检测。

# 安装依赖项

`servus`依赖于`[aiohttp](https://docs.aiohttp.org/en/stable/)`库，所以我将把它作为需求/依赖项安装在我的项目中。

在您的终端中，从与您的`pyproject.toml`相同的目录运行

```
poetry add aiohttp
```

这将更新我们的 *pyproject.toml* 和*诗歌。用新的依赖锁定*。

如果一个合作者想建立我们的项目并为之做出贡献，他们只需要克隆我们的项目文件夹并运行

```
poetry install
```

# 运行代码

首先，我们需要创建一些要运行的代码。

在我们的包源文件夹(`*servus*`)中，创建一个新文件(`*async_api.py*`)。

```
import aiohttp
import asyncio
import pprintasync def make_web_request(url):
    async with aiohttp.ClientSession() as session:
        response = await session.get(url)
        data = await response.json()
    return dataasync def main():
    data = await make_web_request("https://restcountries.com/v3.1/name/nigeria")
    pprint.pprint(data)# Uncomment for Windows :)
# asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())asyncio.run(main())
```

这只是一些基本的代码，向 API 发出异步 web 请求并打印出一些数据。如果您不熟悉异步 Python 编程，您可以查看我之前的文章[用 Python 创建异步 Web 请求](/creating-asynchronous-web-requests-with-python-asyncio-aiohttp-d5469d37cc23)。

有很多方法可以运行我们的代码。

1.  使用`poetry shell`:

*   从与 *pyproject.toml* 相同的目录运行`poetry shell`。这将创建一个新的 shell 实例，并激活一个虚拟环境。如果没有立即点击，那没问题。您只需要记住，要离开这个孤立的环境，键入`exit`而不是传统的`deactivate`。这将“杀死”新的外壳过程。
*   运行`python servus/async_api.py`你会得到一些漂亮的输出(双关语)。

2.使用`poetry run`:

*   运行`poetry run python servus/async_api.py`。就这样。简短、简洁、美观。`poetry`处理激活我们的虚拟环境和运行过程。您也可以使用它与其他程序如`poetry run black .`一起运行黑色格式化程序。

3.激活已有`poetry`虚拟环境:

*   运行`poetry env list --full-path`。您会得到一个为您的项目创建的虚拟环境列表`poetry`。然后你可以像激活任何由`venv`创造的传统虚拟环境一样激活它。
*   我不建议你走这条路。您会遇到许多错误行为。

# 共享您的项目

当您在项目中运行`poetry add`或`poetry install`时，如果`poetry.lock`文件尚不存在，则会生成该文件。`poetry.lock`“锁定”了项目中使用的 Python 包的确切版本，因此很容易创建可再现的和确定性的 Python 环境。

建议您将其添加到版本控制中，就像在 JavaScript 中添加`package-lock.json`一样。

合作者只需要运行`poetry install`就可以跟上速度。

# 进一步阅读

谢谢你的阅读！以下是一些您可能会发现有用的资源:

*   [官方诗歌文献](https://python-poetry.org/)
*   [Python/PyPI 依赖关系图的状态](https://ogirardot.wordpress.com/2013/01/05/state-of-the-pythonpypi-dependency-graph/)
*   [以正确的方式管理 Python 包](https://opensource.com/article/19/4/managing-python-packages)
*   [Asyncio 101 |编写异步 Python 程序](/asyncio-101-writing-asynchronous-python-programs-61b732ca77b1?gi=cb45f23a5b48)

> ***有项目或需要建议吗？*** [***领英***](https://linkedin.com/in/emmanuel-katchy) 上取得联系

*最初发布于*[*https://tobe tek . hashnode . dev/creating-a-python-package-诗词-quickstart-guide*](https://tobetek.hashnode.dev/creating-a-python-package-poetry-quickstart-guide)