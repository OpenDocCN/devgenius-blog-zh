# 鲜为人知但非常有用的科特林函数[第二部分]

> 原文：<https://blog.devgenius.io/less-known-but-very-useful-kotlin-functions-part-ii-673c3457f5a2?source=collection_archive---------3----------------------->

![](img/99e80ee3b13bfe7d76365b27f4dfa0b6.png)

虽然这篇文章是独立的，但我鼓励你去读读第一部分的[*】*](https://medium.com/@thegoldycopythat/less-known-but-very-useful-kotlin-functions-part-i-680fa6889d37)*，如果你还没有的话，因为我也在研究这些函数的本质。*

## *[joinToString()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.sequences/join-to-string.html)*

*`joinToString()`用于将集合转换为字符串。该方法将前缀、后缀、分隔符等作为可选参数，阅读[文档](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.sequences/join-to-string.html)了解更多信息。*

*例如，你有一个股票市场应用程序，每当用户将一支股票添加到他们的观察列表中时，你就将其放入一个集合中，但你的后端不接受集合，他们希望你将股票作为一个 josn 数组字符串发送。*

*不带`joinToString()`*

```
*val builder = StringBuilder()
builder.append("[")
for (i in stocks.*indices*) {
    builder.append(stocks[i])
    if(i != stocks.size - 1) {
        builder.append(", ")
    }
}
builder.append("]")
return builder.toString()*
```

*与`joinToString()`*

```
*stocks.*joinToString*(prefix = "[", postfix = "]")*
```

## *[groupBy()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/group-by.html)*

*`groupBy()`用于根据类别对集合的元素进行分组，它返回一个类别映射作为键，一个列表作为值。*

*例如，在你的股票市场应用程序中，你想把增值的股票和亏损的股票分别分组，你想定义一个类别:增值股票的获利者和亏损股票的亏损者。*

*没有`groupBy()`*

```
*val mapByCategory = HashMap<String, List<Player>>()
val gainers = *mutableListOf*<Stock>()
val losers = *mutableListOf*<Stock>()
for (i in 0..stocks.size - 1) {
    if (stocks[i].gainedToday) {
        gainers.add(stocks[i])
    } else {
        losers.add(stocks[i])
    }
}
map.put("gainers", losers)
map.put("losers", losers)
return map*
```

*用`groupBy()`*

```
*val mapbyCategory = stocks.*groupBy* **{** if (**it**.gainedToday) "gainers" else "losers" **}***
```

## *[减去()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/subtract.html)*

*`subtract()`用于获取该集合中存在而其他集合中不存在的元素。*

*例如，在你的股票市场应用程序中，你想知道投资者 1 在交易什么股票，而投资者 2 没有，反之亦然*

*不带`subtract()`*

```
*val subtractedList = *mutableListOf*<String>()
for (stock in investor1Stocks) {
    if (!investor2Stocks.contains(stock)) {
        subtractedList.add(stock)
    }
}
return subtractedList*
```

*用`subtract()`*

```
*val difference = investor1Stocks.*subtract*(investor2Stocks)*
```

## *[相交()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/intersect.html)*

*`intersect()`用于了解两个集合中的共同元素。*

*例如，在你的股票市场应用程序中，你想知道两个投资者之间的普通股*

*不带`intersect()`*

```
*val commonStocks = *mutableListOf*<String>()
for (stock in investor1Stocks) {
    if(investor2Stocks.contains(stock)){
        topStocks.add(stock)
    }
}
return commonStocks*
```

*用`intersect()`*

```
*val commonStocks = investor1Stocks.*intersect*(investor2Stocks)*
```

## *[联盟()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/union.html)*

*`union()`用于了解两个集合中的所有唯一元素，例如在您的股票市场应用程序中，您想要了解三个投资者交易的所有股票。*

*不带`union()`*

```
*val unionList = *mutableListOf*<String>()
for (stock in investor1Stocks) {
    if (!unionList.contains(stock)) {
        unionList.add(stock)
    }
}
for (stock in investor2Stocks) {
    if (!unionList.contains(stock)) {
        unionList.add(stock)
    }
}
for (stock in investor3Stocks) {
    if (!unionList.contains(stock)) {
        unionList.add(stock)
    }
}
return unionList*
```

*与`union()`*

```
*val allShare = investor1Stocks.*union*(investor2Stocks).*union*(investor3Stocks)*
```

*[](https://medium.com/@thegoldycopythat/less-known-but-very-useful-kotlin-functions-part-i-680fa6889d37) [## 鲜为人知但非常有用的科特林函数[第一部分]

### 下面是一些可以让你的程序员生活轻松很多的函数，因为几乎所有的程序员都发现了它们的用处…

medium.com](https://medium.com/@thegoldycopythat/less-known-but-very-useful-kotlin-functions-part-i-680fa6889d37)*