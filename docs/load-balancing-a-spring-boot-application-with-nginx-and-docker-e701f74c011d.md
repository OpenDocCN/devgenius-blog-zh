# 使用 Nginx(和 Docker)实现 Spring Boot 应用负载平衡

> 原文：<https://blog.devgenius.io/load-balancing-a-spring-boot-application-with-nginx-and-docker-e701f74c011d?source=collection_archive---------0----------------------->

![](img/357d2e23d3c06f62492504352a7778a7.png)

在这篇文章中，我将尝试解释我们如何简单地部署一个 Nginx 服务器来在同一个 Spring Boot 应用程序的实例之间进行请求负载平衡。我们将在 Docker 环境中运行我们的配置。

# 应用程序

我们的应用程序是一个非常简单的 Spring Boot Web 应用程序，使用的是 Kotlin，并不复杂。它仅由 RestController 构成。

它公开了两个方法， *getResult()* 返回控制器存储的数据；鉴于 *updateData()* 使客户端能够更新数据。不用说，在 Spring Bean 中存储状态并不是一个好主意，但是为了简化这个例子，我们可以省略它。

正如我提到的，逻辑不是超级有趣，但无论如何你可以在下面找到控制器代码。

您可以在端口 8080 上构建并运行应用程序。

# DockerFile 文件

下面您可以看到一个简单的 docker 文件，它将我们的应用程序部署到一个 docker 映像中。

DockerFile 从构建目录中复制应用程序 JAR 并配置入口点。

# 负载平衡的配置

[Nginx](https://www.nginx.com/) 是一款开源的 web 服务器，广泛用于负载均衡和反向代理服务器。在这个例子中，Nginx 将在我们应用程序的两个实例中充当负载平衡器。我们将首先为负载平衡配置 Nginx 然后将这个配置传递给 Docker 映像。

# Nginx 配置

下面是一个非常简单的 Nginx 负载平衡配置:

在这个配置中，首先我们定义了两个上游服务器:一个用于端口 8080 上的服务 1，一个用于端口 8080 上的服务 2。(记住我们的 Spring Boot 应用程序运行在端口 8080 上)。

此后，我们指示 Nginx 监听端口 9090，并将所有请求传递给我们的上游服务器。

# Nginx Dockerfile 文件

在下面的最小 docker 文件中，我们提取基本 nginx 映像，并用我们在上一节中创建的映像替换配置:

[https://gist . github . com/itas yurt/42746090 abb 164948 E8 db 9070 cf 29d 98](https://gist.github.com/itasyurt/42746090abb164948e8db9070cf29d98)

# Docker 撰写

我们将使用 docker-compose.yml 组合我们的服务器实例和 Nginx，如下所示:

[https://gist . github . com/itas yurt/df 4b 77 b 8d 013 f 82327 f 02 b 57 ca 7 b 8 f2a](https://gist.github.com/itasyurt/df4b77b8d013f82327f02b57ca7b8f2a)

以下是撰写文件中的一些亮点:

*   我们在同一个端口上创建了公开端口 9090 的 Nginx 服务。Nginx 服务依赖于 service1 和 service2。注意，我们的 nginx 相关配置位于目录*中。/nginx* 。
*   我们从同一个 docker 文件为我们的应用程序创建了两个服务。您可以注意到服务打开了端口 8181 和 8282。实际上，在后面运行这些服务器并不需要这样做，我们将使用这些端口来单独更改每台服务器的数据。
*   服务名与 Nginx 配置中的相同。

# 运行应用程序

我们可以使用下面的命令构建并运行 docker 组合:

```
docker-compose builddocker-compose up
```

可以使用以下 Docker 命令验证容器的初始化:

```
docker ps
```

输出应该类似于:

现在我们可以通过 curl 访问我们的负载平衡应用程序并获取数据(最初是 10)。​

```
curl -XGET localhost:9090/api
```

可以通过以下 curl 命令更新其中一台服务器上的数据:

```
curl -XPUT localhost:8181/api?newData=19
```

请记住，我们已经公开了端口 8181，以便从外部访问服务器。请注意，我们也可以使用 9090，但这会修改随机服务器上的数据。

在我们更新了 service1 上的数据之后；根据 ngnix 选择的服务器，我们对 9090 的 GET 请求将开始返回 10 或 19 中的一个。因此，我们可以验证 Nginx 能够在不同的实例之间分发请求。

# 结论

简而言之，我们在本例中所做的是:

1.  实现了一个简单的 Spring Boot 应用程序。
2.  配置一个 Nginx 负载平衡器，在应用程序的不同实例之间分配请求。
3.  组成 Docker 容器来创建环境。

作为免责声明，请注意应用程序本身、nginx 配置和 docker 组合非常简单，仅用于演示目的。生产配置会更加复杂。

你可以在我的 [Github](https://github.com/itasyurt/lbsandbox/tree/lbexample) 上找到源代码。