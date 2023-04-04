# 弹性搜索 8.2.3 + Kibana +企业搜索—开发者环境设置说明(仅限 Docker)

> 原文：<https://blog.devgenius.io/elastic-search-8-2-3-kibana-enterprise-search-developer-env-setup-notes-docker-only-6c39e7520f80?source=collection_archive---------4----------------------->

![](img/acabd1105b1c4ad8b77e504dd0cef485.png)

哦，天哪…每当我试图设置一个新的弹性堆栈实例时，我真的会感到头疼(在我的开发机器和生产环境中的虚拟机上都是如此)。Elastic 的每个新版本都带来了很棒的新功能和值得一试的东西，但是设置它来运行是一种可怕的体验。这主要是因为 Elastic Co 的人似乎已经放弃了良好的开发人员体验的概念，并加倍努力提供付费云服务。我不能责怪他们，他们需要赚钱做生意**但是**我认为这可能太过分了。无论如何，足够的闲扯让我们进入我在你的开发者机器上运行一些弹性 8.2.3 栈的新鲜笔记。

**注意**:弹性栈服务的安装方式有很多种；这篇文章都是 Docker。这里我将重点介绍三大搜索引擎——弹性搜索、Kibana 和企业搜索。你应该能够了解正在发生的事情，并应用到 Logstash，Beats 等。

# 第 1 部分:获取图像

访问:[https://www.docker.elastic.co/](https://www.docker.elastic.co/)，你会看到所有你可以下载的 docker 图片。如果你在 M1 建筑上(像我一样),手臂图像就是你的朋友。

```
docker **pull** docker.elastic.co/elasticsearch/elasticsearch:8.2.3-arm64docker **pull** docker.elastic.co/kibana/kibana:8.2.3-arm64docker **pull** docker.elastic.co/enterprise-search/enterprise-search:8.2.3-arm64
```

**注意**:如果没有特定于架构的标签，您可以很好地提取图像，但是在某些情况下，如果系统 arch 需要使用仿真层来运行，则可能会导致性能下降。

# 第 2 部分:Docker 合成& Env 文件

为自己创建一个工作区目录。我喜欢在我的主路径之外有一个名为“开发”的目录。(本文的工作区目录是:~/Development/experimental/es-dev)

创建一个名为 docker-compose.yml 的文件和另一个名为。env 在您设置的目录中。创建完成后，编辑 docker-compose.yml，并将下面要点的内容放入其中。

**注意**:以下示例中有几个值需要替换。查找被修订的单词，并用您自己的值替换指示的值。

接下来，编辑。env 文件，并将以下内容放入其中:

最后，创建一个名为 kibana.yml 的文件，并将以下内容放入其中:

kibana.yml 的完整路径放在 kibana 部分的 docker-compose.yml 中。因此，如果这个文件的路径是“/ usr/kuccello/Development/experimental/es-dev/ki Bana . yml ”,那么这个完整路径就是您在 docker-compose 中为这一行输入的内容:

```
- <REDACTED: put your path to your kibanan config file here>kibana.yml:/usr/share/kibana/config/kibana.yml
```

变成了:

```
- /usr/kuccello/Development/experimental/es-dev/kibana.yml:/usr/share/kibana/config/kibana.yml
```

# 第 3 部分:启动它

猜猜看，您已经准备好启动您的本地集群了:

```
docker compose up -d
```

然后访问: [http://localhost:5601/](http://localhost:5601/) 并使用用户名“elastic”和您在。环境文件。

# 结束语

使用 Elastic stack 可以配置很多东西——企业搜索集成的设置来自于查看企业搜索的默认配置文件(我下载了 tar.gz 并提取了文件，然后还引用了:[https://www . Elastic . co/guide/en/enterprise-search/current/configuration . html](https://www.elastic.co/guide/en/enterprise-search/current/configuration.html)

本文只是一个 notes dump，用于设置一个开发友好的集群。为了确保生产安全，你需要做很多事情，你应该阅读 Elastic 网站上的文档。将来，我可能会带着一些生产设置知识重温这篇文章。现在，我希望这能在某种程度上帮助你让你的本地开发设置处于良好的工作状态。

# **最后一个音符**

如果你有更好的想法或建议，请告诉我。我分享是为了学习和帮助。拍手也不会出错——请考虑让我知道你是否觉得留下拍手声有帮助。