# 分析师 Julia:更好的开始——更大的图景(#5)

> 原文：<https://blog.devgenius.io/julia-for-analysts-tips-for-better-beginnings-the-bigger-picture-5-3fe7e4cb110f?source=collection_archive---------10----------------------->

## 回顾一下 Julia 生态系统还提供了什么(修订、日志、调试器)。Julia 只是任何项目(模型/洞察/应用)成功的一小部分，所以在版本控制、实验记录、轻松输出和自动化方面投入时间。

![](img/b553dfcc8a1bdcf9525fe1d699821b68.png)

美国宇航局在 [Unsplash](https://unsplash.com/s/photos/earth?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

# 我希望早点使用的软件包

# Revise.jl

[Revise.jl](https://timholy.github.io/Revise.jl/stable/) 是切片面包以来最好吃的东西！你的(加载的)代码会在你每次修改时神奇地更新，你不需要重启 Julia。

我曾经简单地用`includet()`跟踪我的脚本，但是我现在已经开始为每个迷你分析创建一个专用的包(用`]generate`)。一旦它有了自己的文件夹，它就有了自己的项目规范——还有几个下游的好处。

# PkgTemplates.jl

如果你正在考虑出版你的作品，你应该总是以 [PkgTemplates.jl](https://invenia.github.io/PkgTemplates.jl/) 开始你的项目。

现代 Julia 包的交钥匙解决方案——它可以设置你的文件夹结构、文档、测试、git、Github 动作等等。

# 测试. jl

测试再简单不过了(`using Test`)！我认为一个轻量级测试套件的开销大约是每个函数 2 分钟，这将为您节省无数的调试时间！

我的工作流程:

*   我应用“三原则”——如果我使用某个逻辑 3 次或更多次(或者知道我会这样做)，我将它包装在一个函数中，生成轻量级的文档字符串，并添加一个简单的断言语句来测试预期的输出(`@assert X==Z`)。
*   这个函数存在于`src`文件夹的一个文件中，但是 Revise.jl 包负责重新加载我所做的每一个修改。
*   然后，我将代码移动到我的项目的`test`文件夹中的一个文件中，并用我的项目添加注册它(`test/runtests.jl`中的`include(data_handlers.jl)`行)
*   剩下唯一要做的事情就是用`@assert`替换`@test`(可选地将几个测试打包成`@testset` s)并用`]test`运行所有的测试(在 REPL 包模式下)。

# Logging.jl

标准库的一部分，但仍然很棒。Macro `@info`是你的朋友，你再也不会用 print 了！

您可以使用 [LoggingExtras.jl](https://github.com/JuliaLogging/LoggingExtras.jl) 定制您的日志程序。

# 调试器. jl

Debugger.jl 是一个很好的工具，可以交互式地调试你的代码，而不需要做任何初始设置。

如果你所关心的只是设置一个断点并能够检查上下文，那么 Infiltrator.jl 是一个更轻便的选择。

当我从`my_func_that_breaks()`得到一个奇怪的错误时，我的典型工作流程

```
using Debugger
names(Debugger) # to remind myself of the break_on_error function
break_on_error(true) 
@run my_func_that_breaks()
# once inside, use ? for help and q to quit
```

# JET.jl

一个很棒的代码分析器， [JET.jl](https://github.com/aviatesk/JET.jl) ，可以在你运行代码之前发现你的错误。

简单跑:`using JET; report_and_watch_file("my_script.jl",annotate_types=true)`

# JuliaSyntax.jl

JuliaSyntax.jl 是一个令人兴奋的新语法解析器，它会给你更好的错误信息。我在等 0.1 版本加到我的 startup.jl，看看 [JuliaCon 2022 视频](https://www.youtube.com/watch?v=CIiGng9Brrk)。

# Term.jl

[Term.jl](https://github.com/FedeClaudi/Term.jl) 大多是为了构建漂亮的 CLI 和终端应用(类似于 Python 中的 [Rich](https://rich.readthedocs.io/en/stable/introduction.html)

然而，它有几个日常使用的好方法，例如，`termshow(func)`用于显示可用的方法，即将发布的`@showme`宏将向您显示给定方法实例的定义。

# JuliaFormatter.jl

juliaformat . JL 是一个很棒的节省程序，它让你的代码更具可读性。

你需要做的就是在你的项目目录下运行:`using JuliaFormatter; format(".")`。

# Literate.jl

[Literate.jl](https://fredrikekre.github.io/Literate.jl/v2/) 是你通往识字编程的门户】(https://en . Wikipedia . org/wiki/Literate _ Programming)。

简而言之，你可以把你的代码写成一个易于阅读和版本控制的脚本，但是用一个命令你就可以把它导出到 Markdown，Notebooks 等等。

最好的部分是，你几乎不需要改变任何东西——只需要在这里和那里添加一个`#`就可以实现想要的布局。

识字编程有什么好处？

在数据的世界里，你的代码运行和测试成功通常是不够的，你需要能够解释它并为其他利益相关者和你自己记录它(有时一年有 100 多个项目，你真的记得你做了什么吗？).

配合 Quarto 使用更是如虎添翼。

# 四开

[Quarto](https://quarto.org/) 是“一个基于 Pandoc 的开源科技出版系统”(你可以说它是面向 Python 和 Julia 用户的 Rmarkdown+knitter 的跨平台继承者)。

对于参数化运行和以多种输出格式(演示、HTML 报告等)发布漂亮的报告，我还没有见过更好的工具。

参见[使用四开本](https://quarto.org/docs/computations/julia.html)中的茱莉亚。

我的工作流程通常是将我的脚本放入 Quarto Markdown(使用 Literate.jl ),然后生成一个 HTML 报告(用于分发)和一个用于会议的交互式 Revealjs 演示。作为一名前管理顾问，我无法相信创建如此漂亮和专业的演示是如此容易。

# 重新思考你的整体工作流程

Julia 只是任何项目(模型/洞察/应用)成功的一小部分。投入时间围绕它构建一个健壮的工作流。

下面的概述提供了一些适合我和我的用例的要点。

## 我的项目生态系统

## 项目结构+文档

*   以任何合作者都知道如何导航的方式组织你的项目(例如，使用 PkgTemplates.jl 并从 [Cookiecutter](https://cookiecutter.readthedocs.io/en/stable/) 文件夹结构中获得灵感)
*   首先，在 README.md 文件中描述您的项目，并逐步解释如何运行它(/重新训练模型)
*   稍后，用例子构建适当的文档(例如，Literate.jl + Documenter.jl)

## 版本控制

*   使用 git 的源代码(例如，使用 Github 桌面，它让一切变得毫不费力)
*   数据(例如，使用 DVC 与自动气象站 S3 后端，这是非常便宜和容易设置)

## 将科学放回数据科学中

*   跟踪你的实验，并朝着你的成功指标前进(例如，MLFlow 很棒——你可以使用 [MLFlowClient.jl](https://github.com/JuliaAI/MLFlowClient.jl) 或 [DVC 实验跟踪](https://dvc.org/doc/use-cases/experiment-tracking))

## 发布精美的报告和演示文稿

*   永远没有足够的评论和工件来描述你所应用的逻辑，或者讨论报告和发现的报告(例如，使用 Literate.jl + Quarto)

## 自动化您的工作流程

*   本地:如果你经常在你的 shell 中重复一些命令，将它们封装到一个 [Makefile](https://opensource.com/article/18/8/what-how-makefile) 中，或者在你的 shell 的配置文件中添加一个别名(参见文章#1)
*   在云上:Github Actions 或 Azure Pipelines 可以在每次推送时运行你的测试套件和任何其他检查。这对于打击那些需要很长时间才能浮出水面并且需要几分钟才能安装好的 bug 来说是无价的。

我见过使用但没有经验的荣誉奖:

*   [DrWatson.jl](https://github.com/JuliaDynamics/DrWatson.jl) 的广告词是“你科学探索的完美助手”。它已经被用于几个项目，包括英国的流行病学建模( [Epimap.jl](https://github.com/epimap/Epimap.jl-public) )
*   当你必须与一些远程源保持同步时

*原发布于*[*https://siml . earth*](https://siml.earth/scratchpad/analysts_tips_for_beginnings5/)*。*