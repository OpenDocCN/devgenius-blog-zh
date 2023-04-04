# 使用 Istio 的流量路由

> 原文：<https://blog.devgenius.io/traffic-routing-using-istio-service-mesh-e4701f7bca03?source=collection_archive---------5----------------------->

使用 istio 虚拟服务在微服务内路由流量。

![](img/1c7aefbf227b4b508956c79868b67f00.png)

Denys Nevozhai 在 [Unsplash](https://unsplash.com/s/photos/traffic-routing?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

我在 [Istio](https://istio.io/latest/docs/) 的工作始于大约一年前，当时我想在位于 Kubernetes 集群中两个不同网络的 grpc 客户机和 grpc 服务器之间建立通信。我有一个帖子写的差不多[这里](https://vidhitakher.medium.com/realizing-a-grpc-client-server-communication-in-kubernetes-using-istio-3e17c9678578)。随着时间的推移，我逐渐了解了 Istio 的各种其他功能，以及它在基于微服务的应用程序开发中的广泛应用。

由于大多数应用程序开发都采用了基于微服务的架构，因此当两个服务之间的网络滞后时，它会带来许多麻烦，要求每个微服务通过进行故障注入测试来监控其容错能力，采用断路器模式的使用或通过应用程序的 canary 部署来安全部署服务，以观察应用程序的响应时间，并定期更新底层微服务。

任何微服务应用的关键指标是[红色](https://www.weave.works/blog/the-red-method-key-metrics-for-microservices-architecture/)指标，这可以在另一篇文章中深入讨论。现在，让我们关注 istio 的流量管理如何通过部署一个负责为任何微服务应用程序执行所有横切功能的 sidecar 来为应用程序开发人员解决所有这些问题。

在下面的场景中，我将讨论使用 istio 虚拟服务实现应用流量路由的三个场景。

***用例 1* :将所有流量路由到服务的唯一版本**

每当我们想要将任何微服务中的所有应用流量仅路由到一个特定版本时，可以通过向子集中的版本号添加路由来实现，如下所示，其中 HTTP 请求报头中的值将所有传入流量路由到服务的指定版本。特使负责按照下面虚拟服务中提到的路由规则路由流量。

```
*- apiVersion: networking.istio.io/v1beta1   
  kind: VirtualService 
  metadata: 
   name: reviews 
   namespace: test
  spec:     
    hosts:     
     - reviews     
    http:     
    - route:       
      - destination:           
         host: reviews           
         subset: v1*
```

***用例 2*** : **加权路由**

在某些情况下，部分应用程序流量需要路由到服务的版本 1，而剩余的流量需要路由到版本 2，例如在 A/B 测试和 canary 部署中。这可以通过为不同版本的服务分配权重来实现，如下面的代码片段所示，其中 60%的流量将转发到 v1 服务，其余 40%转发到 v2 版本。以这种方式，请求根据特定的百分比被转发到服务实例。

当应用程序团队想要通过发布各种版本的 web 服务并观察应用程序的[红色指标](https://www.weave.works/blog/the-red-method-key-metrics-for-microservices-architecture/)来审查性能指标时，这种场景对于 A/B 测试很有帮助。

```
*- apiVersion: networking.istio.io/v1alpha3   
  kind: VirtualService 
  metadata: 
   name: reviews 
   namespace: test
  spec:     
    hosts:     
     - reviews     
    http:
    - route:       
      - destination:           
         host: reviews           
         subset: v1
        weight: 60
      - destination: 
         host: reviews
         subset: v2
        weight: 40*
```

***用例 3* :基于用户的路由**

在某些情况下，在向更大的用户组发布产品版本之前，应用程序服务需要向某一组用户公开。这可以通过添加自定义报头“最终用户”并将其映射到用户列表来实现，对于这些用户，流量将被路由到以下代码片段中的 v2 版本，而对于所有其他用户，流量将被路由到微服务的 v1 版本。

这个场景有助于应用程序的 beta 测试。

```
*- apiVersion: networking.istio.io/v1alpha3   
  kind: VirtualService 
  metadata: 
   name: reviews 
   namespace: test
  spec:     
    hosts:     
     - reviews     
    http:  
    - match:  
      - headers: 
         end-user:     
           exact: ram
      route: 
      - destination: 
         host: reviews 
         subset: v2
    - route:       
      - destination:           
         host: reviews           
         subset: v1* 
```

**结论**—istio 的流量路由功能可帮助开发人员根据特定用户的需求配置他们的微服务，并允许在生产部署之前在试运行和较低环境中测试服务的多个版本。除了流量路由，istio 还有各种其他功能，如故障注入、断路器、超时和重试，所有这些都构成了基于微服务的应用的平稳运行，可以从[官方](https://istio.io/latest/docs/tasks/traffic-management/) istio 文档中试用。