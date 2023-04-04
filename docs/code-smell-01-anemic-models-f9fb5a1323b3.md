# 代码气味 01——贫血模型

> 原文：<https://blog.devgenius.io/code-smell-01-anemic-models-f9fb5a1323b3?source=collection_archive---------2----------------------->

## 你的对象是一堆没有行为的公共属性。

![](img/4dd55b02fa3f4174f14499bbad9f1e3a.png)

Stacey Vandergriff 在 [Unsplash](https://unsplash.com/s/photos/bunny?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

协议为空(带有[设置器](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3) / [获取器](https://medium.com/dev-genius/nude-models-part-ii-getters-b039e5ad3427))。

如果我们让领域专家描述一个实体，他/她很难说出它是*“一堆属性”*。

# 问题

*   没有封装。
*   没有[映射](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)到现实世界的实体。
*   重复代码
*   [联轴器](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04)

# 解决方法

1)找责任。

2)保护你的属性。

3)隐藏实现。

4)委派

# 例子

*   DTOs

# 示例代码

## 错误的

## 对吧

# 侦查

检测可以通过复杂的 linters 忽略 setters 和 getters 并计算真实行为方法来实现自动化。

# 也被称为

*   数据类

# 标签

*   贫血的
*   OOP 作为数据
*   包装
*   设置器/获取器
*   易变性

# 更多信息

*   [维基百科](https://en.wikipedia.org/wiki/Anemic_domain_model)
*   [重构大师](https://refactoring.guru/es/smells/data-class)
*   [裸体模特——第一部分:设定者](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3)
*   裸体模特——第二部分:吸粉者
*   [如何分离遗留系统](https://medium.com/dev-genius/how-to-decouple-a-legacy-system-b4ea10db2642)

本文是 CodeSmell 系列的一部分。

[](https://mcsee.medium.com/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

mcsee.medium.com](https://mcsee.medium.com/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)