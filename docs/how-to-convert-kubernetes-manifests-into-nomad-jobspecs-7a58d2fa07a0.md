# 如何将 Kubernetes 清单转换为 Nomad Jobspecs

> 原文：<https://blog.devgenius.io/how-to-convert-kubernetes-manifests-into-nomad-jobspecs-7a58d2fa07a0?source=collection_archive---------3----------------------->

![](img/51671d6b27bfbff4262b3744eda76ccf.png)

多伦多唐谷高尔夫球场的香蒲。Adri Villela 的照片。

自从我开始[探索 Nomad](https://medium.com/@adri-v/list/justintime-nomad-cc5d249a172b) 以来，我喜欢做的事情之一就是获取 [Docker 撰写文件](https://www.baeldung.com/ops/docker-compose)和 [Kubernetes 清单](https://stackoverflow.com/a/55132302)，并将它们翻译成[hashi corp Nomad](https://nomadproject.io/)[job spec](https://developer.hashicorp.com/nomad/docs/job-specification)。我是在 2022 年 3 月为 [Temporal 做的，](https://medium.com/faun/just-in-time-nomad-running-temporal-on-nomad-5fee139f37ea)也是在 2022 年夏天为一个[早期版本的 Tracetest 做的。](https://github.com/lightstep/tracetest-nomad)

在我最新的 Nomadification 项目(TM)中，我得到了在 Nomad 上运行的 [OpenTelemetry 演示应用](https://github.com/open-telemetry/opentelemetry-demo)(当然还有 [HashiQube](https://medium.com/@adri-v/list/hashiqube-bfdcb9c84e10) 、*)。为此，我使用了 [OpenTelemetry 演示应用程序 Helm Chart](https://github.com/open-telemetry/opentelemetry-helm-charts/tree/main/charts/opentelemetry-demo) 作为我的指南。在这样做的过程中，以及其他的 Nomadifications 中，我意识到我从来没有解释过从 Kubernetes 清单到 Nomad jobspecs 的转换过程。*

*所以，你可能已经猜到了，今天，我将把 Kubernetes 清单转换成 Nomad 工作规范，这样，如果你发现自己处于这样一种情况，你会想，“哎呀，很高兴看到这个 Kubernetes 的东西在 Nomad 上运行，”你现在有一个过程了！*

*我将使用我最近将 [OpenTelemetry 演示应用 Helm Charts](https://github.com/open-telemetry/opentelemetry-helm-charts/tree/main/charts/opentelemetry-demo) 转换为 Nomad jobspecs 的工作中的例子来说明这个过程。*

*你准备好了吗？我们开始吧！*

# *清单、舵表和工作规范…哦，我的天！*

*虽然我喜欢与 Kubernetes 和 Nomad 一起工作，但我发现 Kubernetes 领域有一件事非常令人恼火:一个应用程序部署的 Kubernetes 清单是由各种 Kubernetes 对象的 YAML 定义的寻宝游戏组成的。然而，Nomad 采取了不同的方法，使用一个单一的 [HashiCorp 配置语言(HCL)](https://github.com/hashicorp/hcl) [jobspec](https://developer.hashicorp.com/nomad/docs/job-specification) 文件作为定义你的应用的一站式商店。我个人觉得 Nomad HCL 更容易管理，因为移动部分更少，而且当涉及到将 Kubernetes 清单转换为 Nomad jobspecs 时，我发现使用单个文件会使事情简单很多。*

*为了将 Kubernetes 清单转换成 Nomad jobspec，我们首先需要从一个基本的 Nomad jobspec 开始。这将作为在 Nomad 中部署我们的应用程序的模板。*

*让我们从下面的模板 jobspec 开始。请记住，这是我们转换的起点。毕竟，有些服务比其他服务更复杂，因此对于有些服务，我们需要将下面的所有组件包含在我们的 jobspec 中，对于其他服务，我们可能最终会有一个更精简的 jobspec 版本。*

```
*job "<service_name>" {
  type        = "service"
  datacenters = ["dc1"]

  group "<service_name>" {
    count = 1

    network {
      mode = "host"

      port "<port_name>" {
        to = <port_number>
      }
    }

    service {
      name = "<service_name>"
      port = "<port_name>"
      tags = [<tags_here>]

      check {
        <service_check_here>
      }
    }

    task "<service_name>" {
      driver = "docker"

      config {
        image = "<image_name>"
        image_pull_timeout = "25m"
        args = [<args_go_here>]
        ports = ["<port_name>"]
      }

      restart {
        attempts = 10
        delay    = "15s"
        interval = "2m"
        mode     = "delay"
      }

      env {
          <env_vars_here>
      }      

      template {
        data = <<EOF
<env_vars_derived_from_consul>
EOF
        destination = "local/env"
        env         = true
      }

      resources {
        cpu    = 60
        memory = 650
      }

    }
  }
}*
```

*太好了…现在我们有了 jobspec 模板。耶！但是我们需要填补空白，不是吗？那么…我们从哪里开始？*

*因为我们要从 Kubernetes 到 Nomad，所以我们需要查看应用程序的 Kubernetes 清单。幸运的是，我们可以很容易地从 [OTel 舵图报告](https://github.com/open-telemetry/opentelemetry-helm-charts)中获取这些信息，正如你可能已经猜到的，OTel 演示应用有一个[舵图。它还包含了我们可用的渲染 YAML 清单](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/main/charts/opentelemetry-demo)[这里](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/main/charts/opentelemetry-demo/examples/default/rendered/component.yaml)。*

*OpenTelemetry 演示应用程序由许多服务组成。将每个服务的 Kubernetes 清单转换为其对应的 Nomad jobspec 的过程非常相似，因此为了不让您感到厌烦，我将选择一个服务来说明这种转换:featureflagservice。*

# *转化过程*

*有了 Nomad jobspec 模板和 Kubernetes 清单，我们就可以开始转换了！*

> ***注:** *你可以在这里* *找到包含所有 OpenTelemetry Demo App jobspec 文件的回购。**

## *1-拿到库伯内特公司的货单*

*正如我前面提到的，OpenTelemetry 演示应用程序的渲染 YAML 清单可以在这里获得。因为在本教程中，我们只关心 [featureflagservice](https://github.com/open-telemetry/opentelemetry-demo/tree/main/src/featureflagservice) 的 Kubernetes 清单，所以我已经获取了与 [featureflagservice](https://github.com/open-telemetry/opentelemetry-demo/tree/main/src/featureflagservice) 相关的清单，它由一个[部署](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)和一个[服务](https://kubernetes.io/docs/concepts/services-networking/service/)组成，如下所示。*

*下面是 YAML 的[部署:](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/6fdb0d3d0e22e550c30c0d5c61f428bbe8ed3aa1/charts/opentelemetry-demo/examples/default/rendered/component.yaml#L829-L901)*

```
*---
# Source: opentelemetry-demo/templates/component.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-featureflagservice
  labels:
    helm.sh/chart: opentelemetry-demo-0.14.3
    app.kubernetes.io/name: example
    app.kubernetes.io/instance: example
    app.kubernetes.io/component: featureflagservice
    app.kubernetes.io/version: "1.2.1"
    app.kubernetes.io/part-of: opentelemetry-demo
    app.kubernetes.io/managed-by: Helm
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: example
      app.kubernetes.io/instance: example
      app.kubernetes.io/component: featureflagservice
  template:
    metadata:
      labels:
        app.kubernetes.io/name: example
        app.kubernetes.io/instance: example
        app.kubernetes.io/component: featureflagservice
    spec:
      containers:
        - name: featureflagservice
          image: 'ghcr.io/open-telemetry/demo:v1.2.1-featureflagservice'
          imagePullPolicy: IfNotPresent
          ports:

          - containerPort: 50053
            name: grpc
          - containerPort: 8081
            name: http
          env:
          - name: OTEL_SERVICE_NAME
            valueFrom:
              fieldRef:
                apiVersion: v1
                fieldPath: metadata.labels['app.kubernetes.io/component']
          - name: OTEL_K8S_NAMESPACE
            valueFrom:
              fieldRef:
                apiVersion: v1
                fieldPath: metadata.namespace
          - name: OTEL_K8S_NODE_NAME
            valueFrom:
              fieldRef:
                apiVersion: v1
                fieldPath: spec.nodeName
          - name: OTEL_K8S_POD_NAME
            valueFrom:
              fieldRef:
                apiVersion: v1
                fieldPath: metadata.name
          - name: FEATURE_FLAG_GRPC_SERVICE_PORT
            value: "50053"
          - name: FEATURE_FLAG_SERVICE_PORT
            value: "8081"
          - name: OTEL_EXPORTER_OTLP_TRACES_PROTOCOL
            value: grpc
          - name: DATABASE_URL
            value: ecto://ffs:ffs@example-ffspostgres:5432/ffs
          - name: OTEL_EXPORTER_OTLP_ENDPOINT
            value: http://example-otelcol:4317
          - name: OTEL_RESOURCE_ATTRIBUTES
            value: service.name=$(OTEL_SERVICE_NAME),k8s.namespace.name=$(OTEL_K8S_NAMESPACE),k8s.node.name=$(OTEL_K8S_NODE_NAME),k8s.pod.name=$(OTEL_K8S_POD_NAME)
          resources:
            limits:
              memory: 175Mi*
```

*这里是[服务 YAML](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/6fdb0d3d0e22e550c30c0d5c61f428bbe8ed3aa1/charts/opentelemetry-demo/examples/default/rendered/component.yaml#L121-L147) :*

```
*---
# Source: opentelemetry-demo/templates/component.yaml
apiVersion: v1
kind: Service
metadata:
  name: example-featureflagservice
  labels:
    helm.sh/chart: opentelemetry-demo-0.14.3
    app.kubernetes.io/name: example
    app.kubernetes.io/instance: example
    app.kubernetes.io/component: featureflagservice
    app.kubernetes.io/version: "1.2.1"
    app.kubernetes.io/part-of: opentelemetry-demo
    app.kubernetes.io/managed-by: Helm
spec:
  type: ClusterIP
  ports:
    - port: 50053
      name: grpc
      targetPort: 50053
    - port: 8081
      name: http
      targetPort: 8081
  selector:
    app.kubernetes.io/name: example
    app.kubernetes.io/instance: example
    app.kubernetes.io/component: featureflagservice*
```

*呀！这一切看起来让人不知所措，不是吗？幸运的是，它没有看起来那么可怕。别担心…我会指引你的。我们继续吧。*

## *2-准备工作说明书*

*有了我们的 Kubernetes YAMLs，让我们回到前面的 jobspec 模板，并填充一些空白。因为我们知道我们正在使用的是 [featureflagservice](https://github.com/open-telemetry/opentelemetry-demo/tree/main/src/featureflagservice) ，所以我将`<service_name>` 替换为`featureflagservice`，这意味着现在我们的模板看起来像这样:*

```
*job "featureflagservice" {
  type        = "service"
  datacenters = ["dc1"]

  group "featureflagservice" {
    count = 1

    network {
      mode = "host"

      port "<port_name>" {
        to = <port_number>
      }
    }

    service {
      name = "<service_name>"
      port = "<port_name>"
      tags = [<tags_here>]

      check {
        <service_check_here>
      }
    }

    task "featureflagservice" {
      driver = "docker"

      config {
        image = "<image_name>"
        image_pull_timeout = "25m"
        args = [<args_go_here>]
        entrypoint = [<entrypoints_go_here>]
        ports = ["<port_name>"]
      }

      restart {
        attempts = 10
        delay    = "15s"
        interval = "2m"
        mode     = "delay"
      }

      env {
          <env_vars_here>
      }      

      template {
        data = <<EOF
<env_vars_derived_from_consul>
EOF
        destination = "local/env"
        env         = true
      }

      resources {
        cpu    = 60
        memory = 650
      }

    }
  }
}*
```

> ***注意:** *从技术上讲，你可以给你的工作、任务和团队取不同的名字，比如* `*featureflagservice-job*` *，* `*featureflagservice-task*` *和* `*featureflagservice-group*` *(或者任何你想取的名字)，但是为了简单起见(有点缺乏创意)，我决定给它们取同一个名字:* `*featureflagservice*` *。**

*一些有用的术语:*

*   *`job`是控制的单位。工作是你开始、停止和更新的事情。*
*   *`group`是刻度的单位。该组定义了您正在运行的实例数量。*
*   *`task`是工作的单位。任务是您实际想要运行的。*

## *3 端口定义*

*我们需要填充的下一组空白在[网络](https://developer.hashicorp.com/nomad/docs/job-specification/network)节中。更具体地说，端口节中的`<port_name>`和`<port_number>`值。*

*如果我们看看上面的 [featureflagservice](https://github.com/open-telemetry/opentelemetry-demo/tree/main/src/featureflagservice) 的[服务 YAML](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/6fdb0d3d0e22e550c30c0d5c61f428bbe8ed3aa1/charts/opentelemetry-demo/examples/default/rendered/component.yaml#L121-L147) ，你会注意到它公开了两个端口:`50053` (gRPC)和`8081` (HTTP)，每个`spec -> ports -> targetPort`。让我们将这些插入到我们的工作规范中:*

```
*network {
  mode = "host"

  port "http" {
    to = 8081
  }
  port "grpc" {
    to = 50053
  }
}*
```

*正如您在上面的片段中看到的，我们标记(命名)了我们的端口`http`和`grpc`。这些标签将允许我们用一个人性化的标签来指代这些端口，而不是用数字。这意味着，如果一个或两个端口号发生变化，我们只需要在一个地方进行更改。剧透:我们将在 jobspec 的其他地方提到这些端口。*

> ***注意:** *你可以随意给你的端口贴上任何你想要的标签——只要确保它是合理的描述性的。**

## *4-服务定义*

*既然我们已经定义了我们的端口，我们需要注册我们的服务，这是通过[服务](https://developer.hashicorp.com/nomad/docs/job-specification/service)节来完成的。由于我们在上面的[网络](https://developer.hashicorp.com/nomad/docs/job-specification/network)节中有两个端口，我们需要定义两个[服务](https://developer.hashicorp.com/nomad/docs/job-specification/service):每个端口一个。*

*`http`端口的[服务](https://developer.hashicorp.com/nomad/docs/job-specification/service)定义如下:*

```
*service {
  name = "featureflagservice-http"
  port = "http"
  tags = [
    "traefik.http.routers.otel-collector-http.rule=Host(`featureflag.localhost`)",
    "traefik.http.routers.otel-collector-http.entrypoints=web",
    "traefik.http.routers.otel-collector-http.tls=false",
    "traefik.enable=true",
  ]

  check {
    type     = "tcp"
    interval = "10s"
    timeout  = "5s"
  }
}*
```

*值得注意的事项:*

*   *默认情况下，[服务](https://developer.hashicorp.com/nomad/docs/job-specification/service)注册到[咨询](https://consul.io/)。虽然我们没有明确这样说，但这相当于给[服务](https://developer.hashicorp.com/nomad/docs/job-specification/service)节添加了一个`provider = “consul”`属性。你可以向 Nomad 或 Consul 注册你的服务。*
*   *[端口](https://developer.hashicorp.com/nomad/docs/job-specification/service#port)属性是[服务](https://developer.hashicorp.com/nomad/docs/job-specification/service)应用的[网络](https://developer.hashicorp.com/nomad/docs/job-specification/network)端口标签。你完全可以使用你想要的端口号。*
*   *这个[服务](https://developer.hashicorp.com/nomad/docs/job-specification/service)叫做`featureflagservice-http`。同样，您可以随意称呼它，尽管描述性的名称总是有帮助的。*
*   *我们通过 [Traefik](https://traefik.io/) 向外界公开这个[服务](https://developer.hashicorp.com/nomad/docs/job-specification/service)，并且可以通过 URL:[http://feature flag . localhost](http://featureflag.localhost/)访问该服务(因为我是使用 HashiQube 在本地运行这个服务的[)。请记住，您还需要将](https://opentelemetry.io/blog/2022/otel-demo-app-nomad/) [Traefik jobspec](https://github.com/avillela/nomad-conversions/blob/main/otel-demo-app/jobspec/traefik.nomad) 与[featureflagservice job spec](https://github.com/avillela/nomad-conversions/blob/main/otel-demo-app/jobspec/featureflagservice.nomad)一起部署，以便向外界公开该服务。要了解更多关于在 Nomad 上运行 Traefik 的信息，请查看[这篇文章](https://medium.com/@adri-v/just-in-time-nomad-running-traefik-on-hashiqube-7d6dfd8ef9d8)和[这篇文章](https://medium.com/dev-genius/running-hashiqube-using-the-vagrant-docker-provider-3e551c0eca97)。*
*   *[检查](https://developer.hashicorp.com/nomad/docs/job-specification/check)部分对服务进行健康检查，因为[服务](https://developer.hashicorp.com/nomad/docs/job-specification/service)已注册到 Consul，所以健康检查在 Consul 上运行。上面的运行状况检查配置为每 10 秒运行一次，并且有 5 秒钟的时间让运行状况检查查询成功。Nomad 中的健康检查类似于 [Kubernetes 活性探针](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-a-liveness-command)。设置`on_update`属性创建了一个更接近于 [Kubernetes 就绪探测器](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-readiness-probes)的东西。*

*`grpc`端口的服务如下所示:*

```
*service {
  name = "featureflagservice-grpc"
  port = "grpc"

  check {
    type     = "tcp"
    interval = "10s"
    timeout  = "5s"
  }
}*
```

*值得注意的事项:*

*   *因为我们没有公开任何外部服务，所以 Traefik 配置不需要 tags 属性。*
*   *port 属性指的是我们在前面的 network 小节中定义的 grpc 端口。*
*   *我们正在进行与对`http`端口相同的运行状况检查。*

*有关健康检查的其他示例，请查看:*

*   *[推荐服务工作规范](https://github.com/avillela/nomad-conversions/blob/ed12ec3d4092a7816aadd2d761a98f9ef51dfb74/otel-demo-app/jobspec/recommendationservice.nomad#L25-L30)中的 gRPC 健康检查*
*   *基于命令的健康检查，用于检查 [ffspostgres 作业规范](https://github.com/avillela/nomad-conversions/blob/ed12ec3d4092a7816aadd2d761a98f9ef51dfb74/otel-demo-app/jobspec/ffspostgres.nomad#L63-L74)中的数据库连接*

## *5-任务定义*

*好了…现在我们准备定义我们的[任务](https://developer.hashicorp.com/nomad/docs/drivers/docker)。因为我们正在运行容器化的工作负载，所以我们的任务使用了 [Docker 驱动程序](https://developer.hashicorp.com/nomad/docs/drivers/docker)。*

***配置节***

*由于我们使用的是 [Docker 驱动](https://developer.hashicorp.com/nomad/docs/drivers/docker)，我们需要通过配置节向 Nomad 提供以下信息:*

*   ***Docker 图像的名称。**我们从[部署 YAML](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/6fdb0d3d0e22e550c30c0d5c61f428bbe8ed3aa1/charts/opentelemetry-demo/examples/default/rendered/component.yaml#L829-L901) 中的[规范- >模板- >规范- >容器- >图片](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/6fdb0d3d0e22e550c30c0d5c61f428bbe8ed3aa1/charts/opentelemetry-demo/examples/default/rendered/component.yaml#L858)中获取此信息。在这种情况下，图像名称是`ghcr.io/open-telemetry/demo:v1.2.1-featureflagservice`。*
*   ***Docker 映像正在使用的端口。**我们从[部署 YAML](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/6fdb0d3d0e22e550c30c0d5c61f428bbe8ed3aa1/charts/opentelemetry-demo/examples/default/rendered/component.yaml#L829-L901) 中的[规范- >模板- >规范- >容器- >端口](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/6fdb0d3d0e22e550c30c0d5c61f428bbe8ed3aa1/charts/opentelemetry-demo/examples/default/rendered/component.yaml#L860-L865)中获得此信息。在这种情况下，映像需要端口`50053`和`8081`，在我们的 jobspec 的 [network](https://developer.hashicorp.com/nomad/docs/job-specification/network) 节中，我们分别将其命名为 http 和 grpc*

```
*apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-featureflagservice
...
spec:
...
    spec:
      containers:
        - name: featureflagservice
          image: 'ghcr.io/open-telemetry/demo:v1.2.1-featureflagservice'
...*
```

*这转化为`featureflagservice` [任务](https://developer.hashicorp.com/nomad/docs/drivers/docker)的`config`节，如下所示:*

```
*config {
  image = "ghcr.io/open-telemetry/demo:v1.2.1-featureflagservice"
  image_pull_timeout = "25m"
  ports = ["http", "grpc"]
}*
```

*一些值得注意的事项:*

*   *因为在[部署 YAML](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/6fdb0d3d0e22e550c30c0d5c61f428bbe8ed3aa1/charts/opentelemetry-demo/examples/default/rendered/component.yaml#L829-L901) 中没有[参数](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/#define-a-command-and-arguments-when-you-create-a-pod)，所以我们在这个 jobspec 中省略了`args`。如果你想看一个使用`args`的 jobspec 示例，请查看 [Prometheus jobspec](https://github.com/avillela/nomad-conversions/blob/ed12ec3d4092a7816aadd2d761a98f9ef51dfb74/otel-demo-app/jobspec/prometheus.nomad#L27-L40) 及其对应的[部署 YAML](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/2e467f23fdb6a236abc7effb037ef9330bbbb9f1/charts/opentelemetry-demo/examples/default/rendered/prometheus/deploy.yaml#L38-L46) 。*
*   *由于在[部署 YAML](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/6fdb0d3d0e22e550c30c0d5c61f428bbe8ed3aa1/charts/opentelemetry-demo/examples/default/rendered/component.yaml#L829-L901) 中没有[命令](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/#define-a-command-and-arguments-when-you-create-a-pod)，我们在此 jobspec 中省略了`entrypoint`。如果您想查看使用`entrypoint`的作业规范示例，请查看 [OTel 收集器作业规范](https://github.com/avillela/nomad-conversions/blob/ed12ec3d4092a7816aadd2d761a98f9ef51dfb74/otel-demo-app/jobspec/otel-collector.nomad#L54-L58)及其对应的[部署 YAML](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/6fdb0d3d0e22e550c30c0d5c61f428bbe8ed3aa1/charts/opentelemetry-demo/examples/default/rendered/opentelemetry-collector/deployment.yaml#L41-L46) 。*
*   *[image_pull_timeout](https://developer.hashicorp.com/nomad/docs/drivers/docker#infra_image_pull_timeout) 设置为 25 分钟。这是一个可选值，如果您忽略它，它默认为 5 分钟。我将它设置为一个较高的值，因为有时你永远不知道你的网络是否决定给你一根手指，并且[我不希望作业失败，因为它不能在分配的时间内提取图像](https://danielabaron.me/blog/nomad-tips-and-tricks/#increase-docker-pull-timeout)。*

***Env 节***

*我们还没有完全配置完我们的 featureflagservice [任务](https://developer.hashicorp.com/nomad/docs/drivers/docker)。如果您查看[部署 YAML](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/6fdb0d3d0e22e550c30c0d5c61f428bbe8ed3aa1/charts/opentelemetry-demo/examples/default/rendered/component.yaml#L829-L901) ，您会注意到在`env`标签下有许多环境变量:*

```
*env:
  - name: OTEL_SERVICE_NAME
    valueFrom:
      fieldRef:
        apiVersion: v1
        fieldPath: metadata.labels['app.kubernetes.io/component']
  - name: OTEL_K8S_NAMESPACE
    valueFrom:
      fieldRef:
        apiVersion: v1
        fieldPath: metadata.namespace
  - name: OTEL_K8S_NODE_NAME
    valueFrom:
      fieldRef:
        apiVersion: v1
        fieldPath: spec.nodeName
  - name: OTEL_K8S_POD_NAME
    valueFrom:
      fieldRef:
        apiVersion: v1
        fieldPath: metadata.name
  - name: FEATURE_FLAG_GRPC_SERVICE_PORT
    value: "50053"
  - name: FEATURE_FLAG_SERVICE_PORT
    value: "8081"
  - name: OTEL_EXPORTER_OTLP_TRACES_PROTOCOL
    value: grpc
  - name: DATABASE_URL
    value: ecto://ffs:ffs@example-ffspostgres:5432/ffs
  - name: OTEL_EXPORTER_OTLP_ENDPOINT
    value: http://example-otelcol:4317
  - name: OTEL_RESOURCE_ATTRIBUTES
    value: service.name=$(OTEL_SERVICE_NAME),k8s.namespace.name=$(OTEL_K8S_NAMESPACE),k8s.node.name=$(OTEL_K8S_NODE_NAME),k8s.pod.name=$(OTEL_K8S_POD_NAME)*
```

*您可以忽略以`OTEL_K8S_`开头的，因为它们是特定于 Kubernetes 的；然而，我们确实关心这些:*

*   *`OTEL_SERVICE_NAME`*
*   *`FEATURE_FLAG_GRPC_SERVICE_PORT`*
*   *`FEATURE_FLAG_SERVICE_PORT`*
*   *`OTEL_EXPORTER_OTLP_TRACES_PROTOCOL`*
*   *`DATABASE_URL`*
*   *`OTEL_EXPORTER_OTLP_ENDPOINT`*
*   *`OTEL_RESOURCE_ATTRIBUTES`*

*那么我们如何在 Nomad 中配置这些呢？通过[任务](https://developer.hashicorp.com/nomad/docs/drivers/docker)的 [env](https://developer.hashicorp.com/nomad/docs/job-specification/env) 节。这意味着我们的环境变量看起来像这样:*

```
*env {
  FEATURE_FLAG_GRPC_SERVICE_PORT = "${NOMAD_PORT_grpc}"
  FEATURE_FLAG_SERVICE_PATH_ROOT = "\"/feature\""
  FEATURE_FLAG_SERVICE_PORT = "${NOMAD_PORT_http}"
  OTEL_EXPORTER_OTLP_TRACES_PROTOCOL = "grpc"
  OTEL_RESOURCE_ATTRIBUTES = "service.name=featureflagservice"
}*
```

*一些值得注意的事项:*

*   *我们使用的是`NOMAD_PORT_grpc`和`NOMAD_PORT_http`，而不是将`FEATURE_FLAG_GRPC_SERVICE_PORT`的值硬编码为`50053`和`8081`。这些实际上是[运行时环境变量](https://developer.hashicorp.com/nomad/docs/runtime/environment)，遵循`NOMAD_PORT_<label>`命名约定。这可以防止您对端口号进行硬编码，如果端口号因为任何原因在[网络](https://developer.hashicorp.com/nomad/docs/job-specification/network)节中发生变化，这将非常方便，因为您只需要在一个地方更改端口号。*
*   *如果你查看[部署 YAML](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/6fdb0d3d0e22e550c30c0d5c61f428bbe8ed3aa1/charts/opentelemetry-demo/examples/default/rendered/component.yaml#L829-L901) ，你会注意到`OTEL_RESOURCE_ATTRIBUTES`被设置为`service.name=$(OTEL_SERVICE_NAME),k8s.namespace.name=$(OTEL_K8S_NAMESPACE),k8s.node.name=$(OTEL_K8S_NODE_NAME),k8s.pod.name=$(OTEL_K8S_POD_NAME)`。但是我只设置了`OTEL_RESOURCE_ATTRIBUTES`到`service.name=featureflagservice`。为什么？因为[部署 YAML](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/6fdb0d3d0e22e550c30c0d5c61f428bbe8ed3aa1/charts/opentelemetry-demo/examples/default/rendered/component.yaml#L829-L901) 中的其他属性与 Kubernetes 相关，所以我省略了它们。*

***模板节***

*等等…但是为什么`DATABASE_URL`和`OTEL_EXPORTER_OTLP_ENDPOINT`不见了？？如果您查看[部署 YAML](https://github.com/open-telemetry/opentelemetry-helm-charts/blob/6fdb0d3d0e22e550c30c0d5c61f428bbe8ed3aa1/charts/opentelemetry-demo/examples/default/rendered/component.yaml#L829-L901) ，您会注意到上面两个环境变量的值分别是`ecto://ffs:ffs@example-ffspostgres:5432/ffs`和`http://example-otelcol:4317`。*

*这就引出了一个问题……这是如何翻译成游牧语言的？`example-ffspostgres`和`example-otelcol`，分别是 Kubernetes 中 [PostgreSQL](https://www.postgresql.org/) 和 [OpenTelemetry Collector](https://opentelemetry.io/docs/collector/) 的服务名，所以如果我们试图在我们的 jobspec 定义中使用相同的名称，您将会从 Nomad 得到一个严重的错误。*

*我们可以使用这些服务的 IP 地址，但这不是一个好主意，因为服务的 IP 地址肯定会改变，所以如果地址改变，您的 jobspec 将无法部署。*

*我们需要的是一种在给定服务名称的情况下动态获取服务 IP 地址的方法。这就是[领事](https://consul.io/)的作用。除此之外， [Consul 提供了服务发现](https://developer.hashicorp.com/consul/docs/concepts/service-discovery)，这正是我们所需要的。*

*要使用 Consul 服务发现，我们需要以下内容:*

1.  *我们引用的[服务](https://developer.hashicorp.com/nomad/docs/job-specification/service)的名称*
2.  *[游牧模板节](https://developer.hashicorp.com/nomad/docs/job-specification/template)*

*Nomad [模板](https://developer.hashicorp.com/nomad/docs/job-specification/template)节很容易让人想起 Kubernetes 配置图。根据 [Nomad 文档](https://developer.hashicorp.com/nomad/docs/job-specification/template)，模板可以让你“在一个 Nomad 任务中传送配置文件，这些文件是由环境变量、咨询数据、保险库机密或者仅仅是一般配置填充的。”在我们的例子中，我们使用一个模板来查询 Consul 服务，这样我们就可以找到这些服务的 IP 地址和端口号，这样我们就可以填充我们的来填充剩下的两个环境变量，`DATABASE_URL`和`OTEL_EXPORTER_OTLP_ENDPOINT`。其代码如下所示:*

```
*template {
  data = <<EOF
{{ range service "ffspostgres-service" }}
DATABASE_URL = "ecto://ffs:ffs@{{ .Address }}:{{ .Port }}/ffs"
{{ end }}

{{ range service "otelcol-grpc" }}
OTEL_EXPORTER_OTLP_TRACES_ENDPOINT = "http://{{ .Address }}:{{ .Port }}"
{{ end }}
EOF
  destination = "local/env"
  env         = true
}*
```

*值得注意的事项:*

*   *[模板](https://developer.hashicorp.com/nomad/docs/job-specification/template)节在[任务](https://developer.hashicorp.com/nomad/docs/drivers/docker)节中定义。*
*   *从技术上讲，`destination = “local/env”`行可以是任何东西(它基本上是一个标签)，但是真正重要的是`env = true`，它告诉 Nomad 将这些作为环境变量导出*
*   *线路`{{ range service “ffspostgres-service” }}`告诉 Nomad 在 Consul 中寻找一个名为`ffspostgres-service`的服务。一旦它找到服务名，我们就可以分别使用`{{ .Address }}`和`{{ .Port }}`获取服务的 IP 地址和端口号。*
*   *类似地，代码行`{{ range service “otelcol-grpc” }}`告诉 Nomad 寻找一个名为`otelcol-grpc`的服务。一旦它找到服务名，我们就可以分别使用`{{ .Address }}`和`{{ .Port }}`获取服务的 IP 地址和端口号。*

*但是等等…这些服务名到底是从哪里来的？？嗯，还记得我们在上面的**步骤 4** 中定义[服务](https://developer.hashicorp.com/nomad/docs/job-specification/service)时，我们给每个服务取了一个名字吗？*

*`ffspostgres-service`是 [PostgreSQL 服务](https://github.com/avillela/nomad-conversions/blob/ed12ec3d4092a7816aadd2d761a98f9ef51dfb74/otel-demo-app/jobspec/ffspostgres.nomad#L59-L74)的名称。你可以在这里查看游牧服务的定义。(**暂且不提:**注意[服务](https://developer.hashicorp.com/nomad/docs/job-specification/service)的[基于命令的健康检查，以检查数据库连接](https://github.com/avillela/nomad-conversions/blob/ed12ec3d4092a7816aadd2d761a98f9ef51dfb74/otel-demo-app/jobspec/ffspostgres.nomad#L63-L74)。)*

*类似地，`otelcol-grpc`是 OpenTelemetry Collector 的 [gRPC 服务的名称。你可以在这里查看服务定义](https://github.com/avillela/nomad-conversions/blob/ed12ec3d4092a7816aadd2d761a98f9ef51dfb74/otel-demo-app/jobspec/otel-collector.nomad#L190-L199)[。](https://github.com/avillela/nomad-conversions/blob/ed12ec3d4092a7816aadd2d761a98f9ef51dfb74/otel-demo-app/jobspec/otel-collector.nomad#L190-L199)*

*有关咨询服务发现的更多信息，请查看这个 HashiCorp 论坛。此外，Nomad 现在有本地服务发现无领事。欲了解更多信息，请点击查看文档[。](https://developer.hashicorp.com/nomad/docs/networking/service-discovery)*

*关于配置文件使用[模板](https://developer.hashicorp.com/nomad/docs/job-specification/template)节的例子，请点击查看 OpenTelemetry Collector 的工作规范[。](https://github.com/avillela/nomad-conversions/blob/ed12ec3d4092a7816aadd2d761a98f9ef51dfb74/otel-demo-app/jobspec/otel-collector.nomad#L89-L143)*

***重启规则***

*与 [Docker Compose](https://docs.docker.com/compose/compose-file/compose-file-v3/#depends_on) 不同，您不能在 Nomad 中指定服务依赖(Kubernetes 也是如此)。所以，为了确保`Service X`不会因为依赖于还没有开始的`Service Y`而死在你面前，你可以实施一个[重启](https://developer.hashicorp.com/nomad/docs/job-specification/restart)策略。下面是我为 [featureflagservice](https://github.com/open-telemetry/opentelemetry-demo/tree/main/src/featureflagservice) 配置的[重启](https://developer.hashicorp.com/nomad/docs/job-specification/restart)策略:*

```
*restart {
 attempts = 10
 delay = "15s"
 interval = "2m"
 mode = "delay"
}*
```

*上面的[重启](https://developer.hashicorp.com/nomad/docs/job-specification/restart)策略声明 Nomad 将在 2 分钟内尝试重启作业 10 次。它将在两次重启之间等待 15 秒。如果在 10 次重启尝试后。默认情况下，如果作业仍未成功启动，Nomad 将使部署失败，作业将被终止。这由[模式](https://developer.hashicorp.com/nomad/docs/job-specification/restart#mode-values)属性决定，默认为[失败](https://developer.hashicorp.com/nomad/docs/job-specification/restart#fail)。这不是我们想要的，因此我们必须将[模式](https://developer.hashicorp.com/nomad/docs/job-specification/restart#mode-values)设置为[延迟](https://developer.hashicorp.com/nomad/docs/job-specification/restart#delay-1)。这告诉 Nomad 再重新启动作业 10 次。这个循环一直持续到作业最终成功启动。*

***资源分配***

*如果你关注我写的关于 Nomad 的文章，你会知道我是一个超级粉丝，喜欢用 HashiQube 在我的本地机器上运行 Hashi 环境。当然，这意味着我的计算能力比我在数据中心的 Nomad 中运行时要少得多。这意味着我必须非常注意我使用的资源，包括 CPU 和内存。*

*为了获得 CPU 和内存使用的正确值，我必须做一些尝试。首先，我在没有任何资源分配的情况下开始部署 jobspecs，并在 Nomad 中检查这些作业，看看我是否分配了过多或过少的资源。*

*对于内存利用率，我查看了服务的分配仪表板下消耗的资源:*

*![](img/6aa3930169e77fa1b0fbb5a0a39f7158.png)*

*如果你看一下上面的 [featureflagservice](https://github.com/open-telemetry/opentelemetry-demo/tree/main/src/featureflagservice) 的屏幕截图，你可以看到我使用了分配给这个 jobspec 的大约 60%的内存，这相当不错。如果我部署了一个服务，并看到它接近 100%的内存使用率(80%或以上)，我会增加使用的内存量。*

*[如果您喜欢命令行](https://developer.hashicorp.com/nomad/tutorials/manage-jobs/jobs-utilization)，您可以运行:*

```
*export ALLOCATION_ID=$(nomad job allocs -json featureflagservice | jq -r '.[0].ID')
nomad alloc status -stats $ALLOCATION_ID*
```

*样本输出:*

```
*…
Task "featureflagservice" is "running"
Task Resources:
CPU Memory Disk Addresses
0/55 MHz 151 MiB/250 MiB 300 MiB 
Memory Stats
Cache Swap Usage
0 B 0 B 151 MiB
CPU Stats
Percent Throttled Periods Throttled Time
2.89% 0 0
…*
```

*从上面的打印输出中可以看到，CPU 利用率在`55 MHz`的`0 MHz`处，内存利用率在`250 MiB`的`151 MiB`处。*

*对于 CPU 利用率，我查看 Nomad 的[拓扑仪表板](https://www.hashicorp.com/blog/see-your-entire-cluster-at-once-with-nomad-s-topology-visualization)。*

*![](img/07ea14d8d42bcace7511124dbbb482eb.png)*

*根据上面的截屏，你可以看到，对于我的所有服务，我在分配给我的`2 GHz`中，为我的所有 jobspecs(所有 OTel 演示应用 jobspecs)使用了总计`1.21 GHz`的 CPU(如果你好奇，我在 HashiQube 中配置了这个设置[)。通过从分配的资源利用率仪表板查看我的服务的 CPU 利用率，并通过从拓扑仪表板查看我有多少计算能力，我可以调整 CPU 利用率，以达到不会耗尽我分配的资源的值。根据一般经验，我希望确保我的所有服务都使用了 60–75%的分配资源。](https://github.com/avillela/hashiqube/blob/9dd8e08febcb295b44d2a834e9f5e024841a9d00/hashicorp/nomad.sh#L59-L60)*

*因此，记住所有这些，下面是我对 [featureflagservice](https://github.com/open-telemetry/opentelemetry-demo/tree/main/src/featureflagservice) 的[资源](https://developer.hashicorp.com/nomad/docs/job-specification/resources)设置，其中 CPU 以 GHz 为单位，内存以 MiB ( [mebibytes](https://www.majordifferences.com/2018/03/differences-between-megabyte-and.html) )为单位。*

```
*resources {
 cpu = 55
 memory = 250
}*
```

## *6-将所有这些放在一起…*

*现在我们已经准备好了所有的东西，我们最终的 jobspec 如下所示:*

```
*job "featureflagservice" {
  type        = "service"
  datacenters = ["dc1"]

  group "featureflagservice" {
    count = 1

    network {
      mode = "host"

      port "http" {
        to = 8081
      }
      port "grpc" {
        to = 50053
      }
    }

    service {
      name = "featureflagservice-http"
      port = "http"
      tags = [
        "traefik.http.routers.featureflagservice.rule=Host(`feature.localhost`)",
        "traefik.http.routers.featureflagservice.entrypoints=web",
        "traefik.http.routers.featureflagservice.tls=false",
        "traefik.enable=true",
      ]

      check {
        type     = "tcp"
        interval = "10s"
        timeout  = "5s"
      }
    }

    service {
      name = "featureflagservice-grpc"
      port = "grpc"

      check {
        type     = "tcp"
        interval = "10s"
        timeout  = "5s"
      }
    }

    task "featureflagservice" {
      driver = "docker"

      config {
        image = "otel/demo:v1.1.0-featureflagservice"
        image_pull_timeout = "10m"
        ports = ["http", "grpc"]
      }

      restart {
        attempts = 10
        delay    = "15s"
        interval = "2m"
        mode     = "delay"
      }

      env {
        FEATURE_FLAG_GRPC_SERVICE_PORT = "${NOMAD_PORT_grpc}"
        FEATURE_FLAG_SERVICE_PATH_ROOT = "\"/feature\""
        FEATURE_FLAG_SERVICE_PORT = "${NOMAD_PORT_http}"
        OTEL_EXPORTER_OTLP_TRACES_PROTOCOL = "grpc"
        OTEL_SERVICE_NAME = "featureflagservice"
      }

      template {
        data = <<EOF
{{ range service "ffspostgres-service" }}
DATABASE_URL = "ecto://ffs:ffs@{{ .Address }}:{{ .Port }}/ffs"
{{ end }}

{{ range service "otelcol-grpc" }}
OTEL_EXPORTER_OTLP_TRACES_ENDPOINT = "http://{{ .Address }}:{{ .Port }}"
{{ end }}
EOF
        destination = "local/env"
        env         = true
      }

      resources {
        cpu    = 55
        memory = 250
      }

    }
  }
}*
```

*哒哒！！🎉*

# *最后的想法*

*咻！我们今天谈了很多！在一天结束的时候，我希望这能让你明白，把一份 Kubernetes 清单转换成一份 Nomad jobspec 并不是什么难事！只需要一点点知识和耐心。*

*虽然这绝不是一个彻底的转换，但我希望这个小教程能给你信心，让你从“我希望有一个如何在 Nomad 上运行它的例子”变成“我自己也能让它在 Nomad 上运行！”*

*我现在要奖励你一张照片，照片上的菲比和我们亲爱的小兔子正从笼子里往外看。*

*![](img/78e013587db96bf568ea2a59d1d9568b.png)*

*兔子(左)和菲比(右)想打声招呼！Adri Villela 拍摄的照片。*

*和平、爱和准则。🦄 🌈 💫*

*![](img/78aa5a77e3bdf841aa9087ac89ee80fb.png)*

*更多关于 Nomad 的博客文章，请查看我下面的阅读列表:*

*![Adri Villela](img/e70eedd2981b8eb48ebfa3602f42bc0f.png)

[阿德里·维莱拉](https://adri-v.medium.com/?source=post_page-----7a58d2fa07a0--------------------------------)* 

## *适时游牧*

*[View list](https://adri-v.medium.com/list/justintime-nomad-cc5d249a172b?source=post_page-----7a58d2fa07a0--------------------------------)**11 stories**![Picture of a mural featuring an astronaut surrounded by a circle.](img/de08af9cd14a133eb6337987debfb7a4.png)**![](img/90c44c34a8748c582208823f9622fadb.png)**![The nomad logo on a dark Tucows blue background](img/419df386114d12b8672112c7271b5df4.png)*

*对可观察性和/或 [OpenTelemetry](https://opentelemetry.io) 有疑问吗？想要合作开发适用于 Nomad 的 OTel 演示应用程序吗？跟我说话！请随时通过[电子邮件](mailto:devrel@lightstep.com)联系我，或者通过[乳齿象](https://hachyderm.io/@adrianamvillela)或 [LinkedIn](https://www.linkedin.com/in/adrianavillela) 联系我。希望收到你们的来信！*