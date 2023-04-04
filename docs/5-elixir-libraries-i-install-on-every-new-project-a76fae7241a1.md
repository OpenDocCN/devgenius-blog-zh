# 我在每个新项目上安装了 5 个 Elixir 库

> 原文：<https://blog.devgenius.io/5-elixir-libraries-i-install-on-every-new-project-a76fae7241a1?source=collection_archive---------2----------------------->

## 我的用 Elixir/Phoenix 构建的必不可少的工具和库列表

![](img/6b329b36c55d83d757d9d2dfbca29ee0.png)

灵丹妙药式的语言——稍加补充就更好了。

我是[仙丹语言](https://elixir-lang.org)和[凤凰网框架](https://www.phoenixframework.org)的超级粉丝。作为一种函数式语言，它有一个[精心设计的角色模型](https://en.wikipedia.org/wiki/Actor_model)用于并发性，Elixir 有助于避免你在另一个编程环境中可能遇到的各类错误&和其他复杂问题。但是我喜欢在每个项目中使用一些额外的工具，以充分利用这种语言并构建高质量的软件。

这里有五个我认为在开始一个新项目时必不可少的工具。除了一个用于构建管理界面的库之外，所有这些工具都适用于任何 Elixir 项目，而不仅仅是构建在 Phoenix 上的 web 应用程序。

## credo——长生不老药的代号

Credo 是一个 linter &静态分析 linter 工具，帮助开发者提高 Elixir 代码质量。它指出了基于一组可配置选项的重构&风格改进的机会。它可以被配置为发现代码质量问题，比如没有遵循正确的命名约定、语言构造的不正确或不一致的使用等等。我喜欢将 Credo 集成到预提交挂钩中，以确保我和我的团队在提交代码之前解决了错误或警告。

## 透析器—类型检查

尽管我很喜欢用 Elixir 编写代码，但我发现在 Elixir 开发人员的体验中缺少了一件事，那就是静态类型检查。不要误解我——我喜欢由 Elixir 的动态类型实现的开发和迭代速度，但是一旦到了为生产使用而强化代码的时候，静态类型有助于发现 bug 并确保质量。

虽然 Elixir 没有静态分型，但它有一个工具可以捕捉静态分型系统会发现的大多数错误。透析器是一个完全不同的野兽，而不是一个完全静态的打字系统，而且肯定值得自己的职位，所以我不会在这里进入它太多。但要点是，当需要强化您的代码并发现隐藏的错误或其他问题时，透析器是一个必备工具。像 credo 一样，我也喜欢将透析器集成到预提交钩子中，以确保在推送代码之前解决错误。

## TypedEctoSchema 用于 Ecto 模型的自动类型规格

Elixir 支持可选的类型规范( *typespecs* )，用于文档和代码分析工具，如透析器(如上所述)。 [TypedEctoSchema](https://hexdocs.pm/typed_ecto_schema/TypedEctoSchema.html) 是一个为你的应用中定义的 Ecto 数据库模式自动生成类型规范的模块。这意味着更好的结果与透析器和更好的文件，自动。

## kaffy——Phoenix 应用程序的现成管理工具

Kaffy 是一个优秀的工具，可以从你的应用程序中的 Ecto 模式自动生成一个管理界面。如果您熟悉 PHP 的 phpMyAdmin 或 Rails 的 Active Admin，Kaffy 也非常相似。随着应用程序的增长，Kaffy 可能无法满足您的需求，尤其是当您希望构建一个对应用程序的最终用户开放的接口时。但是要让内部使用的管理界面快速运行，Kaffy 是无与伦比的。

## Libcluster —释放 Erlang OTP 的力量

使用 Elixir 最令人惊奇的方面之一是 Erlang OTP(开放电信平台)。除此之外，它还支持开箱即用的强大分布式计算功能。OTP 使得在一组不同的服务器之间分配计算或存储变得非常简单，这也是 Elixir 具有惊人的可伸缩性的原因之一。

即使您没有编写代码来显式地使用 OTP 和集群，当您使用 Phoenix 或其他流行的 Elixir 库和框架时，您也可能利用了它们的优点。例如，Phoenix 的 channels 功能用于轻松实现聊天等实时功能，它利用了 OTP 的优势，如果进行了配置，它将自动跨集群工作。

然而，为了在多服务器环境中充分发挥 OTP 的惊人威力，您需要启用 Erlang 集群。虽然可以手动配置集群，但是在部署环境中这是非常不切实际的:应用服务器可能会启动和关闭，并且可能有未知的 IP 地址。

`[Libcluster](https://github.com/bitwalker/libcluster)`通过为不同的环境配置提供各种不同的集群策略，使集群配置变得简单和自动化&。例如，它支持一个 [Kubernetes](https://kubernetes.io) 策略，该策略支持从 Kubernetes 元数据自动构建一个集群。虽然您需要为您的特定环境选择&配置正确的策略，但是 libcluster 让这变得很容易。

# 结论

Elixir 是一种开箱即用的奇妙语言，但有几个插件可以帮助它更上一层楼。这里描述的工具有助于释放 Elixir 和 OTP 的全部力量，对我来说，是任何新项目的必备工具。

*原载于* [*Blixtdev*](https://blixtdev.com/5-elixir-libraries-i-install-on-every-new-project/) 。

Jonathan 在大大小小的创业公司中拥有 20 多年的工程领导经验。