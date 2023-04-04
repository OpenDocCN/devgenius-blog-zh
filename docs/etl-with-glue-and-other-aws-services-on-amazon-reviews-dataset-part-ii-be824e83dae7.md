# 在[Amazon reviews dataset]上使用 Glue 和其他 AWS 服务的 ETL 第二部分

> 原文：<https://blog.devgenius.io/etl-with-glue-and-other-aws-services-on-amazon-reviews-dataset-part-ii-be824e83dae7?source=collection_archive---------13----------------------->

# 在本文中，我们将继续这个 ETL 实验的第 2 部分。

源代码

【https://github.com/ramthulsi/glue-etl 

# 04-分析:

## a.雅典娜:

# 在本实验中，您将完成以下任务:

*   使用 Amazon Athena 查询数据并创建视图
*   Athena 工作组控制查询访问和成本
*   使用 Amazon QuickSight 构建仪表板

注意:本实验部分将演示 Athena 如何拥有 Glue 的数据存储。如何使用 Athena 对 Glue 数据存储进行按需查询？创建视图并进行分析，用简单的示例查询进行解释。您可以根据自己的需求和数据执行复杂的查询。

# 使用 Amazon Athena 查询数据

## 在 AWS 服务控制台中，搜索 Athena。

![](img/fcff51467fb23f23d55515ada826bd54.png)

*   在查询编辑器中，选择从 Glue Catalog 创建的数据库`glue-etl`。
*   单击名为`analysis`的表格来检查字段。注意:字段 id、sporting_event_id 和 ticketholder_id 的类型应为(double)。

![](img/df9af36cf56e209f75a5d4e6462cdc1b.png)

# 将查询结果记录到 S3

![](img/8659acaaaa749121ffbecdfeee49aaba.png)

# 基于投票的前 10 本书

*   运行以下查询，根据投票结果获得前 10 本书

select product_title，total _ votes from analysis order by total _ votes desc 限制 10；

![](img/dbf6f4d2852f13725a4cc841dba37ff4.png)

# 创建具有最高评价的视图

*   运行以下查询以获得星级> =4 的最佳评级书

select * from 分析，其中星级> = 4；

![](img/d0d7e884a57b93b5a98daff27f5758b7.png)

如上所示，单击创建，然后选择从查询创建视图

将视图命名为`toprated-books`,然后单击 Create

*   查看在左窗格中创建的视图

![](img/1a1df8a910bb55d3bbf6a394d1b73c5c.png)

*   请随时探索雅典娜按需查询

# 05-SageMaker:

# 介绍

*   Amazon SageMaker 是一个端到端的机器学习平台，允许您在 AWS 中构建、训练和部署机器学习模型。这是一个高度模块化的服务，允许您独立使用这些组件。

# 您将了解到:

*   如何使用 Sagemaker 的 Jupyter notebook 组件集成数据湖，使用 Athena 填充数据框进行数据操作。

# 创建 Amazon SageMaker 笔记本实例

*   从 AWS 控制台转到 Amazon Sagemaker

![](img/58c3a30821525f44b54bdffa3320360d.png)

在 Amazon SageMaker 导航窗格中，单击笔记本实例，然后单击创建笔记本实例

![](img/73390cca048a32b94290ff7ace60cd78.png)

# 输入以下值以创建实例:

*   给出您选择的笔记本实例名称，例如`glue-etl-notebook`
*   对于本实验，您可以将笔记本实例类型保留为默认值 ml.t2.medium
*   将弹性推理保留为空。这是为了增加额外的资源。
*   为 Amazon SageMaker 中的笔记本实例选择一个角色，以便与 Amazon S3 进行交互。由于角色不存在，请选择“创建新角色”选项。
*   在“创建 IAM 职责”弹出窗口中，选择您将授予访问权限的特定 S3 时段。在本实验中，选择如下所示的任意 S3 时段，然后单击“创建角色”:

![](img/e26131c47da64b6f5d63753aff4c2b76.png)

*   单击创建笔记本实例。
*   您将看到一个角色被创建，因为您将从 SageMaker 访问 Athena，所以 SageMaker 执行角色必须具有访问 Athena 的必要权限。为此，请在新窗口中单击新创建的 IAM 角色的链接

![](img/5db7f22b9b4017a37f3bf69a884bc68a.png)

*   您的 SageMaker 执行角色中没有可用的 Athena 权限。本例中为“AmazonSageMaker-execution role-20210927t 014192”。

