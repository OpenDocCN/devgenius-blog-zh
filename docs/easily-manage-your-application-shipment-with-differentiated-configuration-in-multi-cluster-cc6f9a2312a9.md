# 借助多集群中的差异化配置，轻松管理您的应用交付

> 原文：<https://blog.devgenius.io/easily-manage-your-application-shipment-with-differentiated-configuration-in-multi-cluster-cc6f9a2312a9?source=collection_archive---------11----------------------->

*由段威@BinaryHB 在推特上发布*

在当今的多集群业务场景下，我们经常会遇到这些典型的需求:分布到多个特定的集群，根据业务需求进行特定的分组分布，以及针对多集群的差异化配置。

KubeVela v1.3 在之前多集群功能的基础上进行迭代。本文将揭示如何使用它进行快速的多集群部署和管理，以解决您的所有焦虑。

# 开始前

1.  准备一个 Kubernetes 集群作为 KubeVela 的控制平面。
2.  确保已经成功安装了 [KubeVela v1.3](https://github.com/oam-dev/kubevela/releases/tag/v1.3.0) 和 KubeVela CLI v1.3.0。
3.  您想要管理的子集群中的 Kubeconfig 列表。我们将以命名为北京-1、北京-2 和美国-西方-1 的三个集群为例。
4.  下载并结合[多集群-演示](https://github.com/oam-dev/samples/tree/master/12.Multi_Cluster_Demo)以更好地理解如何使用 KubeVela 多集群功能。

# 分发到多个指定的集群

分布多个指定集群是最基本的多集群管理操作。在 KubeVela 中，您将使用名为`topology`的策略来实现它。该集群将在属性`clusters`中列出，这是一个数组。

首先，让我们确保将 kubeconfig 切换到控制平面集群，使用`vela cluster join`将其包括在北京-1、北京-2 和美国-西方-1 的 3 个集群中:

```
➜   vela cluster join beijing-1.kubeconfig --name beijing-1
➜   vela cluster join beijing-2.kubeconfig --name beijing-2
➜   vela cluster join us-west-1.kubeconfig --name us-west-1
➜   vela cluster list
CLUSTER        	TYPE           	ENDPOINT                 	ACCEPTED	LABELS
beijing-1      	X509Certificate	https://47.95.22.71:6443 	true
beijing-2      	X509Certificate	https://47.93.117.83:6443	true
us-west-1      	X509Certificate	https://47.88.31.118:6443	true
```

然后打开 multi-cluster-demo，查看`Basic.yaml`:

```
apiVersion: core.oam.dev/v1beta1
kind: Application
metadata:
  name: example-app
  namespace: default
spec:
  components:
    - name: hello-world-server
      type: webservice
      properties:
        image: crccheck/hello-world
        port: 8000
      traits:
        - type: scaler
          properties:
            replicas: 3
        - type: gateway
          properties:
            domain: testsvc-mc.example.com
            # classInSpec : true   If the sub clusters has Kubernetes versions below v1.20 installed, please add this field
            http:
              "/": 8000
  policies:
    - type: topology
      name: beijing-clusters
      properties:
        clusters: ["beijing-1","beijing-2"]
```

可以看到，这个 app 使用了`webservice`类型的组件，通过`topology`策略将 3 个部署分发给北京-1 和北京-2 集群。

请注意，将资源成功分配到受管集群的前提是，它必须包含与控制平面完全相同的命名空间。因为默认情况下每个集群都有`**default**`名称空间，所以在这种情况下我们不用担心。但是假设我们将`**basic.yaml**`中的名称空间改为`**multi-cluster**`，我们将收到一个错误:

```
... 
 Status:    	runningWorkflowWorkflow: mode: DAG
  finished: false
  Suspend: false
  Terminated: false
  Steps
  - id:9fierfkhsc
    name:deploy-beijing-clusters
    type:deploy
    phase:failed
    message:step deploy: step deploy: run step(provider=oam,do=components-apply): Found 1 errors. [(failed to apply component beijing-1-multi-cluster-0: HandleComponentsRevision: failed to create componentrevision beijing-1/multi-cluster/hello-world-server-v1: namespaces "multi-cluster" not found)]Services:
...
```

**在 KubeVela 的未来版本中，我们计划支持一个全面的认证系统，更方便、更安全地:通过 hub cluster 以快速移动的方式在托管集群中创建名称空间。**

创建子集群的命名空间后，返回控制平面集群，创建应用程序并分发资源:

```
➜   vela up -f basic.yaml
Applying an application in vela K8s object format...
"patching object" name="example-app" resource="core.oam.dev/v1beta1, Kind=Application"
✅ App has been deployed 🚀🚀🚀
    Port forward: vela port-forward example-app
             SSH: vela exec example-app
         Logging: vela logs example-app
      App status: vela status example-app
  Service status: vela status example-app --svc hello-world-server
```

我们使用`vela status <App Name>`查看该应用的详细信息:

```
➜   vela status example-app
About: Name:      	example-app
  Namespace: 	default
  Created at:	2022-03-25 17:42:33 +0800 CST
  Status:    	runningWorkflow: mode: DAG
  finished: true
  Suspend: false
  Terminated: false
  Steps
  - id:wftf9d4exj
    name:deploy-beijing-clusters
    type:deploy
    phase:succeeded
    message:Services: - Name: hello-world-server
    Cluster: beijing-1  Namespace: default
    Type: webservice
    Healthy Ready:3/3
    Traits:
      ✅ scaler      ✅ gateway: Visiting URL: testsvc-mc.example.com, IP: 60.205.222.30
  - Name: hello-world-server
    Cluster: beijing-2  Namespace: default
    Type: webservice
    Healthy Ready:3/3
    Traits:
      ✅ scaler      ✅ gateway: Visiting URL: testsvc-mc.example.com, IP: 182.92.222.128
```

北京-1 和北京-2 都发布了相应的资源，它们也显示了外部访问 IP 地址，因此您可以将其公开给您的用户。

# 使用分类标签进行分组

除了上述基本需求，我们还经常会遇到其他情况:跨区域部署到某些集群，指定哪个云提供商的集群等。为了实现类似的目标，可以使用`labels`功能。

这里，假设 us-west-1 集群来自 AWS，我们必须额外应用于 AWS 集群。您可以使用`vela cluster labels add`命令来标记集群。当然，如果有更多与 AWS 相关的集群，如 us-west-2，也将在它们被标记后进行处理:

```
➜  ~ vela cluster labels add us-west-1 provider=AWS
Successfully update labels for cluster us-west-1 (type: X509Certificate).
provider=AWS
➜  ~ vela cluster list
CLUSTER        	TYPE           	ENDPOINT                 	ACCEPTED	LABELS
beijing-1      	X509Certificate	https://47.95.22.71:6443 	true
beijing-2      	X509Certificate	https://47.93.117.83:6443	true
us-west-1      	X509Certificate	https://47.88.31.118:6443	true    	provider=AWS
```

接下来，我们更新`basic.yaml`以添加应用程序策略`topology-aws`:

```
...
  policies:
    - type: topology
      name: beijing-clusters
      properties:
        clusters: ["beijing-1","beijing-2"]
    - type: topology
      name: topology-aws
      properties:
        clusterLabelSelector:
          provider: AWS
```

为了节省您的时间，请直接部署`intermediate.yaml`:

```
➜  ~ vela up -f intermediate.yaml
```

再次检查应用程序的状态:

```
➜   vela status example-app... - Name: hello-world-server
    Cluster: us-west-1  Namespace: default
    Type: webservice
    Healthy Ready:3/3
    Traits:
      ✅ scaler      ✅ gateway: Visiting URL: testsvc-mc.example.com, IP: 192.168.40.10
```

# 差异化配置

除了上述场景，我们倾向于有更多的应用程序战略需求，例如希望分发 5 个副本的高可用性。在这种情况下，使用`override`策略:

```
...        
        clusterLabelSelector:
          provider: AWS
    -  type: override
       name: override-high-availability
       properties:
          components:
            - type: webservice
              traits:
              - type: scaler
                properties:
                  replicas: 5
```

同时，我们希望只有 AWS 集群才能获得高可用性。然后我们可以期待 KubeVela 的工作流程帮我们一把。我们使用以下工作流程:它旨在通过以下方式部署此应用程序，首先通过`deploy-beijing`策略分发到北京的集群，然后向标记为 AWS 的集群分发 5 个副本:

```
...                
                properties:
                  replicas: 5
  workflow:
    steps:
      - type: deploy
        name: deploy-beijing
        properties:
          policies: ["beijing-clusters"]
      - type: deploy
        name: deploy-aws
        properties:
          policies: ["override-high-availability","topology-aws"]
```

然后，我们将上述策略和工作流附加到`intermediate.yaml`并使其成为`advanced.yaml`:

```
...
  policies:
    - type: topology
      name: beijing-clusters
      properties:
        clusters: ["beijing-1","beijing-2"]
    - type: topology
      name: topology-aws
      properties:
        clusterLabelSelector:
          provider: AWS
    -  type: override
       name: override-high-availability
       properties:
          components:
            - type: webservice
              traits:
              - type: scaler
                properties:
                  replicas: 5
  workflow:
    steps:
      - type: deploy
        name: deploy-beijing
        properties:
          policies: ["beijing-clusters"]
      - type: deploy
        name: deploy-aws
        properties:
          policies: ["override-high-availability","topology-aws"]
```

然后部署它，查看应用程序的状态:

```
➜   vela up -f advanced.yaml
Applying an application in vela K8s object format...
"patching object" name="example-app" resource="core.oam.dev/v1beta1, Kind=Application"
✅ App has been deployed 🚀🚀🚀
    Port forward: vela port-forward example-app
             SSH: vela exec example-app
         Logging: vela logs example-app
      App status: vela status example-app
  Service status: vela status example-app --svc hello-world-serverapplication.core.oam.dev/podinfo-app configured

➜   vela status example-app... - Name: hello-world-server
    Cluster: us-west-1  Namespace: default
    Type: webservice
    Healthy Ready:5/5
    Traits:
      ✅ scaler      ✅ gateway: Visiting URL: testsvc-mc.example.com, IP: 192.168.40.10
```

以上是我们这次想与你分享的，谢谢你阅读和尝试。

[我们邀请您进一步探索 KubeVela v1.3，以满足更复杂的业务需求，例如深入挖掘差异化配置，使用`override`应用程序策略覆盖一种类型的所有资源或仅覆盖某些特定组件。](https://kubevela.io/docs/install)