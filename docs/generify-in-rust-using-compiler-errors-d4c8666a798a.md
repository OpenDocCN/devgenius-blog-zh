# 使用编译器错误在 Rust 中生成

> 原文：<https://blog.devgenius.io/generify-in-rust-using-compiler-errors-d4c8666a798a?source=collection_archive---------5----------------------->

当 **rustaceans** 被问及他们选择 Rust 作为他们最喜欢的语言的动机时，最常见的答案之一是**编译器。这是有原因的，任何使用过 Rust 的人最终都会全心全意地爱上这个编译器。它捕捉到的所有错误总是得到很好的解释，有时它甚至直接告诉你如何修复它。**

在本文中，我们将**利用这些错误消息**来一般化一些函数，这些函数最终只带有我们真正需要的特征，以使该函数工作。

不带泛型的启动函数

# 寻找需求

上面显示的代码实现了一个非常基本的 **sum** 函数，返回一个向量中所有`i32`的总和。您可能会对采用第一个值然后在迭代中跳过它而不是将所有值都加到零的决定感到惊讶，但是我这样做是出于叙述的原因，**我们将在奖金部分讨论这个问题。**

问题是，我们知道这段代码应该适用于任何数字类型，而不仅仅是`i32`。我们可以使这个函数通用化，允许用我们想要的任何数值类型调用它。大概是这样的:

相同的函数改变了 T 的 i32

这看起来不错吧？我们可以看到，用`u8`或`f64`替换 T 应该不会有任何问题，一切都应该工作顺利，但是……如果我们通过了`bool`，会发生什么呢？我们可以添加*布尔值*吗？不，我们不能。如果我们尝试编译上面的代码**，我们会得到一个类似这样的错误**:

```
error[E0369]: cannot add `T` to `T`
  --> src/main.rs:12:17
   |
12 |         total = total + i;
   |                 ----- ^ - T
   |                 |
   |                 T
   |
help: consider restricting type parameter `T`
   |
9  | fn sum<T: std::ops::Add<Output = T>>(numbers: Vec<T>) -> T {
   |         +++++++++++++++++++++++++++
```

你可以在这篇文章中看到我们正在努力做的事情，对吗？那就是 Rust 编译器的*魔法*。这里的问题是什么？`cannot add `T` to `T``。这是我们所期望的，我们不能添加布尔或者，idk，Person 的结构。**一个全局型 T 不能工作**。

