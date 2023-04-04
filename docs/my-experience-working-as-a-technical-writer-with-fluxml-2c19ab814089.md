# 我作为 FluxML 技术作家的工作经历

> 原文：<https://blog.devgenius.io/my-experience-working-as-a-technical-writer-with-fluxml-2c19ab814089?source=collection_archive---------11----------------------->

> “开源不能满足的一件事就是文档”——匿名

![](img/90b7873c489bd69335581ad042b92c86.png)

今年夏天，在 Julia Season of Contributions 的帮助下，我开始作为一名技术作家与`FluxML`一起工作，不出所料，这种经历与编写代码截然不同。

在夏季开始的时候，我决定从事一份技术作家的工作，包括为机器学习编写文档和教程。与此同时，我正在学习`Julia`，`FluxML`生态系统对我来说听起来是个完美的地方。我通过谷歌求职季申请了这个职位，但不幸的是，由于职位有限，我无法进入。幸运的是，`Julia`语言决定资助我在接下来的几个月里从事`FluxML`朱丽亚季下的投稿工作！

在下面的博客中，我将分享我的经历和我迄今为止所做的工作，作为朱莉娅贡献季的一部分！

![](img/92f17fb9b5d2972b151a381eb5ab6902.png)

# 文档测试

![](img/82da1f10a2da91f95605b41812dded13.png)

`Flux`的文档缺乏文档测试，使得它的代码示例在每次发布后都变得陈旧。此外，许多当前的例子被认为包含在文档测试中，但是已经过时了。

有些文档测试很容易添加，但大多数需要与导师和社区成员进行深入讨论。此外，以 markdown 格式编写的文档也需要定期测试，以确保不断变化的 API 不会破坏示例。

现在已经为每一个公共 API 编写了 doctest，并且大多数 markdown 示例也包含在这些 doctest 中。其中一些更改尚未合并，但正在审核中！

# 缺少文档字符串

![](img/f87da6b409a3a9c7e06f0393fd53202a.png)

一些`Flux`的公共 API 也没有文档，或者有相对不太清晰的文档。例如，`Flux`提供的大多数神经网络层都有两个构造器——一个用于用预定义的权重和偏差初始化层，另一个用于从分布中生成权重和偏差。在大多数情况下，只有一个构造函数有文档记录，而另一个没有。

另外一个缺少文档字符串的例子是`Flux`的手册。在这里，文档字符串出现在代码库中，但是没有出现在手册中。这种情况通过将文档字符串添加到手册中或为这种缺失的文档字符串创建新的部分来解决。

除了`Flux`之外，`FluxML`下的其他包裹也有类似的问题。其中一些包装，如`NNLib.jl`、`Zygote.jl`、`Optimisers.jl`、`Functors.jl`和`MLUtils.jl`(在`JuliaML`下)也在 Flux 的文档中引用。这些包中丢失的文档字符串被传输到 Flux 的文档中，导致更多的文档字符串丢失。

所有缺失的文档字符串都已添加到各自的手册和功能中，包括其他`FluxML`包！

# 破损的文档

![](img/0f610157e5925b9a1b68f75d75c5c057.png)

没有交叉引用的文档是不完整的，而`Julia`出色的文档包让这变得容易。`Flux`有这些相互参照的地方，但其中一些导致无处可去。

此外，一些文档字符串在手册中没有正确呈现。这些文档破损的情况已经被修复，一旦 PR 被合并，用户就不应该再看到`404`页面或者未渲染的文档串了！

# CI/CD

![](img/bb15f9fff2d6684d8d76d92111a0355f.png)

面向用户的文档由 CI/CD 服务提供，该服务可使此文档始终可供用户部署和使用。对开源项目来说，开源他们的部署方案也是很常见的。

`FluxML`的生态系统在此 CI 服务中有一些小问题。`Zygote.jl`让 doctests 运行两次，使用了两倍的 CI 时间和资源，另一方面，`Flux.jl`在其 CI 和`make.jl`文件中指定了不同版本的 Julia。

此外，`Documenter.jl`允许用户为拉取请求生成文档预览，而`Flux.jl`设置了一个机器人来促进这一点，但是它关闭了很长一段时间。此外，这些文档预览是在`gh-pages`分支中收集的，该分支变得越来越庞大，必须通过自动化工作流程进行清理。我重新启动了这个机器人，并添加了一个工作流程，定期清理这些生成的预览！

