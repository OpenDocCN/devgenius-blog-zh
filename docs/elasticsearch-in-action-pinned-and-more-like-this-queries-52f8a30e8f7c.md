# Elasticsearch in Action: Pinned 和更多类似的查询

> 原文：<https://blog.devgenius.io/elasticsearch-in-action-pinned-and-more-like-this-queries-52f8a30e8f7c?source=collection_archive---------4----------------------->

![](img/95a3991b60dc7d79ed2993deb247df4a.png)

节选自我即将出版的书:弹性搜索在行动

在接下来的几个月里，我将把这些短小精悍的文章以迷你系列的形式呈现在大家面前。节选自我的书[*elastic search in Action，第二版*](https://www.manning.com/books/elasticsearch-in-action-second-edition?utm_source=mkonda&utm_medium=affiliate&utm_campaign=book_konda_elasticsearch_7_23_21&a_aid=mkonda&a_bid=edbc50d4) *。代码在我的*[*GitHub*](https://github.com/madhusudhankonda)*库中。您可以在存储库中找到可执行的 Kibana 脚本，这样您就可以直接在 Kibana 中运行命令。所有代码都根据 Elasticsearch 8.4 版本进行了测试。*

Elasticsearch 有一些专门服务于特殊功能的高级查询。例如，使用`more_like_this`查询找到看起来相似的文档，或者使用`pinned`查询赋予一些文档更高的重要性，等等。在这篇短文中，我们将了解这两个查询。

# 固定查询

当查询你最喜欢的电子商务网站如亚马逊时，你可能会看到一些赞助搜索结果出现在结果集的顶部。假设我们想使用 Elasticsearch 在应用程序中实现这样的功能。好吧，不要烦恼；一个`pinned`的疑问就在眼前。

`pinned`查询有助于将选择的文档添加到结果集中，这样它们就会出现在列表的顶部。这是通过使他们的相关性得分高于其他人来实现的。让我们快速查看一个示例查询，在下面的清单中给出，它演示了这个功能。

```
GET iphones/_search
{
  "query": {
    "pinned":{ 
      "ids":["1","3"], 
      "organic":{ 
        "match":{ 
          "name":"iPhone 12"
        }
      }
    }
  }
}
```

清单中的`pinned`查询有几个移动部分:`organic`和`ids`块。

我们先来看一下`organic`块。它是容纳搜索查询的查询块；在本例中，我们在 iPhone 索引中搜索 iPhone 12。这个查询*理想情况下*应该返回两个文档:iPhone 12 和 iPhone 12 Mini。

但是，当您使用数据运行这个查询时(检查我的 GitHub repo 中的查询和数据)，您会收到文档 iPhone 和 iPhone 13(这两个是*而不是* iPhone 12！)除了 iPhone 12 和 iPhone 12 Mini。

原因在于`ids`字段。该字段包含必须附加到结果并显示在列表顶部的附加文档(由*赞助的*结果)，从而综合创建更高的相关性分数。

`pinned`查询有助于为结果集添加额外的高优先级文档。这些文档胜过列表位置中的其他文档来创建赞助结果。

您可能想知道固定的结果是否有任何得分:一个或一些固定的结果可以优先于其他结果吗？不幸的是，答案是否定的。这些文档按照我们在查询中输入的 id 顺序显示:例如:`"ids":["1", "3"]`。

本文中的另一个查询是“更像这样”，这是下一节的主题。

# 查看更像这样(more_like_this)查询

你可能已经注意到，当你浏览其中一部电影时，网飞或亚马逊 Prime Video(或你最喜欢的流媒体应用程序之一)会向你展示更像这部的*电影。例如，图 12.13 显示了我访问帕丁顿 2 时所有类似的电影。*

![](img/d8c564fd69ea5747e0eb6aa42e868c31.png)

**图 12.13 观看更多此类电影**

用户的需求之一是在一些文档中搜索“相似”或“类似”。比如研究类似 COVID 和 SARS 的论文，或者查询类似*教父*的电影。让我们直接看一个例子来更好地理解用例。

假设我们正在收集一些人的资料列表。为了创建一组概要文件，我们将样本文档索引到概要文件索引中，如下面清单中的代码所示。

```
PUT profiles/_doc/1
{
  "name":"John Smith",
  "profile":"John Smith is a capable carpenter"
}

PUT profiles/_doc/2
{
  "name":"John Smith Patterson",
  "profile":"John Smith Patterson is a pretty plumber"
}

PUT profiles/_doc/3
{
  "name":"Smith Sotherby",
  "profile":"Smith Sotherby is a gentle painter"
}
PUT profiles/_doc/4
{
  "name":"Frances Sotherby",
  "profile":"Frances Sotherby is a gentleman"
}
```

这些文件没什么好奇怪的；它们只是一群普通人的侧写。现在我们已经索引了这些文档，让我们看看如何让 Elasticsearch 获取与文本*温柔的画家*或*能干的木匠*相似的文档，甚至检索具有相似名称的文档，Sotherby。

这正是`more_like_this`查询帮助我们的。下一个清单创建了一个查询来搜索更像 Sotherby 的概要文件。

```
GET profiles/_search
{
  "query": {
    "more_like_this": { 
      "fields": ["name", "profile"], 
      "like": "Sotherby", 
      "min_term_freq": 1, 
      "max_query_terms": 12, 
      "min_doc_freq":1
    }
  }
}
```

`more_like_this`查询接受`like`参数中的文本，该输入文本与`fields`参数中提到的给定字段相匹配。该查询接受一些调优参数，比如查询应该选择的最小术语和文档频率(`min_term`)以及最大术语数量(`max_query_terms`)。如果我们想在显示相似文档时给用户更好的体验，那么`more_like_this`查询是正确的选择。

这差不多就是关于`pinned`和`more_like_this`查询的内容。

不要鼓掌:)

还是[不要跟着](https://mkonda007.medium.com/)我:)

*这些短文节选自我的书*[*elastic search in Action，第二版*](https://www.manning.com/books/elasticsearch-in-action-second-edition?utm_source=mkonda&utm_medium=affiliate&utm_campaign=book_konda_elasticsearch_7_23_21&a_aid=mkonda&a_bid=edbc50d4) *。代码在我的*[*GitHub*](https://github.com/madhusudhankonda)*库中。*

![](img/95a3991b60dc7d79ed2993deb247df4a.png)

行动中的弹性研究