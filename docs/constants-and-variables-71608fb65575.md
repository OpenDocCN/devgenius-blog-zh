# 常量和变量

> 原文：<https://blog.devgenius.io/constants-and-variables-71608fb65575?source=collection_archive---------5----------------------->

## 在 Swift 5+中定义 Swift 常量和变量

## 常量，变量，类型注释，语法规则，以及如何使用它们！

![](img/4dd55866bb4192ed7e4d5bdad3c0d330.png)

**旋转式电话**

B ack 在那一天，我们有了如上图的手机。一部简单的电话，你可以通过旋转拨号盘来拨打电话。在那个时代，这是最基本的。时间快进到现在，现在我们有了“智能”手机，几乎可以为我们做任何事情。像旋转电话一样，我们将介绍一系列多部分的 Swift 基础知识，帮助您做好应对任何挑战的准备。

# **什么是常量和变量？**

对于刚刚入门的程序员来说，你可能会问自己，这些单词是什么，它们的意思是什么？“常量”是指一个固定值，**不能改变**，例如:

`let maximumPhoneDigits = 12`

我们将常量设置为值 12。这说明了每个数字 1-9，包括 0、#和*。单词“let”是我们初始化一个常数的方法。这告诉编译器，“嘿，我在这里，我不能被改变。”

第二，变量更自由一点。当声明一个变量时，我们希望使用“var”关键字。“变量”是指**可以**改变的值，例如:

```
var myAge = 25
myAge = 26
myAge = 27
```

这先告诉编译器，我的年龄现在是 25，但是现在我想把它设置成 26…哦等等，现在是 27。

# 何时使用“let”或“var”

当要编写第一个应用程序时，您可能会问自己，“使用什么会更好？变量还是常数？”最好的解决方案是使用 **let** *(常量)*，根据需要改为 **var** *(变量)*。

# 常量和变量的语法规则

在 swift 中，我们必须遵循一些语法规则。常量和变量名几乎可以包含任何字符，包括 Unicode 字符，例如:

```
let 👶 = "baby"
let π = 3.14159
```

常量和变量名不能包含空白字符、数学符号或箭头。此外，它们不能以数字开头，尽管数字可以包含在名称中第一个字符之后的任何位置。

# 类型注释！

接下来，我们将讨论什么是类型注释，我们如何使用它以及何时使用它。类型注释明确地告诉编译器变量或表达式的类型。我们用冒号(:)声明它们，并以类型结束。当声明一个变量或常量时，我们将使用类型注释来解释它们是什么，例如:

```
var name: String = "Johnny"
var age: Int = 20
var pi: Double = 3.14
var isPizzaGood: Bool = true
```

我们声明的所有这些例子都有一个类型，无论是字符串、整型、双精度还是布尔型。

这是完全可以接受的。但是 Swift 提供了一个完全免费的工具，它集成到我们的语言中，可以自动为我们推断类型，比如:

```
var name = "Appleseed"
var age = 40
var pi = 3.14
var isPizzaBad = false
```

# 打印常量和变量

当打印常量和变量时，我们只想调用括号内的属性名，例如:

```
var name = “George Washington”
print(name)// Prints “George Washington”
```

您可以在 Xcode 底部的调试器窗口中看到该打印语句！接下来，如果你想打印上面的友好欢迎声明。我们可以添加我们称之为快速“字符串插值”的内容，比如:

```
var name = “Steve Jobs”
print(“My friend \(name) created Apple!”)// Prints “My friend Steve Jobs created Apple!”
```

如您所见，我们有一个要打印出来的字符串值，我们添加了\()将一个值添加到字符串中。

# 结论

恭喜你。您已经完成了 Swift 基础知识的第一篇文章！🚀🎉在本文中，我们讨论了:

```
• What a variable and constant is
• How to create them with let (constant) and var(variable)
• Syntax rules
• Type Annotation
• Printing constants and variables
```

看看我即将发表的下一篇文章，在这篇文章中，我们将常量和变量与整数联系起来！