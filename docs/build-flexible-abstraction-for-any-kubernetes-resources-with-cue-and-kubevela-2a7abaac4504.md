# 使用 CUE 和 KubeVela 为任何 Kubernetes 资源构建灵活的抽象

> 原文：<https://blog.devgenius.io/build-flexible-abstraction-for-any-kubernetes-resources-with-cue-and-kubevela-2a7abaac4504?source=collection_archive---------9----------------------->

*由孙建波·库贝拉团队*

这篇博客将介绍如何使用 CUE 和 KubeVela 来构建你自己的抽象 API，以降低 Kubernetes 资源的复杂性。作为平台构建者，您可以动态定制抽象，为您的开发人员根据需求构建由浅入深的路径，适应越来越多的不同场景，满足公司长期业务发展的迭代需求。

# 将 Kubernetes API 对象转换成定制组件

让我们以使用 [Kubernetes StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/) 为例开始这个旅程，我们将把它转换成一个定制的模块并提供功能。

将 StatefulSet 的 YAML 示例保存在本地的正式文档中，并命名为`my-stateful.yaml`，然后执行如下命令:

```
vela def init my-stateful -t component --desc "My StatefulSet component." --template-yaml ./my-stateful.yaml -o my-stateful.cue
```

查看生成的“my-stateful.cue”文件:

```
$ cat my-stateful.cue
"my-stateful": {
    annotations: {}
    attributes: workload: definition: {
        apiVersion: "<change me> apps/v1"
        kind:       "<change me> Deployment"
    }
    description: "My StatefulSet component."
    labels: {}
    type: "component"
}

template: {
    output: {
        apiVersion: "v1"
        kind:       "Service"
            ... // omit non-critical info
    }
    outputs: web: {
        apiVersion: "apps/v1"
        kind:       "StatefulSet"
            ... // omit non-critical info
    }
    parameter: {}
}
```

按如下方式修改生成的文件:

