# 在 C/C++中编译一次与包含保护

> 原文：<https://blog.devgenius.io/pragma-once-versus-include-guards-in-c-c-9bfbdd24dd78?source=collection_archive---------0----------------------->

# 通过切换到#pragma 一次来节省调试时间

如果您已经使用 C 或 C++很长时间了，您可能会遇到“错误:重新定义…”

为了避免这种情况，开发人员通常会添加包含保护来防止头文件被多次导入到一个文件中。比如在 dog.h 中:

```
#ifndef DOG_H
#define DOG_H
// Contents of header file
#endif // DOG_H
```

这防止了许多问题。与此同时，它可能会很乱。

由于丢失了三行中的一行，您可能会以**追踪错误**而告终。更常见的是，您会因为复制代码和忘记重命名头变量而导致错误。如果有一种方法可以用更容易维护、更高效的代码来防止这些和其他错误就好了…

![](img/f58d680d129e2eb9849205a47712699e.png)

由[阿列克谢·图伦科夫](https://unsplash.com/@2renkov?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**引入***# pragma once*预处理指令。将此代码与之前的 dog.h 代码进行比较:

```
#pragma once
// Contents of header file
```

#Pragma once 由预处理器处理，防止程序员出错。虽然 GCC、Clang 和大多数流行的 C/C++编译器都支持它，但它不是 C++标准的一部分。

根据您的代码库，如果没有指定编译器，这可能会引起可移植性的问题。然而，目前不支持它的主要编译器似乎是 IAR C/C++，所以它得到了广泛的支持。

除了减少错误， *#pragma once* 通常被优化以减少预处理器和文件打开的使用，**使其比包含守卫**更快。请注意，许多编译器已经为此进行了优化，因此它们可能同样快。

![](img/3d243b513a90b0723c71a8e7f8e0afbe.png)

[布拉登·科拉姆](https://unsplash.com/@bradencollum?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

使用一次*# pragma*的一个**边缘情况**是如果你有**多个同名的头文件**。因为这个预处理器指令不是标准的一部分，所以每个编译器对它的处理是不同的。

通常，这是通过查看文件名、内容和时间戳来完成的。*如果您计划在项目的多个文件夹中使用 dog.h，那么使用 include guards* 可能会更好。尤其是如果您的构建过程有一个重复的文件(名称和内容)，那么 *#pragma once* 的行为可能是不可预测的。

总之，用 *#pragma 替换一次*的 include 保护是减少程序员错误、提高可读性和提高编译速度的好方法。这样做的时候，要记住重复文件名和目标编译器的极端情况。

试试看！看看切换到 *#pragma once* 如何清理你的代码。

![](img/4219b45f6207809e0d12448759304358.png)

[Towfiqu barbhuiya](https://unsplash.com/@towfiqu999999?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照