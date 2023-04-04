# 每个初学者应该知道什么是可靠

> 原文：<https://blog.devgenius.io/what-every-beginner-should-know-about-solidity-ad2a0e808ff?source=collection_archive---------1----------------------->

智能合同编程语言简介

![](img/bba728b419d412afb5efc06a35ea018b.png)

由 [Kanchanara](https://unsplash.com/@kanchanara?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Solidity 是一种编程语言，用于在各种区块链上实现智能合约，最著名的是以太坊。智能合约是在以太坊状态下控制账户行为的程序。有了 Solidity，你可以为投票、众筹、拍卖、多签名钱包等用途创建合同。

# 句法及其起源

Solidity 是一种花括号语言，这意味着它使用花括号来包含块，这与 Python 等语言相反，Python 中的块是通过缩进来定义的。但是 Solidity 文档说它不仅受 C++和 JavaScript 的影响，还受 Python 的影响。无论如何，从语法的角度来看，你会发现它接近于 C++和其他花括号语言，从普通的老 C 语言中派生出语法，比如 C#和 Java。

如果你现在害怕像 C++这样的语言的可靠性，请不要担心，这里只讨论语法，没有像手动内存管理这样复杂的工作。

下面是一份来自 Solidity [官方文档](https://docs.soliditylang.org/en/v0.8.13/introduction-to-smart-contracts.html#simple-smart-contract)的简单合同，它将演示 Solidity 语法:

```
*// SPDX-License-Identifier: GPL-3.0*
**pragma solidity** >=**0.4.16** <**0.9.0**;

**contract** **SimpleStorage** {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}
```

C++的影响可以在变量声明的语法、循环、重载函数的概念、隐式和显式类型转换以及许多其他细节中看到，与 Python 的影响相反，Python 的影响要小得多，只能在多重继承、super 关键字以及其他一些功能中看到，如 Solidity 的修饰符，这些功能是为了用更有限的功能来模拟 Python 的装饰器而添加的。

你可以在 Solidity 官方文档的[语言影响章节](https://docs.soliditylang.org/en/v0.8.13/language-influences.html)中找到更多关于 Solidity 受哪些语言启发的细节。

# 表达式和控制结构

由于 Solidity 语法类似于 C++语法，它有许多在花括号语言中通常使用的相同表达式和控制结构，如 if、else、while、do、for、break、continue 和 return。它还支持使用 try/catch 语句进行异常处理。

# 面向对象语言

Solidity 是一种面向对象的语言，这意味着它遵循基于类概念的 OOP 范式，可以包含数据和代码，这样的类将是自包含和可重用的实体，就像一个“黑盒”，所以当使用这样的类时，人们不需要担心它的内部。

# 遗产

由于 Solidity 是面向对象的，它也支持一个基本的 OOP 特性，比如继承。在一般的 OOP 中，继承被理解为在另一个基类之上创建自定义类的一种方式，扩展了基类的功能。

在 Solidity 中，继承允许开发人员在现有契约的基础上创建一个契约。

下面是从[文档](https://docs.soliditylang.org/en/v0.8.13/contracts.html#inheritance)中摘录的一段，它展示了可靠性中的继承性:

```
**contract** **Owned** {
    constructor() { owner = payable(**msg.sender**); }
    address payable owner;
} *// Use `is` to derive from another contract. Derived*
*// contracts can access all non-private members including*
*// internal functions and state variables. These cannot be*
*// accessed externally via `this`, though.*
**contract** **Destructible** is Owned {
    *// The keyword `virtual` means that the function can change*
    *// its behaviour in derived classes ("overriding").*
    function destroy() virtual public {
        if (**msg.sender** == owner) selfdestruct(owner);
    }
}
```

# Solidity 中的程序运行在以太坊虚拟机上

与许多其他现代高级语言一样，在 Solidity 上编写的程序(或合同)运行在虚拟机上，而不是设计为用二进制代码编译、由 CPU 直接执行的语言。

正如 Java 程序运行在 JVM 上，Python 程序运行在 PVM 上一样，Solidity 程序运行在 EVM 上，后者代表[以太坊虚拟机](https://en.wikipedia.org/wiki/Ethereum#Virtual_machine)，是以太坊中事务执行的运行时环境。它包含了您对典型虚拟机所期望的一切:堆栈、内存、程序计数器和所有帐户的持久存储，包括合同代码。

# 坚固性是静态类型的

Solidity 是一种静态类型语言，这意味着所有变量类型都必须在声明期间显式定义。这意味着，没有任何“通用”变量类型，如 JavaScript“var”或现代 c++“auto”类型。

Solidity 支持三大类类型:[值类型](https://docs.soliditylang.org/en/v0.8.13/types.html#value-types)、[引用类型](https://docs.soliditylang.org/en/v0.8.13/types.html#reference-types)和[映射类型](https://docs.soliditylang.org/en/v0.8.13/types.html#mapping-types)。每个类别包括许多不同的类型，即值类型是整数、布尔值、地址类型、定点数、契约类型等等。

Solidity 中不存在“未定义”或“空”值的概念，但是新声明的变量总是有一个依赖于其类型的默认值。

# 图书馆

库就像契约，但主要是为了重用。一个库包含其他契约可以调用的函数。坚固性对图书馆的使用有一定的限制。以下是 Solidity 库的主要特征:

*   库函数如果不修改状态，可以直接调用。这意味着只能从库外部调用纯函数或视图函数。
*   库不能被销毁，因为它被认为是无状态的。
*   库不能有状态变量。
*   库不能继承任何元素。
*   库不能被继承。

# 全局变量和函数

尽管有库，但基本的工具函数和变量只是作为全局变量公开，而不是分组存储在某种标准库中。

这包括提供有关区块链、数学函数、解码和编码函数、错误处理函数、地址类型成员等特定信息的属性。