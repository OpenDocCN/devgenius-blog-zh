# Web 抓取和漂亮的汤——相等、复制和解析文档的一部分

> 原文：<https://blog.devgenius.io/web-scraping-with-beautiful-soup-equality-copies-and-parsing-part-of-a-document-570a8531d1b8?source=collection_archive---------1----------------------->

![](img/e8103be4312b66fc314815728a89ee2b.png)

[Jade Aucamp](https://unsplash.com/@jadeaucamp?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我们可以用美汤从网页上获取数据。

它让我们解析 DOM 并提取我们想要的数据。

在这篇文章中，我们将看看如何用漂亮的汤刮 HTML 文档。

# 比较对象是否相等

我们可以比较对象是否相等。

例如，我们可以写:

```
from bs4 import BeautifulSoup
markup = "<p>I want <b>pizza</b> and more <b>pizza</b>!</p>"
soup = BeautifulSoup(markup, 'html.parser')
first_b, second_b = soup.find_all('b')
print(first_b == second_b)
print(first_b.previous_element == second_b.previous_element)
```

然后我们第一个`print`打印`True`，因为第一个`b`元素和第二个元素具有相同的结构和内容。

第二个`print`打印`False`，因为每个`b`元素的前一个元素不同。

# 复制漂亮的汤类物品

我们可以复制漂亮的汤品。

我们可以使用`copy`库来做到这一点:

```
from bs4 import BeautifulSoup
import copymarkup = "<p>I want <b>pizza</b> and more <b>pizza</b>!</p>"
soup = BeautifulSoup(markup, 'html.parser')
p_copy = copy.copy(soup.p)
print(p_copy)
```

该副本被认为与原件相同。

# 仅解析文档的一部分

例如，我们可以写:

```
from bs4 import BeautifulSoup, SoupStrainerhtml_doc = """<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""
soup = BeautifulSoup(html_doc, 'html.parser')
only_a_tags = SoupStrainer("a")
only_tags_with_id_link2 = SoupStrainer(id="link2")def is_short_string(string):
    return string is not None and len(string) < 10
only_short_strings = SoupStrainer(string=is_short_string)print(only_a_tags)
print(only_tags_with_id_link2)
print(only_short_strings)
```

我们只能用`SoupStrainer`选择我们想要的元素。

选择可以用选择器来完成，或者我们可以传入一个`id`，或者传入一个函数来完成选择。

然后我们看到:

```
a|{}
None|{'id': u'link2'}
None|{'string': <function is_short_string at 0x00000000036FC908>}
```

印刷的。

# 结论

我们可以解析文档的一部分，比较解析后的对象是否相等，用漂亮的汤复制对象。