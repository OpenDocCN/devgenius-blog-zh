# 招商银行与 KubeVela 的线下安装实践

> 原文：<https://blog.devgenius.io/china-merchants-banks-practice-on-offline-installation-with-kubevela-4da0790dea73?source=collection_archive---------17----------------------->

*马相伯(云平台开发团队)*

招商银行的云平台开发团队自 2021 年以来一直在内部试用 KubeVela，旨在使用它来增强我们的主要应用交付和管理能力。由于金融保险行业的特殊安全考虑，网络控制措施相对严格，我们的内部网无法直接获取 Docker Hub 映像，也没有可用的 Helm 映像源。因此，为了在内部网中登陆 KubeVela，您必须执行完整的离线安装。

本文将以 KubeVela V1.2.5 版本为例，介绍离线安装实践，帮助其他用户在离线环境下更容易地完成 KubeVela 的部署。

# KubeVela 离线安装解决方案

我们把 KubeVela 的离线安装分为三个部分，分别是 Vela CLI、Vela Core、Addon 离线安装。每个部分主要涉及相关 Docker 映像和 Helm 的包的加载，这可以大大加快离线环境下的部署过程。

在此之前，请确保 Kubernetes 集群版本是`>= v1.19 && < v1.22`。KubeVela 作为控制平面的一种方式依赖于 Kubernetes，它可以放在任何产品或任何云提供商中。同时，也可以使用 Kind 或 Minikube 在本地部署 KubeVela。

# Vela CLI 离线安装

