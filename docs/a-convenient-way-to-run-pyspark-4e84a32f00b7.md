# 运行 PySpark 的便捷方式

> 原文：<https://blog.devgenius.io/a-convenient-way-to-run-pyspark-4e84a32f00b7?source=collection_archive---------7----------------------->

使用 Jupyter Notebook 在本地计算机上运行 Pyspark，同时连接到 Linux 托管的 Apache Spark 独立集群

![](img/9cc85598d80d947dd94f7a5453a789dc.png)

照片来自 [Unsplash](https://unsplash.com/) 上的 [Andrew Ruiz](https://unsplash.com/@andrewruiz)

在本指南中，我将向您展示如何跨 Linux 虚拟机设置 Apache Spark 独立集群。然后，我将向您展示如何在您的本地计算机上将该集群连接到 Jupyter 笔记本 PySpark 会话。

这个示例假设您已经在 Linux 虚拟机上下载并安装了 Apache Spark 3.1.2。此外，它还假设您的 Linux 虚拟机和本地计算机上都有 Jupyter Notebook、Python 3 和任何其他依赖项。

**建立独立集群**

1.  在 Linux 虚拟机的命令提示符下，转到%SPARK_HOME%\bin 文件夹，并运行:

```
spark-class org.apache.spark.deploy.master.Master 
```

这段代码的结果将启动主节点，并给出一个“spark://ip:port”形式的 URL。

2.在另一个终端窗口中，最好是一个 [tmux](https://github.com/tmux/tmux/wiki) 窗口，转到%SPARK_HOME%\bin 文件夹并运行:

```
 spark-class org.apache.spark.deploy.worker.Worker spark://ip:port
```

这段代码将设置 worker 节点并将其连接到主节点。您可以对想要连接到集群的任何其他工作执行相同的步骤。确保您使用在步骤 2 中获得的 URL。

注意:我喜欢在单独的 [tmux](https://github.com/tmux/tmux/wiki) 窗口中执行步骤 2 和 3。这样，您可以从 VM 和终端窗口中分离出来，并且您的 Spark 集群将保持在线。否则，只要与虚拟机的连接关闭，您就必须重新启动集群。

**设置 Jupyter 笔记本**

1.  在 Linux 虚拟机上的另一个 tmux 窗口或终端窗口中运行:

```
jupyter notebook --no-browser 
```

您应该可以看到任何启动的 Jupyter 笔记本会话的标准输出，只是不会出现浏览器。

**SSH 隧道到 Jupyter 笔记本**

现在让我们将您的本地计算机连接到正在运行的 Jupyter 笔记本会话。

1.  在本地笔记本电脑上，打开 tmux/终端窗口并运行:

```
ssh -N -L 8888:localhost:8888 username@hostname
#the username and hostname are your username to the Linux VM and the hostname of the Linux VM. 
```

接下来它会要求您输入密码。一旦输入，如果输入正确，你不会得到回应。如果使用 tmux，保持此窗口运行或分离。

2.接下来，打开互联网浏览器并运行:

```
http://localhost:8888
```

这应该能调出 Jupyter 笔记本会话。如果这是您第一次以这种方式连接，它会要求您输入令牌或密码。您可以从启动 Jupyter 笔记本会话的终端窗口的输出中获得令牌。一旦您登录，只要您在每个 Jupyter 笔记本会话中使用相同的 Linux VM 主机，它就不会再次要求您提供令牌。

如果“http://localhost:8888”不起作用，请咨询您的 IT 团队，让 8888 端口允许 web 流量通过防火墙。

这种方法的一个好处是，在 Jupyter Notebook 会话中，您还应该能够看到 Linux VM 上可用的文件。对于 Linux VM 上的文件结构的 GUI 视图来说，这是一个方便的端口。

将 Jupyter 笔记本连接到 Spark 集群。

最后，让我们连接到正在运行的 Spark 集群。

1.在 Jupyter Notebook 中打开 Python3 内核并运行:

```
import pyspark
import findspark
from pyspark import SparkConf, SparkContext
from pyspark.sql import SQLContext
from pyspark.sql.types import *
from pyspark import SparkConf, SparkContextfindspark.init('/path_to_spark/spark-3.1.2-bin-hadoop3.2') 
#The key here is putting the path to the spark download on your Linux VM.sc=pyspark.SparkContext(master='spark://ip:port',appName='test')
#Use the same 'spark://ip:port' from starting the master node and connecting the worker(s). 
spark = SQLContext(sc)
spark
```

太好了！您应该连接到您的 Linux 虚拟机上运行的 Apache Spark 独立集群。方便的是，现在可以在 Jupyter Notebook 会话中使用 PySpark 编码，类似于任何其他 Python 会话。更好的是，它现在使用你运行的独立集群，有希望比你的本地计算机拥有更多的内存和 CPU。