1.  官方 StatefulSet 网站的例子是由两个对象`StatefulSet`和`Service`组成的复合组件。根据 KubeVela [定制组件](https://kubevela.io/docs/platform-engineers/components/custom-component)的规则，在复合组件中，需要用`template.output`字段来表示资源之一(我们例子中的 StatefulSet)作为核心工作负载，其他辅助对象用`template.outputs`来表示，所以我们做了一些调整，所有自动生成的输出和输出都进行了切换。
2.  然后我们将核心工作负载的 apiVersion 和 kind 数据填入标记为`<change me>`的部分

修改后，您可以使用`vela def vet`进行格式检查和验证。

```
$ vela def vet my-stateful.cue
Validation succeed.
```

经过两步修改后的文件如下:

```
$ cat my-stateful.cue
"my-stateful": {
    annotations: {}
    attributes: workload: definition: {
        apiVersion: "apps/v1"
        kind:       "StatefulSet"
    }
    description: "My StatefulSet component."
    labels: {}
    type: "component"
}

template: {
    output: {
        apiVersion: "apps/v1"
        kind:       "StatefulSet"
        metadata: name: "web"
        spec: {
            selector: matchLabels: app: "nginx"
            replicas:    3
            serviceName: "nginx"
            template: {
                metadata: labels: app: "nginx"
                spec: {
                    containers: [{
                        name: "nginx"
                        ports: [{
                            name:          "web"
                            containerPort: 80
                        }]
                        image: "k8s.gcr.io/nginx-slim:0.8"
                        volumeMounts: [{
                            name:      "www"
                            mountPath: "/usr/share/nginx/html"
                        }]
                    }]
                    terminationGracePeriodSeconds: 10
                }
            }
            volumeClaimTemplates: [{
                metadata: name: "www"
                spec: {
                    accessModes: ["ReadWriteOnce"]
                    resources: requests: storage: "1Gi"
                    storageClassName: "my-storage-class"
                }
            }]
        }
    }
    outputs: web: {
        apiVersion: "v1"
        kind:       "Service"
        metadata: {
            name: "nginx"
            labels: app: "nginx"
        }
        spec: {
            clusterIP: "None"
            ports: [{
                name: "web"
                port: 80
            }]
            selector: app: "nginx"
        }
    }
    parameter: {}
}
```

将 ComponentDefinition 安装到 Kubernetes 集群中:

```
$ vela def apply my-stateful.cue
ComponentDefinition my-stateful created in namespace vela-system.
```

你可以通过`vela components`命令看到一个`my-stateful`组件:

```
$ vela components
NAME        NAMESPACE   WORKLOAD                                DESCRIPTION
...
my-stateful vela-system statefulsets.apps                       My StatefulSet component.
...
```

当您将这个定制的组件放入`Application`时，它看起来像:

```
cat <<EOF | vela up -f -
apiVersion: core.oam.dev/v1beta1
kind: Application
metadata:
  name: website
spec:
  components:
    - name: my-component
      type: my-stateful
EOF
```

# 为组件定义自定义参数

在上一节中，我们已经定义了一个没有参数的 ComponentDefinition。在这一节中，我们将展示如何公开参数。

在本例中，我们向用户公开了以下参数:

*   图像名称，允许用户自定义图像
*   实例名，允许用户自定义生成的 StatefulSet 对象和服务对象的实例名

```
... # Omit other unmodified fields
    template: {
        output: {
            apiVersion: "apps/v1"
            kind:       "StatefulSet"
            metadata: name: parameter.name
            spec: {
                selector: matchLabels: app: "nginx"
                replicas:    3
                serviceName: "nginx"
                template: {
                    metadata: labels: app: "nginx"
                    spec: {
                        containers: [{
                            image: parameter.image

                            ... // Omit other unmodified fields
                        }]
                    }
                }
                    ... // Omit other unmodified fields
            }
        }
        outputs: web: {
            apiVersion: "v1"
            kind:       "Service"
            metadata: {
                name: "nginx"
                labels: app: "nginx"
            }
            spec: {
                ... // Omit other unmodified fields 
            }
        }
        parameter: {
            image: string
            name: string
        }
    }
```

修改后，使用`vela def apply`安装到集群:

```
$ vela def apply my-stateful.cue
ComponentDefinition my-stateful in namespace vela-system updated.
```

然后，作为平台构建者，您已经完成了设置。看看现在的开发者体验如何。

# 开发者体验

你的开发者唯一需要学习的是[开放应用模型](https://oam.dev/)，它总是遵循统一的格式。

## 发现组件

开发人员可以发现并检查`my-stateful`组件的参数，如下所示:

```
$ vela def list
NAME                            TYPE                    NAMESPACE   DESCRIPTION
my-stateful                     ComponentDefinition     vela-system My StatefulSet component.
...snip...$ vela show my-stateful
# Properties
+----------+-------------+--------+----------+---------+
|   NAME   | DESCRIPTION |  TYPE  | REQUIRED | DEFAULT |
+----------+-------------+--------+----------+---------+
| name     |             | string | true     |         |
| image    |             | string | true     |         |
+----------+-------------+--------+----------+---------+
```

更新 ComponentDefinition 不会影响现有的应用程序。只有在下次更新应用程序后，它才会生效。

## 在应用程序中使用组件

开发人员可以在应用程序中轻松指定三个新参数:

```
apiVersion: core.oam.dev/v1beta1
    kind: Application
    metadata:
      name: website
    spec:
      components:
        - name: my-component
          type: my-stateful
          properties:
            image: nginx:latest
            name: my-component
```

剩下唯一要做的就是通过执行`vela up -f app-stateful.yaml`来部署 yaml 文件(假设名称为`app-stateful.yaml`)。

然后您可以看到 StatefulSet 对象的名称、图像和实例数量已经更新。

# 用于诊断或集成的试运行 [#](https://kubevela.io/blog/2022/05/30/abstraction-kubevela#dry-run-for-diagnose-or-integration)

为了确保开发人员的应用程序能正确运行参数，你也可以使用`vela dry-run`命令来验证你的模板的试运行。

```
vela dry-run -f app-stateful.yaml
```

通过查看输出，您可以比较生成的对象是否与您实际期望的对象一致。您甚至可以直接在 Kubernetes 集群中执行这个 YAML，并使用操作的结果进行验证。

```
# Application(website) -- Component(my-component)
---

apiVersion: v1
kind: Service
metadata:
  labels:
    app: nginx
    app.oam.dev/appRevision: ""
    app.oam.dev/component: my-component
    app.oam.dev/name: website
    workload.oam.dev/type: my-stateful
  name: nginx
  namespace: default
spec:
  clusterIP: None
  ports:
  - name: web
    port: 80
  selector:
    app: nginx
  template:
    spec:
      containers:
      - image: saravak/fluentd:elastic
        name: my-sidecar

---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  labels:
    app.oam.dev/appRevision: ""
    app.oam.dev/component: my-component
    app.oam.dev/name: website
    trait.oam.dev/resource: web
    trait.oam.dev/type: AuxiliaryWorkload
  name: web
  namespace: default
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  serviceName: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: k8s.gcr.io/nginx-slim:0.8
        name: nginx
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - mountPath: /usr/share/nginx/html
          name: www
      terminationGracePeriodSeconds: 10
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes:
      - ReadWriteOnce
      resources:
        requests:
          storage: 1Gi
      storageClassName: my-storage-class
```

您也可以使用`vela dry-run -h`查看更多可用的功能参数。

# 使用`context`避免重复

KubeVela 允许您通过`[context](https://kubevela.io/docs/platform-engineers/components/custom-component#cue-context)` [关键字](https://kubevela.io/docs/platform-engineers/components/custom-component#cue-context)引用应用程序的运行时信息。

在上面的例子中，属性中的字段`name`和组件的字段`name`具有相同的含义，所以我们可以使用`context`来避免重复。我们不能在运行时使用`context.name`来引用组件名，因此不再需要`parameter`中的 name 参数。

只需将定义文件(my-stateful.cue)修改如下

```
... # Omit other unmodified fields
template: {
    output: {
        apiVersion: "apps/v1"
        kind:       "StatefulSet"
-       metadata: name: parameter.name
+       metadata: name: context.name

            ... // Omit other unmodified field

    }
    parameter: {
-       name: string
        image: string
    }
}
```

然后通过以下方式部署更改:

```
vela def apply my-stateful.cue
```

之后，开发人员可以立即运行如下应用程序:

```
apiVersion: core.oam.dev/v1beta1
    kind: Application
    metadata:
      name: website
    spec:
      components:
        - name: my-component
          type: my-stateful
          properties:
            image: "nginx:latest"
```

任何系统都没有升级或重启，它们都在根据您的需要动态运行。

# 按需添加运营特征

OAM 遵循“关注点分离”的原则，在开发人员完成组件部分后，操作员可以将特性添加到应用程序中，以控制部署的其余部分配置。例如，操作员可以
控制副本，添加标签/注释，注入环境变量/侧车，添加持久卷，等等。

从技术上讲，特质系统以两种方式工作:

*   修补/覆盖组件中定义的配置。
*   生成更多配置。

定制过程与组件的工作方式相同，它们都使用 CUE，但是对于路径和覆盖有一些不同的关键字，具体可以参考[定制特性](https://kubevela.io/docs/platform-engineers/traits/customize-trait)。

# 操作员对特性的经验

KubeVela 安装后，已经有了一些内置特性。操作员可以使用`vela traits`查看，用`*`标记的性状是一般性状，可以对常见的库本内特资源对象进行操作。

```
$ vela traits
NAME                        NAMESPACE   APPLIES-TO          CONFLICTS-WITH  POD-DISRUPTIVE  DESCRIPTION
annotations                 vela-system *                                   true            Add annotations on K8s pod for your workload which follows
                                                                                            the pod spec in path 'spec.template'.
configmap                   vela-system *                                   true            Create/Attach configmaps on K8s pod for your workload which
                                                                                            follows the pod spec in path 'spec.template'.
labels                      vela-system *                                   true            Add labels on K8s pod for your workload which follows the
                                                                                            pod spec in path 'spec.template'.
scaler                      vela-system *                                   false           Manually scale K8s pod for your workload which follows the
                                                                                            pod spec in path 'spec.template'.
sidecar                     vela-system *                                   true            Inject a sidecar container to K8s pod for your workload
                                                                                            which follows the pod spec in path 'spec.template'.
...snip...
```

以 sidecar 为例，您可以查看 sidecar 的使用情况:

```
$ vela show sidecar
# Properties
+---------+-----------------------------------------+-----------------------+----------+---------+
|  NAME   |               DESCRIPTION               |         TYPE          | REQUIRED | DEFAULT |
+---------+-----------------------------------------+-----------------------+----------+---------+
| name    | Specify the name of sidecar container   | string                | true     |         |
| cmd     | Specify the commands run in the sidecar | []string              | false    |         |
| image   | Specify the image of sidecar container  | string                | true     |         |
| volumes | Specify the shared volume path          | [[]volumes](#volumes) | false    |         |
+---------+-----------------------------------------+-----------------------+----------+---------+

## volumes
+------+-------------+--------+----------+---------+
| NAME | DESCRIPTION |  TYPE  | REQUIRED | DEFAULT |
+------+-------------+--------+----------+---------+
| path |             | string | true     |         |
| name |             | string | true     |         |
+------+-------------+--------+----------+---------+
```

使用侧车直接向容器内注射，应用说明如下:

```
apiVersion: core.oam.dev/v1beta1
kind: Application
metadata:
  name: website
spec:
  components:
    - name: my-component
      type: my-stateful
      properties:
        image: nginx:latest
        name: my-component
      traits:
      - type: sidecar
        properties:
          name: my-sidecar
          image: saravak/fluentd:elastic
```

部署并运行应用程序，您可以看到一辆满载的侧车已经在 StatefulSet 中部署并运行。

组件和特性都可以在任何 KubeVela 系统上重复使用，我们可以将组件、特性和 CRD 控制器打包成一个插件。社区中有越来越多的插件目录。

# 总结

本博客介绍了如何通过 CUE 提供完整的模块化功能。其核心是可以根据用户需求动态增加配置能力，逐步暴露更多的功能和用途，从而降低用户的整体学习门槛，最终提高 R&D 效率。KubeVela 提供的现成功能，包括组件、特性、策略和工作流，也被设计成可插入和可修改的功能。