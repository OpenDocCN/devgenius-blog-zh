# 厌倦了等待？同时在 TestPyPI 上上传您的拉取请求

> 原文：<https://blog.devgenius.io/tired-of-waiting-upload-your-pull-request-on-testpypi-in-the-meantime-4d1d44b5bafc?source=collection_archive---------16----------------------->

![](img/3b1a9aace50c4ffc4677c82f060fa987.png)

由[阿格巴洛斯](https://unsplash.com/@agebarros?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

你有没有遇到过好的储存库不再更新的情况？你在那里解决了一些问题，提出了一个拉取请求，但是没有得到维护者的反馈？厌倦了等待？为什么不自己发表呢？在这里我将和大家分享我在 [suntime](https://github.com/SatAgro/suntime) 发布我的[拉请求](https://github.com/SatAgro/suntime/pull/19)到 TestPyPI 的故事。

# 1.背景

我使用 [suntime](https://github.com/SatAgro/suntime) 作为我的包的依赖项，用于过滤基于日出和日落的时间序列数据，称为 [dayfilter](https://github.com/yasirroni/dayfilter) 。可悲的是，这里有一个几乎两年前的问题。因此，我发出我的[拉请求](https://github.com/SatAgro/suntime/pull/19)来解决这个问题。在等待我的拉取请求的状态时，我在想，为什么不自己发布它呢？

# 2.正在处理您的拉动式请求

在将您的 pull 请求发布到 TestPyPI 之前，您应该首先发出一个 pull 请求。在 GitHub 上，按下您想要发出 pull 请求的存储库的 fork 按钮。之后，将您的 fork 克隆到本地存储库。

git 克隆命令

为您的修改创建一个单独的分支。这是贡献公共存储库的最佳实践，因为它不会将您的修改推送到主分支或主分支。如果用您想要共享的特性的名称来命名您的新分支会更好。

git 创建一个新的特性分支

在做了一些更改之后，您可以添加、提交和推送您的更改到 GitHub 上的 fork 存储库中。然后，您可以从 GitHub 发出一个 pull 请求。

git 添加、提交和推送

# 3.推送至 TestPyPI

首先，创建一个名为 TestPyPI 的新分支。

git 创建一个新的 TestPyPIi 分支

第二，更新 setup.py 文件，避免与最初的维护者发生任何冲突。你应该编辑**包名**、**版本**和 **url** 。对于包名，您应该在 name 部分进行编辑，不要与 package 部分混淆。name 部分是 PyPI 上的包名，而 package 部分是导入包时的包名。对于版本，您可以在原始版本后添加新的点和编号。见下面的模板，原来的 [suntime setup.py](https://github.com/SatAgro/suntime/blob/master/setup.py) ，还有我新的 [suntime setup.py](https://github.com/yasirroni/suntime/blob/test-pypi/setup.py) 。

在 setup.py 上编辑位置

原始 suntime setup.py

TestPyPI 分支上的新 setup.py

第三，做一个 TestPyPI 账号(如果你还没有的话)。可以在[官网](https://test.pypi.org/account/register/)新建一个 TestPyPI 账号。最后，您已经准备好构建并发布到 TestPyPI。

在 TestPyPI 上发布的命令

要阅读关于发布软件包的完整教程，请访问[官方文档](https://packaging.python.org/en/latest/tutorials/packaging-projects)。

# 4.从 TestPyPI 安装

使用下面的例子从 TestPyPI 安装这个包。该选项将使 pip 查看指定的 python 包索引(TestPyPI)，但允许在标准 PyPI 上搜索依赖项。详情请阅读[正式文件](https://pip.pypa.io/en/stable/cli/pip_install/#options)。

从 TestPyPI 安装软件包的 pip 命令也依赖于 PyPI

祝贺您在 TestPyPI 上发布您的拉取请求！你也可以试着在下面安装我的 suntime 的 pull request。

安装 suntime-yasirroni form TestPyPI 的 pip 命令

但是，为什么是 TestPyPI 而不是 PyPI 呢？这是因为 TestPyPI 更像是一个非正式的、个人的和实验性的 Python 软件仓库。如果您觉得您的 pull 请求值得一个真正的 stage，您可以重命名这个包并将其上传到 PyPI。

发布到 PyPI 的命令

这就是本教程的全部内容。我希望你已经发现这是有用的。如果你想阅读另一个与 git 相关的故事，你可以尝试阅读我的关于管理你的公共存储库的私有分支的最佳方法的故事。

[](/manage-your-private-git-branch-9f79cb7796a6) [## 管理您的“私有”git 分支

### 您是否希望在一个存储库中为您的公共和私有分支建立一个单独的分支？你可能会…

blog.devgenius.io](/manage-your-private-git-branch-9f79cb7796a6)