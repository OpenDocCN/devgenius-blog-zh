# 弹性搜索实践:使用距离特征查询提高得分

> 原文：<https://blog.devgenius.io/elasticsearch-in-action-boosting-scores-using-distance-feature-query-7546b2dea284?source=collection_archive---------8----------------------->

![](img/95a3991b60dc7d79ed2993deb247df4a.png)

节选自我即将出版的书:弹性搜索在行动

*在接下来的几个月里，我将针对每个主题以迷你系列的形式展示这些短小精悍的文章。节选自我的书*[*elastic search in Action，第二版*](https://www.manning.com/books/elasticsearch-in-action-second-edition?utm_source=mkonda&utm_medium=affiliate&utm_campaign=book_konda_elasticsearch_7_23_21&a_aid=mkonda&a_bid=edbc50d4) *。代码在我的*[*GitHub*](https://github.com/madhusudhankonda)*库中。您可以在存储库中找到可执行的 Kibana 脚本，这样您就可以直接在 Kibana 中运行命令。所有代码都经过 Elasticsearch 8.4 版本的测试。*

Elasticsearch 有一些专门服务于特定功能的高级查询。例如，使用`distance_feature`查询提高在指定位置提供冰镇饮料的咖啡馆的分数——这是本文的主题。

当搜索经典文献时，我们可能希望添加一个子句来查找 1813 年出版的书籍。除了返回所有的文学经典书籍，我们可以期待找到*傲慢与偏见*(简·奥斯汀的经典)，但我们的想法是将*傲慢与偏见*放在列表的首位，因为它是在 1813 年印刷的。高居榜首无非是提高基于特定子句的查询结果的相关性分数；在这种情况下，我们特别希望 1813 年出版的书籍被赋予更高的重要性。

通过使用`distance_feature`查询，这种特性在 Elasticsearch 中是可用的。该查询获取结果，并且如果它们更接近起始日期(在该示例中为 1813)，则用更高的相关性分数来标记它们中的一些。

`distance_feature`查询也为位置提供了类似的支持。如果我们愿意，我们可以突出显示更靠近提升到列表顶部的特定地址的位置。假设我们想要查找所有供应炸鱼和薯条的餐馆，但是那些名列前茅的餐馆应该在伦敦桥附近的自治市附近。(博罗市场是世界知名的十三世纪工匠食品市场；见[https://boroughmarket . org . uk)](https://boroughmarket.org.uk.))

对于这样的用例，我们可以使用`distance_feature`查询，它致力于找到更接近原始位置或日期的结果。日期和位置是分别声明为`date`(我们也可以声明为`date_nanos`)和`geo_point`数据类型的字段。越接近给定日期或给定位置的结果在相关性分数上被评定得越高。让我们看几个例子来详细理解这个概念。

# 使用地理定位提高附近大学的分数

在搜索英国的大学时，我们希望优先选择离一个地方较近的大学；比如骑士桥方圆 10 公里内的所有大学。我们需要提高这些比赛的分数。

为了试验这个场景，让我们为大学索引创建一个映射，并将位置声明为一个`geo_point`字段。下面的清单创建了四所大学的映射和索引:两所在伦敦，两所在该国其他地方。

```
# Create a mapping of universities index with bare minimum fields
PUT universities
{
  "mappings": {
    "properties": {
      "name":{
        "type": "text"
      },
      "location":{
        "type": "geo_point"
      }
    }
  }
}

# And index a few universities
PUT universities/_doc/1
{
  "name":"London School of Economics (LSE)",
  "location":[0.1165, 51.5144]
}
PUT universities/_doc/2
{
  "name":"Imperial College London",
  "location":[0.1749, 51.4988]
}
PUT universities/_doc/3
{
  "name":"University of Oxford",
  "location":[1.2544, 51.7548]
}
PUT universities/_doc/4
{
  "name":"University of Cambridge",
  "location":[0.1132, 52.2054]
}
```

既然索引和数据已经准备好了，让我们获取大学，提高相关性分数，这样那些靠近伦敦桥的大学就会在列表的顶部。参见图 12.12 中的伦敦地图，图中显示了这些大学在上述地点周围的大致距离。为此，我们使用 distance_feature 查询，它匹配查询标准，但基于查询中提供的附加参数提高了相关性分数。

![](img/ffa052714c764baf07334359be497461.png)

**图 12.12 伦敦地图显示伦敦桥周围的大学**

首先，让我们编写查询，然后深入了解细节。下面的清单在一个 bool 查询中使用了一个`distance_feature`查询来获取大学。

```
GET universities/_search
{
  "query": {
    "distance_feature": {
      "field": "location",
      "origin": [-0.0860, 51.5048],
      "pivot": "10 km"
    }
  }
}
```

执行该查询时，将搜索返回我们的两所大学(伦敦经济学院和伦敦帝国理工学院)的所有大学。此外，如果这些大学中的任何一所位于原点周围 10 公里的范围内(-0.0860，51.5048 代表英国的伦敦桥)，它们的得分就会比其他大学高*。*

*让我们暂停一下，看看`distance_feature`查询是由什么组成的。`distance_feature`查询需要这些属性:*

*   *`field` —文档中的`geo_point`字段*
*   *`origin` —测量距离的焦点(经度和纬度)*
*   *`pivot` —距焦点的距离*

*在上面的查询中，伦敦经济学院(LSE)大学比帝国理工学院离伦敦桥更近；因此，伦敦经济学院以更高的分数名列榜首。*

*我们也可以使用带有日期的`distance_feature`查询，这是下一节的主题。*

# *使用日期提高分数*

*在最后一节中，`distance_feature`查询帮助我们搜索大学，提高了那些更接近某个地理位置的大学的分数。还有一个类似的需求可以通过`distance_feature`查询来满足:如果结果围绕一个日期旋转，则提高结果的得分。*

*假设我们要搜索所有的 iPhone 发布日期，以那些在 2020 年 12 月 1 日前后 30 天内发布的 iPhone 为榜首(没有特别的原因，除了尝试这个概念)。我们可以像上一节一样编写一个类似的查询，只是字段属性将基于日期。让我们首先创建一个 iphone 映射，并将一些 iphone 编入我们的索引。下面清单中的查询可以做到这一点。*

```
*PUT iphones
{
  "mappings": {
    "properties": {
      "name":{
        "type": "text"
      },
      "release_date":{
        "type": "date",
        "format": "dd-MM-yyyy"
      }
    }
  }
}

# Indexing a few documents
PUT iphones/_doc/1
{ 
  "name":"iPhone",
  "release_date":"29-06-2007"
}
PUT iphones/_doc/2
{
  "name":"iPhone 12",
  "release_date":"23-10-2020"
}
PUT iphones/_doc/3
{
  "name":"iPhone 13",
  "release_date":"24-09-2021"
}
PUT iphones/_doc/4
{
  "name":"iPhone 12 Mini",
  "release_date":"13-11-2020"
}*
```

*现在我们有了一个索引，其中包含了一批 iphone，让我们开发一个查询来满足我们的要求:我们将获取所有的 iphone，但对 2020 年 12 月 1 日前后 30 天发布的 iphone 进行优先排序。下一个清单中的查询做到了这一点。*

```
*GET iphones/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "12"
          }
        }
      ],
      "should": [
        {
          "distance_feature": {
            "field": "release_date", 
            "origin": "1-12-2020", 
            "pivot": "30 d" 
          }
        }
      ]
    }
  }
}*
```

*在上面的查询中，我们用一个`must`和一个`should`子句将一个`distance_feature`包装在一个 bool 查询中(我们在以前的文章中学习过`bool`查询)。`must`子句搜索 name 字段中带有 12 的所有文档，并从我们的索引中返回 iPhone 12 和 iPhone 12 mini 文档。我们的要求是优先考虑 12 月 1 日前后 30 天发布的手机(因此，可能是 2020 年 11 月至 12 月发布的所有手机)。*

*为了满足这个要求，should 子句使用`distance_feature`查询来提高与所提到的数据转换日期最接近的匹配文档的分数。该查询从 iphones 索引中获取所有文档。任何在 2020 年 12 月 1 日(原产地)之前或之后 30 天发布的 iPhone 都会以更高的相关性分数返回。*

*请记住，`should`子句返回的所有匹配项都将计入总得分。因此，你应该会看到 iPhone 12 Mini 高居榜首，因为这款 iPhone 的发布日期(" release _ date ":" 13–11–2020 ")更接近于枢轴日期(" origin ":" 01–12–2020 " 30 天)。为了完整起见，下面的代码片段显示了查询结果。*

```
*"hits" : [
      {
        "_index" : "iphones",
        "_id" : "4",
        "_score" : 1.1876879,
        "_source" : {
          "name" : "iPhone 12 Mini",
          "release_date" : "13-11-2020"
        }
      },
      {
        "_index" : "iphones",
        "_id" : "2",
        "_score" : 1.1217185,
        "_source" : {
          "name" : "iPhone 12",
          "release_date" : "23-10-2020"
        }
      }
    ]*
```

*正如你所看到的，iPhone 12 Mini 的得分高于 iPhone 12，因为它是在我们的 pivot 日期前 17 天发布的，而 iPhone 12 的发布时间比这早一点(几乎提前了 5 周)。*

*差不多就是这样。请多跟我学，当然，也请为我鼓掌:)*

**这些短文是从我的书*[*elastic search in Action 第二版*](https://www.manning.com/books/elasticsearch-in-action-second-edition?utm_source=mkonda&utm_medium=affiliate&utm_campaign=book_konda_elasticsearch_7_23_21&a_aid=mkonda&a_bid=edbc50d4) *中摘录的。代码在我的*[*GitHub*](https://github.com/madhusudhankonda)*库中。**

*![](img/95a3991b60dc7d79ed2993deb247df4a.png)*

*行动中的弹性研究*