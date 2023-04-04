# 通过 Wolfram 语言从 Mathematica 与 InfluxDB v2 交互

> 原文：<https://blog.devgenius.io/interacting-with-influxdb-v2-from-mathematica-via-the-wolfram-language-f335de30beaf?source=collection_archive---------20----------------------->

有时候，当你接近危险边缘，并且有稀疏或相当简洁的文档时，你所需要的只是一点点推动…

![](img/f3f51b726bf2a488af1a8d71668404c6.png)

照片由来自 [Pexels](https://www.pexels.com/photo/black-and-white-business-chart-computer-241544/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Lorenzo](https://www.pexels.com/@lorenzocafaro?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

# 灵活的数据争论

我使用 [Mathematica](https://www.wolfram.com/mathematica/) 已经有几年了，有时可能是因为这种语言的范围太广，或者是因为有大量的内置函数可用，有时很难准确找到我需要的。

我经常会发现，经过一段时间的研究后，我会找到一个完全符合我要求的函数，但通常简洁的可用文档并不总能给我提供足够的信息。

有时是参数，有时是返回值，但主要是缺乏具体用例的具体例子。文档是全面的，但由于经常有大量的排列，根本没有空间来包括一切！

在这种情况下，我通常会前往 Mathematica 本身的 [Stack Overflow](https://stackoverflow.com) 或 [Stack Exchange 并进行搜索，但由于相对小众的用户群和/或很少有人做同样的事情，通常情况下，我没有太多可继续的东西——特别是如果它接近出血边缘。](https://mathematica.stackexchange.com)

最近，我一直在用内置了 [Flux](https://www.influxdata.com/products/flux/) 语言的 [InfuxDB v2](https://v2.docs.influxdata.com/v2.0/) 进行工作，我发现这是一个梦想——再加上 Mathematica 和 Wolfram 语言用于原型开发的灵活性和速度，这很快就成为了一个数据争论的梦想工具集。

我找不到任何人这样做的例子或文章，所以我整理了几个有用的 Mathematica Wolfram 语言函数，用于连接 InfluxDB v2 及其 line 协议，可以很容易地为您所用。

# 写入 InfluxDB

以[行协议](https://v2.docs.influxdata.com/v2.0/reference/syntax/line-protocol/)格式准备一些文本数据，并将其存储在一个字符串中，每一行通常以一个\n 结尾。举个例子，

```
"measurementdata,SID=194,SID=5755 w1=7900 1593523543000000000\measurementdata,SID=194,SID=5755 w1=7905 1593523555000000000\n"
```

现在将它传递给下面的示例函数，该函数在准备好的 HTTPRequest[]上调用 URLExecute[]，该准备好的 http request[]包含与 InfluxDB v2 的 API 交互的必要信息。

```
postToInflux[textdata_]:=Module[
{result},
result = URLExecute @ HTTPRequest[
         "YOUR-MACHINE-LOCATION:9999/api/v2/write",
        <| 
        "Method" -> "POST",
        "Headers" -> {"Authorization" -> "Token YOUR-ACCESS-TOKEN"},
        "Query" -> {"org"->"YOUR-ORG","bucket"->"YOUR-BUCKET"},
        "Body" -> textdata
        |>
        ];
Return[result];
]
```

确保在默认的 InfluxDB UI 左上角的 Load Data -> Tokens 下插入您的特定身份验证令牌。此外，关于认证令牌的更多细节可以在[这里](https://v2.docs.influxdata.com/v2.0/security/tokens/)找到。

通常可以成功地批量处理几千个条目，我通常使用一个驱动函数来调用 postToInflux[]，它会传递大约 4000 个条目，这取决于有多少个标签和值。这由你决定。

# 从 InfluxDB 读取 DB

在 InfluxDB 上执行查询是一个类似的操作，下面是一个计算特定桶中测量次数的示例。

```
countItems[]:=Module[
{result},
result = URLExecute @ HTTPRequest[
    "YOUR-MACHINE-LOCATION:9999/api/v2/query",
    <| "Method" -> "POST",
       "Headers" -> {"Authorization" -> "Token YOUR-ACCESS-TOKEN",
                     "Accept"        -> "application/csv",
                     "Content-type"  -> "application/vnd.flux"},
       "Query" -> {"org"->"YOUR-ORG","bucket"->"YOUR-BUCKET"},
       "Body" -> "from(bucket: \"YOUR-BUCKET\") |> range(start: -30d) |> count(column: \"_value\")"
    |>];Return[result];
]
```

函数体可以包含任何查询，我将让您根据自己的情况修改函数。 [Flux 语言](https://v2.docs.influxdata.com/v2.0/reference/flux/)本身非常强大，但是这个函数应该允许您传递几乎任何查询，并允许您适当地处理结果。

我希望它们对你有用。