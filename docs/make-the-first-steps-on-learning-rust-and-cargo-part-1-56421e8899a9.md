# 迈出学习铁锈和货物的第一步！—第一部分

> 原文：<https://blog.devgenius.io/make-the-first-steps-on-learning-rust-and-cargo-part-1-56421e8899a9?source=collection_archive---------4----------------------->

这个故事将涵盖学习生锈的步骤。为了给出一些背景，这是从一个主要用 Java 编程的人的角度来看的，所以可能有一些主题对我来说可能比他们看起来更难，所以请随意分享你的想法和想法！

![](img/8f8a2109e2d3099f39c0e8582363e7c7.png)

Julian Hochgesang 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 介绍

Rust 是现在谈论最多的语言之一。我从来没有更近距离地体验过低级编程语言，所以第一次使用 rust 似乎是一个很好的起点。当然，这是我们在整个学习过程中所走的路，我们希望以后使用这种新语言来创建一个小项目，将所有这些概念付诸实践。

当我们使用“更高级”的语言时，有些东西看起来很“神奇”,被一些花哨的注释或比我们想象的要多得多的东西所掩盖。Rust 在这方面帮助了我们，它让我们稍微回顾一下，重新思考在某些情况下需要做些什么来实现更高的效率。

所以作为引言的最后一点，这是我学习 rust 的过程和目前为止的经历。没有别的事了，让我们开始吧。

# 索引

*   **如何在窗户上安装铁锈**
*   **简单的备忘单**
*   **入门**
*   **Println**
*   **注释**
*   **Let vs Mut**
*   **惊慌失措**
*   **数组**
*   **元组**
*   **功能**
*   **循环**
*   **琴弦**

# 如何在 Windows 上安装 Rust

