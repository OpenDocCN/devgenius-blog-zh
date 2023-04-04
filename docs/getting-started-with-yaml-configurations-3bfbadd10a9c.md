# YAML 配置入门。

> 原文：<https://blog.devgenius.io/getting-started-with-yaml-configurations-3bfbadd10a9c?source=collection_archive---------11----------------------->

## 开始 YAML 配置需要知道的一切。

![](img/1dd2a99e30ddb788c22b29454150087b.png)

[Unsplash](https://unsplash.com/s/photos/programming-language?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [AltumCode](https://unsplash.com/@altumcode?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

**YAML** 是一种人类可读的数据序列化语言，用于以 **key: value** 格式存储信息，类似于 JSON。如果你已经熟悉 JSON，你应该知道这个 **key: value** 格式。但是如果你不熟悉 JSON，那么不要担心，在这篇文章中，你会很容易理解 YAML 配置。

*YAML 代表什么？上面写着* ***YAML 不是标记语言*** *。*

与其他语言相比，YAML 很容易配置，也非常容易阅读和理解。尽管 YAML 更容易配置，但这些文件可以在 DevOps 中做很多事情。

您可以使用**保存 YAML 配置文件。yml** 或**。yaml** 分机。让我们创建一个简单的 YAML 文件。

```
# student informationname: "john"location: 'USA'age: 21male: true
```

在这里，你可以看到，我们以 **key: value** 格式存储信息，我们还可以在 YAML 中存储不同类型的数据，如字符串、整数、布尔等。对于字符串类型，可以使用单引号或双引号。当你需要注释掉一些东西的时候，在注释的开头使用一个 **#** ，如上。

让我们关注下面的例子。

```
employee:
  name: David
  age: 35
  job: writer
  male: true
  languages:
    - English
    - French
    - Chinese
```

我们创建了一个 *employee* 对象，你可以看到，使用 YAML，我们可以存储更多与对象相关的信息，代码清晰易读。在 YAML 配置中，**缩进**更重要，它帮助你定义那个对象的子对象的级别和范围。所以一开始留一个空格，进入不同层次后再逐渐增加。另外，在代码的底部，你会看到另一种数据类型，叫做**列表**，它允许你在同一个字段中存储多个值。

在下面的例子中，你可以看到如何在列表项中存储一个对象。

在列表项中存储对象有不同的方法。你可以在你的浏览器上花上几分钟很容易地找到它们，并且你可以自由地使用任何你喜欢的方法。

接下来我想强调的是，您可以在一个 YAML 文件中创建多个 YAML 配置。参见下面的例子。

上面的例子是我们在 Kubernetes 中使用的示例代码，你可以看到三个破折号`— — -`，它表示 YAML 文件的分离。当把 YAML 文件合并成一个文件时，YAML 的这个功能非常有用。

# 结论

在本文中，我想让您从 YAML 配置开始，这些 YAML 对于任何希望从 Kubernetes 和 DevOps 开始的人都很重要。