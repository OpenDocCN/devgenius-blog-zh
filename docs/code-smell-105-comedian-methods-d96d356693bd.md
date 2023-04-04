# 代码气味 105 —喜剧演员的方法

> 原文：<https://blog.devgenius.io/code-smell-105-comedian-methods-d96d356693bd?source=collection_archive---------7----------------------->

## 使用专业和有意义的名字

![](img/56d74b01ec0c898ff4a7c39c1b6da0bf.png)

斯图尔特·芒罗在 [Unsplash](https://unsplash.com/s/photos/comedy) 上拍摄的照片

> TL；大卫:不要随便或无礼

# 问题

*   可读性
*   非专业工作

# 解决方法

1.选择好的、专业的名字。

# 语境

我们的职业有创造性的一面。

有时我们感到无聊，试着变得有趣。

# 示例代码

## 错误的

```
function erradicateAndMurderAllCustomers();//unprofessional and offensive
```

## 对吧

```
function deleteAllCustomers();//more declarative and professional
```

# 侦查

[X]半自动

我们可以列一个禁忌词的清单。

我们也可以在代码审查中检查它们。

名字是与上下文相关的，所以对于自动过敏者来说这是一个困难的任务。

命名约定应该是通用的，不应该包含文化术语。

# 标签

*   命名

# 结论

在代码中命名事物的方式要专业。

不要试图通过给一个变量起一个愚蠢的名字来成为一个喜剧演员。

您应该编写产品代码，以便未来的软件开发人员(甚至是您)能够轻松理解。

# 关系

[](/code-smell-38-abstract-names-c2306d0846ca) [## 代码气味 38 —抽象名称

### 避免太抽象的名字。名字应该有真实的意义

blog.devgenius.io](/code-smell-38-abstract-names-c2306d0846ca) 

# 更多信息

[](/what-exactly-is-a-name-part-i-the-quest-b812a4b1e0bf) [## 名字到底是什么？—第一部分:探索

### 我们都同意:好名声永远是最重要的。让我们找到他们。

blog.devgenius.io](/what-exactly-is-a-name-part-i-the-quest-b812a4b1e0bf) 

> Gnome 的这种“用户是白痴，被功能迷惑”的心态是一种疾病。如果你觉得你的用户是傻逼，只有傻逼才会用。

*莱纳斯·托沃兹*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)