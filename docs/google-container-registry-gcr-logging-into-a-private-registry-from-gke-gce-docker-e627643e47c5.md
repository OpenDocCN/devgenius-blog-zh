# 谷歌容器注册(GCR):登录到一个私人注册从 GKE，GCE，Docker

> 原文：<https://blog.devgenius.io/google-container-registry-gcr-logging-into-a-private-registry-from-gke-gce-docker-e627643e47c5?source=collection_archive---------5----------------------->

# 文章章节

*   [文章上下文](#9093)
*   [先决条件](#2a9a)
*   [创建 IAM 角色成员的服务帐户](#69d6)
*   [服务账户凭证](#a001)
*   [通过 Docker](#a841) 从计算资源登录
*   [在 GKE 上拉动图像](#3ef1)
*   [结论](#8b83)

# 语境

在最近将部分开发工具链迁移到 Google 云平台的过程中，我们的团队发现了一些配置上的细微差别，我们觉得与他人分享可能有助于解决一些令人头疼的问题。

当通读 Google 文档时，乍一看，似乎我们所要做的就是创建一个服务帐户，并确保该服务帐户拥有访问容器存储桶的权限。所需的权限记录为 [*角色/存储. objectViewer*](https://cloud.google.com/container-registry/docs/access-control) 。看起来很简单，除了这在我们的例子中不起作用。

本演练介绍如何配置从私有 GCR 注册中心提取图像的必要组件。我们将主要使用地形，但也有其他方法来实现这一点。

# 先决条件

*   已启用容器注册表 API
*   一张图片已经被推送到私人注册中心([这里有一个来自谷歌](https://github.com/terraform-google-modules/terraform-google-github-actions-runners/tree/v3.0.0/examples/gh-runner-gke-dind)的工作流程示例)
*   为您的项目安装/配置的 Terraform
*   对地形的一般理解
*   为您的项目安装/配置的 gcloud SDK

# 创建具有 IAM 角色成员身份的服务帐户

以下代码片段概述了完成服务帐户和必要角色的创建所需的 Terraform 代码。请注意，从私有 GCR 注册中心提取映像需要两个角色。

*   storage.objectViewer(查看和获取对象，即容器图像)
*   artifactregistry.reader(查看和获取工件，即容器清单)

```
terraform {
  ...
}resource “google_service_account” “gcr_pull” {
  account_id = “service-account-gcr-pull”
  display_name = “Service Account for Pulling images from GCR”
}resource “google_project_iam_member” “storage_viewer” {
  count = 1
  project = var.project
  role = “roles/storage.objectViewer”
  member = “serviceAccount:${google_service_account.gcr_pull.email}”
}resource “google_project_iam_member” “artifact_reader” {
  count = 1
  project = var.project
  role = “roles/artifactregistry.reader”
  member = “serviceAccount:${google_service_account.gcr_pull.email}”
}output "service_account_email" {
  description = "Service account email (for single use)."
  value       = google_service_account.gcr_pull.email
}
```

# 服务帐户凭据

创建服务帐户后，我们需要生成私钥，以便作为服务帐户向 GCR 私有注册中心进行身份验证。这在 CI/CD 管道和其他自动化流程中非常有用。

注意:您可以使用 Terraform 实现这一点，但是您应该知道，生成的 JSON 文件将以 Terraform 状态存储，这可能是一个安全问题。最安全的替代方法是从 [GCP 控制台](https://cloud.google.com/iam/docs/creating-managing-service-account-keys#console)创建密钥，或者通过下面的代码片段用 gcloud 创建密钥。从控制台获取帐户电子邮件地址是最容易的，但是您也可以从前面的 Terraform 输出中获取。

```
gcloud iam service-accounts keys create ***<output_filename>*** --iam-account=***<account_email>***
```

一旦创建了 JSON 密钥，就可以将文件保存为 Google Secret、HashiCorp Vault Secret 或本地终端中的变量引用。出于测试目的，您可以将 JSON 内容保存到***service _ account _ keys . JSON***。

# 通过 Docker 从计算资源登录

当您在本地文件中有可用的服务帐户密钥时，您可以通过从计算资源(当然安装了 Docker 引擎)登录 Docker 来验证访问和权限。

如果您要启动一个 GCE 实例，您应该确保先前创建的服务帐户与该实例相关联。

```
docker login -u _json_key --password-stdin [https://gcr.io](https://gcr.io) < service_account_keys.json
```

您应该会看到登录成功的响应！你现在可以从你的私人 GCR 注册表中提取图像。

## 这有什么用？

*   验证访问和权限
*   执行本地容器验证
*   对私有的 GitHub/GitLab 运行程序使用这种模式

# 提取 GKE 的图像

最后，我们如何将所有这些联系在一起，以便能够使用托管在我们的私人 GCR 注册中心的图像？

很可能您的组织希望确保您能够控制您的生态系统的开发工具，这通常意味着管理您自己的容器映像骨干。这不仅确保了您对资源的控制，还加强了您的安全状况。

## 您希望确保解决以下配置问题:

*   群集节点使用以前创建的服务帐户
*   您的 Kubernetes 资源利用服务帐户的拉取秘密

```
**# Terraform GKE node pool; adding the service account to you nodes**resource "google_container_node_pool" "node_pool" {

  ...

    node_config {
    machine_type = var.machine_type
    image_type = var.image_type
    disk_size_gb = var.disk_size_gb
    disk_type = var.disk_type
    preemptible = var.preemptible
    ***service_account = var.service_account_email***
  }
}
```

如果您已经配置了您的集群和节点池，您可以轻松地返回到您的 Terraform 配置并更新服务帐户，然后执行 ***terraform apply*** 以进行更新。*这假定您保存了 tfstate 和缓存。

为了从 GKE 资源的私有 GCR 注册表中提取图像，您需要首先设置***service _ account _ key . JSON***作为 Kubernetes 的秘密。

```
kubectl create secret docker-registry ***gcr-pull-secret*** \
--namespace my-custom-app \
--docker-server=https://gcr.io \
--docker-username=_json_key \
--docker-password="$(cat service_account_key.json)" \
--docker-email='***<service_account_email>***'
```

既然这个秘密在所提供的名称空间中是可用的(请密切注意名称空间，这有时会让人出错)，那么您可以设置一个 pod(例如)来使用新形成的秘密。

```
**apiVersion**: v1
**kind**: Pod
**metadata**:
  **name**: test-private-registry
**spec**:
  **containers**:
  - **name**: private-registry-container
    **image**: gcr.io/cool-project-123/my-app:4.5.6
  **imagePullSecrets**:
  - **name**: ***gcr-pull-secret***
```

一旦您应用/部署了 pod，您应该不会有问题接收致命的“权限被拒绝”消息。

# 结论

希望这能帮到一些人。它确实帮助我写出了这篇文章，所以当我不可避免地忘记为什么我似乎不能从我的私人 GCR 注册表中获取图片以从我的 GKE 资源中提取时，我可以参考这篇文章。

*编码快乐！*