最后，我还修复了`Optimisers.jl`文档中导致其 CI 失败的一些问题。

# FluxML 的生态系统

除了 T1 之外，T0 生态系统也有一些文档问题。例如，`Optimisers.jl`和`Zygote.jl`的词作就有缺点，这一点上面已经讨论过了。类似地，`Functors.jl`和`MLUtils.jl`有破碎的文档，丢失的文档串，和一个不完整的手册，这些也已经在上面详细讨论过了！

最后，`Flux`网站上的“生态系统”页面和文档不同步，有冗余信息。“生态系统”页面已经完全修改，并更新了与`Julia`中机器学习相关的最新进展！

# 微小的代码更改

是啊！文档的变化也伴随着微小的代码变化！

如果你是代码库新手，很难区分`Flux`的公共和内部 API 因此，新来者过去会发现很难通过这一关。通过从文档中删除内部实例并在内部 API 前面加上下划线，使 API 变得清晰。

此外，在为`tversky` loss 编写文档测试时，我还偶然发现了一个错误。`tversky`损耗有两个参数`*α*`和`*β*`，Flux 内部计算`*α*` 的值为`1-*β*`。损耗在数学上定义为`1-tversky index`，而`tversky index`在数学上定义为:

> S(P，G；α，β)= | P G |/(| P G |+α| P \ G |+β| G \ P |)
> 其中α和β分别控制 FPs 和 FNs 的罚值大小。

通量以下列方式实现损失-

```
1 — sum(|y .* ŷ| + 1) / (sum(y .* ŷ + β*(1 .- y) .* ŷ + (1 — β)*y .* (1 .- ŷ)) + 1)
```

用下面的代码-

注意术语`(1 .- y) .* ŷ`(误报)是如何乘以`*β*`的，而它应该乘以`*α*`(也就是`1-*β*`)。同样，`y .* (1 .- ŷ)`这个词要乘以`*α*`(也就是`1-*β*`)，而它应该乘以`*β*`。

这一细节使得损失函数的行为与其文档相反。比如说-

这里`ŷ_fnp, y`的损失应该大于`ŷ_fp, y`的损失，因为损失应该给予更多的权重或者惩罚假阴性(默认`*β*`为`0.7`；因此，它应该给 FN 更多的权重)，但是正好相反。

更改损失的实现会产生以下结果-

看起来不错！

(此错误尚未由除我之外的其他人确认，修复仍在审查中。)

# 入门部分

`Flux`有很多教程和例子，但是它们分散在各处，很难浏览。当前的“入门”部分、“概述”部分和“基础”部分为初学者提供了有价值的信息，但这些信息分散在这三个部分中。

此外，这三个部分中的一个在`Flux`的网站上，两个在文档网站上，使得新手很难在这些可黑客攻击但基本的例子之间导航。相反，这三个教程可以移出它们当前的位置，合并到一个名为“入门”的章节中，然后可以添加到文档中并在网站上链接。

我已经开始这一部分的工作，并且已经添加了两个广泛的教程作为 PRs:一个关于线性回归(有和没有`Flux`)和一个关于逻辑回归(有和没有`Flux`)。这些减贫战略目前正在审查中，应在未来几周内合并。

我在为`Flux`和它的邻居库做贡献时度过了一段难以置信的时光，我希望以同样的势头继续这些贡献。我也学到了很多，包括增加文档比增加代码需要更多的讨论。

