# 我最近体验了 rust 的编程，这是我一生中最棒的编程经历之一

> 原文：<https://blog.devgenius.io/i-recently-got-to-experience-programming-in-rust-and-it-has-been-one-of-the-best-programming-5f747643591b?source=collection_archive---------13----------------------->

首先，我是怎么得到使用 Rust 的机会的？

我不得不使用 Rust 作为工具来开发 web assembly 项目，而不必直接编写复杂的 Web assembly 代码。这里的首选方法是使用 wasm-pack rust 库，它有助于将 rust 代码编译成相应的 web 汇编代码。

# 为什么是 Web 汇编？

嗯，Web assembly 就像 Web 的 assembly，尽管它是在浏览器上执行的，但它运行程序的性能接近本地，这在以前仅靠 javascript 是做不到的。

我第一次接触铁锈的感觉如何？

老实说，第一次看到 rust 代码是非常令人畏惧的。

字符串有两种不同的数据类型是怎么回事:“&str”和“string”。还有，你是在告诉我 null 没有对应的类型？

然而，在对 rust 的世界有了更多的涉猎之后，我开始明白，这些特性是如何让程序员的生活变得更容易的。

# 枚举选项替换 null

首先，让我来处理一个不太明显的问题:为什么 rust 中没有空类型？

Null 是在半个多世纪前首次引入的，所以可以肯定地说，它是编程领域中一个相当成熟的概念。曾经被认为是编程的热潮。然而，现在随着编程的发展，程序员们已经意识到这与其说是繁荣，不如说是灾难。

让我们考虑下面的代码，它试图在这样的计算机上获得独立 GPU 的模型:

```
String version = computer.getDiscreteGPU().getModel();
```

然而，许多计算机实际上并没有配备独立的 GPU。那么，这个调用的结果会是什么呢？一种常见的不良做法是返回一个空引用来表示缺少独立的 GPU。不幸的是，getModel()方法将尝试返回空引用的模型，这将导致运行时出现 NullPointerException，并阻止程序进一步运行。想象一下，程序运行在客户的机器上，每次他/她试图运行该程序时，它总是不运行，他/她也不知道问题出在哪里。

这里用空东尼·霍尔的创造者自己的话说:

> “1965 年，我在设计第一个面向对象语言(ALGOL W)的引用综合类型系统。我的目标是确保所有引用的使用都是绝对安全的，由编译器自动执行检查。但是我无法抗拒放入空引用的诱惑，仅仅是因为它太容易实现了。这导致了数不清的错误、漏洞和系统崩溃，在过去的四十年里，这可能造成了数十亿美元的痛苦和损失。”

尽管 Rust 没有 null 类型，但它提供了一个 enum 选项，可以将 null 的概念编码为一个可选值，可以存在也可以不存在。

```
Enum Option {
    Some(T),
    None,
}
```

现在有了 Enum 选项，代码会立即显示计算机是否有独立 GPU(因为独立 GPU 是可选的)。通过这种方式，程序清楚地知道一个给定的值是否允许丢失，并且即使在它不存在时也采取适当的措施。

简而言之，enum 选项包括显式处理值存在和不存在的情况的方法。这里，与空引用相比，enum 选项的优势在于它迫使您考虑值不存在的情况。因此，您可以防止意外的空指针异常。

# Rust 的编译器太棒了。它准确地指出了问题所在。

我喜欢 rust 的另一点是 Rust 的编译器非常善于告诉我们问题出在哪里。例如:

```
error[E0308]: mismatched types
--> src/main.rs:3:11|3 |     greet(hello);|           ^^^^^^^|           ||           expected struct `std::string::String`, found `&str`|           help: try using a conversion method: `hello.to_string()`error: aborting due to previous errorFor more information about this error, try `rustc --explain E0308`.
```

在这里，很明显我们处理的是两种不同的数据类型:std::string::string，又名 String 和&str。虽然 greet()需要一个字符串数据类型，但显然我们传递给它的是&str 类型的东西。编译器甚至提供了如何修复它的帮助。我们只需要使用。to_string()方法和使用`**greet(hello.to_string())**`确实有效！

&str 和 String 之间的区别在于，String 是一种可增长的、堆分配的数据结构，而 str 是内存中某处的不可变的固定长度的字符串。由于 str 的大小未知，我们通常只将其用作参考:&str。&str 可以用来查看存储在任何地方的数据:在静态存储中，在堆分配的字符串中，甚至在非常方便的堆栈上。有些时候，我们需要拥有字符串数据来不断更新它，所以我们在这种情况下使用 string，但有些时候，我们只需要查看或使用别人拥有的字符串，所以我们在这种情况下使用&str。通过拥有两种不同的数据类型，它确保了程序中没有行为不当的字符串。

到目前为止，我非常喜欢用 rust 编码，我希望尽可能地继续用 rust 编码。如果你像我一样想要一个新的、有启发性的、丰富的编码体验，我绝对推荐你试试 rust。