# 高级 VIM——7 个最有用的命令，你希望早点知道

> 原文：<https://blog.devgenius.io/advanced-vim-7-most-useful-commands-youd-wish-you-d-known-earlier-52a0c7ab6d8c?source=collection_archive---------2----------------------->

![](img/f62f31e03f1632eb4c1d1340546702cc.png)

照片由[克里斯托弗·高尔](https://unsplash.com/@cgower?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/computer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

把一个初级开发人员放进 Vim，并告诉他们退出；如果给他们无限的时间，他们可能会写出莎士比亚的全部作品。但是仍然可能不会退出 Vim。

如果你正在读这篇文章，我希望你已经比退出 Vim 更进一步了。在本文中，我将详细介绍一些更高级的用法和命令，这些用法和命令在我的开发过程中给了我很大的帮助。因为是的，它可能是一个令人难以置信的复杂和令人沮丧的工具，但如果你能让它为你工作，它会异常强大。

下面我列出了我在使用 Vim 时学到的七个**最有用的命令和技巧**，它们无疑会**为你节省时间和痛苦。**我们将跳过本文中的简单内容，所以如果你还在学习，我建议你先学习基础知识，然后再回来。对于其余的人，我希望你们能学到一些东西。

> "任何程序都只有在有用的时候才是好的."Linux 的创造者 Linus Torvalds。

# 1.格式化 JSON 块

只要在您的主机操作系统上安装了`jq`，您就可以通过首先选择 JSON 块，然后执行以下命令来格式化它:

```
:%!jq .
```

# 2.缩写

缩写是一个简单的工具，它允许您为 VIM 可以自动完成的常用单词和短语创建一个简写。比如`:ab RTFG Read the fantastic manual`。这将意味着无论何时你输入' *RTFG* '，它将被我们的新术语'*Read the Fantastic Manual '*'所取代。

当您需要在整个文档中输入相同的文本时，缩写可以让您节省键入时间并提高准确性。它也常用于纠正拼写 m̶i̶s̶t̶e̶a̶k̶e̶s 错误，并作为自动更正功能。

## **例题**:

```
**:ab bestlink** [**https://medium.com/@joelbelton/membership**](https://medium.com/@joelbelton/membership)
**:ab** *# Lists all Abbreviations*
**:una bestline** *# Removes bestline Abbreviation*
```

# 3.排序文本

在 VIM 中对文本进行排序非常容易。我们所要做的就是在我们的视觉模式中选择任何我们想要的文本，然后完成命令`:sort`。这将按照字母顺序对我们所有的行进行排序；这在处理 YAML 或 CSS 属性时非常有用，为了达到最佳效果，这些属性应该按字母顺序排序。

我们还可以使用 sort 命令从文档中删除重复的行。

## 示例:

```
**:sort** *# Sort alphabetically*
**:%sort!** *# Sort in reverse*
**:%sort u** *# Remove duplicates*
```

# 4.在多个文件中替换

对于典型的 IDE 来说，替换许多文件中的文本是一项非常有用且简单的任务。然而，当涉及到 Vim 这个非常注重自动化和速度的工具时，它却异常复杂。

首先，我们需要使用`:args`创建一个文件列表，命令将在这些文件上执行。*例如，*让我们选择当前目录中的任意 python 文件:

```
**:args *.py**
```

现在是时候使用我们的参数来运行命令了。要在 Vim 中运行简单的搜索和替换，命令是`:s/<search_term>/<replace_term>/g`。知道了这一点，并使用我们的参数列表，我们可以通过使用以下内容对一系列文件进行搜索和替换:

```
**:argdo %s/From/To/g**
```

最后，一旦我们的更改完成，我们可以使用以下命令保存参数列表中的所有文件:

```
:**argdo** **update**
```

> 您可以应用这里使用的原则，在许多文件的文本上运行各种不同的命令。

# 5.在多行上重复最后一次更改

当在 Vim 中对文档执行修改行时，测试并确保您的命令如您所愿通常很有帮助。幸运的是，Vim 为我们提供了功能，这意味着我们可以通过可视化的代码选择来重复命令。这允许我们快速修改一行，然后对文档中的所有情况重复修改。

首先，执行你选择的命令。然后在可视化编辑器中选择我们的新选择。现在我们可以使用以下命令来重复我们的命令:

```
**:'<,'> normal .**
```

# 6.在文件中添加书签

我发现自己一直在使用的 Vim 的一个核心特性是书签功能。当在单个大文件中的多个位置跳转时，这非常方便。

使用`:marks`我们可以实现我们的书签。为了设置我们的第一个书签，我们可以使用`:ma`这将创建一个名为*‘a’*的标记。

我们要跳回这里所要做的就是执行``a`，我们将直接跳回设置书签时的光标位置。使用命令`:marks`我们可以看到为任何给定文件创建的所有书签。

# 7.Vim 标签

在我的开发生涯中，我花了太多的时间没有意识到 Vim 有标签。**不要像我一样。**

我们可以使用下面列出的命令创建和删除选项卡。在这些选项卡之间移动很简单:`gt`转到下一个选项卡，或者`gT`转到上一个选项卡。你也可以使用他们的索引直接跳转到你想要的标签页，例如:`1gT`将转到我们的第一个标签页。*是的，Vim 标签从 1 开始索引。*

```
:tabe # Open new tab
:tabc # Close current tab
:tabo # Close all but current tab
```

希望您喜欢这个 Vim 命令列表，并且这些技术可以节省您的开发时间。

我定期在 DevOps 上发布文章，专门在 Medium 上发布——如果你想阅读更多，我建议查看下面的故事。

[](https://medium.com/@joelbelton/optimising-docker-performance-the-key-4-techniques-you-need-6440cfebb650) [## 优化 Docker 性能—您需要的 4 项关键技术

### Docker 集装箱和集装箱化技术已经永远改变了云计算产业。成千上万的新…

medium.com](https://medium.com/@joelbelton/optimising-docker-performance-the-key-4-techniques-you-need-6440cfebb650) [](https://medium.com/@joelbelton/learning-containers-from-the-ground-up-a-roadmap-to-success-dd16151b2388) [## 从头开始学习容器——通往成功的路线图

### 容器化已经成为软件开发中的一个重要趋势，并且是一种快速增长的替代方式。

medium.com](https://medium.com/@joelbelton/learning-containers-from-the-ground-up-a-roadmap-to-success-dd16151b2388) 

如果你喜欢这篇文章，但你还不是会员，可以考虑通过 [**注册成为会员**](https://medium.com/@joelbelton/membership) 来支持我和数以千计的其他作家，并无限制地访问 Medium 的优秀作家的内容。你的会员资格直接用你的一部分费用支持我，不会花你更多的钱。

[](https://medium.com/@joelbelton/membership) [## 通过我的推荐链接加入 Medium-Joel Belton

### 阅读乔尔·贝尔顿(以及媒体上成千上万的其他作家)的每一个故事。您的会员费直接支持…

medium.com](https://medium.com/@joelbelton/membership)