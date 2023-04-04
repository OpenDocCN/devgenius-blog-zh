# 弹性搜索——搜索的解决方案

> 原文：<https://blog.devgenius.io/elasticsearch-solution-to-searching-71116220c82f?source=collection_archive---------0----------------------->

企业级搜索和分析引擎

Elasticsearch 是我们自己的数据的全文搜索引擎。它将我们的数据编入索引，并通过 HTTP 接口进行搜索。它是一个基于 Lucene 的分布式搜索引擎。它可以扩展到数 Pb 的数据。它支持多租户和高并发。它近乎实时地提供搜索结果。Elasticsearch 也是一套被称为 ELK Stack 的开源工具的一部分。

## 弹性搜索的使用案例

*   操作日志记录和日志分析(ELK)
*   网站内容和媒体搜索
*   全文搜索
*   事件数据和指标
*   用 Kibana 可视化数据

## 串

![](img/5778ca320a7f1b1d1e9a0ce3c19d4eb0.png)

Elasticsearch 集群是一个分布式的节点集合，每个节点执行一个或多个集群操作。每个节点运行一个 ES 应用程序实例。集群是可水平扩展的。通过向群集中添加额外的节点，我们可以线性扩展群集容量，同时保持类似的性能。通过创建和使用注册令牌来添加节点。

群集中的节点可以根据它们执行的特定操作类型来区分。在高可用性集群中，我们为不同的集群功能指定不同的节点集。要定义节点角色，我们可以这样设置配置— `node.roles: [ master | data | ingest ]`

## 主网点

每个集群在任一时间点都有一个主节点，其职责包括维护集群的健康和状态。主节点充当协调者，用于创建、删除、管理索引，以及将索引和底层碎片分配给集群中的适当节点。

## 主合格节点

符合主节点条件的节点是成为主节点的候选节点。

## 数据节点

数据节点保存实际的索引数据，并处理数据的搜索和聚合。

## 仅协调器节点

这些节点向所有相关的碎片广播查询请求，并将它们的响应聚集成一个全局排序的集合，该集合被返回给客户端。这些节点充当负载平衡器。

## 摄取节点

接收节点可以配置为在数据被接收之前对其进行预处理。由于某些处理器(如 grok 处理器)可能是资源密集型的，因此将单独的节点专用于接收管道是有益的，因为接收处理不会影响搜索操作。否则，数据节点将执行此任务。

在大型云集群中，我们将拥有专用的主节点、2 个或更多接收节点、2 个或更多协调节点和多个数据节点。

因为数据节点存储数据，所以它们应该连接磁盘。固态硬盘用于热数据，硬盘用于冷数据。我们还需要大内存(RAM)来存储数据节点，因为它们可以缓冲数据。

> Elasticsearch 用 Java 编写，在 JVM 中运行进程。它为不同的进程使用线程池。

`GET /_cat/thread_pool/search?v&h=host,name,active,rejected,completed`

## 索引

索引是相似类型文档的集合。这是一个逻辑统一体。物理上它被映射到碎片上。索引与设置、映射、别名和模板相关联。

## 索引别名

别名是可以指向一个或多个索引的虚拟索引名。如果数据分布在多个索引中，这样就不需要跟踪查询哪个特定的索引。

![](img/0cb572e2bef8408796d30de338f218eb.png)

```
GET _cat/aliases?v
POST _aliases
{
  "actions": [
    {
      "add": {
        "index": "index-1",
        "alias": "alias1"
      }
    }
  ]
}
```

索引别名还有助于在不停机的情况下进行索引迁移。

## 陶瓷或玻璃碎片

指数被水平分割成称为*碎片*的小块。碎片是独立的 Lucene 索引。它们是索引的组成部分。

