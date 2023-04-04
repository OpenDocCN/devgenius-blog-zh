# 使用 Apache Airflow、Kubernetes 和 R 创建自动化数据处理管道—第 1 部分

> 原文：<https://blog.devgenius.io/creating-an-automated-data-processing-pipeline-with-apache-airflow-kubernetes-and-r-part-1-925f99b812e7?source=collection_archive---------8----------------------->

![](img/b013a876b8e417f7973c52b11f767dbb.png)

照片由 [Siniz Kim](https://unsplash.com/@siniz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在研究如何自动化数据处理管道时，Apache Airflow 作为编排管道片段的方法被多次提到。但是当我深入研究时，常见的实现似乎建议在与 Airflow 相同的实例上运行操作符，并且使用的操作符是有限的(用`PythonOperator`运行 python，用`BashOperator`运行 shell 脚本)。我们的分析团队主要使用 R，所以我需要某种设置来让我执行 R 脚本，但我也想将任务执行隔离到单独的 VM 实例或应用程序或项目。

我最终发现了`KubernetesPodOperator`,但是从零开始建立一个完整的 Kubernetes 集群来使用 Airflow 似乎需要很多时间，特别是对于没有太多使用 Kubernetes 的人来说。Cloud Composer 是 Google 为 Airflow 创建的一个完全管理的 Kubernetes 集群，它为我解决了这个问题，但是将点连接起来以执行我们的 R 脚本仍然很困难。

BBC 数据科学来拯救我们。他们关于气流和 R 的文章是我需要的最后一篇文章。虽然它利用了 AWS，但我能够替换 Cloud Composer 的一些部分，并使用`KubernetesPodOperator`来代替他们的 AWS 批处理。

# 概观

虽然我们完全可以从头开始构建这一切，设置一个气流实例、一个调度器、一个队列、一个保存 DAG 信息的数据库和一个用于执行我们的任务的节点池，但我转向了 Google Cloud 提供的一个“一体化”解决方案: [Cloud Composer](https://cloud.google.com/composer) 。作为一个小团队的工程师，完全或部分管理的解决方案大大减少了我们的开发时间，让我们可以更专注于开发。Cloud Composer 非常适合这一点。

他们的解决方案包含了我上面列出的大部分内容。它是一个 Kubernetes 集群，由以下部分组成:一个 Flask web server for Airflow(使用 App Engine Flex)，一个将任务分配给工人的调度程序，一个保存 DAG 数据的数据库，一个用于调度任务的 Redis 队列，当然还有工人本身。一切都建立在一个“租户”项目中，这是一个独立的谷歌管理的项目，与您最初的谷歌云项目相关联。几乎所有服务都有日志。并且所有 Dag 和 Airflow 插件都进入云存储桶，使用`syncd`同步到 Airflow。

为了简化 Dag、任务和插件的配置和开发，我创建了一个存储库，以一种干净的方式保存所有这些内容，并允许我们“部署”我们的文件(即，将所有内容上传到云存储的相应文件夹中，然后供 Airflow 使用)。

我们的 Dag 将主要使用`KubernetesPodOperator`将每个任务旋转到它自己的 pod 中，利用 docker 文件来构建它。entrypoint 将是一个小的“bootstrap”脚本，它将通过 Google Cloud 进行身份验证，下载任务必需的 shell 脚本，并执行它。如果没有错误，pod 将关闭，工作流将转到下一个任务，重复此过程。

虽然这种旋转加速和旋转减速确实增加了工作流的执行时间，但它确保了最大的隔离性、可再现性，以及幂等性(如果任务是这样编写的话)。假设用于任务的任何数据输入保持不变，那么任务应该总是以相同的方式执行，并且输出应该总是相同的。每个任务都会生成日志，如果出现问题，我们会对单个任务进行故障排除。此外，任何内存、配置或环境错误都会在单个任务中出现，因此也很容易修复。

# 设置

这个项目的设置将在三个地方进行:存储库、您的本地环境和 Cloud Composer 环境。一旦这些都设置好了，我们就可以开始调整设置和配置了。

## 仓库

这里已经设置了一个供你分叉的空白库[。自述文件应该包含您需要的所有内容，但是我将把一些描述复制到本文中。`common_files`、`dags`、`plugins`和`config`文件夹的内容被部署(用`gsutil rsync`上传)到云存储中，而其他的内容要么只存在于存储库中，要么只存储在本地。](https://github.com/rgutierrez-cotech/airflow-k8s-r-template)

文件夹的布局如下:

**dags**

包含气流的 Dag 和任务代码。为了便于维护，我将任务文件分离到它们自己的模块中。有一个示例 DAG 和任务说明了如何将所有内容连接在一起。

**任务 _ 文件**

这是这个项目的核心。任务在 pod 中执行所需的所有文件都将存在于此。这将需要包含一个外壳脚本，该脚本将由容器下载并运行。在 shell 脚本中，我们将执行特定任务所需的所有代码，因此包括任何相关的 R 脚本、python 脚本等。这里也是。

**图片**

包含我们将使用的任何容器图像的 docker 文件和相关文件

**部署**

包含以前部署的 TAR 存档。该目录中的任何内容都不会在存储库中被跟踪；这些档案将只存在于本地。部署脚本将使用这个目录，在将文件复制到云存储之前，创建文件当前状态的 tar。这里任何已经存在的 TAR 也可以用于部署，作为一种“回滚”部署。如果您提供 TAR 的名称(不包括文件扩展名)，部署脚本将解压缩它并从该归档文件中部署文件。

**常用 _ 档案**

多个任务使用的任何文件都将存在于此。这些将是像对应表，人行横道，代码簿，数据元素字典等东西。基本上，任何不经常改变的，被多个任务使用的，以及你想跟踪改变的。此文件夹是可选的，仅当您的存储库是私有的时才推荐使用。

**配置**

包含每个版本的 Cloud Composer 环境的气流配置文件(`airflow.cfg`)。可用设置可在[气流配置页面](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html)找到。本文稍后将提供更多信息。

**插件**

包含气流插件文件。访问[气流插件页面](https://airflow.apache.org/docs/apache-airflow/stable/plugins.html)查看如何创建插件。关于如何为 Airflow 编写插件的信息将在本文后面提供。

## 局部环境

要处理任务创建并与 Google Cloud 交互，您需要在机器上安装以下包/库:

```
google-cloud-sdk
git-lfs
virtualbox
kubectl
docker-machine
docker
pyenv
pyenv-virtualenv
```

其中三个是可选的。我正在使用`git-lfs`上传一个专有数据分析软件包的二进制文件，它需要被复制到我们的容器映像中，以便我们可以在我们的任务中使用它。`pyenv`和`pyenv-virtualenv`用于创建一个本地 python 环境，模仿 Airflow web 服务器的环境，以便在编写 Dag 和任务时帮助语法高亮、代码完成和林挺。

## 云作曲家

理想情况下，您应该创建至少两个 Cloud Composer 环境，一个生产环境和一个试运行/开发环境。我只使用了两个，并在我们的“登台”环境中完成了我所有的开发和测试，尽管拥有一个单独的开发环境和一个测试环境也是可行的。

创建 Cloud Composer 环境时，请确保您已经选择了希望成为环境“宿主”的项目。我们有单独的项目用于我们的产品化和生产代码，所以我从选择我们的产品化项目开始。

在 Google Cloud 控制台的导航菜单中，找到大数据部分，然后单击 Composer。在顶部，单击“创建”下拉菜单，然后选择“编写器 1”或“编写器 2”。在本教程中，我将使用 Composer 1。

您使用的设置可能与我们的不同，但这些可以作为起点:

```
- Location: us-central1
- Service account: default Compute Engine service account
- Image version: composer-1.17.3-airflow-1.10.15
- Python version: 3
- Web server machine type: composer-n1-webserver-2 (2 vCPU, 1.6 GB memory)
- Cloud SQL machine type: db-n1-standard-2 (2 vCPU, 7.5 GB memory)
- Workers:
    - Nodes: 3
    - Disk size: 50gb
    - Machine type: n1-standard-1
- Number of schedulers: 1
- GKE Cluster
    - Zone: us-central1-c
```

Composer 环境可能需要 15 分钟才能完全部署。这是再喝一杯咖啡或茶的好时机。

一旦部署完毕，我们还需要设置一些东西。

**部署脚本**

一旦部署完成，确保将云存储 bucket 名称复制到部署脚本的`bucket`变量中。

**秘密**

对于创建 Cloud Composer 环境时使用的任何服务帐户，您都需要创建一个密钥。该密钥将用于在您的任务工人 pod 中使用谷歌云服务，如`gcloud`和`gsutil`。为此，我们将利用 Kubernetes 的秘密功能。

在云控制台中，转到 IAM & Admin >服务帐户。点按服务帐户上的三个点，然后选择“管理密钥”。添加一个新的 JSON 键，并将该文件复制到您机器上的项目根文件夹中。不要在存储库中跟踪它。

接下来，我们需要将它添加为 Kubernetes 秘密。在您的计算机上，打开一个终端选项卡或窗口，导航到您的项目根目录，并运行以下命令，填写参数。您可以通过在控制台中转至您的 Cloud Composer 环境并单击“环境详细信息”选项卡来找到所需的信息。

```
$ gcloud container clusters get-credentials CLUSTER_ID \
--zone ZONE \
--project PROJECT
```

该命令为我们提供了到集群的连接凭据。

现在添加我们之前下载的密钥文件作为秘密:

```
$ kubectl create secret generic KEY_NAME \
-- from-file REMOTE_KEY_NAME.json=./LOCAL_KEY_NAME.json
```

远程密钥名称和本地密钥名称可以相同。

您可以通过进入控制台中的 Kubernetes Engine > Configuration 来验证这个秘密的存在。

**环境变量**

我在 Dag 中使用了一些环境变量，现在让我们添加它们。从云控制台，转到您的 Cloud Composer 环境并打开 Airflow UI。在气流中，转到管理>变量。

我们将在这里创建三个变量:

```
- cloud_storage_bucket: BUCKET
- project_id: PROJECT
- valid_instantiators: airflow\r\nuser 1\r\nuser 2
```

填写为 Cloud Composer 创建的云存储空间的名称，并填写运行 Cloud Composer 的项目 id。

`valid_instantiators`是一个换行符分隔的“实例化器”列表，它将触发 Dag。我添加了这一点，以便当我们手动触发 DAG 时，我们可以在任务中访问的`conf`变量中指定一个实例化器，然后根据有效实例化器的列表对其进行检查。创建自定义运行 ID 时将使用指定的实例化器，并且此自定义运行 ID 将用作 DAG 运行的云存储中的输出文件夹。当我在后面谈到示例 DAG/task 时，这将更有意义。

**气流配置**

您可以通过在`config`文件夹的相关文件夹中包含一个`airflow.cfg`文件来定义自定义气流配置。我建议等到您的 Cloud Composer 环境完全实例化后，*然后*复制。cfg 文件保存到这个存储库中的相关文件夹中(`config/staging`用于您的暂存环境，`config/production`用于您的生产环境)。

您可以添加/修改的设置可以在[气流配置页面](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html)上找到。

**气流插件**

如果你想为 Airflow 编写插件，把它们放在`plugins`文件夹中。创建插件时，遵循[气流插件页面](https://airflow.apache.org/docs/apache-airflow/stable/plugins.html)上的说明和示例。

所有 Cloud Composer 和 Airflow 版本都支持插件，但 UI/web 服务器插件**在 Cloud Composer 1 和 Airflow 2 中**不可用。

***重要:*** 如果您正在使用 Cloud Composer 1 环境和 Airflow 1，并且正在编写一个 UI/web 服务器插件(将在 Airflow UI 中加载/使用)， ***您需要禁用 DAG 序列化，以便它能够工作*** *。*基于[该页面](https://cloud.google.com/composer/docs/concepts/versioning/composer-versioning-overview)，DAG 序列化禁用 UI 插件， ***，默认开启*** *。*为此，您需要禁用 airflow.cfg 文件中的两个核心配置设置:`core/store_serialized_dags`和`core/store_dag_code`。访问[本页](https://cloud.google.com/composer/docs/dag-serialization#disable)获取更多信息。

这让我头疼了好几天，我偶然发现了解决方法。我把它贴在这里是为了让你不那么头疼。

## 气流/云合成器提示和技巧

以下是在此回购中使用 Cloud Composer 和资产的一些(仅一个)分类提示和技巧。

**刷新 Cloud Composer 环境/气流**

如果您想要重新部署您的 Cloud Composer 环境，可以通过添加一个“伪”环境变量并修改其值来实现。

导航到 Google Cloud 控制台中的 Cloud Composer 部分，然后单击您的环境。然后转到环境变量选项卡。现在添加一个假的键和值并保存。或者，如果您已经有一个伪变量，只需编辑值并保存即可。这将重新部署 Cloud Composer 环境。这可能需要 15 分钟才能完成。

如果您对 Airflow 配置文件进行了更改，或者正在调试 DAG/插件加载错误，您可能需要重新启动 Airflow web 服务器以查看任何更改是否生效。要重新启动 Airflow web 服务器(仅限 Cloud Composer 1)，您可以从 shell 中运行以下命令，并填入参数:

```
$ gcloud composer environments restart-web-server ENVIRONMENT — location=LOCATION
```

这可能需要 15 分钟才能完成。

# 总结

这是数据处理管道教程的第一部分。我们已经设置了 Cloud Composer 环境，设置了用于创建 Dag 和任务的本地环境，并派生了模板 repo。

在下一部分的[中，我们将创建自己的 DAG 和任务，上传到 Airflow，并运行它！](https://robert-a-gutierrez.medium.com/creating-an-automated-data-processing-pipeline-with-apache-airflow-kubernetes-and-r-part-2-2e95c2e9ae5e)