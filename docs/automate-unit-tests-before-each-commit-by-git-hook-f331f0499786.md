# 通过 Git 钩子在每次提交之前自动进行单元测试

> 原文：<https://blog.devgenius.io/automate-unit-tests-before-each-commit-by-git-hook-f331f0499786?source=collection_archive---------0----------------------->

![](img/4883ce82adea18b331b5f5121981b730.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

你是否曾经在你的分支上推送一个提交，而你的提交失败了，然后你修复了你的提交，并再次推送？

解决这个问题的方法非常简单，感谢 [Git 钩子](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)，它为我们提供了`pre-commit` ，用来检查将要提交的快照，看看你是否忘记了什么，确保测试运行，或者检查你需要检查的代码。

## **引擎盖下是什么:**

如果你深入到`.git`文件夹，你会发现很多挂钩的样品。我们将使用`pre-commit`,所以每当你提交一些代码时，Git 将运行那个脚本。

我们可以直接在`.git`文件夹中添加我们的脚本，但是我们会有一个问题，因为`.git`没有在存储库上共享，所以我们将使用符号链接来解决这个问题。

如果您很忙，并且想要立即提交代码，您可以使用`--no-verify`选项。

## 下面是实现这一点的步骤:

**1-** 在我们的项目目录下创建`script`文件夹。

**2-** 创建`install-hooks.bash`脚本:我们只运行一次这个命令行来设置配置。

**3-** 创建`pre-commit.bash`脚本:命令行，它将在我们每次提交之前自动执行。

创建`run-tests.bash`脚本:Xcode 命令行来运行单元测试。

干得好的🏅每当你提交的时候，Git hook 都会运行你的单元测试🎉

![](img/c8e4f1c4aff0c2bffca7fc645abb0511.png)