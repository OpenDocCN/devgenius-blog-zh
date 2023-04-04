# 好老的配置方法探索。XML 为什么痛苦？YAML 为什么有趣？

> 原文：<https://blog.devgenius.io/why-we-use-xml-today-71c567b87c66?source=collection_archive---------5----------------------->

## 在 YAML 面前捍卫 XML 的最后一战

![](img/47d924f7daf593589bccf89a5eeab890.png)

由[詹姆斯·庞德](https://unsplash.com/@jamesponddotco?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

[](https://blog.frankel.ch/defense-xml/) [## 为 XML 辩护

### 当我开始职业生涯时，XML 无处不在。Java JAR 文件中的元信息——清单——遵循一个…

blog.frankel.ch](https://blog.frankel.ch/defense-xml/) 

这篇文章启发了我，让我分享一下我对 XML 的看法。所以我就用这个故事分享一下我的经历。

> 我们有 JSON 和 YAML，为什么还要用这种标记语言呢？

主要从事前端开发，XML 对我来说很陌生。只在大学看过，没那么懂。

在使用 Spring 框架进行后端开发之后，我看到了 XML 的好处和用途。以前使用过 JSON，但是 XML 似乎更冗长。

# 使用方便

从事一个在 Spring 上运行的企业项目，让我开始编写 XML。不能说我马上就喜欢上了。我被属性、元素、名称空间搞糊涂了。使用它几周后，我对它很有信心。

另一方面， [YAML](https://yaml.org/) 的学习曲线[有点陡峭](https://en.wikipedia.org/api/rest_v1/page/graph/png/Learning_curve/0/d5319f58b5392f830a0a3406a14a65d110da8784.png)。

我们必须与 YAML 和斯瓦格一起创建 SDK。理解语法真的很难。有了模式( [XSD](https://www.w3schools.com/xml/schema_intro.asp) )，你可以轻松地编写 XML。这就是为什么 YAML 至少对我来说如此遥远。

# 模式中的文档

XML 的整个结构都在模式中。任何需要的东西都可以在那里找到。有了它，配置 Spring 或任何其他配置都变得更加容易。

比起 Spring 的注释驱动配置，我甚至更喜欢 XML。通过一个文档中的配置，您可以清楚地看到依赖项并在其中导航。

## 询问

在 JS 中查询 JSON 相对容易。它完全可以与 JavaScript 对象互换。XML 有自己的 XPath，学习起来相当简单。

# 冗长的

JSON 和 YAML 没有 XML 那么冗长。一切都整齐地打包在 XML 中。属性可以描述元素，模式有助于“语法”。

弗兰克尔先生指出了一些我想都没想过的事情。JSON 没有注释，所以他们求助于添加`_comment`作为属性。YAML 在标准和定义布尔方面也很糟糕。

# 摘要

这是一个相当新的领域，但正如 Nicolas 总结的那样，我们更擅长使用久经考验的技术。炒作驱动的技术并不总是最好的选择。XML 有标准，支持所有主要语言。

每个初级 Java 开发人员都应该读一读 Nicolas 的这篇博客，看看我们不是在做一些古老的技术。

[](https://blog.frankel.ch/defense-xml/) [## 为 XML 辩护

### 当我开始职业生涯时，XML 无处不在。Java JAR 文件中的元信息——清单——遵循一个…

blog.frankel.ch](https://blog.frankel.ch/defense-xml/)