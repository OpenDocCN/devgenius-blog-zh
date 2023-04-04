# 代码气味 22 —助手

> 原文：<https://blog.devgenius.io/code-smell-22-helpers-1d5057137ddb?source=collection_archive---------1----------------------->

你需要帮助吗？你要给谁打电话？

![](img/7b03cfc770f0b3e54c7920e3dd530fe6.png)

# 问题

*   可读性
*   最小惊奇原则
*   [双射](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)故障
*   静态方法

[](https://medium.com/dev-genius/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) [## 名字到底是什么？第二部分:康复

### 我们都同意:好名声永远是最重要的。让我们找到他们。

medium.com](https://medium.com/dev-genius/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) 

# 解决方法

1.  找一个合适的名字
2.  如果帮助者是一个库，将所有服务分解为不同的方法。
3.  方法应该总是由对象来完成。[静法](https://medium.com/dev-genius/code-smell-18-static-functions-68d35c15e76)是另一种代码气味。
4.  避免提取[匿名函数的助手。](https://medium.com/dev-genius/code-smell-21-anonymous-functions-abusers-e6f1e3db6a3f)

# 示例代码

## 错误的

注意*静态*方法。

## 对吧

或者我们可以让以前的*助手*无状态，以便重用…

# 侦查

*   代码命名标准应该禁止使用这个名字的类。

# 标签

*   纳米斯

# 结论

这是一个公认的文化名称。而且开发商也不愿意放过旧习惯。

我们必须意识到这种名字给我们带来的伤害。

# 关系

[](https://medium.com/dev-genius/code-smell-18-static-functions-68d35c15e76) [## 代码味道 18 —静态函数

### 又一个全球接入加上懒惰。

medium.com](https://medium.com/dev-genius/code-smell-18-static-functions-68d35c15e76) [](https://medium.com/dev-genius/code-smell-21-anonymous-functions-abusers-e6f1e3db6a3f) [## 代码气味 21 —匿名函数滥用者

### 函数，lambdas，闭包。所以高阶，非声明，和热。

medium.com](https://medium.com/dev-genius/code-smell-21-anonymous-functions-abusers-e6f1e3db6a3f) 

# 更多信息

[](https://medium.com/dev-genius/how-to-decouple-a-legacy-system-b4ea10db2642) [## 如何分离遗留系统

### 改进遗留代码的练习

medium.com](https://medium.com/dev-genius/how-to-decouple-a-legacy-system-b4ea10db2642) 

这篇文章是 CodeSmell 系列的一部分

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)