没有我的导师[@ dhairyalghandhi](https://github.com/DhairyaLGandhi)和很多其他`FluxML`的维护者( [@ToucheSir](https://github.com/ToucheSir) ， [@mcabbott](https://github.com/mcabbott) ，[@ carlolucbello](https://github.com/CarloLucibello)，[@ Dar 点心](https://github.com/darsnack))，这项工作是不可能的。他们对我的问题和凌乱的简历非常有耐心😆

# 附录

## 拉请求

**Flux.jl**

*   添加大量的文档测试+修复`.md`文件中过时的文档:[https://github.com/FluxML/Flux.jl/pull/1916](https://github.com/FluxML/Flux.jl/pull/1916)
*   更新 https://github.com/FluxML/Flux.jl/pull/1978`basic.jl`和`conv.jl`的文件串:[的](https://github.com/FluxML/Flux.jl/pull/1978)
*   更新`upsample.jl`、`recurrent.jl`和`normalise.jl`:[https://github.com/FluxML/Flux.jl/pull/1995](https://github.com/FluxML/Flux.jl/pull/1995)中的文件字符串
*   杂项 docstring 添加和修复:[https://github.com/FluxML/Flux.jl/pull/1998](https://github.com/FluxML/Flux.jl/pull/1998)
*   添加`MLUtils`的文档并修复一些缺失的文档字符串:[https://github.com/FluxML/Flux.jl/pull/1910](https://github.com/FluxML/Flux.jl/pull/1910)
*   构建文档时关闭文档测试:[https://github.com/FluxML/Flux.jl/pull/1915](https://github.com/FluxML/Flux.jl/pull/1915)
*   统一`ecosystem.md`:[https://github.com/FluxML/Flux.jl/pull/1923](https://github.com/FluxML/Flux.jl/pull/1923)
*   再次启动文件机器人！https://github.com/FluxML/Flux.jl/pull/1937
*   添加删除 PR 预览的工作流程:[https://github.com/FluxML/Flux.jl/pull/1943](https://github.com/FluxML/Flux.jl/pull/1943)
*   去掉文档警告和 404 页:[https://github.com/FluxML/Flux.jl/pull/1987](https://github.com/FluxML/Flux.jl/pull/1987)
*   创建一个入门部分，并添加一个新的线性回归示例:[https://github.com/FluxML/Flux.jl/pull/2016](https://github.com/FluxML/Flux.jl/pull/2016)
*   向入门部分添加一个逻辑回归示例:[https://github.com/FluxML/Flux.jl/pull/2021](https://github.com/FluxML/Flux.jl/pull/2021)

**Zygote.jl**

*   只运行一次文档测试:[https://github.com/FluxML/Zygote.jl/pull/1255](https://github.com/FluxML/Zygote.jl/pull/1255)

**optimizer . JL**

*   修复文档测试和损坏的配置项:[https://github.com/FluxML/Optimisers.jl/pull/98](https://github.com/FluxML/Optimisers.jl/pull/98)

**函子. jl**

*   修复丢失的文档和交叉引用:[https://github.com/FluxML/Functors.jl/pull/42](https://github.com/FluxML/Functors.jl/pull/42)

**MLUtils.jl**

*   将缺少的文档字符串添加到手册:[https://github.com/JuliaML/MLUtils.jl/pull/95](https://github.com/JuliaML/MLUtils.jl/pull/95)

## 问题/讨论

**Flux.jl**

*   缺少`Flux.Data.Dataloader`:[https://github.com/FluxML/Flux.jl/issues/1909](https://github.com/FluxML/Flux.jl/issues/1909)的文档字符串
*   不同的`Julia`版本在不同的地方进行文档测试:【https://github.com/FluxML/Flux.jl/issues/1914】T21
*   不一致的“朱丽亚生态系统”文件:[https://github.com/FluxML/Flux.jl/issues/1922](https://github.com/FluxML/Flux.jl/issues/1922)
*   添加一个清理`gh-pages`分公司的工作流程？https://github.com/FluxML/Flux.jl/issues/1940
*   【讨论】:文档测试、文档字符串、文档手册、内部 API 不清晰(针对新人):[https://github.com/FluxML/Flux.jl/issues/1990](https://github.com/FluxML/Flux.jl/issues/1990)
*   【Bug】:在特沃斯基损失中交换了 alpha 和 beta？:[https://github.com/FluxML/Flux.jl/issues/1993](https://github.com/FluxML/Flux.jl/issues/1993)
*   [讨论]:修改了入门指南:[https://github.com/FluxML/Flux.jl/issues/2012](https://github.com/FluxML/Flux.jl/issues/2012)

**Zygote.jl**

*   运行两次的文档测试:[https://github.com/FluxML/Zygote.jl/issues/1199](https://github.com/FluxML/Zygote.jl/issues/1199)

# 奖金

![](img/f1900ee337cdb58ef559c564221a3102.png)