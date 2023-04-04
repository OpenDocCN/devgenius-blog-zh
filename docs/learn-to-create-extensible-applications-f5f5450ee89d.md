# 如何在 C#中创建可扩展的应用程序

> 原文：<https://blog.devgenius.io/learn-to-create-extensible-applications-f5f5450ee89d?source=collection_archive---------0----------------------->

## 反射基础

## 动态发现类型的初学者入门

您是否想过如何在不修改现有代码的情况下轻松扩展您的应用程序？也许甚至感受到了在项目中接收新特性需求的挫败感？

阅读完本文后，您将能够通过应用反射和接口“简单地添加新类”来扩展您自己的应用程序。

# 创建简单的可扩展库

为了演示如何通过应用反射和接口轻松地开始开发可扩展软件，我们将创建一个简单的“规则引擎”来评估学生分数并返回语言熟练程度。

基本思想是 1)拥有一套规则，2)发现和加载规则的机制，以及 3)评估规则。

## 创建规则

**首先**，需要一个接口。接口提供了规则必须实现的公共方法。在这个简单的例子中，该方法只接受一个整数数组。

**其次**，我们将创建两个返回枚举的规则。为了简洁起见，我将所有代码包含在一个代码块中，并删除了导入。

**最后**，我们将动态地发现规则，加载它们，并提供一种方法来执行熟练程度评估。

请注意 LoadRules 方法。它主要扫描程序集以查找实现 IProficiencyRule 接口的类型，实例化规则，并返回所有发现的规则的列表。

然后，该类提供一个公共方法，根据学生成绩(整数)的数组来评估熟练程度。

*EvaluateProficiencyLevel* 遍历每个规则，并调用规则的*check experience*方法。

## 扩展应用程序

目前，我们只实施了一些规则来判断一个学生是初学者还是熟练者。

实现“中间”规则不再需要我们修改代码。现在，添加新规则很简单，只需添加一个实现了 *IProficiencyRule* 接口的新类。

新规则现在由 *ProficiencyChecker* 类自动发现。

```
**Resources for the curious**[Practical Reflection in .NET by Jeremy Clark](https://app.pluralsight.com/library/courses/practical-reflection-dotnet/table-of-contents)
[C# Interfaces by Jeremy Clark](https://app.pluralsight.com/library/courses/using-csharp-interfaces/table-of-contents)
[Developing Extensible Software by Miguel Castro](https://app.pluralsight.com/library/courses/developing-extensible-software/table-of-contents)
```