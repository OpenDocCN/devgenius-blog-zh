# 如何配置 Apache Spark (Databricks)将日志发送到 Azure Log Analytics

> 原文：<https://blog.devgenius.io/how-to-configure-apache-spark-databricks-to-send-logs-to-azure-log-analytics-865050ccc147?source=collection_archive---------1----------------------->

![](img/7f48c11262e4bd6c7e1f595fcf1c0522.png)

[brilliantprogrammer](http://www.brilliantprogrammer.com)

在本教程中，我们将配置 spark 集群，将日志发送到 Azure log analytics 工作区。如您所知，日志对于错误调试是必不可少的。

让我们直接进入设置:

在这里，我们使用[这个](https://github.com/mspnp/spark-monitoring)监控库。该库支持 Azure Databricks 10.x (Spark 3.2.x)及更早版本。

# 设置库的步骤:

## 步骤 1:克隆存储库[ [链接](https://github.com/mspnp/spark-monitoring.git)。

![](img/e6a1d4a2a24deade92036ada644c424e.png)

## 第二步:设置你的 Azure [数据块工作空间](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal)。

## 步骤 3:安装 Azure Databricks CLI 并设置身份验证。

*   运行 bash 命令。

> *数据块配置—令牌*

*   然后它会询问您的数据块主机—输入您的数据块 URL https://adb- <workspace-id>。 <random-number>.azuredatabricks.net</random-number></workspace-id>

之后，输入您的 Databricks 工作空间的令牌。如果你不知道如何设置令牌，那么请阅读本博客。

## 步骤 4:安装

*   **Java 开发套件(JDK)1.8 版**

**安装 Java:**

*   **Scala 语言 SDK 2.12**

*   阿帕奇 Maven 3.6.3
*   从[网站](https://maven.apache.org/download.cgi)下载 Maven。

![](img/0e736b8407329f14343a804936ea171b.png)

*   解压缩 tar.gz 文件

> *tar -xf {name}.tar.gz*

*   现在打开您解压文件的终端，并键入以下命令。

## **第五步:构建 Azure 数据块监控库**

**使用 docker**

*   打开终端并键入以下命令[首先将目录更改为/spark-monitoring]

![](img/21088ce51ee8dd77f7ac6a3a395c21c1.png)![](img/f72c22cdfc09dec966e386a5a05885b6.png)

## 步骤 6:配置数据块工作区

*   **创建目录**

> dbfs mkdirs dbfs:/databricks/spark-monitoring

*   打开/src/spark-listeners/scripts/spark-monitoring . sh 文件，添加您的日志分析工作区密钥和 id。

![](img/68dd9322768efa6adf5bc1491303d1c7.png)![](img/6bbbd3a7110dae4e072e1a4511433847.png)

出于安全原因，您也可以在这里使用 azure key vault

*   现在输入您的 AZ_SUBSCRIPTION_ID，AZ _ RSRC _ 团体 _ 名称，AZ _ RSRC _ PROV _ 名称空间，AZ _ RSRC _ 类型，AZ _ RSRC _ 名称

**设置日志分析工作区**

*   打开日志分析工作区

![](img/2a16df3b72d3c87f3920ddccee6a1ad9.png)

*   点击创建

![](img/fae5856d324cbcaba5084eb1e04c928d.png)

*   写下实例和资源组的名称，然后单击“查看和创建”

![](img/d19dbc2c98a90243bef0e42e373b5cee.png)

*   只需在 vault 中定义您的日志分析工作区机密。

![](img/fedc41e4848900b6286f4aa6c9dc9974.png)

*   现在，在您的数据块笔记本中创建范围
*   在数据块 url 的末尾追加机密/创建范围

![](img/402b0825f2104dc9087a780496d5a2e0.png)

*   输入作用域名称，现在从 azure key vault 获取 azure key vault DNS 名称和密钥资源 ID

访问您存储日志分析工作区机密的 azure vault，复制 vault URI 并将其粘贴到资源 ID 的 DNS 名称和资源 ID 中。

![](img/a48087579926fb0c8dc60bcec0d13543.png)

*   现在，点击创建

![](img/557302a8bc1a39ad4d0f3495106eec2c.png)

## 步骤 7:现在，将 src/spark-listeners/scripts/spark-monitoring . sh 复制到步骤 6 中创建的目录中，并将所有 jar 文件复制到 src/target 中

## 步骤 8:现在在 databricks 工作区中创建集群并打开高级选项

*   首先选择您的首选集群

![](img/eb170815c7cc1652424eb1bf34a1ff03.png)

*   现在打开高级选项，像这样再添加两个环境变量

格式:{ {机密/{您的作用域名}/{您在密钥库中的机密名称}}}

![](img/9c95f3985c1c9d80b858994855a09db3.png)

*   此外，将该脚本添加到您的 Init 脚本中，如下所示

![](img/b270752d52fdb8b591711e15d2eb5607.png)

现在点击创建集群

**通过向 azure logs analytics 发送日志进行测试**

*   在 azure databricks 中创建新笔记本

![](img/c571ef8cb50556067134e4396dfdc009.png)

运行两个单元

*   访问 Azure 日志分析工作区现在访问日志部分

![](img/388d05930d0ce077d817d69324e036b1.png)

*   现在，您会看到自定义日志[如果您没有看到，请重新启动您的集群并等待一段时间，否则您会错过教程的某些部分]

![](img/0dd243cc4daaa9d154b51107b25e48f1.png)

*   写一个查询并点击运行，你可以看到你的日志

![](img/ba0d3fd3ecee26ad65ff7815e2ff4a68.png)

恭喜你，你已经成功地配置了我们的 spark 集群来将日志发送到 Azure log analytics 工作区。如果你想要更多这样的数据工程的东西。

> 请跟我来，也为这篇文章鼓掌。