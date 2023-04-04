# 弹性搜索:摄取管道

> 原文：<https://blog.devgenius.io/elasticsearch-ingest-pipelines-6335d349a0a?source=collection_archive---------3----------------------->

![](img/89705072cd0531af4cf52367fd402b79.png)

照片由 [JJ 英](https://unsplash.com/@jjying?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/pipelines?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

什么是好人？很高兴在这个系列中再次见到你😁！今天我们来看看**摄取管道。**

摄取管道允许我们应用转换，例如*删除字段*、*提取信息*，甚至*在索引文档之前丰富我们的数据*。

![](img/059ad292991ba367aa7e95b7fbba776b.png)

如果我们有一个文档，并且我们想要提取有意义的信息，我们通过摄取管道运行该文档，并且应用不同的处理器，例如*剖析处理器*来提取时间戳，或者*移除处理器*来删除字段，等等。

转换完成后，生成的文档将被索引。一个流水线由几个可配置的任务组成，这些任务被称为[处理器](https://www.elastic.co/guide/en/elasticsearch/reference/current/processors.html)。随着每个处理器相继运行，传入的文档会进行某些修改。Elasticsearch 包括几个可配置的处理器。
我们可以使用节点信息 API 来获取可用处理器的列表。

```
GET _nodes/ingest?filter_path=nodes.*.ingest.processors
```

使用摄取 API 或 Kibana 的摄取管道功能，我们可以构建和管理摄取管道。管道由 Elasticsearch 存储在[集群状态](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-state.html)中，这是一种内部数据结构，记录了每个节点所需的各种数据。

让我们使用[摄取 API](https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest-apis.html)来创建我们的第一个管道。

```
PUT _ingest/pipeline/my-first-pipeline
{
  "version": 1,
  "description": "My very first pipeline",
  "processors": [
    {
      "set": {
        "description": "Set default value of balance",
        "field": "balance",
        "value": 0
      }
    },
    {
      "set": {
        "description": "Set default value of status field",
        "field": "status",
        "value": true
      }
    },
    {
      "lowercase": {
        "field": "name"
      }
    },
    {
      "remove": {
        "field": "unnecessary field"
      }
    }
  ]
}
```

我们知道，当我们在*真*时得到*确认*时，我们的管道就创建好了。现在，让我们通过使用[模拟管道 API](https://www.elastic.co/guide/en/elasticsearch/reference/current/simulate-pipeline-api.html) 来测试我们的管道。

```
POST _ingest/pipeline/my-first-pipeline/_simulate
{
  "docs": [
    {
      "_source": {
        "name": "Abdoul-Bagui",
        "unnecessary field": "Field to remove"
      }
    }
  ]
}
```

启动时，我们得到以下输出:

```
{
  "docs": [
    {
      "doc": {
        "_index": "_index",
        "_id": "_id",
        "_version": "-3",
        "_source": {
          "name": "abdoul-bagui",
          "balance": 0,
          "status": true
        },
        "_ingest": {
          "timestamp": "2022-08-30T21:11:25.3094683Z"
        }
      }
    }
  ]
}
```

如您所见，我们的文档已经过转换。*名称*字段全部改为小写，添加了两个字段(*状态*和*余额*)为默认值，删除的字段(*不需要的字段*)不再保留。

我们的管道已创建并正常工作，我们可以通过执行以下操作将其添加到索引请求中:

```
POST users/_doc?pipeline=my-first-pipeline
{
  "name": "Trevor Noah",
  "unnecessary field": "to remove"
}
```

如果您已经有了一个索引，并且想要给它附加一个管道，那么您可以使用 [reindex](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html) 。例如，让我们创建一个填充了*用户*的*账户*索引:

```
PUT accounts/_bulk
{ "create":{ } }
{"name": "Bill Gates"}
{ "create":{ } }
{"name": "Elon Musk"}
{ "create":{ } }
{"name": "Tony Stark"}
```

现在让我们在索引上使用 *reindex* 并指定上面创建的管道。

```
POST _reindex
{
  "source": {
    "index": "accounts"
  },
  "dest": {
    "index": "new_accounts",
    "op_type": "create",
    "pipeline": "my-first-pipeline"
  }
}
```

*new_accounts* 索引是用我们转换后的值创建的。您可以通过运行 *GET new_accounts/_search 来检查它。*

```
{
 ...
 "hits": [
      {
        "_index": "new_accounts",
        "_id": "eVqp8IIBqJSo_EdApqQO",
        "_score": 1,
        "_source": {
          "name": "bill gates",
          "balance": 0,
          "status": true
        }
      },
      {
        "_index": "new_accounts",
        "_id": "elqp8IIBqJSo_EdApqQO",
        "_score": 1,
        "_source": {
          "name": "elon musk",
          "balance": 0,
          "status": true
        }
      },
      {
        "_index": "new_accounts",
        "_id": "e1qp8IIBqJSo_EdApqQO",
        "_score": 1,
        "_source": {
          "name": "tony stark",
          "balance": 0,
          "status": true
        }
      }
    ]
 ...
}
```

在我推荐您阅读的[上一篇文章](https://medium.com/@mhdabdel151/metricbeat-with-local-elasticsearch-and-kibana-c330c902e473)中，我们看到了如何配置 Metricbeat，我们只需在 Metricbeat 配置文件( *metricbeat.yml)* 中提到管道，就可以将管道添加到这个 beat，因此看起来像这样:

```
output.elasticsearch:
  hosts: ["localhost:9200"]
  protocol: "https"
  username: "elastic"
  password: "your-password"
  pipeline: my-first-pipeline
  ssl:
    enabled: true
    ca_trusted_fingerprint: "your_fingerprint"
```

管道有更多的用途，您可以有条件地应用管道，处理管道故障，甚至获得管道使用统计信息。请随意查看关于该主题的官方文档[以获取更多信息。](https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest.html#ingest)

今天到此为止。感谢您的阅读，如果您对本文有任何问题或评论，请在下面留下您的评论。

我们下次再见，看更多的帖子🚀。

阿卜杜尔-巴吉