![](img/d11637227bc61f45e0b20255360467f4.png)

单击权限选项卡中的附加策略按钮。

*   通过“Athena”过滤策略，选中 AmazonAthenaFullAccess 托管策略，然后单击屏幕底部的附加策略按钮。关闭新窗口/选项卡。

![](img/6d2a36a907f274b1c157716ef39d9784.png)![](img/f148a8b0b87b0e22844709161029b12b.png)

*   等待笔记本实例被创建，状态变为 Inservice，然后在“Actions”列中单击 Open Jupyter。

![](img/8bc08161d931f30c23808c2e8095a941.png)

笔记本界面在单独的浏览器窗口中打开。

![](img/3f573c28f57cdb3831c98e5e50516e9a.png)

# 将 SageMaker Jupyter 笔记本连接到 Athena

在 Jupyter 笔记本界面中，单击新建。对于内核，选择 conda_python3

![](img/bc2c67eec9368a36c327288138d6b058.png)

*   在笔记本中，执行以下命令来安装雅典娜 JDBC 驱动程序，并在顶部工具栏中，单击运行。(PyAthena 是亚马逊雅典娜 JDBC 驱动程序的 Python DB API 2.0 (PEP 249)兼容客户端。)

```
import sys
!{sys.executable} -m pip install PyAthena
```

![](img/cfe2bf91eda272cdcfca7ad49c9c9433.png)

您可以将 Athena 表数据从数据湖加载到 Pandas 数据框，并应用机器学习。

您的 Athena 数据库名称，这是您在之前的实验中创建的 Glue 数据库名称，例如`glue-etl`。更新 S3 桶的位置，使其看起来像`s3://amazon-dataset-reviews-thulasi/athenaquery/`。

```
from pyathena import connect
import pandas as pd
conn = connect(s3_staging_dir='s3://amazon-dataset-reviews-thulasi/athenaquery/',
region_name='us-west-2')

df = pd.read_sql('SELECT * FROM "glue-etl"."analysis" order by total_votes limit 10;', conn)
df
```

![](img/253748c6e5364f2142f785bc77f6f91d.png)

# 你也可以用 matplotlib 做一些分析和绘图

```
df = pd.read_sql('SELECT total_votes, \
       review_date, \
       count(*) as ratings, \
       avg(star_rating) as avg_star_rating \
       FROM "glue-etl"."analysis" \
       group by 1,2 \
       order by 1,2 limit 500;', conn)
df
```

![](img/591b48995b8249fbc233b3a284098abb.png)

```
import matplotlib.pyplot as plt 
df.plot(x='review_date',y='avg_star_raging')
```

![](img/e83ef470c82b9343f3027d05f6e398e1.png)

# 清理:

*   转到 SageMaker 笔记本实例控制台
*   选择您创建的笔记本实例，例如`glue-etl-notebook`
*   选择操作→停止，笔记本实例将需要几分钟时间停止。
*   笔记本实例停止后，选择操作→删除，然后在确认弹出窗口中选择删除以删除该实例。

# 06-数据 Brew:

# 数据布鲁实验室

*   在本实验中，我们将使用 AWS Glue DataBrew 探索 S3 的数据集，并清理和准备数据。
*   为此，我们将首先设置一个在 DataBrew 中使用的 IAM 角色，并为 DataBrew 作业的结果设置一个 S3 存储桶。

CloudFormation Stack Deployment 在您的 S3 桶所在的区域部署以下模板。

```
File Location:

cloudformation/databrewlab.yaml

Enter Parameters: (Below are mine, please enter yours accordingly)

SourceBucket: "amazon-dataset-reviews-thulasi"
SourceKey: "analysis/amazon_reviews_us_Books_v1_02.tsv.gz" 
```

![](img/44c8fc9454444dedb25fc4ca18076183.png)![](img/91742bdd24cd80b6e97923ce2c5f84ef.png)

*   选中“我承认…”复选框，然后单击“创建堆栈”来创建堆栈
*   部署堆栈后，单击 Outputs 选项卡查看更多信息
*   记下将在实验中使用的 DatasetS3Path、DataBrewLabRole 和 DataBrewOutputS3Bucket 的值

# 创建项目

## 导航到 AWS Glue DataBrew 服务

![](img/f694f5bb991c76fb9369bd6bdf0b3e0c.png)

*   在 DataBrew 控制台上，选择项目

![](img/2339dd61f879ac8ecf8e8487f84fee36.png)

