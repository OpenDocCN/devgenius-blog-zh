# 漂亮汤的网页抓取——属性和字符串

> 原文：<https://blog.devgenius.io/web-scraping-with-beautiful-soup-attributes-and-strings-2f10f706251b?source=collection_archive---------1----------------------->

![](img/7d382b7898348a01b7f10af82c064f98.png)

照片由[史蒂夫·曾](https://unsplash.com/@stevetsang?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我们可以用美汤从网页上获取数据。

它让我们解析 DOM 并提取我们想要的数据。

在这篇文章中，我们将看看如何用漂亮的汤刮 HTML 文档。

# 操纵属性

我们可以用漂亮的汤来操纵属性。

例如，我们可以写:

```
from bs4 import BeautifulSouptag = BeautifulSoup('<b id="boldest">bold</b>', 'html.parser').b
tag['id'] = 'verybold'
tag['another-attribute'] = 1
print(tag)
del tag['id']
del tag['another-attribute']
print(tag)
```

我们只是在`tag`字典中添加和删除条目来操作属性。

然后第一个`print`语句打印出来:

```
<b another-attribute="1" id="verybold">bold</b>
```

第二个打印:

```
<b>bold</b>
```

# 多值属性

美丽的汤与具有多个值的属性一起工作。

例如，我们可以解析:

```
from bs4 import BeautifulSoupcss_soup = BeautifulSoup('<p class="body bold"></p>', 'html.parser')
print(css_soup.p['class'])
```

然后我们把`[u’body’, u’bold’]`印出来。

在我们将字典转换回字符串之后，所有的值都将被添加:

```
from bs4 import BeautifulSouprel_soup = BeautifulSoup('<p>Back to the <a rel="index">homepage</a></p>', 'html.parser')
rel_soup.a['rel'] = ['index', 'contents']
print(rel_soup.p)
```

将打印出`print`语句:

```
<p>Back to the <a rel="index contents">homepage</a></p>
```

如果我们用 LXML 解析一个带有 n XML 的文档，我们会得到相同的结果:

```
from bs4 import BeautifulSoupxml_soup = BeautifulSoup('<p class="body strikeout"></p>', 'lxml')
print(xml_soup.p['class'])
```

我们仍然得到:

```
['body', 'strikeout']
```

印刷的。

# `NavigableString`

我们可以在标签中获取文本。例如，我们可以写:

```
from bs4 import BeautifulSoupsoup = BeautifulSoup('<b class="boldest">Extremely bold</b>', 'html.parser')
tag = soup.b
tag.string
print(type(tag.string))
```

然后我们得到:

```
<class 'bs4.element.NavigableString'>
```

印刷的。

`tag.string`属性在`b`标签中有一个可导航的字符串。

我们可以通过编写以下代码将其转换为 Python 字符串:

```
from bs4 import BeautifulSoupsoup = BeautifulSoup('<b class="boldest">Extremely bold</b>', 'html.parser')
tag = soup.b
tag.string
unicode_string = str(tag.string)
print(unicode_string)
```

然后 `‘Extremely bold’`被打印。

我们可以用不同的字符串替换可导航的字符串。

为此，我们写道:

```
from bs4 import BeautifulSoupsoup = BeautifulSoup('<b class="boldest">Extremely bold</b>', 'html.parser')
tag = soup.b
print(tag.string)
tag.string.replace_with("No longer bold")
print(tag.string)
```

然后我们看到:

```
Extremely bold
No longer bold
```

印刷的。

# `BeautifulSoup Object`

`BeautifulSoup`对象表示整个解析后的文档。

例如，如果我们有:

```
from bs4 import BeautifulSoupdoc = BeautifulSoup("<document><content/>INSERT FOOTER HERE</document", "xml")
footer = BeautifulSoup("<footer>Here's the footer</footer>", "xml")
doc.find(text="INSERT FOOTER HERE").replace_with(footer)
print(doc)
print(doc.name)
```

然后我们看到:

```
<?xml version="1.0" encoding="utf-8"?>
<document><content/><footer>Here's the footer</footer></document>
```

从第一次`print`调用开始打印。

并且:

```
[document]
```

从第二次`print`调用开始打印。

# 注释和其他特殊字符串

Beautiful Soup 可以解析注释和其他特殊字符串。

例如，我们可以写:

```
from bs4 import BeautifulSoupmarkup = "<b><!--Hey, buddy. Want to buy a used parser?--></b>"
soup = BeautifulSoup(markup, 'html.parser')
comment = soup.b.string
print(type(comment))
print(soup.b.prettify())
```

然后我们可以从带有`soup.b.string`属性的`b`元素中获取注释字符串。

所以第一个`print`叫版画:

```
<class 'bs4.element.Comment'>
```

第二次`print`调用打印:

```
<b>
 <!--Hey, buddy. Want to buy a used parser?-->
</b>
```

# 结论

我们可以操纵属性，用漂亮的汤处理字符串。