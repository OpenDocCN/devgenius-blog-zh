# 代码气味 109-自动属性

> 原文：<https://blog.devgenius.io/code-smell-109-automatic-properties-8d7b3a956aff?source=collection_archive---------1----------------------->

## 如果您组合 4 种代码气味会发生什么？

![](img/61439a25501c25f7db38a37542cf861b.png)

照片由 [Kony](https://unsplash.com/@konyxyzx) 在[un plash](https://unsplash.com/s/photos/shoot)上拍摄

> TL；灾难恢复:避免获取者，避免设置者，避免元编程。想想行为。

# 问题

*   信息隐藏违规
*   [易变性](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e)
*   [失效快速原则](https://codeburst.io/fail-fast-3f3f036032b0)违反
*   设置属性时出现重复代码

# 解决方法

1.[拆下自动沉降器和吸气剂](/refactoring-001-remove-setters-e5acdf60462c)

# 语境

设置者和获取者是糟糕的行业惯例。

许多 IDEs 都喜欢这种代码气味。

一些语言为构建贫血模型和 dto 提供了明确的支持。

# 示例代码

## 错误的

```
class Person
{
  public string name 
  { get; set; }
}
```

## 对吧

```
class Person
{
 private string name 

 public Person(string personName)
 {
 name = personName;
 //imutable
 //no getters, no setters
 }
}
```

# 侦查

[十]自动

这是一种语言特征。

我们应该避免不成熟的语言，或者禁止他们最糟糕的做法。

# 标签

*   包装

# 结论

在暴露我们的财产之前，我们需要仔细考虑。

第一步是停止思考属性，只关注行为。

# 关系

[](/code-smell-28-setters-5b0e764049aa) [## 代码气味 28-设置器

### 初级程序员做的第一个练习。IDEs、教程和高级开发人员一直在教他们这种反模式。

blog.devgenius.io](/code-smell-28-setters-5b0e764049aa) [](/code-smell-68-getters-68571a0f7fa8) [## 代码气味 68-吸气剂

### 得到东西是普遍和安全的。但这是一种非常糟糕的做法。

blog.devgenius.io](/code-smell-68-getters-68571a0f7fa8) [](/code-smell-70-anemic-model-generators-99aca77391fc) [## 代码气味 70-贫血模型生成器

### TL；DR:请勿创建贫血对象。更不用说自动工具了。

blog.devgenius.io](/code-smell-70-anemic-model-generators-99aca77391fc) [](/code-smell-40-dtos-ca35f5d8f7c9) [## 代码气味 40—dto

### dto 被广泛使用，并且“解决”了真正的问题，是吗？

blog.devgenius.io](/code-smell-40-dtos-ca35f5d8f7c9) [](/code-smell-01-anemic-models-f9fb5a1323b3) [## 代码气味 01-贫血模型

### 您的对象是一堆没有行为的公共属性。

blog.devgenius.io](/code-smell-01-anemic-models-f9fb5a1323b3) 

# 更多信息

[](https://www.w3schools.com/cs/cs_properties.php) [## C#属性(获取和设置)

### 在我们开始解释属性之前，您应该对“封装”有一个基本的了解。的意思是…

www.w3schools.com](https://www.w3schools.com/cs/cs_properties.php) [](https://codeburst.io/laziness-i-meta-programming-34ef4abdafc6) [## 懒惰一:元编程

### 元编程是神奇的。这就是我们不应该使用它的主要原因。对…有许多可怕的后果

codeburst.io](https://codeburst.io/laziness-i-meta-programming-34ef4abdafc6) [](https://codeburst.io/lazyness-ii-code-wizards-18cc5672b642) [## 惰性二:代码向导

### 代码生成器完成了我们的艰苦工作。但是我们不再需要它们了。

codeburst.io](https://codeburst.io/lazyness-ii-code-wizards-18cc5672b642) [](/refactoring-001-remove-setters-e5acdf60462c) [## 重构 001-删除设置器

### 设置器违反了不变性，增加了意外耦合

blog.devgenius.io](/refactoring-001-remove-setters-e5acdf60462c) [](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) [## 变种人的邪恶力量

### 变异就是进化。它是由查尔斯·达尔文爵士提出的，我们在软件行业中使用它。但是有些事情…

codeburst.io](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) [](https://codeburst.io/fail-fast-3f3f036032b0) [## 快速失败

### 失败是时髦的。赚钱比思考容易得多，失败不是耻辱，让我们把这个想法带到我们的…

codeburst.io](https://codeburst.io/fail-fast-3f3f036032b0) 

> 没有什么比在紧迫的截止日期下工作，并且一边走一边花时间清理更难的了。

*肯特·贝克*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程大报价

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) 

*更多内容尽在*[*blog . dev genius . io*](http://blog.devgenius.io)*。*