# 服务到服务的 Spring 5 + OAuth2 集成

> 原文：<https://blog.devgenius.io/service-to-service-spring-5-oauth2-integration-1fcdcec55a39?source=collection_archive---------4----------------------->

![](img/b3395ebb0435b22e134fd8ffbabc52b1.png)

我在为我的一个宠物项目寻找正确的方法来保护深层后端的微服务。在考虑了多种技术后，我决定选择总部位于 JWT 的 OAuth2。然而，互联网上的大多数指南和教程都涉及一个用户——要么是客户，要么是员工，但在后端，我们通常没有用户，我们有服务相互交谈。我花了几天时间研究，又花了几天时间构建了一个简约的原型，现在我很高兴与更多的观众分享。

在本文中，我们将构建基于 Spring5 的微服务，它既是 OAuth2 (OIDC)客户端，也是资源服务器。作为 OIDC 提供商，我使用 Keycloak。

***免责声明:*** 该系统尚未投入生产，开发仅用于演示目的。使用时风险自担。我试图只保留所需的最少代码，因此性能优化、高级设计、测试、被动调用和其他技巧不在讨论范围之内。我还假设你知道 OAuth2、OICD 和春天这些术语，否则，我很惊讶你还在读🤔。

***需求:*** Java 17(可以降级到 8 但你得改几行代码)运行你的 Spring Boot 服务，Docker(可选)运行 Keycloak 的实例，cURL(可选)。

**我们在建造什么。**
我们正在构建三种通用的 Spring Boot 微服务。在现实世界中，它们可能是 API 网关或常规服务，但在这个演示中，这些只是空的样板服务。每一个都有需要保护的资源 API。

![](img/6b0d42a155d7ef69742164ecf6ea3b84.png)

系统结构

你可能会问，为什么要提供三种服务。因为这样更有趣。请参见访问控制模型。

![](img/687277275affa21fba4235e9e6460562.png)

访问控制模型

它的意思是:我们有*服务 1* ，它可以从*服务 2* 中获取*资源 2* ，但不能从*资源 1* 中获取。每个资源都是一个简单的 REST API 端点。

