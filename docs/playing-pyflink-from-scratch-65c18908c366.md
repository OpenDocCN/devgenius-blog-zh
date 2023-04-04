# 从头开始玩 PyFlink

> 原文：<https://blog.devgenius.io/playing-pyflink-from-scratch-65c18908c366?source=collection_archive---------7----------------------->

## 了解 Kubernetes 上的 PyFlink 1.15.2

![](img/15fe318e42b0a1076955bc97a4c6169c.png)

照片由[汉斯约格·凯勒](https://unsplash.com/@kel_foto)在 [Unsplash](https://unsplash.com/photos/xy6r-F-w70c) 拍摄

最近在尝试玩 PyFlink，但是网上能找到的资料不多，大部分资料都有点过时了。因此，我将记录我是如何设置环境并实际使用 PyFlink 进行实验的。

开始之前，让我们设定实验目标。

1.  来运行 PyFlink 而不是 Flink，因为我不会用 JAVA 编码。
2.  要在 K8s 上运行，而不是 docker 或单机。
3.  使用 Flink 的最新版本 1.15.2。

我简单描述一下整个实验过程。

1.  创建一个 K8s 集群。
2.  部署 PyFlink 集群。
3.  从本地机器上传作业。
4.  在仪表板中检查作业状态。

现在，相信你们都明白实验过程了，那就开始准备前提条件吧。

*   不要使用 M1 芯片。[目前稳定的版本 1.15.2 不支持 M1 芯片](https://issues.apache.org/jira/browse/FLINK-25188)，所以在 M1 上安装 PyFlink 会很痛苦。
*   JAVA 11
*   python 3.6–3.8

一切准备就绪后，让我们开始吧！

# 创建 K8s 集群

我个人的偏好是用 [K3s](https://k3s.io/) 创建集群而不是 minikube。

有几个原因。

1.  更容易共享环境。我的实验环境在 EC2 上，所以知道位置的人都可以用。
2.  它更节省资源。minikube 对系统要求高，而 K3s 本身是为物联网设备设计的，相对来说资源效率较高。
3.  K3s 是普通 K8s，没有什么特别之处。

根据[官方文件](https://docs.k3s.io/quick-start)设置集群后，需要进行一些额外的设置。

创建一个命名空间。

*   `kubectl create ns flink`

创建服务帐户。

*   `kubectl create serviceaccount flink -n flink`

授予对服务帐户的授权。

*   `kubectl create clusterrolebinding flink-role-bind --clusterrole=edit --serviceaccount=flink:flink`

# 构建 PyFlink 集群

要构建 PyFlink 集群，我们需要准备容器映像来首先支持 PyFlink，如下所示。

[https://hub.docker.com/r/wirelessr/pyflink](https://hub.docker.com/r/wirelessr/pyflink)

构建容器的`Dockerfile`也可以在 Dockerhub 中获得。

接下来在本地下载 flink 神器的最新稳定版。

[https://www . Apache . org/dyn/closer . Lua/flink/flink-1 . 15 . 2/flink-1 . 15 . 2-bin-Scala _ 2.12 . tgz](https://www.apache.org/dyn/closer.lua/flink/flink-1.15.2/flink-1.15.2-bin-scala_2.12.tgz)

解压缩后，我们必须添加[一些需要的](https://stackoverflow.com/questions/68761409/jcapemkeyconverter-is-provided-by-bouncycastle-an-optional-dependency-to-use-s)但没有打包到其中的脂肪罐。

从以下网址下载 bcprov-jdk15on 和 bcpkix-jdk15on jar 文件

*   [https://mvn repository . com/artifact/org . bouncy castle/BC prov-JDK 15 on](https://mvnrepository.com/artifact/org.bouncycastle/bcprov-jdk15on)
*   [https://mvn repository . com/artifact/org . bouncy castle/bcp kix-JDK 15 on](https://mvnrepository.com/artifact/org.bouncycastle/bcpkix-jdk15on)

然后将它们移动到文件夹中

> *。/flink-1.15.2/lib*

一切准备就绪后，我们就可以在 K8s 上运行 PyFlink 集群了。

```
./bin/kubernetes-session.sh \
 -Dkubernetes.namespace=flink \
 -Dkubernetes.jobmanager.service-account=flink \
 -Dkubernetes.rest-service.exposed.type=NodePort \
 -Dkubernetes.cluster-id=flink-cluster \
 -Dkubernetes.jobmanager.cpu=0.2 \
 -Djobmanager.memory.process.size=1024m \
 -Dresourcemanager.taskmanager-timeout=3600000 \
 -Dkubernetes.taskmanager.cpu=0.2 \
 -Dtaskmanager.memory.process.size=1024m \
 -Dtaskmanager.numberOfTaskSlots=1 \
 -Dkubernetes.container.image=wirelessr/pyflink:1.15.2-scala_2.12-python_3.7
```

有两个关键点值得注意。

1.  `rest-service.exposed.type=NodePort`。为了允许用户访问 PyFlink 集群，并简化实验过程，我们选择使用`NodePort`作为外部接口。
2.  我们必须指定能够支持 PyFlink 的`container.image`，在本例中，就是前面提到的 [Dockerhub](https://hub.docker.com/r/wirelessr/pyflink) 。

通过使用`kubectl get svc -n flink`，我们可以知道仪表板(flink-cluster-rest)正在哪个端口上运行。这个仪表板位置也将是我们稍后要提交的作业的入口点。

# 提交工作

大多数 Flink 示例将使用`examples/batch/WordCount.jar`作为第一个 Hello World。

所以我们也可以用这个 Jar 文件来测试 Flink 是否能正确运行。

```
./bin/flink run \
  -m ${flink-cluster-rest}:${port} \
  examples/batch/WordCount.jar
```

提交后，我们可以在仪表板中查看结果，通常情况下，它会被成功执行。但是这个例子是基于 JAVA 的，所以我们将使用一个基于 Python 的例子来验证 PyFlink。

在开始之前，我们需要在本地安装 PyFlink 包。

> *pip3 安装 apache-flink==1.15.2*

如果我们没有使用 M1 芯片，这个命令应该安装成功，没有任何问题。

与 WordCount 示例相同，Python 在`examples/python/table/word_count.py`中也有相应的 WordCount 示例。

然而，Python 示例需要稍加修改才能正常工作。找到这个注释行:

> *除去。如果提交到远程集群，请等待*

按照说明移除上面的`.wait`，然后我们尝试提交 Python 作业。

```
./bin/flink run \
  -m ${flink-cluster-rest}:${port} \
  -py ./examples/python/table/word_count.py
```

返回控制面板应该会显示作业也在正常执行。

# 结论

关于 PyFLink 的资料很少，无论是环境安装还是编码参考，希望这篇文章对你有帮助。

然而，本文并没有详细解释 Flink 集群的细节以及如何开发 PyFlink，我将把这些细节留给以后的文章。