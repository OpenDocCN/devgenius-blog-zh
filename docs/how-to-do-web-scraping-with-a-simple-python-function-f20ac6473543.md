# 如何用一个简单的 Python 函数进行 Web 抓取

> 原文：<https://blog.devgenius.io/how-to-do-web-scraping-with-a-simple-python-function-f20ac6473543?source=collection_archive---------0----------------------->

## 5 行代码，互联网就是你的了。

![](img/d425592043c4b2ab5d82c52953e71670.png)

[图片来自 Freepik 上的故事集](https://fr.freepik.com/vecteurs-libre/www-concept-illustration_8426454.htm)

只需点击几下鼠标，就能访问所有网络数据。

**有趣的是，一旦你了解 Python，做网络搜集也很容易。**

但是有几个库和解决方案允许你使用 Python。我已经测试了一些，以下是我的建议。

## 不一定要复杂。

我记得我第一次做网页抓取的时候。我正在按照一个教程从亚马逊检索消费者评论，那时，我正在使用 **scrapy** 。

这个 Python 抓取工具有一个复杂的结构，用称为“蜘蛛”的对象来浏览网页并提取信息。

使用它是乏味的。至少对于我所需要的。

我只想从网页中检索特定的字段，以获得消费者的反馈。事实上，我觉得我在用一把大锤敲碎一个坚果。

## 你只需要知道适合你需求的库。

谢天谢地，一年后，在做一个将维基百科页面转换成播客 的小项目时，我发现了另一个 Python 库。

那个真的很好用，我也没要求更多。

是 [**bs4**](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) ，代表**美汤 4** 。

有了这个库的`BeautifulSoup`对象，再加上`requests`库，您就拥有了开始 web 抓取所需的一切。

## 5 行代码，仅此而已！

正如所承诺的，下面是 5 行代码，定义了一个 Python 函数来抓取网页。

> 它是如何工作的？

假设您有一个 web 页面的 url，例如`link = "http://medium.com"`，并且您想从该页面获取数据。

然后在定义了`link`变量之后运行`soup = parse(link)`，你的`soup`将会是一个**对象，包含你的目标网页**的所有数据。

## **下一步:处理数据**

接下来要做的是解析检索到的 HTML 内容。

根据你想要什么，说明会有所不同。但是这里有一些你应该知道的常识。

*   `soup.findall("p")`让您查找 HTML 文档中段落的所有元素。
*   `soup.find(id="identifier")`让您找到文档中 id 为“identifier”的元素(如果有)。
*   一般来说，您将使用`soup.find(SOMETHING)`来查找**一个特定的元素**和`soup.findall(SOMETHING)`来查找**所有与您正在寻找的特征相匹配的元素**。

# 结论

现在你可以毫不费力地从任何网页上获取数据。

当然，提取你想要得到的确切信息仍然需要一些精力。你必须深入到 HTML 内容中，找到 Python 指令来筛选你想要的数据。

—

## 看看你能用网络抓取技巧做些什么…

*   搜集在线词典📕

[](https://medium.com/mlearning-ai/i-scraped-an-online-dictionary-37577876eab5) [## 我在网上搜集了一本字典

### 做了自己的数据集用于数据分析。

medium.com](https://medium.com/mlearning-ai/i-scraped-an-online-dictionary-37577876eab5) 

*   或者进入知识的海洋🌎

[](/listening-to-wikipedia-python-tutorial-8c65bb3b449a) [## 收听维基百科(Python 教程)

### 如果你宁愿听维基百科而不是读它们，并且你已经安装了 Python，那么你就来了…

blog.devgenius.io](/listening-to-wikipedia-python-tutorial-8c65bb3b449a) 

**感谢**的阅读！
如果你喜欢这个故事，请随时**评论，**👏**或者关注我**！

[](https://medium.com/@jmpion/membership) [## 通过我的推荐链接加入 Medium-Jean Meunier-Pion

### 阅读 Jean Meunier-Pion 的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接…

medium.com](https://medium.com/@jmpion/membership)