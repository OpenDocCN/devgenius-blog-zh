# 你的应用程序是一个整体吗？

> 原文：<https://blog.devgenius.io/is-your-app-a-monolith-41721571d79d?source=collection_archive---------21----------------------->

![](img/218faedb98fa4d998cf67bc51219fb54.png)

照片由 [imgix](https://unsplash.com/@imgix?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](/s/photos/network?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

你构建的大多数应用程序，尤其是在学习编码时，可能都是自包含程序。所有的特性，从数据处理到用户界面，都在一个单一的存储库中，这个存储库可以满足您的小型项目或应用程序的所有需求，并且反过来也很容易部署。Ruby on Rails 或 Node/Express 服务器是以这种方式实现全功能 web 应用程序的常见方式。[这种设计模式被称为单体架构](https://en.wikipedia.org/wiki/Monolithic_application)，是传统且最流行的软件设计方式。

随着应用程序变得越来越大，越来越复杂，这种设计风格就变得有问题了。例如，假设你建立了一个大型的令人印象深刻的论坛应用程序，并且它越来越受欢迎。有一天，一个讨论帖子像病毒一样传播开来，突然成千上万的用户加入并发帖，给你的主机提供商带来了意想不到的负担。在大多数情况下，这只会使你的网站崩溃，迫使你为更大的服务器买单，或者等待需求，失去所有的参与。

如果你正在使用一个更智能的云托管提供商，他们可能会[看到新的流量，意识到你当前的服务器正在接近其极限，并根据需要创建一个新的服务器实例来帮助处理新的流量。](https://cloud.google.com/load-balancing)实现这一点的方法是创建一个运行您的应用程序的新虚拟机，复制您现有的部署。你的单块应用程序越大，功能越全，这就变得越困难，因为你可能会有很多庞大的论坛应用程序及其所有资源的冗余副本。

就计算成本，空间和带宽而言，这可能很快变得非常昂贵。这看起来也很浪费，因为只有网站的一个特定元素(比如在一个论坛中保持流量和发帖频率)可能会过载，而你的应用程序的其他部分可能在其原始的单个实例中完全正常。你如何设计一个程序来避免这种冗余？

让我们考虑另一个不那么极端的例子:当你继续开发你的论坛时，你可能会优化旧的代码或者增加新的特性。每当您更改某些内容并希望将其部署到您的托管应用程序时，整个应用程序都需要停机进行维护，以便添加新功能，因为必须重新部署单一的整体代码库。你如何避免这种情况？

答案很简单:[实现分布式应用架构。](https://www.tutorialspoint.com/software_architecture_design/distributed_architecture.htm)不是在一个巨大的代码库中包含应用程序的所有功能，而是将关注点分离到单独的组件(或服务)中，然后让这些单独但友好的组件相互通信，以创建大规模全功能应用程序。

是否要添加新功能？好吧！只需向应用程序添加一个新组件，并将其连接到其他组件。您只需要更改与新功能交互的代码，让所有未受影响的服务单独运行，不受干扰地自由运行。

当你的前台、账号创建、数据操作和所有其他功能都运行顺利的时候，你的新论坛的帖子创建功能是否收到了危险的高负载？你的负载平衡器可以根据需要简单地创建论坛服务的新实例，同时让你的其他功能完善的应用照常运行。

随着大规模 web 应用变得越来越普遍，这种分布式体系结构变得越来越流行。尽管微服务(这是最流行的实现)解决了传统代码库结构的许多突出问题，但它们复杂和相互关联的本质引入了一系列全新的令人困惑的问题。即使微服务真的很棒，很强大，很酷，但重要的是要真正考虑这样巨大的复杂性增加对您的应用程序来说是否必要。维护如此多不同的服务器，[许多都有自己单独的数据库](https://microservices.io/patterns/data/database-per-service.html)，保持它们彼此兼容，[通过不同的异步消息传递模式控制它们的互连性，](https://medium.com/better-programming/rabbitmq-vs-kafka-1ef22a041793)都是基础设施编码方面的巨大障碍，这些困难几乎完全与通常的应用功能开发过程无关。

如果您愿意在开始时大幅增加复杂性和工作量，以节省时间和资源，那么将您的代码库从传统的整体结构转换为现代的分布式结构可以使大规模应用程序部署成为一个更加顺畅和高效的过程。许多大型软件公司已经遵循这些设计实践，即使你只是自己编码，如果你认为你的应用程序可以使用性能和可用性的提升，这绝对值得研究！

[](https://microservices.io/index.html) [## 什么是微服务？

### 微服务——也称为微服务架构——是一种构建应用程序的架构风格…

微服务. io](https://microservices.io/index.html) [](https://aws.amazon.com/microservices/) [## 什么是微服务？AWS

### 微服务是软件开发的架构和组织方法，其中软件由…

aws.amazon.com](https://aws.amazon.com/microservices/) [](https://algorithmia.com/blog/introduction-to-microservices) [## 微服务介绍:什么是微服务？使用案例和示例

### 来源:DZone 微服务架构可以通过调整团队设计和发布代码的方式来加快团队的速度，并且…

algorithmia.com](https://algorithmia.com/blog/introduction-to-microservices) [](https://cloud.google.com/appengine/docs/standard/python/microservices-on-app-engine) [## Google App Engine 上的微服务架构

### 微服务指的是开发应用程序的一种架构风格。微服务允许大型应用程序…

cloud.google.com](https://cloud.google.com/appengine/docs/standard/python/microservices-on-app-engine)