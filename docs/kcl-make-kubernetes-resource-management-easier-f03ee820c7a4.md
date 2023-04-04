# KCL —使 Kubernetes 资源管理更容易

> 原文：<https://blog.devgenius.io/kcl-make-kubernetes-resource-management-easier-f03ee820c7a4?source=collection_archive---------10----------------------->

什么是 KCL

[KCL (Kusion 配置语言)](https://github.com/KusionStack/KCLVM)是一种开源的基于约束的记录和函数语言。KCL 通过成熟的编程语言技术和实践，改进大量复杂配置的编写，致力于围绕配置构建更好的模块化、可扩展性和稳定性，逻辑编写更简单，自动化速度快，生态扩展性好。

当我们部署软件系统时，我们不认为它们是固定的。不断发展的业务需求、基础设施需求和其他因素意味着系统在不断变化。当我们需要快速改变系统行为，并且改变过程需要昂贵且冗长的重构和重新部署过程时，业务代码的改变往往是不够的。配置可以为我们提供一种低开销的方式来改变系统功能。例如，我们经常为我们的系统配置编写如下所示的 JSON 或 YAML 文件。

*   JSON 配置

```
{
    "server": {
        "addr": "127.0.0.1",
        "listen": 4545
    },
    "database": {
        "enabled": true,
        "ports": [
            8000,
            8001,
            8002
        ],
    }
}
```

*   YAML 构型

```
server:
  addr: 127.0.0.1
  listen: 4545
database:
  enabled: true
  ports:
  - 8000
  - 8001
  - 8002
```

我们可以根据需要选择将静态配置存储在 JSON 和 YAML 文件中。此外，配置还可以用高级语言存储，这允许更灵活的配置，可以对其进行编码、呈现和静态配置。KCL 就是这样一种配置语言。我们可以编写 KCL 代码来生成 JSON/YAML 和其他配置。在本文中，我们将重点介绍如何使用 KCL 来生成和管理 Kubernetes 资源，并通过一些简单的示例让您快速入门。我们将在接下来的文章中进一步展开。

为什么使用 KCL

当我们管理 Kubernetes 资源时，我们经常手动维护它，或者使用 Helm 和 Kustomize 工具来维护我们的 YAML 配置或配置模板，然后通过 kubectl 工具将资源应用到集群。但是，作为一名“YAML 工程师”，每天维护 YAML 配置无疑是琐碎枯燥的，而且容易出错。例如如下:

```
apiVersion: apps/v1
kind: Deployment
metadata: ... # Omit
spec:
  selector:
    matchlabels:
      cell: RZ00A
  replicas: 2
  template:
    metadata: ... # Omit
    spec:
      tolerations:
      - effect: NoSchedules
        key: is-over-quota
        operator: Equal
        value: 'true'
      containers:
      - name: test-app
          image: images.example/app:v1 # Wrong ident
        resources:
          limits:
            cpu: 2 # Wrong type. The type of cpu should be str
            memory: 4Gi
            # Field missing: ephemeral-storage
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: is-over-quota
                operator: In
                values:
                - 'true'
```

*   YAML 中的结构化数据是非类型化的，并且缺乏验证方法，因此无法立即检查所有数据的有效性。
*   YAML 的编程能力很差。很容易写出不正确的缩进，也没有逻辑判断等常用的代码组织方法。容易写大量重复配置，维护困难。
*   Kubernetes 的设计比较复杂，用户很难理解所有的细节，比如上面配置中的`toleration`和`affinity`字段。如果用户不理解调度逻辑，可能会错误地省略或添加多余的内容。

因此，KCL 希望在 Kubernetes YAML 资源管理中解决以下问题:

*   使用**生产级高性能编程语言**到**编写代码**提高配置的灵活性，如条件语句、循环、函数、包管理等特性提高配置复用能力。
*   提高**配置语义验证**在代码层面的能力，如可选/必填字段、类型、范围等配置检查。
*   提供**编写、组合和抽象配置块**的能力，如结构定义、结构继承、约束定义等。

如何使用 KCL 生成和管理 Kubernetes 资源

先决条件

首先可以访问 [KCL 项目主页](https://github.com/KusionStack/KCLVM)按照说明下载安装 KCL，然后准备一个 [Kubernetes](https://kubernetes.io/) 环境。

生成 Kubernetes 清单

我们可以写下面的 KCL 代码，命名为`main k`。KCL 的灵感来源于 Python。它的基本语法非常接近 Python，简单易学。配置模式很简单，`k [: T] = v`，其中`k`表示配置的属性名，`v`表示配置的属性值，`: T`表示可选的类型注释。

```
apiVersion = "apps/v1"
kind = "Deployment"
metadata = {
    name = "nginx"
    labels.app = "nginx"
}
spec = {
    replicas = 3
    selector.matchLabels = metadata.labels
    template.metadata.labels = metadata.labels
    template.spec.containers = [
        {
            name = metadata.name
            image = "${metadata.name}:1.14.2"
            ports = [{ containerPort = 80 }]
        }
    ]
}
```

在上面的 KCL 代码中，我们声明了一个 Kubernetes `Deployment`资源的`apiVersion`、`kind`、`metadata`、`spec`等变量，并分别赋予相应的内容。特别是，我们将分配在`spec.selector.matchLabels`和`spec.template.metadata.labels`字段中被重用的`metadata.labels`字段。可以看出，与 YAML 相比，KCL 定义的数据结构更加紧凑，通过定义局部变量可以实现配置重用。

我们可以通过执行下面的命令行来获得一个 Kubernetes YAML 文件

```
kcl main.k
```

输出是

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

当然，我们可以将 KCL 与 kubectl 和其他工具一起使用。让我们执行以下命令并查看结果:

```
$ kcl main.k | kubectl apply -f -

deployment.apps/nginx-deployment configured
```

从命令行可以看出，与直接使用 YAML 配置和 kubectl 应用的部署体验完全一致。

通过 kubectl 检查部署状态

```
$ kubectl get deploy

NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           15s
```

编写代码来管理 Kubernetes 资源

在发布 Kubernetes 资源时，我们经常会遇到需要动态指定配置参数的场景。比如不同的环境需要设置不同的`image`字段值来生成不同环境下的资源。对于这个场景，我们可以通过 KCL 条件语句和`option`函数动态接收外部参数。基于上面的例子，我们可以根据不同的环境调整配置参数。例如，对于下面的代码，我们编写了一个条件语句，并输入了一个名为`env`的动态参数。

```
apiVersion = "apps/v1"
kind = "Deployment"
metadata = {
    name = "nginx"
    labels.app = "nginx"
}
spec = {
    replicas = 3
    selector.matchLabels = metadata.labels
    template.metadata.labels = metadata.labels
    template.spec.containers = [
        {
            name = metadata.name
            image = "${metadata.name}:1.14.2" if option("env") == "prod" else "${metadata.name}:latest"
            ports = [{ containerPort = 80 }]
        }
    ]
}
```

使用 KCL 命令行`-D`标志接收外部动态参数:

```
kcl main.k -D env=prod
```

输出是

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

上面代码片段中的`image=metadata.name+": 1.14.2" if option ("env")=="prod" else metadata.name + ": latest"`是指当动态参数`env`的值设置为`prod`时，图像字段的值为`nginx: 1.14.2`；否则就是‘nginx:latest’。因此，我们可以根据需要将 env 设置为不同的值，以获得不同内容的 Kubernetes 资源。

KCL 还支持在配置文件中维护 option 函数的动态参数，比如写`kcl.yaml`文件。

```
kcl_options:
  - key: env
    value: prod
```

使用下面的命令行简化 KCL 动态参数的输入过程，可以获得相同的 YAML 输出。

```
kcl main.k -Y kcl.yaml
```

输出是:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

然后

本文简要介绍了使用 KCL 编写配置的快速入门，以及使用 KCL 来定义和管理 Kubernetes 资源。

在这个阶段，Kustomize 和 Helm 已经逐渐发展成为 Kubernetes 配置定义和管理领域事实上的标准。熟悉 Kubernetes 的小伙伴可能更喜欢写显式配置。Kustomize 和 Helm 在使用 KCL 编写和渲染配置文件方面有什么异同？考虑到很多合作伙伴已经在使用 Helm 和 Kustomize 这样的工具，我将在下一篇文章中介绍 KCL 方法来编写相应的配置代码。

更多精彩内容，请访问:

*   KCL:[https://github.com/KusionStack/KCLVM](https://github.com/KusionStack/KCLVM)
*   库辛:[https://github.com/KusionStack/kusion](https://github.com/KusionStack/kusion)
*   konfig:[https://github.com/KusionStack/Konfig](https://github.com/KusionStack/Konfig)

欢迎加入我们的交流社区:

*   [https://github.com/KusionStack/community](https://github.com/KusionStack/community)