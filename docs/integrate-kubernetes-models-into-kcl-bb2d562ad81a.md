# 将 Kubernetes 模型集成到 KCL 中

> 原文：<https://blog.devgenius.io/integrate-kubernetes-models-into-kcl-bb2d562ad81a?source=collection_archive---------21----------------------->

# **什么是 KCL**

[KCL (Kusion 配置语言)](https://github.com/KusionStack/KCLVM)是一种开源的基于约束的记录和函数语言。KCL 通过成熟的编程语言技术和实践，改进大量复杂配置的编写，致力于围绕配置构建更好的模块化、可扩展性和稳定性，逻辑编写更简单，自动化速度快，生态扩展性好。

更多关于 KCL 的文件在 https://medium.com/p/f03ee820c7a4 的最后一个博客[和 KCL 网站](https://medium.com/p/f03ee820c7a4)[https://github.com/KusionStack/KCLVM](https://github.com/KusionStack/KCLVM)

# 来自 Kubernetes

# 1.Kubernetes OpenAPI 规范[](https://kcl-lang.github.io/docs/user_docs/guides/working-with-k8s/from_kubernetes#1-kubernetes-openapi-spec)

从 Kubernetes 1.4 开始，引入了对 OpenAPI 规范的 alpha 支持(在捐赠给 OpenAPI 计划之前称为 Swagger 2.0)，API 描述遵循 [OpenAPI 规范 2.0](https://github.com/OAI/OpenAPI-Specification/blob/main/versions/2.0.md) 。而且从 Kubernetes 1.5 开始，Kubernetes 支持[直接从源代码中提取模型，然后生成 OpenAPI 规范文件](https://github.com/kubernetes/kube-openapi)，自动保持规范和文档与操作和模型保持最新。

此外，Kubernetes CRD 使用[open API v 3.0 validation]([https://Kubernetes . io/docs/tasks/extend-Kubernetes/custom-resources/custom-resource-definitions/# validation](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/#validation))来描述自定义模式(除了内置属性 apiVersion、Kind 和 metadata 之外)，APIServer 使用该模式在资源创建和更新阶段验证 CR。

# 2.KCL OpenAPI 支持[](https://kcl-lang.github.io/docs/user_docs/guides/working-with-k8s/from_kubernetes#2-kcl-openapi-support)

`kcl-openapi`工具支持从 Kubernetes OpenAPI/CRD 中提取和生成 KCL 模式。 [KCL OpenAPI 规范](https://kcl-lang.github.io/docs/tools/cli/openapi/spec)定义了 OpenAPI 规范和 KCL 语言特性之间的映射。有关该工具的快速入门，请参见 [KCL OpenAPI 工具](https://kcl-lang.github.io/docs/tools/cli/openapi/)

# 3.从 Kubernetes 迁移到 KCL[](https://kcl-lang.github.io/docs/user_docs/guides/working-with-k8s/from_kubernetes#3-migrate-from-kubernetes-to-kcl)

Kubernetes 内置模型的完整 OpenAPI 定义存储在 [Kubernetes OpenAPI-Spec 文件](https://github.com/kubernetes/kubernetes/blob/master/api/openapi-spec/swagger.json)中。以这个文件作为输入，KCL OpenAPI 工具可以生成相应版本的所有模型模式。在接下来的小节中，我们将以一个部署发布场景为例，介绍如何从 Kubernetes 迁移到 KCL。假设您的项目使用[Kubernetes Deployment]([https://Kubernetes . io/docs/concepts/workloads/controllers/Deployment/](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/))来定义部署配置，迁移到 KCL 只需要以下步骤:

# 3.1 根据型号 [](https://kcl-lang.github.io/docs/user_docs/guides/working-with-k8s/from_kubernetes#31-write-config-based-on-the-models) 编写配置

我们提供开箱即用的`kusion_models`包供您快速启动。它包含一个精心设计的前端模型，名为`[Server schema](https://github.com/KusionStack/konfig/blob/main/base/pkg/kusion_models/kube/frontend/server.k)`。您可以通过初始化`Server schema`来声明它们的配置。关于模式及其属性的描述和用法，请参考[服务器模式文档](https://kusionstack.io/docs/reference/model/kusion_models/kube/frontend/doc_server)。

# 3.2 构建您的自定义前端模型[](https://kcl-lang.github.io/docs/user_docs/guides/working-with-k8s/from_kubernetes#32-build-your-custom-frontend-models)

现有的 KCL 模型可能无法满足您的特定业务需求，那么您也可以设计自己的定制前端模型包。在`kusion_kubernetes`目录中([https://github . com/KusionStack/konfig/tree/main/base/pkg/ku sion _ Kubernetes](https://github.com/KusionStack/konfig/tree/main/base/pkg/kusion_kubernetes))，有一份生成的 Kubernetes 1.22 模型的副本，你可以基于它设计你的定制模型。您还可以开发您的定制脚本来迁移您的配置数据，就像`kube2kcl`工具所做的那样。

## 3.2.1 将 Kubernetes 部署转换成 KCL 模式[](https://kcl-lang.github.io/docs/user_docs/guides/working-with-k8s/from_kubernetes#321-convert-kubernetes-deployment-into-kcl-schema)

在 Konfig 存储库中的`base/pkg/kusion_kubernetes`目录下，我们已经有了一个[生成的 Kubernetes 1.22 模型](https://github.com/kcl-lang/konfig/blob/main/base/pkg/kusion_kubernetes/api/apps/v1/deployment.k)的副本。您可以跳过这一步，使用现有的模型，或者如果需要，您可以生成其他版本的模型。

现在让我们生成一个 1.23 版本的 Kubernetes 模型。从[Kubernetes v 1.23 open API Spec](https://github.com/kubernetes/kubernetes/blob/release-1.23/api/openapi-spec/swagger.json)中，我们可以找到`apps/v1.Deployment`模型的定义，以下是部分摘录:

```
{
    "definitions": {
        "io.k8s.api.apps.v1.Deployment": {
            "description": "Deployment enables declarative updates for Pods and ReplicaSets.",
            "properties": {
                "apiVersion": {
                    "description": "APIVersion defines the versioned schema of this representation of an object. Servers should convert recognized schemas to the latest internal value, and may reject unrecognized values. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources",
                    "type": "string"
                },
                "kind": {
                    "description": "Kind is a string value representing the REST resource this object represents. Servers may infer this from the endpoint the client submits requests to. Cannot be updated. In CamelCase. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds",
                    "type": "string"
                },
                "metadata": {
                    "$ref": "#/definitions/io.k8s.apimachinery.pkg.apis.meta.v1.ObjectMeta",
                    "description": "Standard object's metadata. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata"
                },
                "spec": {
                    "$ref": "#/definitions/io.k8s.api.apps.v1.DeploymentSpec",
                    "description": "Specification of the desired behavior of the Deployment."
                },
                "status": {
                    "$ref": "#/definitions/io.k8s.api.apps.v1.DeploymentStatus",
                    "description": "Most recently observed status of the Deployment."
                }
            },
            "type": "object",
            "x-kubernetes-group-version-kind": [
                {
                    "group": "apps",
                    "kind": "Deployment",
                    "version": "v1"
                }
            ]
        }
    },
    "info": {
        "title": "Kubernetes",
        "version": "unversioned"
    },
    "paths": {},
    "swagger": "2.0"
}
```

您可以将上面的规范保存为`deployment.json`并运行`kcl-openapi generate model -f deployment.json`，KCL 模式将会生成并输出到您当前的工作区。其他 Kubernetes 模型也可以保存在这个 spec 文件中，并且可以类似地生成。

## 3.2.2 设计定制前端型号[](https://kcl-lang.github.io/docs/user_docs/guides/working-with-k8s/from_kubernetes#322-design-custom-frontend-models)

由于 Kubernetes 内置模型是原子性的，对初学者来说有点复杂，我们建议将 Kubernetes 的本机模型作为后端输出模型，并设计一批前端模型，这些模型可以成为更抽象、更友好和更简单的用户界面。可以参考`[Server Schema in the Konfig repo](https://github.com/kcl-lang/konfig/blob/main/base/pkg/kusion_models/kube/frontend/server.k)`中的设计模式。

## 3.2.3 迁移配置数据[](https://kcl-lang.github.io/docs/user_docs/guides/working-with-k8s/from_kubernetes#323-migrate-the-configuration-data)

您可以开发自定义脚本来自动迁移配置数据。KCL 稍后将为这个脚本提供编写框架和编写指南。

# 4.从库伯内斯迁移到 CRD[](https://kcl-lang.github.io/docs/user_docs/guides/working-with-k8s/from_kubernetes#4-migrate-from-kubernetes-crd)

如果您开发了 CRDs，那么您可以生成 CRD 模式的 KCL 版本，并在此基础上声明 CRs。

*   从 CRD 生成 KCL 模式

```
kcl-openapi generate model --crd --skip-validation -f <your_crd.yaml>
```

# 5.CRD 至 KCL 示例

*   命令

```
kcl-openapi generate model --crd -f ${your_CRD.yaml} -t ${the_kcl_files_output_di} --skip-validation
```

*   test_crontab_CRD.yaml:

```
# Deprecated in v1.16 in favor of apiextensions.k8s.io/v1
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  # name must match the spec fields below, and be in the form: <plural>.<group>
  name: crontabs.stable.example.com
spec:
  # group name to use for REST API: /apis/<group>/<version>
  group: stable.example.com
  # list of versions supported by this CustomResourceDefinition
  versions:
    - name: v1
      # Each version can be enabled/disabled by Served flag.
      served: true
      # One and only one version must be marked as the storage version.
      storage: true
  # either Namespaced or Cluster
  scope: Namespaced
  names:
    # plural name to be used in the URL: /apis/<group>/<version>/<plural>
    plural: crontabs
    # singular name to be used as an alias on the CLI and for display
    singular: crontab
    # kind is normally the CamelCased singular type. Your resource manifests use this.
    kind: CronTab
    # shortNames allow shorter string to match your resource on the CLI
    shortNames:
      - ct
  preserveUnknownFields: false
  validation:
    openAPIV3Schema:
      type: object
      properties:
        spec:
          type: object
          properties:
            cronSpec:
              type: string
            image:
              type: string
            replicas:
              type: integer
```

*   命令

```
kcl-openapi generate model -f test_crontab_CRD.yaml -t ~/ --skip-validation --crd
```

*   output ` ~/models/stable _ example _ com _ v1 _ cron _ tab . k '

```
"""
This file was generated by the KCL auto-gen tool. DO NOT EDIT.
Editing this file might prove futile when you re-run the KCL auto-gen generate command.
"""
import kusion_kubernetes.apimachinery.apis

schema CronTab:
    """stable example com v1 cron tab
    Attributes
    ----------
    __settings__ : {str:str}, default is {"output_type": "STANDALONE"}, optional
         existence of this attribute indicates that the model will be treated standalone by KCLVM.
    apiVersion : str, default is "stable.example.com/v1", required
         APIVersion defines the versioned schema of this representation of an object. Servers should convert recognized schemas to the latest internal value, and may reject unrecognized values. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources
    kind : str, default is "CronTab", required
         Kind is a string value representing the REST resource this object represents. Servers may infer this from the endpoint the client submits requests to. Cannot be updated. In CamelCase. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds
    metadata : apis.ObjectMeta, default is Undefined, optional
        metadata
    spec : StableExampleComV1CronTabSpec, default is Undefined, optional
        spec
    """

    apiVersion: "stable.example.com/v1" = "stable.example.com/v1"
    kind: "CronTab" = "CronTab"
    metadata?: apis.ObjectMeta
    spec?: StableExampleComV1CronTabSpec

schema StableExampleComV1CronTabSpec:
    """stable example com v1 cron tab spec
    Attributes
    ----------
    cronSpec : str, default is Undefined, optional
        cron spec
    image : str, default is Undefined, optional
        image
    replicas : int, default is Undefined, optional
        replicas
    """

    cronSpec?: str
    image?: str
    replicas?: int
```

# 然后

更多信息，请访问(如果你喜欢，你可以开始，谢谢😊):

*   KCL:【https://github.com/KusionStack/KCLVM 
*   库辛:[https://github.com/KusionStack/kusion](https://github.com/KusionStack/kusion)
*   konfig:[https://github.com/KusionStack/Konfig](https://github.com/KusionStack/Konfig)

欢迎加入我们的交流社区:

*   [https://github.com/KusionStack/community](https://github.com/KusionStack/community)