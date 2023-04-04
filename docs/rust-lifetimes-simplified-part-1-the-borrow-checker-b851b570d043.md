# 锈蚀寿命简化第 1 部分——借用检查器

> 原文：<https://blog.devgenius.io/rust-lifetimes-simplified-part-1-the-borrow-checker-b851b570d043?source=collection_archive---------4----------------------->

![](img/3ef91724821f03ed0075273f087c3aec.png)

生存期和借用检查器规则。

很长一段时间以来，我一直在与铁锈作斗争。老实说，一开始，与借货员的不断斗争相当令人沮丧。随着我对语义理解的加深，我对这些存在的原因有了深入的了解。在本文中，我将简要介绍 rust borrow checker，它的必要性，以及何时需要处理生存期。这是一个由两部分组成的系列。在第 1 部分中，我将重点介绍借款检查器，并分享一些例子，这些例子围绕着借款检查器检查什么以及它背后的直觉是什么。

我建议阅读我在这篇文章之前发表的两篇文章。

[](https://betterprogramming.pub/quick-look-into-variables-and-immutability-in-rust-writing-safe-code-from-the-start-b31635478779) [## 《铁锈》中的变量和不变性

### 从一开始就编写安全代码

better 编程. pub](https://betterprogramming.pub/quick-look-into-variables-and-immutability-in-rust-writing-safe-code-from-the-start-b31635478779) [](https://betterprogramming.pub/understanding-rust-ownership-model-by-example-5d586ec5e8e4) [## 通过示例了解 Rust 所有权模型

### 了解作为 Rust 所有权体系基础的 3 条规则

better 编程. pub](https://betterprogramming.pub/understanding-rust-ownership-model-by-example-5d586ec5e8e4) 

了解了所有权、生存期和作用域之后，现在让我们来看看 rust 生存期和关于引用的借用检查器。

# 几乎总是关于推荐人！

首先要理解的是，**生存期**是 Rust Lang 机制的一部分，用于确保在运行时处理内存中的引用时不会出现问题。rust 可以在编译时检查所有可能导致问题的实例。对于生存期和作用域也有简单的检查，但是我们将把重点放在本文的参考文献上。目标始终不变。安全内存处理。

引用是指向内存位置的指针。无论是结构还是共享数据引用，编写安全程序必须遵循的一个规则是，**引用必须始终指向有效的内存位置**。更重要的是**，引用必须指向它想要引用的数据**。这是不可谈判的生锈。这也意味着在**时间的任何一点，如果一个对象有一个引用**，它都不能从内存中被删除。**任何时候都不能存在无效参考**。rust 必须在编译时确保这一点。

这就是借款检查器的作用。

# **借用检查器**

借用检查器是 Rust 编译器的一部分，确保引用总是有效的。代码中不能有指向内存中无效引用的歧义。

在我们深入研究生存期，即生存期的显式定义之前，让我们先看一下借用检查器。我会写一个单独的帖子。

这篇文章为显式生存期提供了一个基础，并允许你进入借入检查器的思维，看看发生了什么。下面的文字直接来自 Rust 编译器的文档。我将逐一介绍每条规则，并举例说明，使其更加清晰。

> 借款支票是 Rust 的“秘方”——它的任务是强制执行许多属性:
> 
> 所有变量在使用前都被初始化。
> 
> 你不能移动同一个值两次。
> 
> 当一个值被借用时你不能移动它。
> 
> 当一个地方被可变地借用时，你不能访问它(除非通过引用)。
> 
> 当一个地方被永久借用时，你不能改变它。

# 所有变量在使用前都被初始化。

```
**fn** main() {
    **let** a : **i32** ;
    **let** b : &**i32**; 
    println!(**"Value of a is {} {} "**, a ,b )
}
```

在任何其他编程语言中，这都是有效的语句/程序。当我们试图在 rust 中运行这个程序时，下面是你得到的错误。

```
error[E0381]: borrow of possibly-uninitialized variable: `a`
 --> src/main.rs:3:34
  |
3 |     println!("Value of a is {}", a)
  |                                  ^ use of possibly-uninitialized `a`
  |
```

无论是基本类型还是引用类型，每个变量都必须初始化。如果是原始类型，也可以借用或克隆你的变量。因为我们没有与之相关的值，所以我们不能借用/克隆它。承认错误。

# 你不能移动同一个值两次

现在考虑这个例子。

```
#[derive(Debug)]
**struct** Employee {
    **id** : **i32** }

**fn** main() {
    **let** a = Employee{**id**: 43} ;
    **let** b  = a ;
    **let** c= a ;
}
```

我们首先定义一个新的 Struct Employee 并将它存储在变量 a 中。Rust 会将值从 a 移动到 b，此时 a 不能再用了。但是在下一行我们再次尝试赋值`c = a ;`。这时借位检查器意识到 a 已经被移动，并且它的生命周期已经结束。我们不能再次移动相同的值。

```
error[E0382]: use of moved value: `a`
 --> src/main.rs:9:12
  |
7 |     let a = Employee{id: 43} ;
  |         - move occurs because `a` has type `Employee`, which does not implement the `Copy` trait
8 |     let b  = a ;
  |              - value moved here
9 |     let c= a ;
  |            ^ value used here after move 
```

# 当一个值被借用时你不能移动它。

考虑下面的程序:

```
#[derive(Debug)]
**struct** Employee {
    **id** : **i32** }

**fn** main() {
    **let** a = Employee{**id**: 43} ;
    **let** b  = &a ;
    **let** c= a ;
    println!(**"Employee c  : {:?} "**, b )
}
```

在这个程序中，我们在变量 b 中创建了一个对 a 的引用。此时，a 和 b 都有效。现在在下一行中，我们通过将 c 赋值给 a，将 a 的值移动到 c 中，此时，`a`被删除。如果我们尝试使用指向 a 的`b`——rust 知道有一个对 a 的引用，它在我们移动值之后被使用。这意味着会有一个悬空指针。错误消息清楚地提到了这一点。

```
error[E0505]: cannot move out of `a` because it is borrowed
  --> src/main.rs:9:12
   |
8  |     let b  = &a ;
   |              -- borrow of `a` occurs here
9  |     let c= a ;
   |            ^ move out of `a` occurs here
10 |     println!("Employee c  : {:?} ", b )
   |                                     - borrow later used here
```

# 当一个地方被可变地借用时，你不能访问它(除非通过引用)

考虑上面例子的修改形式。

```
#[derive(Debug)]
**struct** Employee {
    **id** : **i32** }

**fn** main() {
    **let mut** a = Employee{**id**: 43} ;
    **let** b  =  &**mut** a ;
    **let** c= &a ;
    b.**id** = 55;
    println!(**"Employee c  : {:?} "**, c )
}
```

在这个程序中，我们创建了一个可变雇员。这意味着我可以随时更新我的员工 id 的值。我们给变量`b`赋值一个对 a 的可变引用——这意味着 b 现在可以更新 a 内部的值，因为它是可变的。

现在让我们创建另一个变量 c，它是对 a 的不可变引用。因为我们已经有了对 a 的可变引用，并且我们将来会更新引用值，所以 rust 不允许不可变借用。它立即抛出一个错误，详细信息如下。

```
error[E0502]: cannot borrow `a` as immutable because it is also borrowed as mutable
  --> src/main.rs:9:12
   |
8  |     let  b  =  &mut a ;
   |                ------ mutable borrow occurs here
9  |     let c= &a ;
   |            ^^ immutable borrow occurs here
10 |     b.id = 55;
   |     --------- mutable borrow later used here
```

如果原始值被可变引用更新，我们就不能创建不可变引用。只能有一个可变引用。

有趣的是，如果你仅仅通过删除行`b.id = 55;`来运行程序，程序编译的很好。Rust borrow checker 足够聪明，知道我们不会修改值，所以这是一个安全的操作。

```
#[derive(Debug)]
**struct** Employee {
    **id** : **i32** }

**fn** main() {
    **let mut** a = Employee{**id**: 43} ;
    **let** b  =  &**mut** a ;
    **let** c= &a ;
    b.**id** = 55;
    println!(**"Employee c  : {:?} "**, c )
}
```

这运行得非常好。

# 当一个地方被永久借用时，你不能改变它。

这个例子就像上一个例子，除了我们不能有一个可变的引用，如果它已经被不可变地借用了。想象一下，银行出纳员查看您的余额以批准取款，而后台办公室的某人在没有通知出纳员的情况下更新了值。

```
#[derive(Debug)]
**struct** Employee {
    **id** : **i32** }

**fn** main() {
    **let mut** a = Employee{**id**: 43} ;
    **let** c= &a ;
    **let** b  =  &**mut** a ;
    println!(**"Employee c  : {:?} "**, c )
}
```

我们只是在获得可变引用之前不可变地借用了一个引用。

```
error[E0502]: cannot borrow `a` as mutable because it is also borrowed as immutable
  --> src/main.rs:9:16
   |
8  |     let c= &a ;
   |            -- immutable borrow occurs here
9  |     let  b  =  &mut a ;
   |                ^^^^^^ mutable borrow occurs here
10 |     println!("Employee c  : {:?} ", c )
   |                                     - immutable borrow later used here
```

# 结论

在理解生命周期的第 1 部分中，我们看到了借用检查器在识别引用错误和内存问题方面非常聪明。它隐式地理解变量的生存期，并且确切地知道无效引用可能存在于何处；并确保您的变量和引用有足够长的寿命来安全地执行操作。任何导致违规的操作都会被编译器自动指出。

对于编译器来说，上面所有的例子都很容易检测到错误。但是在有些情况下，开发人员需要显式定义引用的生存期，然后编译器可以帮助进行所有的检查，而不会有任何歧义。我们必须明确定义 rust 中的寿命。从这里开始，事情变得非常有趣。

这是该系列的第 2 部分！

***敬请关注，请不要忘记订阅并关注更多内容。***