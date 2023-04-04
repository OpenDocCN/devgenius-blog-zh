# 带 Oban 的 Elixir 中的后台作业处理

> 原文：<https://blog.devgenius.io/background-job-processing-in-elixir-with-oban-fafb89a9ec21?source=collection_archive---------2----------------------->

## 在本文中，我们将探讨如何使用 Oban，这是一个用于 Elixir 的健壮的后台作业处理系统。

![](img/6c3357b6d25bda658209beee342255a3.png)

作为 [Erlang OTP](https://www.erlang.org) 的一部分， [Elixir 语言](http://elixir-lang.org)拥有*惊人的*对并发和执行异步任务的内置支持。这是真的:你通常可以将任务放在你的 Elixir 应用程序的后台，而不必过于担心并发性、性能、锁定等。

这对于某些类型的任务很有效，但是对于其他任务，简单地将一个作业放到后台会有一些缺点:当一个作业失败时会发生什么？如果服务器崩溃会发生什么？如何监控和管理负载和并发性？

如果这些场景对您的应用程序很重要，您将需要某种管理后台作业的系统。在本帖中，我们将探索 [Oban 后台作业处理系统](https://getoban.pro)。特别是，我们将涵盖:

*   什么是 Oban，为什么您可能需要它
*   如何在 Elixir 应用程序中设置和安装 Oban
*   如何定义和排队后台作业
*   如何运行作业和管理队列

我们开始吧！

# Oban 是什么？

Oban 是一个用于 Elixir 语言的高性能后台作业处理系统。它让我们定义作业，将它们放入队列，并在我们的应用程序做其他工作时在后台处理它们。作业是持久的，所以如果服务器或进程崩溃，它不会丢失。Oban 将处理这些场景中的重试和失败逻辑。此外，它还允许我们监控和管理作业队列，这样我们就可以密切关注有多少作业正在被处理，作业是否失败等等。

Oban 使用标准的 [Postgres 数据库](https://www.postgresql.org)来保存和管理作业。这与其他一些可能使用其他基础设施工具的作业处理系统形成对比，例如 [Redis](https://redis.io) 或 [RabbitMQ](https://www.rabbitmq.com/) 。在不深入这些不同工具之间的本质性能比较的情况下，可以肯定地说，向技术堆栈添加额外的服务会增加大量的复杂性，因此使用 Postgres 是一个很好的特性。

如果您已经将 Postgres 与 [Ecto](https://hexdocs.pm/ecto/Ecto.html) (这是 Phoenix 中的默认设置)一起使用，那么您可以使用现有的应用程序数据库，不过如果您愿意，也可以配置一个备用数据库。除非您的操作规模非常大，否则使用默认应用程序数据库通常是一个不错的起点。

虽然 Oban 可以与任何类型的 Elixir 应用程序一起工作，但它是用 Phoenix 框架构建的 web 应用程序的自然选择。Web 应用程序都是关于实时交互和快速响应的，所以在后台处理某些任务通常是有意义的。例如，如果一个应用程序在注册时向一个新用户发送了一封欢迎邮件，那么在 web 请求过程中等待邮件发送完成是没有意义的。更好的方法是立即呈现页面，并将电子邮件投递到后台队列中。

虽然 Oban 是开源的，可以免费使用，但他们确实提供了可选的 Pro 功能，包括用于监控和管理作业队列的[极其流畅的 web 界面](https://getoban.pro/oban)以及其他功能。

# 使用 Oban 运行后台作业

现在我们有了关于 Oban 的*背景*(这是一个可怕的双关语)，让我们看看如何在我们的 Elixir 应用程序中安装和使用 Oban。

## 安装 Oban

这一节的大部分内容是对 Oban 文档中所描述内容的回顾，但包含在这里是为了帮助您开始。请参阅完整的文档以了解更多有关其他配置选项的信息。

首先，您需要在您的应用程序中安装 Oban，方法是在您的`mix.exs`文件中添加`oban`并运行`mix deps.get`:

```
# mix.exs 
def deps do 
  [ {:oban, "~> 2.13"} ] 
end
```

Oban 将使用我们现有的 Ecto repo(通常是 Postgres)来持久化作业。我们需要创建一个迁移来创建和配置所需的表:

```
mix ecto.gen.migration add_oban_jobs_table
```

然后，在生成的迁移中:

```
defmodule MyApp.Repo.Migrations.AddObanJobsTable do
  use Ecto.Migration   def up do 
    Oban.Migrations.up(version: 11) 
  end   def down do 
    Oban.Migrations.down(version: 1) 
  end 
end
```

最后，我们将在我们的`config/config.exs`中配置 Oban:

```
config :my_app, Oban, 
  repo: MyApp.Repo, 
  plugins: [Oban.Plugins.Pruner], 
  queues: [default: 10]
```

为了让我们的后台作业在运行测试时立即执行，我们还将在`config/test.exs`中添加配置:

```
# config/test.exs 
config :my_app, Oban, testing: :inline
```

配置选项的完整列表可在 [Oban 中找到。配置文件](https://hexdocs.pm/oban/Oban.Config.html)。

# 定义工作

用 Oban 定义我们的背景工作很简单。我们简单地创建一个使用`Oban.Worker`的模块，指定一些选项，并定义一个`perform`函数来执行:

```
defmodule MyApp.MyImportantJob do 
  use Oban.Worker, queue: :events   @impl Oban.Worker 
  def perform(%Oban.Job{args: args}) do 
    IO.inspect(args) :ok 
  end 
end
```

在上面的示例中，我们只指定了作业应该放在哪个队列中，但是也可以指定其他选项:

```
use Oban.Worker, 
  queue: :events, 
  priority: 3, 
  max_attempts: 3, 
  tags: ["user"], 
  unique: [period: 30]
```

参见[工人文档](https://hexdocs.pm/oban/Oban.Worker.html)了解可用选项的全部细节。

# 入队和执行

创建一个作业就像创建一个具有所需参数的作业，并调用`Oban.Insert()`将其入队一样简单。

```
MyApp.MyImportantJob.new(%{id: 1, params: []}) 
  |> Oban.insert()
```

现在我们的作业已经入队——但是它还不会执行，因为我们还没有开始我们的队列。如果您很好奇，此时您可以暂停并检查 Postgres 数据库，以查看排队的作业。在我们的`oban_jobs`表中，您应该可以找到我们刚刚在`available`状态下排队的作业:

```
1 | available | events | MyApp.MyImportantJob | {"x": 10} | ...
```

当我们准备好运行我们的作业时，我们所要做的就是启动队列！我们通过调用带有队列名和并发限制的`Oban.start_queue`来实现这一点:

```
Oban.start_queue(queue: :events, limit: 4)
```

注意，如果我们定义了多个队列，我们可以用不同的选项独立地启动和停止它们。

在这个简单的例子中，我们的作业只是检查所提供的*参数*的内容，所以当我们启动队列时，我们应该会看到输出到控制台的日志消息。在“真正的”工作中，我们可能会执行一些操作并将结果存储回数据库中。

# 后续步骤

既然我们已经介绍了 Oban 的基础知识，我们就有足够的知识开始在我们的 Elixir 应用程序中使用它来运行简单的后台作业——但是还有很多我们没有介绍的 Oban 可以做的事情，例如:

*   [预定任务](https://hexdocs.pm/oban/Oban.Plugins.Cron.html)(类似于 cron)
*   [重复数据删除](https://hexdocs.pm/oban/Oban.html#module-unique-jobs)
*   [多任务处理器集群支持](https://hexdocs.pm/oban/Oban.Peer.html)

无论您正在构建哪种 Elixir 应用程序，使用 Oban 来管理后台作业都是对工具箱的一个很好的补充。

如果你想了解更多关于用 Elixir 和 Phoenix 构建应用程序的知识，请查看这些相关的帖子！

[](/why-your-next-frontend-might-be-the-backend-b43ff1ca9720) [## 为什么你的下一个 Web 应用前端可能是后端

### LiveView 和其他实时服务器端渲染 HTML 技术如何改变前端的编写方式

blog.devgenius.io](/why-your-next-frontend-might-be-the-backend-b43ff1ca9720) [](/5-elixir-libraries-i-install-on-every-new-project-a76fae7241a1) [## 我在每个新项目上安装了 5 个 Elixir 库

### 我的用 Elixir/Phoenix 构建的必不可少的工具和库列表

blog.devgenius.io](/5-elixir-libraries-i-install-on-every-new-project-a76fae7241a1) 

*原载于 2022 年 10 月 28 日 https://blixtdev.com*[](https://blixtdev.com/background-job-processing-in-elixir-with-oban/)**。**

*Jonathan 在大型和小型创业公司中拥有超过 20 年的工程领导经验。*