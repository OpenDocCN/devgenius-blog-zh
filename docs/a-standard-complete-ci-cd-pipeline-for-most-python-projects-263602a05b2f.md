# 适用于大多数 Python 项目的标准且完整的 CI/CD 管道

> 原文：<https://blog.devgenius.io/a-standard-complete-ci-cd-pipeline-for-most-python-projects-263602a05b2f?source=collection_archive---------0----------------------->

## 编程；编排

## 对于大多数 Python 项目来说，这是您能找到的最完整的(也是标准的)CI/CD 管道。现在花更多的时间维护你的项目，而不是修复 CI/CD 问题。

![](img/f6cf062d26b10f303d6026b1aca93f6a.png)

托管和使用 GitHub 服务以获得平和的心态|来源— Somraj Saha(作者)

你是否曾经花费大量时间修补 CI/CD 工具，而不是为你的 Python 项目编写代码？

我确实做了！有时[诗歌](https://python-poetry.org/)由于虚拟环境无法安装依赖项。或者其他时候，由于某些原因，依赖关系不会缓存。最重要的是，CI/CD 工具很难调试 dude 来掩盖错误消息。

因此，我分享这个 GitHub 动作工作流，我在我的大多数 Python 项目中都使用它。它开箱即用，无需任何修补&让您走上发布项目的正确道路。工作流程非常简单，但并没有在维护最佳编码标准所需的一些最主要的 CI/CD 原则上妥协。保持它的最小化也意味着，您可以自由地在它的基础上进行进一步的改变和改进。

也就是说，以下是您从该工作流程中获得的内容，开箱即用，无需任何更改:

*   林挺&将所有 PRs &上的`Pylint`、`Black`、&、`isort`的代码格式化推送到远程存储库。
*   在合并 PR 之前，运行集成测试套件来捕捉任何重大变更。
*   缓存依赖项以加快工作流执行时间。
*   上传覆盖报告到 [CodeCov](https://about.codecov.io/) 用于后续的覆盖报告。

因此，如您所见，该工作流并没有做太多事情，但确保了最低限度的 CI/CD 原则得到遵守。最重要的是，您很快就会看到，您可以在此基础上进行构建。

# 关于工作流程

Python 的包管理场景并不值得称道(**来源**:&[【2】](https://news.ycombinator.com/item?id=10000479))。再加上那些打包问题，由于 virtualenv 的需求，设置 CI/CD 工具也相当复杂(至少在 GitHub Actions 上)。因此，我在互联网上搜索，为 Python 项目找到了最佳的 CI/CD 设置。虽然即时可用的诗歌对于本地开发来说是一个很好的 CLI 工具，但是它并不适合 CI/CD 平台。有了诗歌，你可以管理本地的 virtualenvs，就像从你的终端上发布你的项目一样简单！

但那是体力劳动。作为开发人员，我们经常提交&定期推送至远程存储库。重复的手工任务容易出错，因此增加了 bug 或破坏项目变更的机会。因此，我的目标是在不花费太多时间设置 CI/CD 工具的情况下解决这个问题。

目标是使设置尽可能简单和最小化，但应符合 CI/CD 原则的现代标准。

换句话说，设置应该能够执行林挺和/或格式化任务，运行测试套件，生成覆盖报告，并将报告上传到 CodeCov。这些是任务，设置**应该至少有**。因此，极简主义的原则被铭记在心。

我还假设大多数项目都托管在 GitHub 仓库中，所以设置只对具有 GitHub 动作的**起作用。如果你想使用其他 CI/CD 平台，如[Travis CI](https://www.travis-ci.com/)/[circle CI](https://circleci.com/)，那么你可能会想去别处看看。**

也就是说，您可以将下面共享的代码片段复制到项目的`.github`目录下一个恰当命名的`<NAME-OF-THE-WORKFLOW>.yml`中。例如，我通常将文件命名为`test_suite.yml`。GitHub 可以从那里自动识别你的工作流文件。一旦您将提交推送到远程存储库，工作流就应该启动了。您可以在`https://github.com/<GITHUB-USERNAME>/<PROJECT-NAME>/actions?query=workflow%3A%22Test+Suite%22`访问它。

也就是说，这里是 CI/CD 管道的代码片段。随意复制+粘贴。😉

适合大多数 Python 项目的标准 CI/CD 管道

# 工作流程的简要概述

如果你像我一样不耐烦，想浏览一下这篇文章，你应该知道以下几点:

*   工作流在 PR & push 事件上执行。当有人制作公关时，`Test Suite`工作流就会运行。当您将本地提交推送到远程存储库时，也会发生同样的情况。
*   该工作流由两个作业组成:`linter` & `test`。后者依赖于前者。因此，如果`linter`失败，将跳过`test`的执行。
*   `linter`运行在 Ubuntu 虚拟机上&为林挺&安装`Pylint`、`Black`、`isort`格式化代码。它们也被缓存以减少执行时间。
*   `test`分别运行在一台 MacOS、一台 Ubuntu &一台 Windows VM 与 Python 版本- `3.8` & `3.9`。请注意，这些运行是并行发生的，与彼此的执行状态无关。
*   `test`作业还会缓存&安装存储在`.venv`目录下的 virtualenv。然后用 PyTest 运行测试套件，PyTest 会生成一个`coverage.xml`报告并上传到 CodeCov。

因此，正如您所看到的，即使工作流尽可能保持最小化，它仍然完成了很多任务。事实上，这些任务中的大部分对于保持项目的最低质量标准是必不可少的。

无论如何，在简要概述了工作流的作用之后，让我们更深入地看看每一行代码是为了什么而编写的。下一节将尽可能详细地描述它。

# 对工作流程的深入解释

文件的顶部是`name: Test Suite`键值对。它描述了 GitHub 在其 web UI 中显示的工作流的名称。接下来的一行，`on: [pull_request, push]` pair 描述了应该触发工作流的事件。

`jobs:`部分描述了应该并行运行的不同任务(没有必要，稍后会详细介绍)。作为一个极简主义者，这个工作流程描述了两个工作:a `linter` & a `test`。作业的名称有意保持自描述性。正如前面提到的，当一些代码被推送到存储库或者 PR 被创建时，`linter`执行林挺操作。而`test`作业在推的代码或 PR 中启动一系列测试。

也就是说，每个作业都必须被分配一个操作系统，该操作系统被分配了`runs-on:`关键字。虽然这些[作业并行运行](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstrategymax-parallel)，但是它们可以相互依赖。因此，如果相关作业由于某些原因而提前失败，它们也可以在完成之前停止。

现在是有趣的部分。`steps:`键描述了要执行的工作流程/命令。因此，`linter`作业首先执行一个 [Git Checkout](https://github.com/actions/checkout) ，然后在后续步骤中设置一个适当的 Python 版本。

接下来的几个步骤涉及到缓存依赖项以减少工作流执行时间。[动作/缓存](https://github.com/actions/cache)动作加载之前缓存的依赖项。它还使用签名的密钥来标识正确的缓存。

如果依赖项没有从缓存中加载，那么`pip`安装`Black`、`Pylint`、&、`isort`用于林挺。

任务的最后一步是执行前面提到的林挺格式化工具。`Pylint, Black & isort`有合理的缺省值，因此它们的传递没有任何附加的参数。

最后来到`test`工作。你很快就会看到，这份工作在一定程度上反映了上一份`linter`工作。

立即使用`needs:`键，该作业被称为依赖于`linter`作业的完成。因此，`test`不会并行执行，但如果`linter`失败，也不会执行。使用`fail-fast: true`对启用快速失效选项。

除了上述策略之外，该作业还被设置为在具有多个 Python 版本的多个操作系统平台上运行。这是用`matrix:`键设定的，其中`os` & `python-version`有其值。`os` & `python-version`值分别接受 OS&Python 版本的一个数组。

下一行设置工作流应该运行的虚拟环境的默认 shell。

如前所述，每个工作流都必须使用`runs-on`关键字分配一个操作系统来运行。用于`test`任务的`runs-on`键接受一个变量，该变量将遍历在`matrix.os`下设置的每个值。因此，允许工作流为不同的操作系统& Python 版本运行后续步骤的多个实例！

接下来的两个步骤与`linter`开始它的执行过程非常相似，但是有一个警告。基于在`matrix.python-version`下设置的值，每个 OS 实例也将有一个 Python 实例。

现在，工作流使用`[snok/install-poetry](https://github.com/snok/install-poetry)`动作安装诗歌，而不是像在`linter`中那样安装`pip`。它将 poem 配置为在项目目录中设置 virtualenvs，然后可以在下一步中轻松地缓存它。

缓存操作缓存整个 virtualenv，而不是依赖项。因此，只有当缓存的`.venv`没有恢复时，poem 才会安装依赖项。

随后，`.venv`被激活& `pytest`然后运行测试套件。传递给`pytest`的参数确保了调试&的最大详细程度，以根目录中的`.xml`文件格式报告输出。生成的报告然后使用`[codecov/codecov-action](https://github.com/codecov/codecov-action)`将文件上传到 [CodeCov](https://about.codecov.io/) 。

CodeCov 动作接受一个 API 令牌，您必须将它复制并作为一个 [Secret](https://docs.github.com/en/actions/reference/encrypted-secrets) 环境变量传入。CodeCov 令牌可以在`https://codecov.io/gh/<GITHUB-USERNAME>/<PROJECT-NAME>`找到(针对 GitHub 上托管的项目)。最后，在最后，CodeCov 操作被设置为失败，如果它出错。

最辉煌的工作流程并不像听起来那么简单。如果您的项目是开源的，那么生产级软件应该按照以下标准进行彻底的测试和格式化，这就带来了它的复杂性。即便如此，仍然有很大的空间来进一步改进和改变。下一节将探讨如何进一步构建这个工作流。

# 进一步改进的空间

正如其他无数次提到的，管道保持极简，目的是:*为进一步的变化和/或改进留有余地*。

根据个人需求，还可以做出更多的改变/改进。我能想到的一些改进是:

*   启用一个`release`事件，在其中测试、格式化、linted、构建&包，然后用诗歌上传到 PyPi。
*   考虑到可伸缩性，linters 和代码格式化程序可以并行运行，而不是顺序运行。
*   在`release`时标记并更新一个`CHANGELOG.md`文件。

还有很多。可能性是无限的&只受项目和个人维护者需求的限制。

但总而言之，这里分享的代码应该足以满足 GitHub 上的大多数开源 Python 项目。

如果你觉得我错过了什么，那么在[推特](https://twitter.com/Jarmosan)、[电子邮件](mailto:somraj.mle@gmail.com)和/或[上联系我，问我任何事情](https://github.com/Jarmos-san/Jarmos-san/discussions/categories/q-a)。

[**订阅**我的简讯](https://jarmos.ck.page/newsletter) &获取直接发送到您收件箱的个性化内容或文章更新！