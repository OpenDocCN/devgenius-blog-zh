# 代码气味 88 —惰性初始化

> 原文：<https://blog.devgenius.io/code-smell-88-lazy-initialization-48e1b95a949e?source=collection_archive---------1----------------------->

![](img/60b7f12dac09d1a3abfde2cbc53a8a9a.png)

萨姆·索罗门在 [Unsplash](https://unsplash.com/s/photos/lazy) 上拍摄的照片

*又一个过早优化模式*

> *TL；DR:不要使用延迟初始化。请改用对象提供程序。*

# 问题

*   令人惊讶的副作用
*   过早优化
*   快速违规失败
*   实现耦合
*   最不令人惊讶的违反原则
*   无效用法
*   易变性
*   事务性和多线程应用问题
*   [调试问题](https://martinfowler.com/bliki/LazyInitialization.html)

# 解决方法

1.  用第一类对象注入职责

# 示例代码

## 错误的

```
class Employee
  def emails
    @emails ||= []
  end def voice_mails
    @voice_mails ||= []
  end
end
```

## 对吧

```
class Employee
  attr_reader :emails, :voice_mails def initialize
    @emails = []
    @voice_mails = []
  end
end
#We can also inject a design pattern to externally deal
#with voice_mails so we can mock it in our tests
```

# 侦查

*   惰性初始化是一种常见的模式，用于检查未初始化的变量。
*   检测它们应该很简单。

# 标签

*   过早优化

# 结论

[单例模式](https://dev.to/mcsee/singleton-the-root-of-all-evil-50bh)是另一种经常与惰性初始化结合的反模式。

我们必须避免过早的优化。如果我们有真正的性能问题，我们应该使用代理、门面或更独立的解决方案。

[](/code-smell-32-singletons-d6d7b0a9f251) [## 代码气味 32 —单件

### 世界上最常用和最著名的设计模式正在给我们带来巨大的伤害。

blog.devgenius.io](/code-smell-32-singletons-d6d7b0a9f251) [](/code-smell-12-null-64fbd7792a7c) [## 代码气味 12 —空

### 程序员使用 Null 作为不同的标志。它可以提示缺席、未定义的值、错误等。

blog.devgenius.io](/code-smell-12-null-64fbd7792a7c) 

# 更多信息

*   [维基百科](https://en.wikipedia.org/wiki/Lazy_initialization)
*   马丁·福勒

> *我们要停止为程序员优化，开始为用户优化。*

杰夫·阿特伍德

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)