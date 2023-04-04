# terraform init 命令有什么作用？

> 原文：<https://blog.devgenius.io/what-does-the-terraform-init-command-do-552c7d81ba7b?source=collection_archive---------0----------------------->

![](img/bf8beda703ac0cb1af7d9b18b17b6673.png)

Benjamin Voros 在 Unsplash 上的原始照片。

在启动和运行 Terraform 时，您可能会使用的第一个命令是`terraform init`。

当您编写一个新的 Terraform 配置或从版本控制系统(如 Github)中下载一个配置时，您可能会首先运行`terraform init`。本文将简要介绍它是什么以及为什么要运行它。

首先，`init`是 *initialize* 的缩写，它所做的基本工作就在名字里:它初始化事物。

更具体地说，您正在初始化各种步骤来准备工作目录。

例如，`terraform init`做的一件事就是验证和配置配置的`backend`(如果有的话)。如果那个`backend`改变了，那么在执行其他`terraform`命令如`plan`或`apply`之前，你必须再次运行`terraform init`。

`terraform init`还在目录中查找配置中的`module`块，然后根据配置中`source`提供的参数追踪这些模块的源代码。这就构建出了模块树。

`terraform init`做的另一个步骤是搜索提供者的配置，然后安装任何与提供者相关的插件。如果您想知道什么是提供商，我将引用 Terraform 的文档来快速解释:

> Terraform 依靠称为“提供商”的插件来与云提供商、SaaS 提供商和其他 API 进行交互。
> 
> Terraform 配置必须声明它们需要哪些提供者，以便 Terraform 可以安装和使用它们。此外，一些提供者需要配置(如端点 URL 或云区域)才能使用。

— [供应商平台](https://www.terraform.io/docs/language/providers/index.html)

`terraform init`从公共 Terraform 注册中心或第三方注册中心查找并下载这些提供商。

事实上，这是生成`.terraform.lock.hcl`的部分，您可能已经注意到，它是在您第一次运行`terraform init`时生成的。

这个锁文件`.terraform.lock.hcl`的内容包含关于提供者的信息；在以后的命令运行中，Terraform 将引用该文件，以便使用与文件生成时相同的提供程序版本。要了解更多细节，让我们再次参考 Terraform 文档:

> 目前，依赖关系锁文件只跟踪*提供者*的依赖关系。Terraform 不会记住远程模块的版本选择，因此 Terraform 将始终选择符合指定版本限制的最新可用模块版本。您可以使用*精确的*版本约束来确保 Terraform 总是选择相同的模块版本。

— [依赖锁文件](https://www.terraform.io/docs/language/dependency-lock.html)上的 Terraform 文档

关于`terraform init`需要知道的另一件有用的事情是，多次运行是安全的。运行多次不会损坏任何东西，也不会有害。

当您运行`terraform init`时，您应该看到输出显示“Terraform 已经成功初始化！”

然后它会鼓励您使用`terraform plan`，这可能是您想要运行的下一个命令。我将在以后的文章中讨论这个问题，但希望这个概述对 Terraform 的基础知识有所帮助。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)