你知道用`a + b`在 Rust 里会发生什么吗？它调用`a.add(b)`。这个功能从何而来？从 [**中添加**](https://doc.rust-lang.org/std/ops/trait.Add.html) **特质**。Rust 的所有数字类型都实现了这一特性，因此，当编译器知道我们正在使用某个数字类型时，它总是可以调用这个函数。

# 帮助的深度

在错误信息的帮助下，编译器告诉我们应该做什么来解决这个问题。我们需要只允许实现 Add 特征的类型。这样一来，编译器知道它可以调用该类型的 add 函数，除此之外，它还会禁止我们对像`bool`这样不能添加的类型使用这个函数。

但是这个**比**更好，只是指定类型应该能够使用`add`。如果我们只指定 Add 特征，我们仍然会得到一个错误。

```
error[E0308]: mismatched types
  --> src/main.rs:12:11
   |
9  | fn sum<T: std::ops::Add>(numbers: Vec<T>) -> T {
   |        - this type parameter
10 |     let mut total = numbers[0];
   |                     ---------- expected due to this value
11 |     for i in numbers.into_iter().skip(1) {
12 |         total = total + i;
   |                 ^^^^^^^^^ expected type parameter `T`, found associated type
   |
   = note: expected type parameter `T`
             found associated type `<T as Add>::Output`
help: consider further restricting this bound
   |
9  | fn sum<T: std::ops::Add + Add<Output = T>>(numbers: Vec<T>) -> T {
   |                         +++++++++++++++++
```

为什么？`total`是 T，`i`是 T .默认情况下，Add 使用 Self 作为自变量。所以，我们知道`total.add(i)`是合法的。类型不匹配从何而来？嗯，问题是 Rust 不能确定`total.add(i)` 返回类型是否为 t。在 Rust **中，你可以添加两个苹果，得到一个橙子** 🫤，这听起来很奇怪，但它提供了灵活性。如果你知道什么是标量积，你就知道你可以把两个向量相乘，得到一个数🤔。Add 允许这样的东西，如果你再看一下[**Add**](https://doc.rust-lang.org/std/ops/trait.Add.html)**trait**，你会发现它包含了一个叫做 **Output** 的东西，这是通用的。

该类型指定返回值的类型。我们将加法的结果存储在`total`中，所以它应该与 total 的类型相同。我们特别需要`T: Add<Output = T>`。**听着耳熟？应该的，这正是编译器告诉我们在第一次编译尝试中必须使用的。生锈的错误就是这么好。**

# 最后的细节

有了 Add trait fix，我们现在可以再次调用一个 build 来知道我们是否已经修复了问题，并且我们真的已经修复了问题。这还没有编译，但现在错误不同了，我们可以很容易地知道如何应用与上面相同的知识来解决这个问题:

```
error[E0507]: cannot move out of index of `Vec<T>`
  --> src/main.rs:12:18
   |
12 |     let mut total = numbers[0];
   |                     ^^^^^^^^^^
   |                     |
   |                     move occurs because value has type `T`, which does not implement the `Copy` trait
   |                     help: consider borrowing here: `&numbers[0]`
```

Rust slices 返回对其内容的引用，因为它们是这些内容的所有者。当我们调用索引操作符`[]`时，我们得到的是对存储的数字的引用，而不是数字。为什么在使用泛型之前有效？因为数字实现了 [**复制**](https://doc.rust-lang.org/stable/std/marker/trait.Copy.html) 的特质。当我们尝试但不可能移动时，复制特征的成员将在新位置生成值的副本，就像这里。

所以，我们可以看到我们的函数需要什么来保持正常工作？我们应该**将副本添加到 T** 的边界。这次我们为什么不遵循帮助提示呢？嗯，编译器很好，但有时帮助建议并不是最好的。如果我们尝试修复，我们会发现加法会再次中断，因为`total`的类型不再是`T`，而是`&T`。我们将以这个函数结束:

工作通用函数

看看我们在添加 Copy 时是如何将 Add 特征移到 where 子句中的。当您指定不止一个特征时，使用`where`被认为是一个好的实践。`impl <T: Clone + Copy>`和`impl<T> ... where T: Clone + Copy`一样，但第二个更具可读性，特别是当我们不断添加特征或类型时。

还要看看 main 中使用的一个类型如何不是原始类型，它是来自 [**vector2d**](https://docs.rs/vector2d/latest/vector2d/) 机箱的一个 vector。该向量可以被复制，并且可以被添加到其自身，从而生成相同类型的向量。它满足所有的界限，所以它是有效的。**我们的通用函数适用于任何板条箱中能够满足最基本要求的任何类型**。

# 结论

Rust 中泛型的最佳用途是当函数只需要它确切需要的东西时(*我将在以后的文章中展示更多的理由)*和**谁能比任何人都更清楚编译器需要什么来使代码工作？请求编译器输入会引起错误，这有点卑鄙，但它确实有效，所以不要害怕编译错误，修复代码迭代错误接收的输入。**

# 奖金

你还记得我说过，从零开始执行求和的决定吗？那是因为我们不能只把 0 定义为 t，这样做会导致这个错误:

```
error[E0308]: mismatched types
  --> src/main.rs:27:24
   |
23 | fn sum<T>(numbers: Vec<T>) -> T
   |                  - this type parameter
...
27 |     let mut total: T = 0;
   |                    -   ^ expected type parameter `T`, found integer
   |                    |
   |                    expected due to this
   |
   = note: expected type parameter `T`
                        found type `{integer}`
```

数字 0 是一个整数。数字 0.0 是一个浮点数。没有一个是两者都是。0 也不是向量。我们该如何报道？嗯，你可以创造你自己的特征，或者你可以使用为亚历克斯·克赖顿自己制作的[](https://crates.io/crates/num)**号板条箱。这个箱子定义了很多有用的特征，超过了 Rust 原始数字，比如 [**零**](https://docs.rs/num/0.4.0/num/trait.Zero.html) 。Zero 提供了一个函数`zero()`,它返回该类型的 zero 的等价物，允许我们这样做:**

**从零开始的加法函数**

**完成了。这可以工作，但是我们也失去了与 Vector2d 的兼容性。添加额外的特征将允许我们进行不同的实现，也许还会进行一些优化，但是我们会失去一些兼容性，所以每次都要考虑到这一点。**

*****小备注*** *，默认特征在这里也是有效的，因为所有 Rust 原语的默认值都是零，但并非所有的都是如此。例如，矩阵实现可能使用乘法单位矩阵作为默认值，在这种情况下，函数将是错误的。这就是为什么我更喜欢直截了当地说什么是需要的，附加的身份价值，零。***

# **自己动手**

**如果你想要一个关于这个主题的练习，我建议你尝试实现像**平均**、**模式**和**中位数**这样的函数。根据您决定如何实现它们，每一个都需要不同的特征，所以它们是一个很好的实践。*提示:如果您在处理* `*usize*` *(您会明白我的意思)时发现一些问题，请检查 num 机箱。***

**![](img/fa77859a445ede7405b4f275203f2b68.png)**

**费里斯说再见**

**这个帖子到此为止。希望我能尽快带着更多回来。我将在这里留下一个包含完整示例的存储库:**

**[](https://github.com/kriogenia/medium/tree/main/generify_with_compiler_errors) [## medium/generify _ with _ compiler _ errors at main kriogenia/medium

### 存放我在 Medium 帖子上使用的示例的库——Medium/generify _ with _ compiler _ errors 在 main …

github.com](https://github.com/kriogenia/medium/tree/main/generify_with_compiler_errors)**