**设置 OIDC 提供商— Keycloak**
没有特别的原因，我为什么选择了 [Keycloak](https://www.keycloak.org/) 。有多家供应商提供不同的许可证、价格和功能。Keycloak 是[阿帕奇-2.0](https://github.com/keycloak/keycloak/blob/main/LICENSE.txt) 并且极其强大。但是从理论上讲，您可以毫不费力地用您选择的授权服务交换 Keycloak。首先，默认选项是用 docker 启动它。在这种情况下，您将系统部署在您的*本地主机*端口 *8080* 上，用户名和密码为 *admin/admin* :

```
docker run -p 8080:8080 -e KEYCLOAK_USER=admin -e KEYCLOAK_PASSWORD=admin quay.io/keycloak/keycloak:16.1.0
```

创造一个新的境界。我把我的领域命名为 ***demo*** 。

![](img/13df9d4c80013ebc2d491a11f53b2a27.png)

创建领域

让我们创建范围。对于每个受保护的资源，我都有一个个人范围。于是，我有了这些: *service1:resource1* ， *service1:resource2* ， *…* ， *service3:resource3* —一共六个。

![](img/853ada229d3891260c41d55f6f117bb3.png)

创建客户端范围

现在，我们需要创建一个客户端。从 *service1* 开始，我们需要将其保密，启用 OAuth2 中的服务帐户[“客户端凭证授予”](https://oauth.net/2/grant-types/client-credentials/)，并从上表中设置可访问的范围。

![](img/1f2a97194473e815b9efe441e4b82eb6.png)

配置客户端

![](img/c4a728e8a24f0d6e9abab3f568d19f43.png)

为服务分配客户端范围

对*服务 2* 和*服务 3* 进行同样的操作。之后，Keycloak 应该就可以使用了。要测试它，获取您的客户机 ID 和密码(在 Credentials 选项卡上)，并发出 cURL 请求:

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'grant_type=client_credentials&client_id=***<your_id>***&client_secret=***<your_secret>***' "[http://localhost:8080/auth/realms/***demo***/protocol/openid-connect/token](http://localhost:8080/auth/realms/demo/protocol/openid-connect/token)"
```

如果一切顺利，你应该会得到一个带有访问令牌的响应，你可以在 https://jwt.io/的[查看:](https://jwt.io/)

```
{“access_token”:”***eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJxcmttSWVET2YzQVlmcGRUZHJGVG52QjBMbjd3WkVMMmthZWprTkpuTVd3In0.eyJleHAiOjE2NDA3NTYyNDgsImlhdCI6MTY0MDc1NTk0OCwianRpIjoiZjI0ZTU2MWItY2E2Ny00NDdiLWE4OWMtNjczNjY3NjdhOWEzIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL2F1dGgvcmVhbG1zL2RlbW8iLCJzdWIiOiJhNjNjMTFiOC1iN2FkLTQwNjktYTU3YS05NmQ4YzA0MDcyNmEiLCJ0eXAiOiJCZWFyZXIiLCJhenAiOiJzZXJ2aWNlMSIsImFjciI6IjEiLCJzY29wZSI6InNlcnZpY2UyOnJlc291cmNlMiBzZXJ2aWNlMzpyZXNvdXJjZTIiLCJjbGllbnRJZCI6InNlcnZpY2UxIiwiY2xpZW50SG9zdCI6IjE3Mi4xNy4wLjEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjE3LjAuMSJ9.Bb_i9FR23cvxIFmUwb_-AQQxCP35baSTVcROoSlJ1NyGWDcirz2BRY4UdFEldYQ02Gro3dwfErr-g-4blE4sYkYuQcj-TJSq-QAgvCQvtZ-J_sC-06pyFqFC3St1EVYKNyEWdnJGjCooe0YtWYUERQpdltHifQ23fOlAQbE3lwg0n5nCuypAzUpb7SFEyD0vOHyiYYMEUJiW8gF8Fw_ygsgbmUt8stHfFYd-oLjiQvrnk5G-dv8vhv4e4Z9DzeNAorwR59OhfxV8Mc30H1SRX4he8trcCuZL_HPglnN0pvLKFhP2kxGLIrv0r5ShMx4Pihjx7Q-e-R2aR-0cwTL14Q***”,”expires_in”:300,”refresh_expires_in”:0,”token_type”:”Bearer”,”not-before-policy”:0,”scope”:”service2:resource2 service3:resource2"}
```

**Spring 应用**
你可以在我的 Github 库看一下[。这三个服务中的每一个都是相同的服务。*服务 1* 的代码与注释](https://github.com/1petr0v/service-to-service-oidc-demo)一起提供给[。在运行它之前，您必须用您的客户端 ID、密码和域名更新](https://github.com/1petr0v/service-to-service-oidc-demo/tree/main/service1)`[application.yaml](https://github.com/1petr0v/service-to-service-oidc-demo/blob/main/service1/src/main/resources/application.yaml)`属性。 [Readme](https://github.com/1petr0v/service-to-service-oidc-demo/blob/main/README.md) 文件解释了如何启动服务和运行简单的集成测试。代码本身非常简单，但是我将解释一些细节。

首先，它是一个通用的 Spring Boot 应用程序。[依赖关系](https://github.com/1petr0v/service-to-service-oidc-demo/blob/main/service1/build.gradle) `org.springframework.boot:spring-boot-starter-oauth2-client`和`org.springframework.boot:spring-boot-starter-oauth2-resource-server`表示服务将被用作客户端和资源服务器。创建 REST 端点需要依赖关系【the web 客户端执行对其他服务的 HTTP 调用需要`org.springframework.boot:spring-boot-starter-webflux`。

`application.yaml`文件包含服务器端口和提供者配置片段和端点。 ***注意:*** *您的提供者端点可能不同，尤其是如果您使用了不同的领域名。*

类`[OAuth2LoginSecurityConfig](https://github.com/1petr0v/service-to-service-oidc-demo/blob/main/service1/src/main/java/com/test/service1/config/OAuth2LoginSecurityConfig.java)`是对`WebSecurityConfigurerAdapter`的扩展，指定了 web 端点的行为。为了让它工作，你需要用`@EnableWebSecurity`注释这个类。我还用`@EnableGlobalMethodSecurity(prePostEnabled = true)`对它进行了注释，这将启用`@PreAuthorize(“…”)`注释的用法，您将在控制器中看到。这并不重要，但更像是“生活质量”的特征。

类`[CustomConfigs](https://github.com/1petr0v/service-to-service-oidc-demo/blob/main/service1/src/main/java/com/test/service1/config/CustomConfigs.java)`只为`WebClient` bean 而存在。如前所述，`WebClient`只是一个 HTTP 客户端来执行对其他服务的调用。我选择它主要是因为它提供了与 OAuth2 客户端最简单的集成，但是人们可以用他们选择的 HTTP 客户端替换它。

而最后一个类是用`@RestController`标注的`[EndpointController](https://github.com/1petr0v/service-to-service-oidc-demo/blob/main/service1/src/main/java/com/test/service1/controller/EndpointController.java)`。该类有两个资源方法，每个都用`@GetMapping`和`@PreAuthorize(“hasAuthority(‘SCOPE_…’)”)`进行了注释。还记得吗，我们在 Keycloak 中创建了 *service1:resource1* ？这些是这些资源的范围。我还添加了一个完全允许的 API 方法`/public/jump/{toService}/{toResource}`，它仅用于演示目的。该调用与`http://service1/public/jump/service2/resource1`一样，将代表*服务 1* 调用*服务 2:资源 1* ，其中*服务 1* 必须被授权调用*服务 2:资源 1* 。

而且…差不多就是这样。我希望你喜欢读这篇文章。请在下面的评论区告诉我你的想法。

谢谢大家！