要在 windows 上安装它，你只需进入[这里](https://www.rust-lang.org/tools/install)并下载它。

# 简单备忘单

这里有一个简单的备忘单，让我们继续这个故事的其余部分。

```
rustup --version -> check version.
rustup update -> check for updates.
rustc --version -> check version for the compiler.
cargo --version -> check version for the package manager.
rustc hello.rs -> compile the "hello.rs" file, this command outputs an executable file which can be used to run the compiled program.
cargo init -> initializes a rust project.
cargo new test_project -> creates the test_project project.
cargo run -> compiles and runs project.
cargo build -> compile development version of the project.
cargo build --release -> compile production version of the project.
```

# 入门指南

让我们从使用 cargo 创建一个项目开始:

```
cargo init
```

这将创建基本的文件结构，以便我们可以继续前进。接下来，我们将检查它自动创建的第一个文件:

这个基本文件输出消息“Hello，world！”在控制台里。要执行项目，您只需执行:

```
cargo run // compiles and runs the project.
```

# Println

让我们从理解 Println 指令开始。

从这个例子中得到的想法:

*   **第 1 行**—“pub”保留字表示该函数是公共的。
*   **第 4 行**——您可以在 println 语句中使用字符串格式，如下所示，您只需要根据需要多次使用“{}”。

接下来，让我们了解如何从另一个文件调用这个文件(*又名*导入)。

从中得到的想法:

*   **第 1 行** -我们将打印文件包含在主文件中，确保我们可以访问其功能。
*   **第 4 行**——我们可以写下文件名，然后指定“::”，并写下我们想要执行的函数名。

在使用 print 语句时，您还可以做其他事情，例如格式化“{}”中的内容。下面是备忘单:

```
{:.4} -> displays 4 decimal places of precision.
{:6.4} -> displays 4 decimal places of precision, with spaces on the left totalling 6 spaces.
{:06.4} -> displays 4 decimal places of precision, with 0’s on the left totalling 6 digits.
{:?} -> Can be used to display a tuple and all the values that it might compreend.
{}\t -> Will add a tab after the value that is going to be printed.
```

在 rust 中你可以做的另一件有趣的事情是引用相同范围内的变量，例如，下面的内容将没有问题:

现在让我们想象一下，我们有一些 0 和 1 代表一个二进制数:

第一个 println 为我们提供了到十进制数的转换，第二个将负责继续显示二进制数。因为这里还有一些其他要点需要提及，所以让我们把它分成几个小步骤:

*   **第 2 行** - 0b 前缀表示显示的数字将是一个二进制数。在此之后，我们可以开始把我们的数字二进制。下划线被用作人类可读的分隔二进制数字的方式，以便我们更容易理解，这个数字的最后一部分是“u8”。这是指定变量类型的可选符号。如果我们知道这些不会超过 u8，那就非常方便了，因此有助于提高程序的内存效率。
*   **第 3 行** -默认行为是将二进制数转换成十进制数。
*   **第 4 行** -数字以二进制形式显示，左侧填充零，直到 8 位数字，并显示为二进制“b”。

# 评论

要添加评论，您只需使用“//”即可，例如:

当你使用双斜杠时，编译器会忽略它们右边的所有内容。

# Let vs Mut

在 rust 中，默认情况下，所有变量都是不可变的，这意味着如果你只使用“let”来声明一个变量并改变它的值，你将会得到一个编译错误。为了确保变量是可变的，你可以在声明中使用“let mut ”:

这给出了一个类似这样的消息“不能给不可变变量赋值两次”。

要解决这个问题，您应该:

# 恐慌

“panic”宏的存在是为了让你暂停程序的执行。用法如下:

```
panic!()
```

无论是在您的开发流程中，还是当您需要让调用者知道某件事情失败的原因时，您都可以使用 panic 宏。

# 数组

为了创建阵列，您可以:

```
let array = [1, 2, 3];
```

这将转化为一个不可变的数组。要允许更改值，您需要使用“mut ”,因此它将变成:

```
let mut array = [1, 2, 3];
```

在 Rust 中，所有的元素都需要是相同的数据类型，如果你不遵守这一点，你将会得到一个编译错误。假设您如上所述声明了数组，现在您可以像这样简单地更改它的值:

```
array[0] = 4;
```

在我们想要指定一个没有值的数组的情况下(以便以后我们可以添加它们)，我们需要两件重要的事情，我们想要数组元素尊重的类型和它的大小:

```
let array: [i32; 10];
```

现在我们想用数字填充它，假设我们只想做所有的数字:

```
array = [1, 1, 1, 1, 1, 1, 1, 1, 1, 1];
```

如果我们有 1000 个数字而不是 10 个数字，这难道不是一种更简单的方法吗？有，你可以这样做:

```
// 10 element array
array = [1; 10];
// 1000 element array
otherArray = [1; 1000];
```

# 元组

rust 中的 tuple 是对不同数据类型进行分组的简单方法。可变性的规则是适用的，所以如果在最初定义值之后需要更改值，不要忘记“mut”。

语法如下:

```
let mut tuple: (type1, type2, type3) = (value1, value2, value3);
```

访问元组中的值与我们正在使用的略有不同，因为您需要如下操作:

```
tuple.0; // gets you value1
tuple.1; // gets you value2
tuple.2; // get you value3
```

# 功能

rust 中的函数和其他语言中的一样，都有一种特定的方式来描述如何返回值。默认情况下，如果没有指定要返回的内容，rust 函数将返回空“()”，这称为单元返回类型。为了定义一个返回类型，我们可以做如下工作:

# 环

rust 中的循环引入了一个名为“loop”的保留关键字，让我们从一个例子开始:

让我们来理解这段代码:

*   **第 4 行**——在 Rust 中，有可能从**循环**中返回值，所以我们创建了一个变量来获取返回值。我们还可以看到保留字“**循环**”，用于识别这种类型的循环。
*   **第 6 行**——为了在循环指令中返回一些东西，我们可以使用 **break** 字，不仅可以退出循环(在这种情况下，如果条件满足)，还可以将一些值作为“return”传递。

# 用线串

要在 Rust 中声明一个可变字符串，我们需要使用一个特殊的符号，要连接字符/字符串，逻辑是相同的，我们创建一个字符串对象，并使用一个名为“push_str”的函数向我们正在创建的字符串中添加更多内容，查看下面的示例:

# 结论

这个故事是通过对 Rust 的一些基本实验完成的，我们将继续发展我们的技能，以便我们可以开始构建一个简单的项目来进一步使用这种语言进行培训，并希望在我们的日常活动中做一些我们可以做的事情。我们可能会觉得，到目前为止，这与其他编程语言没有太大的不同，实际上，我们是对的。我们觉得 Rust 学习曲线中更难的部分还在后面，所以一定要等待关于这个主题的下一次更新。

感谢您花宝贵的时间阅读这个故事，我们希望它在某些方面对您有所帮助。对于任何反馈，您必须确保使用下面的评论。如果你喜欢这个故事，鼓掌，如果你想继续关注这个和其他类型的故事，也可以随意关注。

# 参考

[https://serokell.io/blog/learn-rust](https://serokell.io/blog/learn-rust)

https://www.rust-lang.org/tools/install