# 代码气味 16 —连锁反应

> 原文：<https://blog.devgenius.io/code-smell-16-ripple-effect-c5a2db81f37e?source=collection_archive---------1----------------------->

微小的变化会产生意想不到的问题。

![](img/faa6e9543a8242143c8320fd59a4e960.png)

[杰克·廷德尔](https://unsplash.com/@jtindall)在 [Unsplash](https://unsplash.com/s/photos/big-wave) 上的照片

# 问题

*   连接

# 解决方法

1.  解耦。
2.  用测试覆盖。
3.  重构并隔离正在改变的东西。
4.  依赖于接口。

# 例子

*   遗留系统

# 示例代码

## 错误的

## 对吧

# 侦查

*   在问题发生之前发现它们并不容易。[突变测试](https://en.wikipedia.org/wiki/Mutation_testing)和[单点故障](https://en.wikipedia.org/wiki/Single_point_of_failure)的根本原因分析可能会有所帮助。

# 标签

*   遗产

# 结论

处理遗留和耦合系统有多种策略。我们应该在这个问题爆发之前解决它。

# 关系

*   [代码气味 08 —协作的长链](https://medium.com/dev-genius/code-smell-08-long-chains-of-collaborations-68aa0207ddd0)

# 更多信息

*   [如何分离遗留系统](https://medium.com/dev-genius/how-to-decouple-a-legacy-system-b4ea10db2642)

> 建筑是耦合和凝聚之间的张力。

尼尔·福特

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)