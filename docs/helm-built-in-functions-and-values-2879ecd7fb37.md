# Helm —内置函数和值

> 原文：<https://blog.devgenius.io/helm-built-in-functions-and-values-2879ecd7fb37?source=collection_archive---------8----------------------->

## 高级头盔用法

![](img/cd216ad8060118905a3e1311e12c3633.png)

在我之前的文章中，我向你展示了如何创建你自己的`HELM Chart`:[舵——创建舵图](https://medium.com/geekculture/helm-create-helm-chart-7084666fab90)。今天来说说`Helm`内置函数和值。

# 定义舵图

让我们快速回顾一下如何创建一个`Helm Chart`。图表包是文件夹的集合，文件夹名称是图表包的名称。例如，要创建一个`mychart`图表包:

```
$ helm create mychart
Creating mychart
$ tree mychart/
mychart/
├── charts
├── Chart.yaml
├── templates
│   ├── deployment.yaml
│   ├── _helpers.tpl
│   ├── ingress.yaml
│   ├── NOTES.txt
│   └── service.yaml
└── values.yaml2 directories, 7 files
```

让我们仔细看看`templates`目录:

*   **NOTES.txt:** “帮助文本”为图表。这在用户运行 helm install 时显示给用户。
*   **deployment.yaml** :创建 Kubernetes 部署的基本清单
*   **service.yaml** :创建部署服务的基本清单
*   **ingress.yaml** :创建 ingress 对象的资源清单文件
*   **_helpers.tpl** :放置模板助手的地方，可以在整个图表中重用

在这里我们了解了每个文件的用途，然后我们可以删除模板目录下的所有文件，然后自己创建模板文件:

```
$ rm -rf mychart/templates/*.*
```

# 创建模板

这里我们在 templates 目录下创建一个非常简单的模板 ConfigMap: `configmap.yaml`文件:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: mychart-configmap
data:
  myvalue: "Hello World"
```

事实上，现在我们有了一个可安装的图表包，可以用`helm install`命令安装:

```
$ helm install mychart ./mychart
NAME: mychart
LAST DEPLOYED: Thu Jun 23 11:38:18 2022
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

在上面的输出中，我们可以看到已经创建了 ConfigMap 资源对象。然后，我们可以使用以下命令查看实际的模板呈现的资源文件:

```
$ helm get manifest mychart
---
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mychart-configmap
data:
  myvalue: "Hello World"
```

现在我们看看上面的 ConfigMap 文件是否正是我们在模板文件中设计的，让我们删除当前的文件`release`:

```
$ helm delete mychart
release "mychart" deleted
```

# 添加一个简单的模板

我们可以看到上面定义的配置图的名称是固定的，但是这通常不是一个好的做法。我们可以通过插入发布版本的名称来生成资源的名称。例如，这里的配置图名称为:`mychart-configmap`，需要使用 Chart 的模板定义方法。

现在让我们重新定义上面的`configmap.yaml`文件:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
```

我们用模板指令`{{ .Release.Name }}-configmap`替换了名称，并将发布的名称注入到模板中，这样最终生成的 ConfigMap 名称以发布的名称开始。这里的发布模板对象是`Helm`中的内置对象之一，还有很多其他的内置对象，我们后面会讲到。

现在让我们重新安装我们的图表包，注意 ConfigMap 资源对象的名称:

```
$ helm install mychart2 ./mychart
NAME: mychart2
LAST DEPLOYED: Thu Jun 23 11:40:19 2022
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

让我们再查一遍清单。

```
helm get manifest mychart2
---
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mychart2-configmap
data:
  myvalue: "Hello World"
```

现在你可以看到`metadata.name`变成了`mychart2-configmap`。

# 排除故障

我们使用模板来生成资源文件列表，但是如果要调试的话就很不方便了。我们不可能每次都部署一个实例来验证模板是否正确。

幸运的是，Helm 为我们提供了`--dry-run --debug`选项，当您执行`helm install`这两个参数时，您可以打印相应的值和最终生成的资源列表文件，而无需实际部署一个版本。例如，让我们调试上面创建的图表包:

```
$ helm install mychart2 --dry-run --debug ./mychart
install.go:173: [debug] Original chart version: ""
install.go:190: [debug] CHART PATH: /tmp/mychartNAME: mychart2
LAST DEPLOYED: Thu Jun 23 11:44:56 2022
NAMESPACE: default
STATUS: pending-install
REVISION: 1
TEST SUITE: None
USER-SUPPLIED VALUES:
{}COMPUTED VALUES:
affinity: {}
autoscaling:
  enabled: false
  maxReplicas: 100
  minReplicas: 1
  targetCPUUtilizationPercentage: 80
fullnameOverride: ""
image:
  pullPolicy: IfNotPresent
  repository: nginx
  tag: ""
imagePullSecrets: []
ingress:
  annotations: {}
  enabled: false
  hosts:
  - host: chart-example.local
    paths: []
  tls: []
nameOverride: ""
nodeSelector: {}
podAnnotations: {}
podSecurityContext: {}
replicaCount: 1
resources: {}
securityContext: {}
service:
  port: 80
  type: ClusterIP
serviceAccount:
  annotations: {}
  create: true
  name: ""
tolerations: []HOOKS:
MANIFEST:
---
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mychart2-configmap
data:
  myvalue: "Hello World"
```

现在我们可以通过使用它来轻松测试代码，而不必每次都安装一个 release 实例，但需要注意的是，这并不能保证 K8s 本身一定会接受生成的模板。调试之后，您仍然需要转到 Install an actual release 实例进行验证。

# 内置对象

刚才我们使用`{{.Release.Name}}`将发布的名称插入到模板中。这里的发布是`Helm`的内置对象。以下是一些常用的内置对象，需要时可以直接使用:

## 释放；排放；发布

*   **发布**:这个对象描述了发布本身。它包含几个对象:
*   发布名称:发布名称
*   发布时间:发布时间
*   发布名称空间:版本的名称空间(如果清单中没有包含)
*   发布服务:发布服务的名称(总是 Tiller)。
*   发布修订版:此版本的修订号，从 1 开始累加。
*   发布 IsUpgrade:如果当前操作是升级或回滚，则设置为 true。
*   发布 IsInstall:如果当前操作是安装，则设置为 true。

## 价值观念

*   **值**:从文件和用户提供的文件传入模板的值`values.yaml`。默认情况下，Values 为空。

## 图表

*   **图表** : `Chart.yaml`文件的内容。所有图表对象都将从此文件中获取。图表指南中的[图表指南](https://github.com/kubernetes/helm/blob/master/docs/charts.md#the-chartyaml-file)中列出了可用字段，可在此处查看。

## 文件

*   **文件**:提供对图表中所有非特殊文件的访问。虽然它不能用来访问模板，但可以用来访问图表中的其他文件。请参见“访问文件”一节。
*   文件。Get 是一个按名称获取文件的函数(. Files.Get config.ini)
*   文件。GetBytes 是以字节数组而不是字符串的形式获取文件内容的函数。这对于图片之类的东西很有用。

## 能力

*   **功能**:这提供了关于 K8s 集群所支持的功能的信息。
*   能力。APIVersions 是一组版本信息。
*   能力。APIVersions.Has $version 指示是否在群集上启用版本(batch/v1)。
*   能力。KubeVersion 提供了查找 Kubernetes 版本的方法。它有以下值:Major、Minor、GitVersion、GitCommit、GitTreeState、BuildDate、GoVersion、编译器和平台。
*   能力。TillerVersion 提供了查找 Tiller 版本的方法。它具有以下值:SemVer、GitCommit 和 GitTreeState。

## 模板

*   Template:包含当前正在执行的模板的信息

## 名字

*   名称:当前模板的文件路径(如 mychart/templates/my template . YAML)

## 基本路径

*   BasePath:当前图表模板目录的路径(如 mychart/templates)。

上述值可以在任何顶级模板中使用，注意内置值总是以大写字母开头。这也符合命名惯例。当你创建自己的名字时，你可以自由地使用适合你的团队的惯例。

# 值文件

上面的内置对象之一是`Values`，它提供对传入图表的值的访问。Values 对象的值有 4 个来源:

*   **图表包中的 values.yaml** 文件
*   **父图表包的 values.yaml** 文件
*   通过`helm install`或`helm upgrade -f`或参数传入的自定义 yaml 文件(我们在上节课已经学过)`--values`
*   参数中的值通过了`--set`

例如，这里我们重新编辑`mychart/values.yaml`文件，清除所有默认值，并添加一个新数据:(values.yaml)

```
course: k8s
```

然后我们可以在上面的`templates/configmap.yaml`模板文件中使用这个值:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  course: {{ .Values.course }}
```

然后做一次预演:

```
$ helm install mychart --dry-run --debug ./mychart
install.go:173: [debug] Original chart version: ""
install.go:190: [debug] CHART PATH: /tmp/mychartNAME: mychart
LAST DEPLOYED: Thu Jun 23 11:52:05 2022
NAMESPACE: default
STATUS: pending-install
REVISION: 1
TEST SUITE: None
USER-SUPPLIED VALUES:
{}COMPUTED VALUES:
course: k8sHOOKS:
MANIFEST:
---
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mychart2-configmap
data:
  myvalue: "Hello World"
  course: k8s
```

我们可以看到`ConfigMap`中的值当然是渲染为 k8s，因为在 default values.yaml 文件中，这个参数的值是 k8s。类似地，我们可以很容易地通过参数`--set`覆盖该值:

```
helm install mychart2 --dry-run --debug ./mychart --set course=python3
install.go:173: [debug] Original chart version: ""
install.go:190: [debug] CHART PATH: /tmp/mychartNAME: mychart2
LAST DEPLOYED: Thu Jun 23 11:53:33 2022
NAMESPACE: default
STATUS: pending-install
REVISION: 1
TEST SUITE: None
USER-SUPPLIED VALUES:
course: python3COMPUTED VALUES:
course: python3HOOKS:
MANIFEST:
---
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mychart2-configmap
data:
  myvalue: "Hello World"
  course: python3
```

因为`--set`比默认的`values.yaml`文件具有更高的优先级，所以我们的模板被生成为 course: python3。

values 文件还可以包含更多结构化内容，例如，在 values.yaml 文件中，我们可以创建一个课程部分，并向其中添加一些关键字:

```
course:
  k8s: devops
  python: django
```

现在我们稍微修改一下模板:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  k8s: {{ .Values.course.k8s }}
  python: {{ .Values.course.python }}
```