# 本地云:Ubuntu 上 MicroK8s 中的 S3+火花+火山(第 1 部分)

> 原文：<https://blog.devgenius.io/cloud-native-on-premise-s3-spark-volcano-in-microk8s-on-ubuntu-part-1-4c9898f967fe?source=collection_archive---------4----------------------->

我使用基于 Ubuntu 的系统进行本地开发已经有一段时间了，由于我在 2022 年的目标是为计算和数据管道创建一个云原生多租户机器学习平台，我认为我需要解决的第一件事是如何为数据和 ML 作业部署 Kubernetes 原生调度程序。Spark 就是这样一个支持数据管道和机器学习作业的框架。在两篇文章(第 1 部分和第 2 部分)的结尾，我将演示一个端到端的用例，提交 Spark 作业，处理由在 Ubuntu 系统上的 MicroK8s 中运行的 Volcano 管理的存储在 S3 的数据。

## 云原生

Cloud-native 意味着它可以完全在云中运行，但是考虑到我们正在尝试将它部署在本地系统上，它应该也可以在“本地”运行。这意味着无论在哪里部署 Kubernetes，您都应该能够运行您的软件。这可以是像 AWS EKS 或谷歌的 GKS 或 Azure 这样的 Kubernetes 即服务产品，也可以是开源或付费 Kubernetes(如 IBM OpenShift)发行版在您的私有数据中心的一些部署。

## 多租户

这是一个关键的方面，也是企业软件世界中男孩和男人(或者女孩和女人)的区别。您希望能够为 Kubernetes 的工作在租户(及其成员)之间开拓和共享资源。我将在下面讨论火山如何解决这个问题。

但是现在，让我们来看看在本地开发环境中安装基础设施的细节。

# Ubuntu 快照和 Docker/micro k8

关于 Ubuntu snaps，我不会说太多细节，但我敢说，就在他们的 Linux 操作系统中打包应用程序而言，这是 Ubuntu 想出的最好的东西之一。它的主要目标是允许用户轻松地将一个管理好的包安装到 Ubuntu 中，管理甚至清理。

你可以在 Ubuntu 中安装 Docker 和 MicroK8s 作为快照。

我不会在这里谈论 Docker，因为已经有足够多的文献详细描述了 Docker。使用 Ubuntu snap 安装 Docker 的命令如下:

> `*sudo snap install docker*`

