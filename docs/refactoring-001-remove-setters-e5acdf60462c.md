# 重构 001 —移除设置器

> 原文：<https://blog.devgenius.io/refactoring-001-remove-setters-e5acdf60462c?source=collection_archive---------4----------------------->

## Setters 违反了不变性并增加了意外耦合

![](img/0be77f59ffbd6a15d73f2de56713d9a2.png)

图片由[com break](https://pixabay.com/users/comfreak-51581/)来自 [Pixabay](https://pixabay.com/)

> TL；DR:使你的属性私有以利于可变性

# 解决的问题

*   易变性
*   setXXX()违反了良好的命名策略，因为它在[映射器](https://medium.com/@mcsee/what-is-software-9a78c1172cf9)上不存在
*   意外[耦合](https://medium.com/@mcsee/coupling-the-one-and-only-software-design-problem-869e293a9f04)

# 相关代码气味

[](/code-smell-28-setters-5b0e764049aa) [## 气味代码 28 —设定器

### 初级程序员做的第一个练习。ide、教程和高级开发人员不断教他们这种反模式。

blog.devgenius.io](/code-smell-28-setters-5b0e764049aa) [](/code-smell-01-anemic-models-f9fb5a1323b3) [## 代码气味 01——贫血模型

### 你的对象是一堆没有行为的公共属性。

blog.devgenius.io](/code-smell-01-anemic-models-f9fb5a1323b3) 

# 步伐

1.找到 setters 的用法

2.如果要设置基本属性，请将它们移动到构造函数中，并移除该方法

3.如果你需要改变一个偶然属性，它不是一个设置器。删除 setXXX 前缀

# 示例代码

## 以前

## 在...之后

# 类型

[X]半自动

我们应该用 ide 检测 setters(除非他们使用元编程)。

如果我们有良好的覆盖率，我们也可以删除它们，看看哪些测试失败了。

# 标签

*   易变性

# 相关重构

*   移除吸气剂
*   在构造函数中传递基本属性
*   初始化构造函数中的基本属性

本文是重构系列的一部分。