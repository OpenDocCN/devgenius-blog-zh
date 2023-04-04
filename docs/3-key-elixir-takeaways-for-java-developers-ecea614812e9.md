# Java 开发人员的 3 个关键灵丹妙药

> 原文：<https://blog.devgenius.io/3-key-elixir-takeaways-for-java-developers-ecea614812e9?source=collection_archive---------15----------------------->

## 你从面向对象的函数式编程中需要什么

![](img/75f64e3869ee4b6b4ad274b22cf7a553.png)

丹·法雷尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

研究表明了函数式编程是如何击败 OOP 的。我们不能改变事实，但我们可以在日常编码中加入习惯。

我将介绍 Elixir 的 3 个关键要点，它们将提高您的 Java 编码技能。

# 1.平静的

类似于[流水线](https://elixirschool.com/en/lessons/basics/pipe-operator/)，我们在 Java 中有[流](https://www.baeldung.com/java-8-streams-introduction)。我们很容易读懂代码，只需按顺序排列函数，这就是流水线的作用。我们也可以在 Java 中通过使用流来做到这一点。

> Java 流 API

```
myList
    .stream()
    .filter(s -> s.startsWith("c"))
    .map(String::toUpperCase)
    .sorted()
    .forEach(System.out::println);
```

> 酏剂流水线

```
def pp(x) do 
	:io_lib.format("~p", [x])
	|> :lists.flatten
	|> :erlang.list_to_binary
end
```

# 2.小功能

从 Elixir 中学到的一点就是用代码编写小函数。超过这个限制意味着你需要另一个函数。该功能的测试也将是最少的，所以节省开发时间。

# 3.错误处理

Elixir 完全是关于'*让它崩溃'*范式的，所以错误处理占有特殊的位置。

乔·阿姆斯特朗是 Elixir 编码领域的大玩家，所以他写了一篇关于错误处理的简单文章。我们都对遗漏错误处理感到内疚。这里的关键点是始终为开发人员和用户提供不同的错误消息。

Elixir 在函数中提供了模式匹配，因此您可以使用相同的函数名添加 *nil* 检查。在 Java 中，我们应该总是在前面放置空检查，或者用可选的进行检查。