MicroK8s 是 Ubuntu 的 Kubernetes 发行版，原生运行在 Ubuntu 中。与 Kubernetes 的其他单节点发行版如 [minikube](https://minikube.sigs.k8s.io/docs/) 相比，MicroK8s 不在虚拟机内运行。例如，Minikube 在 VirtualBox 这样的管理程序中作为来宾运行，这是默认的。也就是说，我相信新版本的 Minikube 现在支持在裸机上运行，甚至可以在容器中运行。总之，事不宜迟，您可以用下面的命令安装 MicroK8s:

> `sudo snap install microk8s --classic`

我建议您给添加一个别名。bash_aliases(或者您为 shell 使用的任何别名文件)。我使用 bash，所以在我的。bash_aliases 下面的条目:

> `*alias kubectl='microk8s.kubectl'*`

microk8s.kubectl 是 microk8s 附带的 kubectl 客户机，为了将来方便起见，可以直接将 kubectl 命名为这个客户机。要检查 Docker 和 Kubernetes 是否都在运行，您可以运行以下命令:

> `*docker images*`

假设 docker 是最近才安装的，那么您应该得到如下所示的文本

存储库标记图像 ID 创建的大小

> `*microk8s.kubectl cluster-info*`

应该会返回如下内容:

> Kubernetes 控制平面运行在[https://127 . 0 . 0 . 1:16443](https://127.0.0.1:16443)
> 
> 要进一步调试和诊断集群问题，请使用“kubectl cluster-info dump”。

为了让 MicroK8s 使用图像，让我们启用内置的 Docker 注册表、DNS 和本地主机访问。

> `*microk8s enable registry*`
> 
> `*microk8s enable dns*`
> 
> `*microk8s enable host-access*`

我刚刚花了一些时间浏览了一些安装 Docker 和 MicroK8s 的基础知识，以确保我们都在同一页面上。

对于接下来的两个组件，让我们克隆我的 github repo:[https://github.com/zlgonzalez/spark-k8s](https://github.com/zlgonzalez/spark-k8s)

# 本地堆栈

现在我们需要一个在本地工作的 S3 文件系统。最简单的方法是使用 LocalStack 在本地运行 S3。对于 S3 来说，免费版的 LocalStack 拥有我们需要的所有功能。

无论如何，要运行它，从 spark-k8s，运行

> `*./scripts/localstack*`

这将在 Docker 映像中运行 LocalStack。开始时可能需要一些时间，因为如果在本地找不到，就必须从 DockerHub 下载图像。使用以下命令确认它正在运行

> `*docker ps -f "name=localstack"*`

您应该看到如下所示的一行

> 容器 ID 映像命令创建的状态端口名称
> 4e 05d 750 ca 1a local stack/local stack " docker-entry point . sh " 12 分钟前最多 12 分钟(正常)0 . 0 . 0 . 0:4510–4559->4510–4559/TCP，::::4510–4559->4510–4559/TCP，0.0.0:4566- 【T17

如果您的计算机上没有安装 aws cli，请首先执行以下操作

> `*pip install awscli*`
> 
> `*aws configure*`

并为被请求的内容添加任何您想要的属性。由于这是 LocalStack，所有这些配置都将被忽略。

现在让我们在 LocalStack S3 中创建一个 bucket，我们可以将它用于主应用程序 jar 和一些我们可以使用的数据。我们将创建一个新的桶，并将 jar 和数据复制到桶中。

> `*./scripts/awsl s3 mb s3://spark*`
> 
> `*cd data*`
> 
> `*../scripts/awsl s3 cp a.txt s3://spark*`
> 
> `*../scripts/awsl s3 cp spark-examples_2.12-3.1.3.jar s3://spark*`

# 火花

如果你已经走了这么远，我会要求你多一点耐心。虽然我们现在已经成功地构建和部署了我们的数据基础设施，但是我们仍然需要一个数据管道框架来开始处理我们的数据。这个框架将会是火花。

我们的最终目标是安装 Spark 操作器，然后让任何 Spark 作业使用 Volcano 来调度将运行 Spark 执行器的 Kubernetes pods。

假设您按顺序阅读了本文，那么您应该已经克隆了我的 github repo。

让我们首先使用 Ubuntu 中我们最喜欢的 snap 工具安装 terraform。

> `*sudo snap install terraform --classic*`

完成之后，让我们从 spark-k8s/terraform 创建 Spark 操作符

> `*terraform init*`
> 
> `*terraform apply*`

这应该会在我们的 Kubernetes 集群中创建 Spark 操作符。让我们验证一下

> `*kubectl -n spark-operator get pods*`

您应该得到类似于

> 名称就绪状态重启年龄
> my-release-spark-operator-66 cb5 bccb 6-bnl6z 1/1 运行 0 12m

就是这样。现在，Spark、S3、Docker 和 Kubernetes 都在您的本地环境中运行。

咻，那绝对是一口。让我们继续，最后使用所有这些基础设施运行一个作业。

我们来看两个例子:计算圆周率和计算字数。在这两种情况下，主应用程序 jar 将从 S3 检索，对于字数统计作业，它还将从 S3 检索数据有效负载。

因此，再次从 spark-k8s github 回购:

> `*kubectl apply -f ./operator/microk8s/spark-pi-s3.yaml*`
> 
> `*kubectl apply -f ./operator/microk8s/wordcount-s3.yaml*`

如果要重新运行这两个命令，请确保删除操作员创建的 SparkApplication CRD 的实例。您可以使用以下命令检查已经创建的实例

> `*kubectl get SparkApplication*`

您可以使用命令删除 SparkApplication 的实例

> `*kubectl delete SparkApplication spark-pi*`

原来如此！总之，我们:

1.  使用 Snap 安装 Kubernetes
2.  使用 Snap 安装 Docker
3.  为 S3 安装的本地堆栈
4.  安装了 SparkOperator 来运行 Kubernetes 和 Docker 中的作业，使用 S3 作为我们代码和数据的来源

在第 2 部分中，我们将讨论如何使用 Volcano batch scheduler，这样我们就可以实现更好的资源管理和其他功能。

我希望你读这篇文章的时候和我分享在我的本地桌面上构建云原生系统的经历时一样开心。

直到下一次…