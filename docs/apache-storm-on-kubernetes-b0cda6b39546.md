# 库伯内特岛上的阿帕奇风暴

> 原文：<https://blog.devgenius.io/apache-storm-on-kubernetes-b0cda6b39546?source=collection_archive---------5----------------------->

如今，当您谈到开源流处理技术时，大多是 Apache Spark、Apache Flink 或 Apache Beam。然而，在过去，有一种叫做 Apache Storm 的技术，许多企业今天仍在使用这种技术，你不能一下子把它们全部转换过来。这项技术是遗留的，似乎主要的云提供商没有很好地提供支持(Azure HDInsight 3.6 [storm](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-36-component-versioning) (仅 1.1 版本)，在 [HDInsight 4.0](https://docs.microsoft.com/en-us/azure/hdinsight/storm/migrate-storm-to-spark) 、[甚至没有提到 AWS EMR 上的 Storm](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-hadoop.html) 和 GCP [Dataproc](https://cloud.google.com/dataproc/docs/concepts/configuring-clusters/cluster-properties) 上的 Apache storm。)而 Kubernetes 上的[部署](https://github.com/kubernetes/examples/blob/master/staging/storm/README.md)是 2018 年才更新的。

那么开发人员需要如何在 Apache Storm 上快速入门学习环境呢？我将在这篇文章中展示我的发现。

# 阿帕奇风暴

[](https://www.simplilearn.com/tutorials/big-data-tutorial/apache-strom) [## Apache Storm:数据模型、架构和组件

### Apache Storm 是一个实时流处理系统，在这个 Apache Storm 教程中，您将了解到它的全部内容…

www.simplilearn.com](https://www.simplilearn.com/tutorials/big-data-tutorial/apache-strom) [](https://docs.microsoft.com/en-us/azure/hdinsight/storm/apache-storm-overview) [## 什么是 Apache Storm - Azure HDInsight

### Apache Storm 允许您实时处理数据流。Azure HDInsight 允许您轻松创建 Storm…

docs.microsoft.com](https://docs.microsoft.com/en-us/azure/hdinsight/storm/apache-storm-overview) 

# 成分

Apache Storm 中的主要组件有:

光轮:主节点

主管:员工/执行者节点

动物园管理员:Nimbus 和管理员之间的所有协调都是通过动物园管理员集群完成的

UI:像 Spark UI 一样，您可以看到拓扑、节点

拓扑:拓扑是计算的图形(像 [Spark DAG](https://data-flair.training/blogs/dag-in-apache-spark/) )

# 部署风暴

有一个非官方的阿帕奇风暴舵图。我选择 OpenShift (RedHat 企业版 Kubernetes)部署 Storm。

掌舵图不是为 OpenShift 构建的，所以容器映像使用的是 root 用户。因为默认情况下，OpenShift 不允许任何 serviceaccount 运行特权容器，所以我们必须[禁用它](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html)。

```
oc adm policy add-scc-to-user privileged system:serviceaccount:mystorm:default
```

## 安装舵图

由于一些优秀的人已经花时间有一个掌舵图表，部署是容易的。

```
helm repo add gresearch [https://g-research.github.io/charts](https://g-research.github.io/charts)
helm install my-storm gresearch/storm -n mystorm
```

# 运行基本风暴拓扑

我们将运行一些示例风暴拓扑。我们需要先编译编写 Storm 拓扑的 Java 程序。

## 安装 Java

[](https://developers.redhat.com/blog/2018/12/10/install-java-rhel8) [## 如何在红帽企业版 Linux 8 |红帽开发者上安装 Java 8 和 11

### Red Hat Enterprise Linux (RHEL) 8 将支持 Java 的两个主要版本:Java 8 和 Java 11。在这个…

developers.redhat.com](https://developers.redhat.com/blog/2018/12/10/install-java-rhel8) 

```
sudo yum install java-11-openjdk-devel
```

## 安装 Maven

 [## 安装 Apache Maven

### Apache Maven 的安装是一个简单的过程，提取归档文件并添加带有 mvn 的 bin 文件夹…

maven.apache.org](https://maven.apache.org/install.html) 

设置 Maven 环境

```
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk
export PATH=/tmp/apache-maven-3.8.5/bin:$PATH
```

默认情况下，最新版本 Maven 禁用外部 http repo。

[](https://stackoverflow.com/questions/67001968/how-to-disable-maven-blocking-external-http-repositories) [## 如何禁用 maven 阻塞外部 HTTP 仓库？

### 我找到了一个解决方案，通过检查 Maven git 存储库中的提交，它负责默认的…

stackoverflow.com](https://stackoverflow.com/questions/67001968/how-to-disable-maven-blocking-external-http-repositories) 

解决方法是

```
<mirror>
 <id>maven-default-http-blocker</id>
 <mirrorOf>external:http:*</mirrorOf>
 <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
 <url>[http://0.0.0.0/](http://0.0.0.0/)</url>
</mirror>
Comment out above line
```

## 在本地运行 Storm 拓扑

```
wget [https://dlcdn.apache.org/storm/apache-storm-2.3.0/apache-storm-2.3.0.tar.gz](https://dlcdn.apache.org/storm/apache-storm-2.3.0/apache-storm-2.3.0.tar.gz)# compile
mvn clean install -DskipTests=true# local run
cd examples/storm-starter/
# Example 1: Run the ExclamationTopology in local mode (LocalCluster)
~/apache-storm-2.3.0/bin/storm jar target/storm-starter-*.jar org.apache.storm.starter.WordCountTopology -local
```

## 在远程集群中运行拓扑

事实证明，您需要连接到 Storm nimbus 主机才能远程提交拓扑。因此，我只需将拓扑远程复制到 apache storm cluster supervisor 节点，以连接到 nimbus 主机。

```
# copy to supervisor node
oc rsync ./target pod/my-storm-supervisor-754f6b4dbf-5qj74:/tmp -c supervisor# ssh to supervisor node
oc rsh -c supervisor my-storm-supervisor-754f6b4dbf-5qj74
storm jar target/storm-starter-*.jar
```

## 奔跑

```
# Example 2: Run the RollingTopWords in remote/cluster mode
org.apache.storm.starter.WordCountTopology WordCount -c nimbus.host=my-storm-nimbus
```

## 删除拓扑

```
storm kill WordCount -c nimbus.host=my-storm-nimbus
```

Apache Storm 始终支持多层拓扑，例如 Python。下面是一个例子:

[](https://github.com/apache/storm/blob/v2.4.0/examples/storm-starter/src/jvm/org/apache/storm/starter/WordCountTopology.java) [## 2.4.0 版的 storm/word count topology . Java Apache/storm

### 此文件包含双向 Unicode 文本，其解释或编译可能与下面显示的不同…

github.com](https://github.com/apache/storm/blob/v2.4.0/examples/storm-starter/src/jvm/org/apache/storm/starter/WordCountTopology.java) 

主要代码是使用 python 来运行 python 拓扑

```
public static class SplitSentence extends ShellBolt implements IRichBolt {public SplitSentence() {
            super("python", "splitsentence.py");
        }
```

示例 Python 拓扑

[](https://github.com/apache/storm/tree/v2.4.0/examples/storm-starter/multilang/resources) [## storm/examples/storm-starter/multilang/resources at v 2 . 4 . 0 Apache/storm

### 阿帕奇风暴之镜。在 GitHub 上创建一个帐户，为 apache/storm 开发做贡献。

github.com](https://github.com/apache/storm/tree/v2.4.0/examples/storm-starter/multilang/resources) 

```
import stormclass SplitSentenceBolt(storm.BasicBolt):
    def process(self, tup):
        words = tup.values[0].split(" ")
        for word in words:
          storm.emit([word])SplitSentenceBolt().run()
```

如果想在 Azure Kubernetes 服务上部署 Apache Storm，可以参考我之前的文章[置备 AKS](/increased-developer-productivity-with-kubernetes-virtual-cluster-7e478870d7aa) ，然后部署 helm chart。

# 附录

[](https://storm.apache.org/releases/current/Tutorial.html) [## 辅导的

### 在本教程中，您将学习如何创建 Storm 拓扑并将它们部署到 Storm 集群。Java 将是主要的…

storm.apache.org](https://storm.apache.org/releases/current/Tutorial.html)  [## 常见的拓扑模式

### 本页列出了风暴拓扑中的各种常见模式。批处理基础螺栓内存缓存+字段…

storm.apache.org](https://storm.apache.org/releases/2.4.0/Common-patterns.html)  [## 暴风-官方图片|码头中心

### Apache Storm 是一个免费的开源分布式实时计算系统。

hub.docker.com](https://hub.docker.com/_/storm) 

https://livebook.manning.com/book/storm-applied