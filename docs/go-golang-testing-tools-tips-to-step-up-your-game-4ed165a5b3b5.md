# Go (Golang):测试工具和技巧，提升您的游戏水平

> 原文：<https://blog.devgenius.io/go-golang-testing-tools-tips-to-step-up-your-game-4ed165a5b3b5?source=collection_archive---------5----------------------->

![](img/46eb1ea2895e36059833e58c762b4ca7.png)

[自由股票](https://unsplash.com/@freestocks?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我在个人项目中使用了这篇文章中提到的所有工具和技巧。更多详情，请看这里:【https://github.com/rafael-piovesan/go-rocket-ride

# #1 测试容器

当我想到写这篇文章的时候，首先想到的工具是***test containers-Go***([https://golang.testcontainers.org/](https://golang.testcontainers.org/))*。*根据自己的文档:

> (…)是一个 Go 包，它简化了为自动化集成/冒烟测试创建和清理基于容器的依赖项。干净、易于使用的 API 使开发人员能够以编程方式定义应该作为测试的一部分运行的容器，并在测试完成时清理这些资源。

这意味着您可以在应用程序的集成和/或冒烟测试最需要的时间和地点，以某种无缝的方式，在不依赖外部资源的情况下，以编程方式创建、交互和处置容器。实际上，不再需要保存和维护各种 docker-compose 文件、复杂的 shell 脚本等等。您可以根据需要生成任意多的容器，每个容器用于一个特定的场景，并在运行测试所需的时间内保存它们。

下面是我如何建立一个 Postgres 容器，并在我的集成测试中使用它(更多细节见[https://github.com/rafael-piovesan/go-rocket-ride](https://github.com/rafael-piovesan/go-rocket-ride)):

创建 Postgres 测试容器的实用程序代码

如果你觉得这很有趣，那么也来看看另外两个项目:

*   ***docker test***([https://github.com/ory/dockertest](https://github.com/ory/dockertest))，旨在提供与 *Testcontainers-Go 相同的易用、更广泛和通用的 API*
*   ***Gnomock***([https://github.com/orlangure/gnomock](https://github.com/orlangure/gnomock))，专注于少数几个专业的，但却被很多项目普遍使用的流行预置。

# #2 作证

***作证***(【https://github.com/stretchr/testify】)*到现在已经这么长时间了，几乎不需要任何介绍。但是，因为这是一个有用工具的列表&提高测试质量的技巧，所以不能被遗漏。最流行的特性包括用于创建模拟对象的简单断言和机制，这些模拟对象也可用于检查对它们的期望。*

*断言示例摘自:[https://github.com/stretchr/testify](https://github.com/stretchr/testify)*

*模拟示例摘自:[https://github.com/stretchr/testify](https://github.com/stretchr/testify)*

# *#3 嘲弄*

****mock***([https://github.com/vektra/mockery](https://github.com/vektra/mockery))是一个补充工具，与***evidence 的 Mock*** 包一起使用，利用其创建 Mock 对象和自动生成 Golang 接口 Mock 的功能，这些功能可以在您的项目源代码中找到。它有助于减少(大量)样板编码任务。*

*模拟对象示例摘自:[https://github.com/vektra/mockery](https://github.com/vektra/mockery)*

# *#4 Gofakeit*

****Gofakeit***([https://github.com/brianvoe/gofakeit](https://github.com/brianvoe/gofakeit))是一个随机数据生成器。目前，它提供了 160 多种功能，涵盖了一些不同的主题/类别，如*人物*、*动物*、*地址*、*游戏*、*汽车*、*啤酒*等等。这里的关键词是*随机*。在计划和编写测试时，随机性是需要考虑的一个重要方面。比起在测试用例中使用常量值，使用随机生成的值通常是更好的选择，这样我们就更有信心，即使面对未知的(随机)输入，我们的代码仍然会按照我们期望的方式运行。*

*例子摘自:[https://github.com/brianvoe/gofakeit](https://github.com/brianvoe/gofakeit)*

# *#5 Gock*

****Gock***([https://github.com/h2non/gock](https://github.com/h2non/gock))是一个针对 Golang 的 HTTP 服务器嘲讽和期望库，它的灵感很大程度上来自于其流行的和更老的等效物 NodeJs，名为[***Nock***](https://github.com/nock/nock)。与***Pact-go***([https://github.com/pact-foundation/pact-go](https://github.com/pact-foundation/pact-go))不同，它是一个轻量级的解决方案，通过拦截任何`http.Client`使用的`http.DefaultTransport`或`http.Transport`发出的请求来工作。*

*如何使用 Gock 的示例*

# *#6 测试夹具*

****test fixtures***([https://github.com/go-testfixtures/testfixtures](https://github.com/go-testfixtures/testfixtures))模仿了为数据库应用程序编写测试的[“Ruby on Rails’way”](http://guides.rubyonrails.org/testing.html#the-test-database)，其中样本数据保存在 fixtures 文件中。在执行每个测试之前，测试数据库被清理，夹具数据被加载到数据库中。以下是我如何在我的项目中使用的一个例子(详见[https://github.com/rafael-piovesan/go-rocket-ride](https://github.com/rafael-piovesan/go-rocket-ride)):*

*将测试夹具加载到 Postgres 实例中的实用程序代码*

*此外，当与***test containers-Go***以及可选的任何迁移工具结合时，这是一个很好的匹配，因为您可以在您的测试中以编程方式编排所有这些工具，如下所示:*

*测试夹具样本文件*

*把所有东西放在一起*

# *#7 测试主要功能*

*你有没有想过如何测试`main`你的应用程序的功能？假设您想检查代码在缺少任何环境变量的情况下的行为。假设您至少有一个简单的日志来告诉我们发生了什么，一种可能的方法是实际检查您的应用程序的输出，并查找所述日志，就像这样(参见[https://github.com/rafael-piovesan/go-rocket-ride](https://github.com/rafael-piovesan/go-rocket-ride)的更多细节):*

# *#8 使用构建标签的独立测试*

*这个想法是根据类型(即单元、集成、冒烟和 e2e)或者任何其他你认为合适的标准来分离测试，因为我们使用的是 Go 的内置机制，叫做 [***构建约束***](https://pkg.go.dev/cmd/go#hdr-Build_constraints) ，也就是众所周知的 ***构建标签*** 。您可以利用它的功能，在您的测试文件中有这样的内容:*

*然后，要选择您想要运行的测试，只需调用指定了`-tags`标志的`go test`命令:*

```
*# run all unit tests
go test -tags=unit ./...# run only use case related tests
go test -tags=usecase ./...*
```

## *参考*

*   *[https://github.com/rafael-piovesan/go-rocket-ride](https://github.com/rafael-piovesan/go-rocket-ride)*
*   *[https://go.dev/doc/code#Testing](https://go.dev/doc/code#Testing)*
*   *[https://bmuschko.com/blog/go-testing-frameworks/](https://bmuschko.com/blog/go-testing-frameworks/)*
*   *[https://github.com/orlangure/gnomock](https://github.com/orlangure/gnomock)*
*   *[https://github.com/ory/dockertest](https://github.com/ory/dockertest)*
*   *[https://github.com/testcontainers/testcontainers-go](https://github.com/testcontainers/testcontainers-go)*
*   *[https://github.com/stretchr/testify](https://github.com/stretchr/testify)*
*   *[https://github.com/vektra/mockery](https://github.com/vektra/mockery)*
*   *[https://github.com/brianvoe/gofakeit](https://github.com/brianvoe/gofakeit)*
*   *[https://github.com/h2non/gock](https://github.com/h2non/gock)*
*   *[https://mickey.dev/posts/go-build-tags-testing/](https://mickey.dev/posts/go-build-tags-testing/)*