Elasticsearch [建议](https://www.elastic.co/guide/en/elasticsearch/reference/current/size-your-shards.html#shard-size-recommendation)每个碎片小于 65GB (AWS [建议](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/sizing-domains.html)小于 50GB)，因此我们可以创建基于时间的索引，其中每个索引包含 16-20GB 的数据，为数据增长提供一些缓冲。

> 主碎片和复制碎片

获取索引碎片`GET _cat/shards/index`

碎片的生命周期:初始化→开始→重新定位→未分配

```
{
    "settings" : {
        "index" : {
            "number_of_shards" : 8,
            "number_of_replicas" : 2
        }
    }
}
```

## 传输日志/存储缓冲器

Lucene 提交的开销太大，无法对每个单独的更改都执行，因此每个分片副本还会将操作写入其事务日志，称为 translog。每个碎片都有一个事务日志。translog 中的数据只能通过 Lucene 提交保存到磁盘上。在失败的情况下，这将被重放以提交未保存的更改。在提交期间，内存中的所有段都被合并成一个段并保存到磁盘。

![](img/1682380b914f88ff66d61558d024a5e4.png)![](img/bc52e19e229ec8e5ef6c857d84cffb32.png)

> 刷新—将内存缓冲区内容复制到内存中新创建的段，并清除 translog。每秒钟都会发生。
> 
> 刷新—内存中的段被写入磁盘。较小的段被合并成较大的段。

## 片段

Lucene 索引被分成称为段的更小的文件。段是倒排索引。段是不可变的。Lucene 按顺序在所有段中搜索。所以有很多段会影响性能。Elasticsearch 通过删除已删除的文档来合并片段以创建新的片段。合并还有助于将较小的片段组合成较大的片段，因为较小的片段具有较差的搜索性能。

## 文档

文档是传递给 Elasticsearch 进行存储的信息单元。文档是存储在 Elasticsearch 索引中的 JSON 文件，被视为基本存储单元。文档是不可变的。在更新的情况下，旧文件被新文件替换。doc 响应中的 _version 字段是过去的事情，现在已经没有意义了。

## 菲尔德斯

*数据类型，如二进制、布尔、关键字、数字、日期、文本、地理形状、搜索类型*

## 元字段

> **_index —** 索引名称
> **_type
> _id —** 文档的唯一 id
> **_ source—**应用任何分析器/转换之前的原始 JSON 文档。
> **_all** —包含文档中的所有其他字段

ES 对象与数据库的指示性映射:

> MySQL = > Databases = > Tables = > Row = > Column = > Index
> elastic search =>indexes =>Types =>Documents =>Properties =>Mapping

## 专家系统使用的内部数据结构

> 倒排索引—针对文本数据
> BKD 树—数字、日期、地理空间数据
> doc_values —排序&聚合

## 分析器

Elasticsearch 提供了**分析器**来定义文本应该如何被索引和搜索。分析器在索引过程中用于将短语和表达式解析成它们的组成术语。在索引中定义的分析器由单个记号赋予器和任意数量的记号过滤器组成。

![](img/cb8dc3aabac8ca1b9c91ef92d50cfc6f.png)

分析仪有三个组件:

*   字符过滤器`(html_strip)`
*   记号赋予器`(standard)`
*   令牌过滤器`(lowercase)`

Elasticsearch 有很多内置的分析器、令牌化器和令牌过滤器。

```
POST /_analyze
{
  "text": "This text will be analyzed   with STANDARD analyzer"
  "analyzer": "standard"
}

POST /_analyze
{
  "text": "This text will be analyzed   with STANDARD analyzer"
  "char_filter": [],
  "tokenizer": "standard",
  "filter": [ "lowercase" ]
}
```

## 索引模板

索引模板是用来创建新索引的框架。

## 索引生命周期管理

每个索引经历不同的阶段(热→暖→冷→删除)。基于预定义的配置，ILM 将从一个阶段到另一个阶段的模式索引。

![](img/3d9b1f50c765e77688c6e06ee841ad97.png)

## 摄取管道

摄取管道允许我们在索引文档之前应用转换，如*字段删除*、*信息提取、*甚至*数据丰富*。一个流水线由几个被称为[处理器](https://www.elastic.co/guide/en/elasticsearch/reference/current/processors.html)的可配置任务组成。Elasticsearch 将管道存储为集群状态下的内部数据结构。

```
GET _nodes/ingest?filter_path=nodes.*.ingest.processors
PUT _ingest/pipeline/blog-demo-pipeline
{
  "version": 1,
  "description": "Demo pipeline for medium blog",
  "processors": [
    {
      "set": {
        "description": "Set default value of Tag",
        "field": "StoryTag",
        "value": "Data Engineering"
      }
    },
    {
      "lowercase": {
        "field": "author"
      }
    },
    {
      "remove": {
        "field": "external_reads"
      }
    }
  ]
}

POST _ingest/pipeline/blog-demo-pipeline/_simulate
{
  "docs": [
    {...}
  ]
}
POST users/_doc?pipeline=blog-demo-pipeline
{...}

POST _reindex
{
  "source": {
    "index": "existing index name"
  },
  "dest": {
    "index": "new index name",
    "op_type": "create",
    "pipeline": "blog-demo-pipeline"
  }
}
```

## 数据复制

![](img/90f5e6f4ae8a2816d8b8115b4eae0d20.png)

Elasticsearch 有**主分片**和**副本分片**的概念。

将数据从主碎片复制到副本碎片的过程称为**数据复制**。数据复制的核心考虑因素是副本和主副本的延迟( **Lag** )。如果 Lag 始终为 0，则它是可靠性最高的实时复制。Elasticsearch 借助**文档复制**和**段复制**实现数据复制。

## 文件索引

Elasticsearch 的输入数据在存储之前会进行分析和标记。Lucene 库通常只存储经过分析的令牌。Elasticsearch 还将收到的原始文档存储在一个名为 _source 的特殊字段中。虽然会占用额外的存储空间，但 _source 字段在提供文档更新功能方面至关重要，也是重新索引操作所必需的。

## 文档路由

Elasticsearch 使用路由算法在索引时将我们的文档分发到底层碎片。每个文档将在一个且只有一个主碎片中被索引。文档均匀分布，因此不会有任何碎片过载。

路由算法是一个简单的公式，其中 Elasticsearch 在索引或搜索期间推导出文档的碎片:

```
shard = hash(id) % number_of_shards
```

哈希函数需要一个惟一的 id，通常是一个文档 ID，甚至是一个由用户提供的自定义 ID。

*注意:文档不是从主碎片中检索的，但是 ES 利用自适应副本选择(ARS)从复制组中挑选一个碎片。*

## 专家系统中的索引请求流程

1.  该请求由协调节点接收。
2.  该节点将文档路由到它们的索引和碎片。
3.  主碎片和副本碎片将文档写入(并行)translog。
4.  文档被规范化(映射和分析)并存储在内存缓冲区中。
5.  索引被刷新，以便可以搜索。
6.  Lucene 在磁盘上提交新的段。

## 文档搜索

> **查询阶段** —协调节点将请求路由到索引中的所有碎片。这些碎片独立地执行搜索，并创建按相关性分数排序的结果的优先级队列。所有碎片都将匹配文档的文档 id 和相关分数返回给协调节点。协调节点创建一个新的优先级队列，并对结果进行全局排序。协调节点创建一个优先级队列，对所有碎片的结果进行排序，并返回前 10 个命中结果。
> **获取阶段** —协调节点然后从碎片中请求原始文档。碎片丰富了文档，并将它们返回到协调节点。
> **文档评分** —词频(TF)、逆词频(IDF)、常模。`relevance_score = TF * IDF`。我们可以在查询对象的同一层附加一个`indices_boost` 对象。这将提高提升指数的优先级。现在 ES 用 Okapi BM25 算法计算相关度。

有多种类型的搜索查询谓词:

> Term，Terms
> ids
> 存在
> 范围
> 通配符
> 前缀
> regexp
> match_phrasse，multi_match，match_all
> 模糊
> 同义词

## 过滤器与查询上下文

**过滤上下文**针对所提供的查询提供是/否的答案。默认情况下，过滤器被缓存，它们不会影响文档的相关性分数。**查询上下文，**显示每个文档与查询的匹配程度。它利用分析器来做出决定。结果包括相关性分数。

除非是全文搜索或相关性分数类型的搜索过滤器，否则建议使用上下文搜索。过滤器通常比查询更快。

## 分页方法

*   **from/size**—`from`参数定义了我们想要从开始跳过的项目数。`size`参数是返回的最大命中数。
*   **_scroll API** —用于检索大量结果。它类似于 SQL 数据库中的游标。不建议用户请求使用。它应该在批处理模式下使用。
*   **搜索 _ 之后**
*   **时间点(PIT)**

## 聚合

> 指标聚合—总和、最小值、最大值、平均值
> 数字/非数字指标聚合
> 存储桶聚合—对组中的查询结果进行排序
> 管道聚合—从一个阶段到另一个阶段的管道聚合

## 数据流

数据流简化了时序数据的处理。它处理翻转索引别名和索引，并为后备索引定义公共映射和设置。它利用索引语句管理(ISM)策略。

## 配置

> flush _ threshold _ size
> index _ buffer _ size
> refresh _ interval
> threadpool . bulk . queue _ size

## K8s 上的安装

```
helm repo add bitnami https://charts.bitnami.com/bitnami
# helm repo add elastic https://helm.elastic.co
helm install elasticsearch --set master.replicas=3,coordinating.service.type=LoadBalancer bitnami/elasticsearch
kubectl port-forward svc/elasticsearch-master 9200
curl localhost:9200
```

*注:ES 使用 9200 进行 API &搜索，9300 进行节间通信。*

## K8s 运算符

```
# Elastic
kubectl create -f https://download.elastic.co/downloads/eck/2.5.0/crds.yaml
kubectl apply -f https://download.elastic.co/downloads/eck/2.5.0/operator.yaml

# opensearch
helm repo add opensearch-operator https://opster.github.io/opensearch-k8s-operator/
helm install opensearch-operator opensearch-operator/opensearch-operator
```

## 使用 python

```
# pip install elasticsearch
# pip install opensearch-py 

from elasticsearch import Elasticsearch 
from elasticsearch import helpers
import pandas as pd

df = (
    pd.read_csv("wiki_movie_plots_deduped.csv")
    .dropna()
    .sample(5000, random_state=42)
    .reset_index()
)

url = 'http://root:root@localhost:9200'  
es = Elasticsearch(url)  
# es = Elasticsearch([{'host': 'localhost', 'port': 9200}])

mappings = {
        "properties": {
            "title": {"type": "text", "analyzer": "english"},
            "ethnicity": {"type": "text", "analyzer": "standard"},
            "director": {"type": "text", "analyzer": "standard"},
            "cast": {"type": "text", "analyzer": "standard"},
            "genre": {"type": "text", "analyzer": "standard"},
            "plot": {"type": "text", "analyzer": "english"},
            "year": {"type": "integer"},
            "wiki_page": {"type": "keyword"}
    }
}

index_name = "movies"

index_exists = es.indices.exists(index = index_name)

if not index_exists:  
  es.indices.create(index = index_name, mappings = mapping, ignore=400)

# curl -XGET [http://localhost:9200/retail_store]
docs = []
for i, row in df.iterrows():
    doc = {
        "title": row["Title"],
        "ethnicity": row["Origin/Ethnicity"],
        "director": row["Director"],
        "cast": row["Cast"],
        "genre": row["Genre"],
        "plot": row["Plot"],
        "year": row["Release Year"],
        "wiki_page": row["Wiki Page"]
    }
    docs.append(doc)

helpers.bulk(es, docs, index=index_name, doc_type='_doc')

query = {  
  "query" : {  
    "bool" : {  
      "must" : {  
         "match_phrase" : {  
            "cast" : "jack nicholson",
         }  
      },
      "filter": {"bool": {"must_not": {"match_phrase": {"director": "roman polanski"}}}},  
    }  
  }  
}

results = es.search(index=index_name, body=query, size = 20)

# get all hits
# helpers.scan(client=es, query=query, index=index_name) 

es.delete(index = index_name, id = doc_id)
# es.delete_by_query(index = index_name, query = query)

es.indices.put_settings(index=index_name, body={"key": "value"})

es.indices.delete(index='movies') 
```

**设置**包括特定于索引的属性，如碎片数量、分析器等。**映射**用于定义文档及其字段应该如何存储和索引。我们为每个字段定义数据类型，或者对未知字段使用动态映射。

*注意:对于批量 API，在使用 cURL 时使用* `*Content-Type: application/x-ndjson*`

## 分层索引

Elasticsearch 允许我们为数据节点设置不同的层和不同的硬件配置文件。我们通过在`elasticsearch.yml`配置文件中设置`node.role`属性来做到这一点。

> hot—data _ hot
> Warm—data _ Warm
> Cold—data _ Cold
> Frozen—data _ Frozen

![](img/d86ae12dd83015f31f545be5e938b4df.png)

## 基本 API

```
POST <index-name>/_search?explain=true
GET <index1>,<index2>,<index3>/_search 
GET /_cluster/health
GET /_cat/indices?h=index
GET index/_settings
GET index/_mapping
DELETE /document-index/_doc/id
POST document-index/_delete_by_query?conflicts=proceed
{
 "query": {
 "match_all": {}
 }
}
GET /_analyze
{
  "analyzer" : "standard",
  "text" : "Hello, from Elastic Search."
}
```

## 与火花互动

```
# via package
--packages org.elasticsearch:elasticsearch-hadoop:7.10.1
# or via pip
!pip install elasticsearch-hadoop

df = spark.read
    .format("org.elasticsearch.spark.sql")
    .option("es.nodes","http://host:9200")
    .option("es.read.metadata", "true")
    .option("es.read.field.include", "text,user")
    .load("index/type")

df.write
    .format("org.elasticsearch.spark.sql")
    .option("es.nodes","http://localhost:9200")
    .option("es.write.operation", "upsert")
    .save("index/type")
```

## 弹性搜索管理工具

[脑波强化器](https://github.com/lmenezes/cerebro)

## 使用插件定制弹性搜索

Elasticsearch 有一个基于界面的插件架构，允许扩展和定制 ES 功能。插件通常是保存在特定位置的打包工件(jar、zip、rpm)文件。我们可以使用`elasticsearch-plugin`命令行工具来安装、列出和删除插件。一些常见的插件类别有:

> API 扩展插件
> 快照插件
> 发现插件
> 映射器插件
> 集成插件

```
GET _cat/plugins
```

## 使用 Elasticsearch 的公司

> Swiggy，Quora，AutoDesk，Adobe，沃尔玛，Grab，Tinder，优步，Visa，Compass，Pearson，Pinterest，Wikimedia，网飞

## 瓶颈

*   **集群管理器** —当节点数量超过 300 时，系统变慢，集群重启变慢
*   **分片分配器**
*   没有针对不良查询的**准入控制器** —没有对不良查询的主动预防
*   **数据保留时间越长，存储成本**越高
*   插件和核心之间直接**耦合**，无资源分割
*   Elasticsearch 不允许多数据中心安装。

## AWS 中的云产品

AWS 提供 OpenSearch 服务，这是 Elasticsearch 的一个分支。该服务提供两种产品:托管和无服务器。AWS 产品具有以下优势:

> 增强的安全性
> 警报
> 性能分析器
> SQL 查询支持
> 索引管理
> k 近邻搜索

## OpenSearch 的未来工作正在进行中

*   远程存储
*   智能缓存
*   跨集群和完整集群复制

## 摘要

简而言之，专家系统索引可以总结为以下步骤

*   向 API 发送数据
*   数据被路由到索引、节点和碎片
*   映射、标准化和分析
*   磁盘持久性
*   可供搜索的数据

感谢阅读！！