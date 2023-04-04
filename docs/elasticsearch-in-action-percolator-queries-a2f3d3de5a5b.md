# 弹性搜索:过滤器查询

> 原文：<https://blog.devgenius.io/elasticsearch-in-action-percolator-queries-a2f3d3de5a5b?source=collection_archive---------3----------------------->

![](img/95a3991b60dc7d79ed2993deb247df4a.png)

节选自我即将出版的书:弹性搜索在行动

*在接下来的几个月里，我会将这些短小精悍的文章以迷你系列的形式呈现在大家面前。节选自我的书*[*elastic search in Action，第二版*](https://www.manning.com/books/elasticsearch-in-action-second-edition?utm_source=mkonda&utm_medium=affiliate&utm_campaign=book_konda_elasticsearch_7_23_21&a_aid=mkonda&a_bid=edbc50d4) *。代码在我的*[*GitHub*](https://github.com/madhusudhankonda)*库中。您可以在存储库中找到可执行的 Kibana 脚本，这样您就可以直接在 Kibana 中运行命令。所有代码都根据 Elasticsearch 8.4 版本进行了测试。*

在本文中，我们探索了所有关于`percolate`查询的内容。

给定一个输入，搜索一组文档非常简单。我们所要做的就是，如果有任何符合给定标准的结果，就从索引中返回搜索结果。这满足了搜索用户标准的要求，这也是我们目前在查询结果时所做的。

Elasticsearch 还满足另一个要求:当用户当前的搜索产生负面结果时通知用户，但结果将在未来某个日期可用。比方说，一个用户在一个电子商务图书销售网站上搜索一本*Python Action*的书，但是不幸的是，我们没有这本书的库存。不满意的顾客离开网站。然而，一两天后，我们进了一些新货，这本书被添加到库存中。现在，当这本书重新出现在我们的库存中时，我们希望通知用户，以便用户可以购买它。

Elasticsearch 通过提供一个叫做`percolate`的特殊查询来支持这种用例，这个查询使用 percolator 字段类型。

`percolate`查询与我们通常的搜索查询机制相反，它不是针对文档运行查询，而是搜索给定文档的查询。

乍一看，这是一个有点难以理解的奇怪概念，但是我们将在本节中揭开它的神秘面纱。下图显示了普通查询和过滤查询之间的差异。

![](img/0ce27561ec7628d8e69e3b05261fbb8a.png)

**图 12.14 正常与渗滤液查询**

让我们通过首先索引一组文档来检查运行中的`percolate`查询。下面的列表将三本技术书籍编入`tech_books`索引。请注意，我们还没有包括 Python 书籍。

```
PUT tech_books/_doc/1
{
  "name":"Effective Java",
  "tags":["Java","Software engineering", "Programming"]
}

PUT tech_books/_doc/2
{
  "name":"Elasticsearch crash course",
  "tags":["Elasticsearch","Software engineering", "Programming"]
}

PUT tech_books/_doc/3
{
  "name":"Java Core Fundamentals",
  "tags":["Java","Software"]
}
```

既然我们已经在图书目录索引中植入了几本书，我们可以期待用户使用简单的匹配/术语查询来搜索图书。(我在讨论中省略了这些查询，因为我们已经掌握了它们。)然而，并不是所有的用户查询都产生结果；例如，有人搜索 *Python in Action* 这本书不会找到。下面的清单演示了这一点。

```
GET tech_books/_search
{
  "query": {
    "match": {
      "name": "Python"
    }
  }
}
```

从用户的角度来看，搜索以令人失望的结果结束:搜索时没有归还图书。当脱销的 Python 书籍变得可用时，我们可以通过通知用户来使我们的查询更上一层楼。这正是我们可以使用过滤器的地方。

正如我们将文档编入索引一样，percolators 对于一组查询有自己的索引，并期望查询被编入索引。我们需要为 percolator 索引定义一个模式。姑且称之为`tech_books_percolator`，如下清单所示。

```
PUT tech_books_percolator
{
  "mappings": {
    "properties": {
      "query": { 
        "type": "percolator"
      },
      "name": { 
        "type": "text"
      },
      "tags": { 
        "type": "text"
      }
    }
  }
}
```

这个清单定义了保存`percolator`查询的索引。需要注意的几件事是

*   它包含一个查询字段来保存用户(失败的)查询。
*   查询字段的数据类型必须是 percolator。

模式的其余部分由从最初的`tech_books`索引借用的模式定义组成。

正如我们用各种数据类型定义字段一样，如文本、长整型、双精度型、关键字等。，Elasticsearch 也提供了一个过滤器类型。在清单 12.30 中，查询字段被定义为 percolator，它期望一个查询作为字段值，我们很快就会看到。

既然我们已经准备好了 percolator 索引(`tech_books_percolator`)映射，下一步就是存储查询。这些查询通常不向用户返回结果(比如 Python 示例)。

在现实世界中，没有结果的用户查询将被索引到这个 percolator 索引中。将用户失败的查询整理成 percolator 索引的过程可以在搜索应用程序内部完成，但不幸的是，这超出了这里讨论的范围。想象一下，上面清单中的查询没有产生结果，而是被发送到 percolator 的索引中进行存储。下一个清单提供了存储查询的代码。

```
PUT tech_books_percolator/_doc/1
{
  "query" : { 
    "match":{
      "name":"Python"
    }
  }
}
```

正如您所看到的，清单显示了查询的索引，这不同于普通文档的索引。如果您还记得开始章节中的文档/索引操作，那么每当我们索引一个文档时，我们都使用带有名称-值对的 JSON 格式的文档。然而，奇怪的是，这一个有一个匹配查询。

这个查询存储在文档 ID 为 1 的`tech_books_percolator`索引中。可以想象，这个索引随着失败的搜索而不断增长。唯一值得注意的是，JSON 文档由用户发出的查询组成，这些查询没有返回肯定的结果。

拼图的最后一块是当我们的股票更新时搜索`tech_books_percolators`指数。作为书店老板，我们应该进货，也许，下次我们收到新货时，让我们假设 Python 书在新货中。我们现在可以将它索引到我们的`tech_books`索引中，供用户搜索和购买，如清单所示。

```
PUT tech_books/_doc/4
{
  "name":"Python in Action",
  "tags":["Python","Software Programming"]
}
```

现在我们已经为 Python 书籍建立了索引，我们需要重新运行用户失败的查询。但是这次不是在`tech_books`索引上运行查询，而是在`tech_books_percolator`索引上运行。针对`percolator`索引的查询有一个特殊的语法。让我们先把它写下来，然后再回来讨论它。

```
GET tech_books_percolator/_search
{
  "query": {
    "percolate": { #A
      "field": "query", #B
      "document": { #C
        "name":"Python in Action",
        "tags":["Python","Software Programming"]
      }
    }
  }
}
```

如清单所示，percolate 查询需要两位输入:一个值为 query 的`field`(这与代码清单前面 percolator 映射中定义的属性一致)和一个`document`，这是我们在`tech_books`索引中索引的同一个文档。我们需要做的就是检查是否有任何查询与给定的文档相匹配。幸运的是， *Python 中的*有一个匹配(您可能还记得，我们之前在 percolator 索引中索引了一个查询)。

现在给定一个文档(清单 12.32 中定义的 Python 文档)，我们可以从`tech_books_percolator`索引返回一个查询。这让我们可以通知用户，他们要找的书又有货了！注意，我们可以用特定的用户 ID 扩展存储在`tech_books_percolator`索引中的查询。

这就是所有的过滤器。它们有点难以理解，因为有几个移动的部分，但是一旦你理解了用例，实现它就没那么难了。请记住，必须始终有一个自动、半自动甚至手动的过程来同步用户对存储在 percolator 索引中的数据执行的操作。

*这些短文是从我的书*[*elastic search in Action，第二版*](https://www.manning.com/books/elasticsearch-in-action-second-edition?utm_source=mkonda&utm_medium=affiliate&utm_campaign=book_konda_elasticsearch_7_23_21&a_aid=mkonda&a_bid=edbc50d4) *中摘录的。代码在我的*[*GitHub*](https://github.com/madhusudhankonda)*库中。*

![](img/95a3991b60dc7d79ed2993deb247df4a.png)

行动中的弹性研究