# 酏剂 1.14 —新增内容及其重要性

> 原文：<https://blog.devgenius.io/elixir-1-14-whats-new-and-why-it-matters-8ee238d223d3?source=collection_archive---------3----------------------->

## 仙丹 1.14 刚刚发布。了解新内容，更重要的是，了解它的重要性。

![](img/6db2e10467be72776d6892db8b94dce9.png)

Jan Ranft 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

[Elixir 1.14 现已发布](https://elixir-lang.org)，对已经令人惊叹的语言进行了几项重大改进。虽然有很多变化，但出现最多的主题之一是围绕检查和调试——一些受欢迎的变化改善了开发和调试体验。

在本文中，我们将深入探讨 1.14 中的新特性，更重要的是，为什么这些变化如此重要。我们将在这里关注更大和更重要的变化，但是还有大量的其他变化、错误修复和改进。有关变更的完整列表，请参见[酏剂变更日志](https://github.com/elixir-lang/elixir/blob/v1.14/CHANGELOG.md)。

# 使用 Kernel.dbg/2 进行更好的调试

记录和检查输出是 Elixir 的一种常见调试技术，尤其是在高度并发的应用程序中，传统的调试方法(如断点)可能难以使用。这通常涉及到`IO.inspect/2`的使用，它打印一个值的“已检查”表示。新的`Kernel.dbg/2`通过在`IO.inspect/2`提供的基础上增加额外的上下文来提供更好的调试功能。

让我们来看看变更日志中提供的例子。下面是调用`dbg`的代码可能的样子:

```
# In my_file.exs
feature = %{name: :dbg, inspiration: "Rust"}
dbg(feature)
dbg(Map.put(feature, :in_version, "1.14.0"))
```

下面是我们在执行代码时看到的输出:

```
$ elixir my_file.exs
[my_file.exs:2: (file)]
feature #=> %{inspiration: "Rust", name: :dbg}

[my_file.exs:3: (file)]
Map.put(feature, :in_version, "1.14.0") #=> %{in_version: "1.14.0", inspiration: "Rust", name: :dbg}
```

除了改进的调试输出，`Kernel.dbg`还为其他工具提供了一个可配置的集成点:例如，`IEx`与`dbg`集成以提供断点行为，这样您就可以在使用`IEx`运行时检查应用程序的状态。

## 为什么它很重要

对于 Elixir，调试可能是一个棘手的话题，因为工具和经验往往落后于其他流行语言。Kernel.dbg 的引入并没有完全解决缺点，但它是一个受欢迎的补充，是朝着正确方向迈出的一步。它既增强了现有的调试输出方法(通常使用`IO.inspect`、`IO.puts`)，又通过为其他工具提供一个集成点来支持更高级的方法。

# 更好的 IO.inspect 输出:基于表达式的检查

补充新的`Kernel.dbg/2`功能的是对`IO.inspect/2`本身的改进。使用变更日志中的例子，让我们看看之前的`inspect`行为:

```
iex(1)> [:apple, :banana]
[:apple, :banana]iex(2)> MapSet.new([:apple, :banana])
#MapSet<[:apple, :banana]>
```

在上面的例子中，我们看到我们的列表表达式产生了一个我们可以复制和粘贴的有效的灵丹妙药代码表示——但是我们的`MapSet`表达式没有！

随着 1.14 中的变化，像`MapSet`这样的内部结构现在产生的`inspect`输出是有效的灵丹妙药:

```
iex(1)> MapSet.new([:apple, :banana]) |> MapSet.put(:pear)
MapSet.new([:apple, :banana, :pear])
```

## 为什么它很重要

酏剂的 REPL 是酏剂开发过程中非常重要的一部分——价值*检验*是每个评估步骤的一部分。能够从表达式中复制和粘贴输入和输出值是 REPL 循环的一个重要部分，在 1.14 之前，这有时会因为输出不是有效的 Elixir 代码而变得更加困难。对于任何花时间使用 Elixir 控制台的人来说，这一变化将是一个受欢迎的补充。

# 新主管类型:分区主管

Elixir 1.14 引入了一个新的`[PartitionSupervisor](https://hexdocs.pm/elixir/main/PartitionSupervisor.html)`，它可以用来管理多个相同的进程(默认情况下等于核心的数量)，然后可以通过您选择的某个*分区键*来分别路由请求。这种新的管理器可以为任何进程提供简单的并发和负载平衡。

让我们快速看一下发行说明中的示例:

```
partitioning_key = self()
ErrorReporter.report({:via, PartitionSupervisor, {Reporters, partitioning_key}}, error)
```

在这个例子中，`ErrorReporter` 模块通过分区管理器在几个`Reporter`进程中平衡负载。所提供的`partitioning_key`指定了应该使用哪个进程——在本例中，通过使用`self()`,调用者确保了单个进程总是使用同一个报告者，但是您可以很容易地通过其他值(如用户 ID 或主机名)进行划分。

## 为什么它很重要

`PartitionSupervisor`作为一种嵌入式解决方案，有助于解决一个常见的性能瓶颈:单个消费者流程因请求而过载。Elixir 最大的优势之一是它构建在 Erlang/OTP 之上，并且开箱即用，对并发性提供了真正惊人的支持。像`PartitionSupervisor`这样的新增功能有助于进一步增强这种支持。

# 重述—酏剂 1.14

Elixir 是一种非常棒的编程语言，而且还会变得越来越好。虽然 1.14 的更新并不是惊天动地的，但它带来了许多重要的人体工程学改进，使其更容易使用。有关酏剂 1.14 的更多信息，包括如何安装或升级，请参见[酏剂安装页面](https://elixir-lang.org/install.html)。

[*乔纳森*](https://blog.devgenius.io/@jonnystartup) *在创业公司大&小有超过 20 年的工程领导经验。如果你喜欢这篇文章，* [*请考虑给乔纳森留个提示*](https://www.buymeacoffee.com/jonnystartup) *！*