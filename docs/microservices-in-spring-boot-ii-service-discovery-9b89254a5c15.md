# Spring Boot 微服务 II-服务发现

> 原文：<https://blog.devgenius.io/microservices-in-spring-boot-ii-service-discovery-9b89254a5c15?source=collection_archive---------2----------------------->

微服务基本上是一个更大的模型的小模型，详细解释一切是一篇文章[这里](https://medium.com/nerd-for-tech/microservices-in-spring-boot-i-3fb961daaf99)。所以根据之前的[文章](https://medium.com/nerd-for-tech/microservices-in-spring-boot-i-3fb961daaf99)，你可能已经注意到微服务的 URL 是硬编码的。现在，这是一个非常糟糕的做法，原因如下

Url 可能会改变，这会导致我们在代码中改变它。

云部署的服务有不同的 URL。Aws 服务以 aws 为前缀等等。因此很难找到一个共同的模式。

使用这种方法无法实现负载平衡。

我们可以有多种环境。

![](img/a736315c5c7ea20fce5a6449044c2166.png)

照片由[卢卡·布拉沃](https://unsplash.com/@lucabravo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

硬编码的一种替代方法是将 URL 移动到应用程序文件中，也就是说，将其用作环境变量，但问题是这种方法也是“硬编码”,尽管它为我们节省了一些打字或几行代码。因此，为了避免这种情况，我们有一种设计模式或方法叫做**服务发现**。顾名思义，服务发现基本上就是客户端搜索微服务。我们有一个名为*发现*或发现服务器的服务器层，准确地说，它接收来自客户端的请求，搜索微服务并将 URL 返回给客户端。

![](img/ab1275001c57100621e0acb52f2c3d19.png)

图 1.0

上图描述了服务发现的过程。分解图像中的注释:-

步骤 x 基本上是客户端向服务器请求服务 c。

步骤 y 是服务 C 向服务器提供它的 URL，服务器再将它传递给客户机。

步骤 z 是步骤 y 的结果，其中客户端在从发现服务器获得服务 C 的 URL 后直接调用服务 C。

由于客户端正在请求服务，因此上述过程也称为客户端发现。与此相反，服务端发现也以非常相似的方式发生。默认情况下， *spring 提供了客户端发现*实现，因此我们将在本文中继续讨论。现在，服务发现是一件很平常的事情。不同的技术使用不同的库来帮助和促进开发者。在 Spring 中，像 Eureka、Ribbon、Zuul、Hysterix 等库普遍用于服务发现和其他服务。由*网飞*建造的尤里卡可以说是其中使用最多的，我们将在这篇文章中关注尤里卡。

Eureka 采用客户机-服务器模式。尤里卡服务器基本上是一种微服务，其行为类似于**发现服务器**并帮助寻找其他服务器，而尤里卡客户端是那些**消费来自尤里卡服务器的响应**的微服务。换句话说，除了发现服务器之外的一切都是客户机。例如:-在图 1.0 中(如上)，菱形的发现是服务器，而**所有其他的**都是客户端。

使用 Eureka 包括三个基本步骤

>设置 Eureka 服务器。

>注册微服务以使用服务器，即让微服务发布其对发现的响应。

>使用来自发现的响应来查找服务器

详细说明每个步骤

***设置尤里卡服务器:-***

Eureka server 不过是一个 spring boot 项目，具有 eureka server 或 spring cloud discovery 依赖项。因此，我们将从 spring starter 创建一个新项目，并在任何 IDE 中打开它。注意，在 java 8 的情况下，我们不需要任何其他的依赖来让 eureka 服务器产生。对于 java 9 或 java 10 及以上版本，我们需要添加 jaxb 依赖项(jaxb-api、jaxb-impl、jaxb-runtime 和 activation)来使我们的应用程序工作。这是因为 jaxb 在 java 9 及更高版本中已被弃用。

此外，在启动应用程序之前，我们需要将以下内容添加到我们的项目中

在我们的主类中，也就是我们的配置类中，我们需要在@SpringBootApplication 下面添加 **@EnableEurekaServer** 。这告诉 spring 我们的项目是服务器。

其次，在我们的应用程序中，我们需要添加属性

![](img/328f0d6f7629e4add0589fc77ba71e41.png)

以上是必需的，因为尤里卡服务器通过*默认也是客户端*。它可以链接到其他服务器或其他注册服务。因此，为了避免这种情况，我们需要添加以下内容。现在，启动应用程序并切换到[*http://localhost:8761/*](http://localhost:8761/)*将给出一个定义良好的 UI，显示关于我们服务器的各种细节。*

****注册微服务使用服务器****

*现在，我们需要将现有的微服务转换成 eureka 客户端。为了做到这一点:-*

*首先，我们需要在现有微服务的 pom 中添加[客户端依赖](https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-eureka-client/3.1.0)，即 spring 应用。*

*其次，我们需要将 spring cloud 版本属性添加到我们的项目中。为此，只需在 java-version 下方的<properties>和</properties>之间添加以下内容。定义版本是必要的，因为我们将来可能会有很多云，所以定义一次是必要的。*

> *格林威治。释放*

*同样为了更好地理解，使用*spring . application . name = name _ you _ want*为您的应用程序添加一个名称。一个以你的名字命名的服务将被注册到 eureka ui 上。*

*对所有可用的微服务重复相同的步骤，我们将很好地使用我们的服务注册。*

*现在一个有效的问题可能会出现，关于*客户如何找到尤里卡服务器？*对上述情况的合理解释是，eureka 当前运行的端口是默认端口。因此，客户端检查默认端口，在那里找到 eureka 并注册自己。如果我们改变端口，那么我们需要明确地为我们的客户端指定服务器端口号。*

****消耗来自发现的响应****

*消费响应通常适用于我们硬编码 URL 的服务。因此，为了消费响应，我们需要:-*

*将 **@LoadBalanced** 添加到 rest 模板或我们用来调用微服务的 web 客户端实例化之上。用更好的术语解释，如果我们是用 rest-template 来做对微服务的 API 调用，那么在 rest template 初始化的时候加上上面的注释。(即 RestTemplate t= new restTemplate，因此添加在该行的正上方)。*

*现在，转到硬编码的 URL，将硬编码替换为已在 eureka discovery 中注册的客户端的名称。起初我们在做*

> *[http://localhost:8082/movies/create user](http://localhost:8082/movies/createUser)*

*现在，我们将替换为*

> *[http://movie-demo-test/movies/](http://movie-demo-test/movies/)*

*其中，movie-info-service 是注册到 eureka 的客户端的名称，也类似于 localhost:8082*

*该注释被称为@LoadBalanced，因为除了发现之外，它还进行负载平衡。假设，我们有 5 个电影演示测试实例。在服务发现时，spring 实际上会检查每个微服务瞬间的负载信息，并给出一个可能的最佳信息。虽然默认的实现不是最好的，但是它以最好的方式完成了它的工作。*

*此外，Spring 提供了 **DiscoveryClient，**一个接口，帮助我们在各种实例之间进行选择，从而帮助定制负载平衡。发现服务器使用“*心跳”*模型监控其客户端的**正常运行时间**。如果某项服务停止运行，负载均衡器会注意到这一点，并自动管理向其发送的负载。*

*这是 spring boot 中服务发现和微服务基础的一个非常简单的例子。从这里开始的下一步可能是深入研究负载平衡和容错。*

*希望你喜欢上面的文章。如果你想要类似的基于 java 或 node/js 技术的文章，请在评论中分享。我很乐意挖掘和写一些类似的东西。*

**感谢阅读！。**