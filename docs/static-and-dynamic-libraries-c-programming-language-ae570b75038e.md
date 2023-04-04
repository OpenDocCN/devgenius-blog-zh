# 静态和动态库— C 编程语言

> 原文：<https://blog.devgenius.io/static-and-dynamic-libraries-c-programming-language-ae570b75038e?source=collection_archive---------7----------------------->

![](img/cea532018ddc9d7248775204feee3c7f.png)

来源:https://time.com/5812697/bird-library-livestream/

一个**库**是一个行为实现的集合，用一种语言编写，有一个定义良好的接口，通过这个接口调用行为。例如，想要编写高级程序的人可以使用库来进行系统调用，而不是一次又一次地实现这些系统调用。此外，该行为还提供给多个独立的程序重用。程序通过语言的机制调用库提供的行为。例如，在简单的命令式语言(如 C)中，通过使用 C 的普通函数调用来调用库中的行为。区别库函数调用和同一个程序中另一个函数调用的是代码在系统中的组织方式。[1]

# 使用图书馆的重要性:

像 C 语言这样的编程语言提供了所需的基本特性。这种提到的语言甚至不包含从键盘读取和打印到屏幕上的输入输出(I/O)功能。在 60 年代和 70 年代，开发人员应该实现他们自己的库，这些库执行常见的例程——处理字符串、打印消息、执行数学运算、处理数据结构、时间操作等等。但是在 1989 年，ANSI 开发了一个 C 的标准规范，命名为 **C89** ，其中一步就是创建一个**标准库。**1999 年，使用 **C99 标准**向该库中添加了一些特性。

使用 C 语言的开发随着标准库的实现而增加，原因如下[2]:

*   它的功能(例如`printf`和`atoi`)工作正常，因为它们经过严格测试，并且它们的代码是以最有效的方式编写的，以便产生可能的最佳性能。
*   它节省了开发时间，避免了编写通用的基本例程。
*   它的功能是可移植的，这意味着它可以在任何运行开发的程序的计算机上实现。

虽然标准库非常有用，但是在大多数情况下，它是不够的。随着软件项目的进展，代码行也会增加。将程序分成模块并创建一个库是一个很好的实践，它允许**以更简单的方式**实现未来的例程，以及**读取、测试和调试**代码。通过库，在另一个项目中实现任何这些功能都是可行的，并且使用另一个开发人员编写的功能也很容易——库是一种协作工具。

# 图书馆是如何工作的？

## 静态库

它们是在源代码被静态链接器编译之前加入源代码的目标文件的集合，使源代码完全独立。为了深入了解关于静态库的信息，[有一个博客](https://medium.com/@iheb.yah/static-libraries-c-programming-language-2dc70b3dfbdb)是我几个月前写的。

## 动态库

> 简单地说，共享库/动态库是在运行时为每个需要它的应用程序动态加载的库。动态链接不需要复制代码，只需将库名放入二进制文件中即可。实际的链接发生在程序运行时，此时二进制文件和库都在内存中。

## 如何创建它们？

创建静态库的过程在[以前的博客中有描述。](https://medium.com/@iheb.yah/static-libraries-c-programming-language-2dc70b3dfbdb)另一方面，创建动态库有两个步骤:

1.  创建目标代码。
2.  创建库。

假设在名为`main.c`的文件中有一个调用`calculator`函数的入口点:

```
#include "header.h"
#include <stdio.h>/**
 * main - Entry Point
 *
 * Return: Always 0.
 */
int main(void)
{
    printf("4 + 2 = %d\n", calculator('+', 4, 2));
    printf("4 - 2 = %d\n", calculator('-', 4, 2));
    printf("4 * 2 = %d\n", calculator('*', 4, 2));
    printf("4 / 2 = %d\n", calculator('/', 4, 2));
    return (0);
}
```

`calculator`函数根据传递的 char 变量的值返回数学运算的结果:

```
#include "header.h"/**
 * calculator - Executes basic math operations
 *
 * @op: char that indicates math operation executed.
 * @num1: number one.
 * @num2: number two.
 * Return: addition if op is '+'
 *         subtraction if op is '-'
 *         multiplication if op is '*'
 *         division if op is '/'
 *         otherwise, 0.
 */
int calculator(char op, int num1, int num2)
{
    if (op == '+')
        return (num1 + num2);
    if (op == '-')
        return (num1 - num2);
    if (op == '*')
        return (num1 * num2);
    if (op == '/')
        return (num1 /num2);
    return (0);
}
```

`header.h`文件包含`calculator`的原型函数，以便向编译器指示其数据类型和参数:

```
#ifndef __HEADER__
#define __HEADER__int calculator(char op, int num1, int num2);#endif
```

因此，创建的文件如下:

```
$ ls
calculator.c  header.h  main.c$
```

要创建`calculator.c`的目标文件，需要使用编译器 GCC:

```
$ gcc -Wall -fPIC -c calculator.c$ ls
calculator.c  calculator.o  header.h  main.c$
```

使用的标志是`-Wall`，它打印所有警告信息，以及`-c`，它创建目标文件`.o`。`-fPIC` flag 生成与位置无关的代码，该代码访问全局偏移表(GOT)中存储的所有常量地址。这个表被可执行程序用来在运行时查找函数的地址——在这个例子中是`calculator`——以及编译时未知的全局变量。*动态加载器—* 是操作系统的一部分—在程序启动时解析得到的条目[4]。

一旦创建了目标文件，在本例中为`calculator.o`，就创建了库。可以使用以下命令行:

```
$ gcc -shared -Wl,-soname,libcalc.so -o libcalc.so *.o$ ls
calculator.c  calculator.o  header.h  **libcalc.so** main.c$
```

标志生成一个共享对象，在与另一个对象文件编译时，它可以被链接成一个可执行文件。`-Wl,options`将选项传递给链接器:在本例中为`-soname`，以指示库的二进制 api 兼容性(默认为 0)。`-o` flag 将输出库保存在一个给定名称为`libcalc.so`的文件中。

# 静态库和动态库的区别

# 静态库

*   是建筑环境的一部分。
*   目标文件被添加到可执行文件中
*   拥有。延期

## 优势:

*   它们更快，因为所有模块都在同一个文件中。
*   它们的分布和安装更容易。
*   避免依赖问题。

## 缺点:

*   使用更多内存空间为每个可执行文件创建一个副本
*   较慢的编译过程所有的源代码都要重新编译。

# 动态库:

*   是运行时环境的一部分
*   目标文件的地址被添加到执行文件中
*   有。所以扩展

## 优势:

*   无用的内存空间:只有在复制时才能多次使用
*   源代码不会被重新编译
*   更快的编译过程

## 缺点:

*   可能导致应用程序中的依赖性问题
*   如果移除库，会出现兼容性问题

1.  普拉萨德德什潘德。 [*变形检测利用函数调用图分析*](https://dx.doi.org/10.31979/etd.t9xm-ahsc) (论文)。圣何塞州立大学图书馆。[doi](https://en.wikipedia.org/wiki/Doi_(identifier)):[10.31979/etd . t9xm-ahsc](https://doi.org/10.31979%2Fetd.t9xm-ahsc)。
2.  编程。c 标准库函数。链接:[https://www.programiz.com/c-programming/library-function](https://www.programiz.com/c-programming/library-function)