*   单击创建项目
*   在项目详细信息部分，输入`glue-etl-databrew`作为项目名称

在选择数据集部分，选择新数据集并输入`glue-etl-databrew-amazonbooksreview`

![](img/496456c0ad04e5c193688c059a633023.png)

在“连接到新数据集”部分，选择“数据湖/数据存储”下的亚马逊 S3

输入 DatasetS3Path，该路径可在事件引擎团队仪表盘或 CloudFormation 堆栈的输出部分获得

![](img/886c4dd293d5ef29eac051297ecac9fc.png)![](img/b84061711fe6ccd26956cf25d2c479e5.png)

在采样部分，保留默认配置值

![](img/02d6d47027dde40d88d8d2de21a56b7e.png)

在“权限”部分，从下拉列表中选择角色 data brew-lab-DataBrewLabRole-xxxxx

![](img/e35e87ffde1981cfdb6784a5a3af1d7b.png)

单击创建项目粘附数据 Brew 将创建项目，这可能需要几分钟时间。

![](img/6a7f0a62f7ac3a2c5f76339ac085c178.png)

# 浏览数据集。

*   创建项目后，您将看到网格视图。这是默认视图，其中数据样本以表格格式显示。

![](img/69fad834418634c2b5b196c55767d21b.png)

## 网格视图显示

*   数据集中的列
*   每列的数据类型
*   已找到的值范围的摘要
*   数字列的统计分布

单击模式选项卡

架构视图显示从数据集推断出的架构。在“架构”视图中，可以看到每一列中数据值的统计信息。

## 在模式视图中，您可以

*   选中列旁边的复选框以查看列值的统计信息摘要
*   显示/隐藏列
*   重命名列
*   更改列的数据类型
*   通过拖放列来重新排列列顺序

单击配置文件选项卡

在配置文件视图中，您可以运行数据配置文件作业来检查和收集有关数据的统计摘要。数据概要是对结构、内容、关系和派生的评估。

点击运行数据配置文件

在“作业详细信息”和“作业运行示例”面板中，保留默认值。

![](img/5ad6fbbafd36e4521c420e135b4110a5.png)

*   在作业输出设置部分，选择名为 data brew-lab-databrewoutputs 3 bucket-xxxxx 的 S3 存储桶和一个文件夹名称(如 data-profile)

![](img/84c8ed588378a513f4ececd64fb5f062.png)

*   在“权限”部分，选择名为 data brew-lab-data brew lab role-xxxxx 的 IAM 角色
*   将所有其他设置保留为默认值
*   单击创建并运行作业
*   数据配置文件作业大约需要 5 分钟完成。您可以在等待时继续下面步骤 15 的其余实验，然后返回到以下步骤来检查数据集的配置文件。

## 从 DataBrew 控制台左侧的菜单中单击 Jobs。

*   单击配置文件作业选项卡查看配置文件作业列表。
*   您可以在此屏幕上看到您的个人资料作业的状态。

![](img/696417e3f8af1bbfaf96224b63d72b3c.png)

*   配置文件作业成功完成后，单击查看数据配置文件

![](img/efe0e97db371f1dbbfb32196727538a9.png)

*   您也可以从项目中的“概要文件”选项卡访问数据概要文件。
*   数据配置文件显示了数据集中的行和列的摘要、有效的列和行的数量以及列之间的相关性。

## 单击“列统计”选项卡，查看数据值的逐列细分。

![](img/cc023bdb496f931ef759f7cf2f12579d.png)

# 准备数据集

在本节中，我们将对数据集应用以下转换。

*   将 review_date 列的日期格式从 YYYY-MM-dd 转换为 month*ddd，*yyyy
*   将 review_date 列拆分为三个新列(年、月、日),以便按这些列对数据进行分区

单击 review_date 列名称旁边的#图标

![](img/bdc63818a7e6e9f9201d0498ce62ae09.png)

*   选择日期时间格式，选择格式`month*ddd, *yyyy`并应用。

![](img/d980c6373833d6cc6c53eb45a56e19fe.png)

*   更多转换
*   我们将 review_date 细分为三列，即年、日、月

![](img/103965f7fcc7338ef1a9ce0801eb5ac3.png)

*   应用后

![](img/d89e363d7538bc27caf9befaeadfcda2.png)

*   请随意探索 Databrew 中的选项，让您的双手变脏。使用这个神奇的工具可以做更多的事情。

在“作业详细信息”和“作业运行示例”面板中，保留默认值。