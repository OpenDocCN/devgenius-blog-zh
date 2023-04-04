# 将 Azure 上的 MLOPS 与 MLFlow 啮合

> 原文：<https://blog.devgenius.io/meshing-mlops-on-azure-with-mlflow-c1c1ffa59e32?source=collection_archive---------5----------------------->

![](img/66b08a3c69c39d46f70aacc863273e35.png)

高层建筑

这是 3 部分系列的第一部分，接下来是—

[**2。MLFLOW** 上的分散式 m lops](https://medium.com/@masterkeshav/decentralized-mlops-on-mlflow-ab25199f7ba4)

[**3。MLFLOW 上的分散式 MLOPs 规模化生产模式**](https://medium.com/@masterkeshav/decentralized-mlops-on-mlflow-productizing-model-for-scale-c99ed4ed625)

# 搭建舞台

典型的网状架构意味着一个中央自助式开放标准互操作平台，为域(总线)提供访问管理、数据管理、数据共享、治理、合规性和**本质上的 MLOPs** 等功能。在这篇博客中，我们将利用 MLFlow 建立 ML 生命周期，ML flow 是一个用于管理 ML 生命周期的开源机器学习平台和框架。参考[https://mlflow.org/](https://mlflow.org/)

网格平台团队利用 MLFlow 为所有领域团队集成中央" **MLFlow 服务器和注册中心**"服务，以满足他们各自的 MLOps 需求。上面的架构将“**领域数据产品开发人员**”角色进一步分解为 3 个角色

*   数据工程师
*   **b .)数据科学家&**
*   ML 工程师

![](img/522deb92b8378891f9c498a8e153539d.png)

数据发布工厂

“**数据发布工厂**是一个高度专业化的数据集成、工程、数据发布系统，用于**操作原始数据摄取**和**构建分析丰富的数据产品**。它是一个 ETL 框架，用于构建高质量、可靠、可维护和可测试的数据管道。它具有内置的集成功能，可以产生所需的沿袭、一致的文件格式(如 delta)、版本、可观察性(遥测、监控、数据新鲜度)指标和更广泛的组织合规性。它是一个有效的低代码 ETL 框架，由中央平台团队为所有领域团队开发，以增加和创建数据开发一致性并加速网格。

![](img/f70858cb92c1894b304ea01787cb074f.png)

分散网格方法

***分散式方法*** 建议中央自助服务平台充当促进者和边界治理守护者。在中央自助控制平面上，域数据是可发现和可预订的。数据科学家能够从控制平面上的不同领域发现、订阅数据和功能(功能存储)。然后，DS 社区可以实验、训练、调整和注册 ML 值资产，他们可以将特性发布回特性存储，并实验和建模工件。这些都被版本化和维护。然后，它们可以作为组织资产在整个组织中广泛访问。DS 充当模型的控制和质量管理员。

然后，整个组织的领域 ML 工程师可以发现、访问和采用理想的任何 3 种(请求-响应、批处理、事件流)评分模式的模型。维护评分管道是领域 MLE 的工作。

领域数据工程师维护源数据集。维护模型质量是负责数据科学家的责任。类似地，维护 ML 评分管道是领域 ML 工程师的职责。他们都继续在自助服务平台上发布可观察性和服务健康指标，以实现完全透明、信任和可见性。DPF(数据发布工厂)ETL 框架及其内置的沿袭、可观察性和服务健康接口有助于高效集成和加速分散式开发。

参考我的系列来理解和建立更丰富的背景知识。

*   [分散领域数据工程第一部分](https://medium.com/@masterkeshav/decentralized-domain-data-engineering-part-i-c1ada63c2023)
*   [分散领域数据工程第二部分](https://medium.com/@masterkeshav/decentralized-domain-data-engineering-part-ii-842a8589250c)

![](img/074a295ee0ae009148ec38780ac3e798.png)

参考架构

在 2022 年**数据+人工智能峰会**上的这个演讲引起了我的共鸣，提升了我的推荐，我强烈推荐在这里听这个演讲:[https://www.youtube.com/watch?v=cZwjL0vps5k&t = 915s](https://www.youtube.com/watch?v=cZwjL0vps5k&t=915s)

本博客的目的是实际操作，针对中央平台团队，并为域团队建立一个集中式 MLFlow 服务器和注册中心，以在平台上建立 MLOPs 作为域采用的开放标准。这是一个非常实用的演示，一步一步地演示如何设置服务器，并通过演示 2 个领域团队利用这一点来评估**价值成果。**

**托管 MLFlow 的 Windows 服务器**

我们利用我们在这里设置[的 Windows VM，并利用它来设置我们的 MLFlow 服务器。](https://medium.com/@masterkeshav/serving-data-products-with-delta-sharing-protocol-33b1d4d051a0)

我们首先在这个虚拟机上安装来自【Python.org】Python 版本 Python 3 . 10 . 8 | T2 的最新 Python 版本。

确保安装还在 Windows 虚拟机上配置环境变量。

![](img/9c47aec33912f3ef48f31b08ee3e1c2c.png)

窗口虚拟机上的环境变量

![](img/6e6e160af005442db1080a3f34e37d41.png)

确认 Python 安装

![](img/611e6808d569af11894ea11c5cbe4545.png)

创建目录

并创建一个 Python 虚拟环境。

![](img/d712b63f0d7ba6a7beb5153dfcb69b0c.png)

在目录 environment_folder 上创建一个 Python 虚拟环境

**我们创建了一个虚拟环境，PIP 在我们的虚拟环境上安装了 ml flow**。

从[https://git-scm.com/download/win](https://git-scm.com/download/win)在 Windows 虚拟机上安装 Bash

以及来自[https://learn . Microsoft . com/en-us/CLI/Azure/install-Azure-CLI-windows 的 Azure CLI？tabs=azure-cli](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows?tabs=azure-cli)

一旦通过我们就可以确认。

![](img/3462e78cd092a9482c78c65caea89def.png)

登录 Azure 订阅

我们现在应该能够从 BASH 连接到您的 Azure 帐户。

登录 Azure 并创建 Azure Blob 存储帐户

![](img/9fcbcd286c30ec5ca30b7ee294fec78d.png)

Azure Blob 存储帐户

该帐户将作为 MLFLow 服务器的工件和指标的存储。

![](img/e6e4b219ab532ee98257b597b1a8469a.png)

创建一个容器

![](img/26cd07f44c96da8685f07f9d68be339b.png)

在 Blob 存储容器上创建目录

![](img/b944f53c7cf910cec063aed67b337bd7.png)

为后续步骤复制存储密钥

上 Bash，用配置好的 Azure Blob 存储连接 MLFlow，并启动 MLFlow 服务器。此时，服务器已经启动..

![](img/02a758e08323a0c6cff5a6c9080371d0.png)

MLFlow 服务器

可以通过 URL [确认本地的服务器 http://localhost:5000/#/experiments/0](http://localhost:5000/#/experiments/0)

![](img/234a451aea52fa9f67ea3289e2363e03.png)

MLFlow 服务器

此时，服务器还不能从外部完全访问。要启用它，我们需要在虚拟机上添加一个入站规则。

![](img/7290cbc272c5764aa2d4ab5efc67953d.png)

添加入站规则以允许入站连接

在 Windows 虚拟机上进一步打开端口 5000

![](img/1b82688912cb2ea56f075c38a222909b.png)

在 Windows 虚拟机上打开端口 5000

[http://<IP>:5000/#/experiments/0](http://20.10.175.162:5000/#/experiments/0)

在这一点上，它在外部被激活并发挥作用。

![](img/0f7b33ac4da714a37da1784b527ef589.png)

MLFlow 外部功能

**领域团队利用 MLFLow**

中央团队已经准备好服务器，现在我们将演示两个领域团队使用 Azure Synapse 和 Azure Databricks 来开发、注册他们的 ML 模型，并将其用于他们的 e2e mlop。

**第一个域用户:Azure Synapse Spark Pool**

领域团队使用 Azure Synapse，并在波士顿住房数据上开发了一个回归模型。它们引用 MLFLow URI 并在服务器上记录必要的模型度量、参数、工件。他们的开发之旅被大大简化和结构化了。数据工程师通过数据发布工厂帮助构建必要的管道，数据科学家对结果执行所需的分析和严格要求，ML 工程师进一步将该模型与来自该平台中央服务器的实时数据相集成，以便为组织启用和支持评分分析。这个过程是高度分散的。

![](img/332aa0f88bc9b804a2688432621ae6ae.png)

在 Azure Synapse 上开发和注册模型

![](img/e0d002e4547f4994ef7a6ac8c0d8bd1d.png)

在 MLFlow 服务器上注册的实验

![](img/76098cebc6b497601f6694ab464f77dd.png)

所有工件和度量。

![](img/667c44e0120a47e250be4c44139c60ff.png)

后端存储。

**第二域用户:Azure Databricks**

![](img/caf91ca84a8704ecde4f6e12cd5b93ed.png)

数据块回归

![](img/7906d72c74b7d1a422699c1a2460633a.png)

领域团队 ML

作为 Databricks 或 Azure Synapse 笔记本上的配置，请确保 Blob 的配置已设置，并且库依赖项已就位。

![](img/ab0d6870fad0d070f7bd4b0d7be83618.png)

域团队存储连接

![](img/ffc2ce66bd4249c9904e342b78a3a0ee.png)

度量和工件

![](img/c0d88e7b629c63a13b918bb95f0d434b.png)

文物储存

这两个领域团队都成功地拥有并利用了他们的 MLOPs 的 MLFlow，而平台团队也为此提供了便利。

MLFlow 模型的详细信息被完整记录以供参考 [MLflow 模型-ml flow 1 . 29 . 0 文档](https://www.mlflow.org/docs/latest/models.html)

**摘要**

*概括本博客中提出的观点——这是网状平台上 MLOPs 标准化的实际演示。虽然这只是这个想法的一个粗略的表述，但是它从根本上确立了这个概念和对它的需求。通过深思熟虑的工程和解决方案，可以进一步完善 MLFLow 订阅的整体体验，并将其包装在一个干净的场所中。这成为组织网状 MLOPs 转型的基础。让我们加入* [*系列的下一个*](https://medium.com/@masterkeshav/decentralized-mlops-on-mlflow-ab25199f7ba4) *！*