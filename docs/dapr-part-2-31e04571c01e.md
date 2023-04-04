# DAPR —第二部分

> 原文：<https://blog.devgenius.io/dapr-part-2-31e04571c01e?source=collection_archive---------3----------------------->

![](img/226f1266d4f55fe43134210ea308dedc.png)

在[第 1 部分](https://medium.com/dev-genius/dapr-part-1-229b78081378)中，我已经解释了 DAPR 的关键概念，在这一部分我们将看到状态存储的示例实现，使用本地 **kubernets** 集群、 **redis** 缓存和 **dotnet core 5** 。

你可以在这里找到示例项目。

先决条件:

*   [Minikube](https://minikube.sigs.k8s.io/docs/)
*   码头工人
*   库贝内特斯(库贝特尔)
*   点网核心 5.0
*   [Dapr](https://dapr.io/)

这是一个简单的 dotnet core 5.0 Web API 项目，有几个端点:

*   后期添加客户端
*   获取 getClient
*   删除删除客户端

我们希望使用 dapr SDK for DotNet Core 将客户端数据保存在 redis 缓存中。

**安装和基本使用**

1.  **启动 minikube**

```
minikube start
```

**2。在您的 kubernetes 集群上本地安装 DAPR**

```
dapr init -k
```

**3。将 Redis 安装到您的集群中**

```
dapr init -k
```

并检查状态

```
kubectl get pods
```

，您应该会看到 3 个节点处于运行状态。

要获取 redis 密码，请使用

```
export REDIS_PASSWORD=$(kubectl get secret --namespace default redis -o jsonpath="{.data.redis-password}" | base64 --decode)
```

**4。在本地运行 DAPR API 项目**

```
cd src/DaprSamples/SampleStateStore_Redis
dotnet build
dapr run --app-id controller --app-port 5000 -- dotnet run
```

，其中 app-id 为 Dapr 定义了应用的名称。

**5。将 DAPR 组件添加到 Kubernetes 集群**

*5.1。创建 redis-statestore.yaml*

并补充

```
apiVersion: v1
kind: Secret
metadata:
  name: redis
data:
  redis-password: <REPLACE WITH BASE64 ENCODED PASSWORD>
---
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: statestore
  namespace: default
spec:
  type: state.redis
  version: v1
  metadata:
  - name: redisHost
    value: localhost:6379
  - name: redisPassword
    secretKeyRef:
      name: redis
      key: redis-password
```

在这里，我们使用 redis 缓存为持久性和恢复创建一个状态存储组件。

*5.2。部署到 Kubernetes 集群*

```
deploy -f redis-pubsub.yaml apply
```

6.使用它

6.1.将客户端添加到商店

```
POST [http://localhost:5000/addClient](http://localhost:5000/addClient)
Content-Type: application/json{
    "Id": "1",
    "FirstName" : "Evgeny",
    "LastName" : "Grishchenko"
}
```

要验证 redis(商店):

```
redis-cli -h localhost -a <YOUR REDIS PASSWORD>
KEYS *
HGET <OUR KEY NAME>
```

是的，该值存储为哈希类型。在这种情况下，键的名称应该是`controller||<client id>`

6.2.从商店获得客户

```
GET [http://localhost:5000/getClient/1](http://localhost:5000/getClient/1)
Content-Type: application/json{
    "id": "1",
    "firstName": "Evgeny",
    "lastName": "Grishchenko"
}
```

6.3.从存储中删除客户端

```
DELETE [http://localhost:5000/deleteClient/1](http://localhost:5000/deleteClient/1)
```

# **解释**

在我们的 ubernetes 集群中，我们有类型为`state.redis.`的 Dapr 组件`statestore`

为什么我们需要这额外的一层？我们可以直接从我们的 dotnet 核心项目中使用 redis。

但是如果我需要换成 Memcached、MongoDB 或者 Azure SQL 呢？

**使用 Dapr，您不需要更改源代码中的任何内容，您只需要使用另一个 Dapr 状态存储组件。**

[在这里](https://docs.dapr.io/operations/components/setup-state-store/supported-state-stores/)你可以找到受支持的州商店列表。