●首先，你需要通过查看 KubeVela [发布日志](https://github.com/oam-dev/kubevela/releases)下载你想要的`vela`的二进制版本

●解压缩二进制文件，并在`$PATH`中配置适当的环境变量

○解压缩二进制文件

*`tar -zxvf vela-v1.2.5-linux-amd64.tar.gz`

*`mv ./linux-amd64/vela /usr/local/bin/vela`

○设置环境变量

*`vi /etc/profile`

*`export PATH="$PATH:/usr/local/bin"`

*`source /etc/profile`

○通过`vela version`验证 Vela CLI 的安装

```
CLI VERSION: V1.2.5
Core Version:
GitRevision: git-ef80b66
GOLANGVERSION: Go1.17.7
```

●至此，Vela CLI 已经离线部署！

# Vela 核心离线安装

●在离线部署 Vela Core 之前，首先需要在离线环境下安装 [Helm](https://helm.sh/docs/intro/install/) ，其版本需要满足`v3.2.0+`

●准备 Docker 图像。Vela Core 的部署主要涉及 5 个映像，您需要首先访问 extranet 中的 Docker Hub 下载相应的映像，然后加载到离线环境中

○从 Docker Hub 提取图像

*`docker pull oamdev/vela-core:v1.2.5`

*`docker pull oamdev/cluster-gateway:v1.1.7`

*`docker pull oamdev/kube-webhook-certgen:v2.3`

*`docker pull oamdev/alpine-k8s:1.18.2`

*`docker pull oamdev/hello-world:v1`

○将图像保存到本地磁盘

*`docker save -o vela-core.tar oamdev/vela-core:v1.2.5`

*`docker save -o cluster-gateway.tar oamdev/cluster-gateway:v1.1.7`

*`docker save -o kube-webhook-certgen.tar oamdev/kube-webhook-certgen:v2.3`

*`docker save -o alpine-k8s.tar oamdev/alpine-k8s:1.18.2`

*`docker save -o hello-world.tar oamdev/hello-world:v1`

○在离线环境中重新加载映像

*`docker load vela-core.tar`

*`docker load cluster-gateway.tar`

*`docker load kube-webhook-certgen.tar`

*`docker load alpine-k8s.tar`

*`docker load hello-world.tar`

●下载 [KubeVela Core](https://github.com/oam-dev/KubeVela/releases) ，复制到离线环境，使用 Helm 重新打包

○重新打包 KubeVela 源代码，并将图表包离线安装到控制集群

*`helm package kubevela/charts/vela-core --destination kubevela/charts`

*`helm install --create-namespace -n vela-system kubevela kubevela/charts/vela-core-0.1.0.tgz --wait`

○检查输出

```
KubeVela Control Plane Has Been successfully set up on your cluster.
```

●至此，Vela Core 已经下线部署！

# 插件脱机安装

●首先下载[目录源](https://github.com/oam-dev/catalog)并复制到离线环境

●在这里，我们将以众多插件之一的 VelaUX 为例。首先准备它的 Docker 镜像，VelaUX 主要涉及 2 个镜像，你需要先访问 extranet 从 Docker Hub 下载相应的镜像，然后加载到离线环境

○从 Docker Hub 提取图像

*`docker pull oamdev/vela-apiserver:v1.2.5`

*`docker pull oamdev/velaux:v1.2.5`

○将图像保存到本地磁盘

*`docker save -o vela-apiserver.tar oamdev/vela-apiserver:v1.2.5`

*`docker save -o velaux.tar oamdev/velaux:v1.2.5`

○在离线环境中重新加载映像

*`docker load vela-apiserver.tar`

*`docker load velaux.tar`

●安装 VelaUX

○通过 Vela CLI 安装 VelaUX

*`vela addon enable catalog-master/addons/velaux`

○检查输出

```
Addon: velaux enabled Successfully.
```

●如果集群安装了路由控制器或 Nginx 入口控制器，并且还与可用域链接，您可以部署外部路由以使 VelaUX 可访问。这里以 Openshift 路由为例，如果您愿意，也可以选择 Ingress

```
apiVersion: route.openshift.io/v1
kind: Route
metadata:
name: velaux-route
namespace: vela-system
spec:
host: velaux.xxx.xxx.cn
port:
  targetPort: 80
to:
  kind: Service
  name: velaux
  weight: 100
wildcardPolicy: None
```

●检查安装

```
curl -I -m 10 -o /dev/null -s -w %{http_code} [http://velaux.xxx.xxx.cn/applications](http://velaux.xxx.xxx.cn/applications)
```

●至此，VelaUX 已经下线部署！同时，对于其他类型的插件的离线部署，进入[目录源](https://github.com/oam-dev/catalog)的相应目录，重复上述步骤，就可以永久完成所有插件的离线部署。

# 总结

在离线部署期间，我们也试图保存在 extranet 中部署后生成为 YAML 文件的 Vela Core 和 Addon 的资源，并在离线环境中重新部署它们，但是由于涉及到各种不同的资源，并且需要解决许多其他授权问题，这种方式非常麻烦。

通过 KubeVela 的离线部署实践，我们希望它能帮助您更快地在离线环境中构建一套完整的 KubeVela。离线安装对大多数开发者来说是一个很大的痛点，我们也看到 KubeVela 社区正在引入全新的 [velad](https://github.com/oam-dev/velad) ，一个完全离线的、高度可靠的安装工具。Velad 可以通过将许多步骤合二为一来帮助自动完成，例如准备集群、下载和打包映像、安装等。此外，它还支持许多功能:在 Linux 机器(如阿里云 ECS)上，我们可以本地启动一个集群来安装 Vela-Core；当启动 KubeVela 控制平面时，不必担心它后面的机器意外关闭时，它的数据会丢失；Velad 可以将来自控制平面集群的所有数据存储到传统数据库中(如部署在另一个 ECS 上的 MySQL)。

在即将到来的近期版本中，招行将加大在 KubeVela 开源社区的力度，积极建设:企业级能力、多集群上的增强、离线部署和应用级可观察性。我们还将为金融行业的用户场景和业务需求做出贡献，推动云原生生态实现更轻松高效的应用管理体验，最后但同样重要的是，欢迎您作为社区成员加入我们的旅程。