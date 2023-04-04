# 语义标签如何影响 SEO？

> 原文：<https://blog.devgenius.io/how-semantic-tags-affect-seo-89c7b1992440?source=collection_archive---------9----------------------->

## 这篇文章不是关于 SEO 的。

> 这篇文章是关于如何与 SEO 和 HTML 交朋友的。

![](img/b62d666eacf43a713046486af7056cf5.png)

# meta 标签是 SEO 的重要组成部分。

Meta 标签定义了一个关于 HTML 的元数据。关于如何在搜索引擎优化中元标签的影响是有规则的。

## 标题和描述

Title meta 标签定义了页面的标题，同时 description 定义了页面的描述。

**温馨提示:**

1.  标题/描述必须包含与页面内容相关的信息。
2.  标题长度小于大 **50** 和小 **70** 符号，描述的最大长度为 155 个符号。

# 2)图像没有你理解的那么明显

图像对于 SEO 也非常重要，除了 alt 属性之外，图像可以告诉搜索机器人更多的信息。

**提示**:

1.  搜索机器人不索引 CSS 中的图像，而是使用 HTML 中的图像。换句话说，尽量使用 **img** 标签，而不是**背景图像**。
2.  使用**图片**标签来提高您的流量。搜索机器人试图分析每个图像的元数据，因为它包含图像的标题和描述。
3.  还在可能的地方到处使用 **srcset** 属性。

# 3)使用 JSON-LD

JSON-LD -是一个保存 W3C 编写的规则的 JSON，正如你所理解的，Google Bot 为 JSON-LD 准备数据。

```
{
  "@context": "[https://json-ld.org/contexts/person.jsonld](https://json-ld.org/contexts/person.jsonld)",
  "@id": "[http://dbpedia.org/resource/John_Lennon](http://dbpedia.org/resource/John_Lennon)",
  "name": "John Lennon",
  "born": "1940-10-09",
  "spouse": "[http://dbpedia.org/resource/Cynthia_Lennon](http://dbpedia.org/resource/Cynthia_Lennon)"
}
```

> *例句片段官方*[*docs*](https://json-ld.org/)*。*

```
<script type="application/ld+json">
 {
  "@context": "https://schema.org/",
  "@type": "Recipe",
  "name": "Party Coffee Cake",
   "author": {
    "@type": "Person",
    "name": "Mary Stone"
   },
  "datePublished": "2018-03-10",
  "description": "This coffee cake is awesome and perfect for parties.",
  "prepTime": "PT20M"
 }
</script>
```

这段代码展示了如何在 HTML 中嵌入 JSON-LD，但是你必须明白:

> 当您的内容是静态的并且可能不会改变时，请使用 JSON-LD。

# 4)编写机器人. txt

**robots.txt** 只是 **txt** 文件，包含搜索 Bot 索引更用心的页面。

*   `user-agent`:规则将适用的用户代理机器人。
*   `allow`:是要扫描的 URL。
*   `disallow`:禁止扫描的网址。
*   `sitemap`:站点地图的完整 URL 文件。

```
# https://www.robotstxt.org/robotstxt.htmlUser-agent: *Disallow:
```

> *默认来自 CRA 的 robots.txt。*

## 陷阱:

1.  有些浏览器不读 robots.txt
2.  robots.txt 没有唯一的语法，浏览器阅读 robots.txt 的方式不同。了解一下[这个](https://developers.google.com/search/docs/advanced/robots/robots_txt?hl=ru#syntax)。
3.  如果在 robots.txt 中禁用了 URL，仍然可以通过链接从其他站点解析它。

# 5)订阅机制

```
<link rel="alternate" type="application/rss+xml" href="https://example.com/rssfeed">
```

这段代码允许你在网站上订阅，你会收到通知。但是对于 SEO 改进最好的一面是:**你的站点将会被其他站点优先使用**。

# 6)谷歌 Favicon 优先。

这很简单。尝试为你的网站添加你的收藏夹图标。

# 7)语义标签如何影响 SEO

有一个流行的问题:

> 我们用语义标签做什么？

流行的答案是:

1.  使其可读性更强，更易于维护。
2.  ***语义标签对于 SEO 很重要。(?？？)***

## 语义标签对 SEO 很重要？

*谁说他们重要了？
你觉得 section 标签比 div 重要吗？*

## 毫无疑问，除了这一条，它们都很重要。

> J ***ohn(回答)30:07*** *我想我们已经用标题非常简要地讨论过这个问题了。但这里本质上是相似的。对于某些类型的元素，它确实给了我们更多的关于页面内容的上下文。但是像页脚这样的东西并不能给我们更多的信息。所以一定量的语义 HTML 对于 SEO 来说肯定是有意义的。对于可用性和不同浏览器类型的兼容性来说，这无疑是有意义的。但并不一定意味着对 SEO 很重要。*

从中我们能了解到什么？

1.  有些语义标签很重要，有些则不重要。
2.  **语义标签影响可用性和兼容性。**

哪些语义标签真的很重要？好的，我们知道页脚标签不能携带更多的信息，但是其他标签呢？

> *页面中真的只能存在一个* ***h1*** *标签吗？我们来定义一下这个。*

Google ***“如何在 html 中创建标题”*** 让我们发现页面的 HTML 代码，我们会明白 ***h1*** 标签是我们发现的所有页面的主标题和单个标题。

## 这真的很重要。

> “不要做所有的 H1，然后使用 CSS 让它看起来像普通文本，因为我们看到竞争对手的人抱怨这一点。如果用户关闭 CSS 或者 CSS 无法加载，那看起来真的很糟糕。”

正如你所理解的，谷歌搜索机器人真正关心的是 ***h1*** 标签。

好吧，我们知道默认标签(除了语义标签)很重要，但是 ***属性*** 呢？

# 属性。

我们不再讨论**头**标签内的标签和属性，只讨论**体**标签。

## 一个标签。

当你创建一个**一个**标签时，你必须传递 **href** 属性，当你这样做时，谷歌机器人自动通过 URL 检查网站。而如果你添加一个***rel = " no follow "***Google bot 将不会访问该站点。hreflang 还定义了站点的当前语言。如果 **URL** 被阻止，则 **alt** 属性会创建一个替代文本，对于 SEO 来说，这有一个更好的方面，如果用户使用屏幕阅读器，搜索机器人会提升你的站点。

## 其他属性。

1.  郎提高搜索引擎优化
2.  ***咏叹调-**** 也做得到。

# 结论。

1.  我们不知道所有影响搜索引擎优化的标签。
2.  更好的优化会带来更好的 SEO。
3.  有些标签不会影响搜索引擎优化，但会优化代码性能。
4.  还有 **aria-*** 和 **lang** 属性可以使用不分标记。
5.  排在第二位的 JSON-DL 和元数据是标签。
6.  我们不知道语义标签如何影响搜索引擎优化，但我们知道他们这样做。
7.  如果你有能力在 HTML 中使用图像，那就使用它们。不要在 CSS 中使用图像。

# 主要结论。

**CMS** 和 **SEO** 工作人员可以提高你的 **SEO** 而不改变 **HTML** 代码，并且在大多数情况下: **CMS 和 SEO 提高你的站点排名而不需要编程干预。**