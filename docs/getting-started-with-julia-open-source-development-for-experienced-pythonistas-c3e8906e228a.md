# 为有经验的 Pythonistas 开始 Julia 开源开发

> 原文：<https://blog.devgenius.io/getting-started-with-julia-open-source-development-for-experienced-pythonistas-c3e8906e228a?source=collection_archive---------8----------------------->

作为一个主要从事 python 项目的人，从你的第一个 Julia 包开始可能有点令人生畏。在 2022 年，作为一名 python 用户生活在 GitHub 上是相当舒适的，因为有大量的自动化框架用于林挺、测试和文档。虽然不像 Python 生态系统那样多样化，但 Julia 确实有实现相同目标的工具。在这里，我们将试图帮助你理解 Julia 中的包开发机制，以及如何将它集成到 GitHub 中。本文将讨论如何设置 Julia repo 的机制，如何在 GitHub 中自动化一些基本的持续集成工作流，以及如何自动构建文档。

关于 Julia 包开发的一般信息:[这篇来自 Julia 的官方文档和 YouTube 视频的文章](https://julialang.org/contribute/developing_package/)非常有用，应该被认为是一个先决条件，尽管一些关键点后来被翻新。但是在我们建立一个库之前，我们必须首先了解 Julia 的代码在你的计算机上的位置，以及它们是如何组织的。

# Julia 开发环境的心智模型

![](img/9420ab5f738dc04c91755fe3d378f4f3.png)

照片由 [Jonathan Chng](https://unsplash.com/@jon_chng?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

虚拟环境在 Julia 中被称为*项目*环境，它们的配置将在其他[文章](https://towardsdatascience.com/how-to-setup-project-environments-in-julia-ec8ae73afe9c)中介绍。在这里，我们将讨论如何考虑这些环境，以及在出现问题时应该做些什么。我们在`conda`中称之为*通道*的东西在 Julia 中被称为*注册表*。注册表包含了大量软件包的所有依赖信息。[通用](https://github.com/JuliaRegistries/General)注册表可以认为是 Julia 的`pypi`。Julia 管理包的方式和 python 管理包的方式以及 Julia 管理包的方式之间最大的区别在于，Julia 包都保存在**一个地方**。虽然对于熟悉 python 虚拟环境的人来说，这听起来有些奇怪，但是这个决定实际上是非常明智的，因为系统上使用的不同 Julia 包将存储在一个特定的散列下(例如`Graphs.jl`的 1.7.2 版本存储在`~/.julia/packages/Graphs/6MoQZ`下，不需要复制到系统的其他地方。每个项目本质上都有一组指令，告诉你可以访问`Manifest.toml`文件中的哪些包。从某种意义上说，`Project.toml`与`requirements.txt`的作用相似，而`Manifest.toml`与`conda`中的物理虚拟环境目录的作用相似。

如果你已经使用 python 虚拟环境足够长的时间，你无疑已经破坏了你的环境几次，并且需要对整个环境进行核爆。在 Julia 中，这可能发生在注册表更新期间，或者如果您编辑了一些不应该编辑的代码。修复方法实际上和 python 中的完全一样，你可以删除单个的包或者用[销毁你的整个*通用*注册表](https://stackoverflow.com/questions/66114900/unsatisfiable-requirements-detected-for-the-package-in-julia)，这取决于你的注册表的混乱程度，然后只需安装你正在处理的任何项目的`Project.toml`文件所需的所有包。

# 创建新的包

现在我们知道了朱莉娅的线人住在哪里。我们可以创建一个新项目并开始开发。在朱莉娅·REPL 中，运行以下命令来安装`PkgTemplates`:

```
using Pkg
Pkg.add("PkgTemplates")
```

要创建您的包，请在朱莉娅·REPL 中执行以下命令:

```
julia> using PkgTemplates
julia> t = Template(;
           user="<GITHUB_USERNAME>",
           license="MIT",
           dir="./",
           authors=["<YOUR NAME>"],
           plugins=[
               Git(ssh=true),
               GitHubActions(),
               Documenter{GitHubActions}()
           ]) 
julia> t("MyPkg.jl")
```

注意`dir="./"`假设您使用当前目录来开发 Julia 包。如果未提供该字段，包裹将被放置在`~/.julia/dev`。命令完成后，您应该有一个包含`.git`和`.github`工作流的`MyPkg.jl`目录，准备好被推送到 GitHub。

# 连续累计

![](img/aad58d4c643b7c4ff5a3e7b496f974bb.png)

丹尼尔·劳埃德·布兰克-费尔南德斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

使用该模板，我们创建了一个空项目，其中包含所有必需的文件，包括三个 GitHub 工作流`yml`文件:

```
% tree .github                                                                                                         
.github
└── workflows
    ├── CI.yml
    ├── CompatHelper.yml
    └── TagBot.yml
```

这些文件确实是

*   `CI.yml`包含测试工作流，这也是使用`Documenter.jl`自动生成的文档工作流所在的地方。
*   更新你的包依赖关系，本质上是 Julia 的依赖机器人。
*   `TagBot.yml`运行一个 [TagBot](https://github.com/JuliaRegistries/TagBot) 工作流，为你注册的 Julia 包创建标签、发布和变更日志，很像`pypi`上的 python 代码的 [twine](https://pypi.org/project/twine/) 。

因为这个包已经被创建为一个 git repo，使用你的 GitHub 用户名，你只需要确保这个 repo 存在于 GitHub 上，然后使用。

```
> git push -u origin main
```

在当前状态下，repo 在`.github`下的 GitHub 动作会根据`test/runtests.jl`中的内容自动测试你的代码。现在，您可以开始开发和测试您的代码了！