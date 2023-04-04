# Ruby:闭包和 Arity

> 原文：<https://blog.devgenius.io/ruby-closures-and-arity-f7345cfaf8df?source=collection_archive---------3----------------------->

![](img/1e90c651bd22a82d35efe2c5c947359d.png)

法比安·格罗斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在这篇文章中，我想讨论一下 Ruby 语言中的闭包和 arity。首先，一些定义。闭包是可以传递和执行的代码块，这意味着它们基本上是未命名的方法。在 Ruby 中，三种类型的闭包是 Procs、Blocks 和 Lambdas。Arity 指的是关于传入 Procs、Blocks 和 Lambdas 的参数数量的规则。

即使您以前没有听说过术语闭包，您可能也已经熟悉了将块传递给方法。例如:

```
Output:123
```

在这段代码中，被传递给 **#each** 方法的代码块是

```
**{ |num| puts num }**
```

块通常与迭代器一起使用，以允许用户在调用方法时有更大的灵活性。但是它们是如何实现的呢？一种方法是使用 **yield** 关键字，它向块屈服，然后返回函数。例如:

```
Output:123
```

这是一个使用 **yield** 关键字的 **#each** 方法的简单实现。在第 3 行， **yield** 调用传入该方法的任何块，并将传入 **array[i]** 作为参数。**数组[i]** 可通过块参数 **num** 在块中访问。使用 **yield** 关键字允许我们编写自己的使用块的方法。但是如果我们在没有块被传入的情况下调用 **yield** 会怎么样呢？

```
Output:closures.rb:3:in `block in each’: **no block given (yield) (LocalJumpError)**
```

这引发了奇怪命名的 **LocalJumpError** ，因为该方法试图调用该块，但它不在那里。要创建一个有或没有块都有效的方法，我们可以使用方法 **#block_given？**。

```
Output:123
```

**#block_given？如果给定了一个块，方法返回 true，否则返回 false。因此，上面更新的 **#each** 方法将把每个数组值生成到一个块中(如果给定了一个)，否则将只输出每个数组值。**

到目前为止，我们一直在使用隐式块参数。它们是在其他参数之后传入的，我们访问它们的唯一方式是使用 **yield** 关键字。但是，有一种方法可以使用显式块参数。

```
Output:123
```

使用' **&** '给了我们一个变量，我们可以用它来显式地调用传入的块。虽然在这个方法中没有什么不同，但是这样做的好处是，您可以将块传递给原始方法中的另一个方法，因为现在可以用变量引用它。顺便提一下，以'**&【T3]'开头的参数必须始终是最后一个参数，否则将会引发错误。那么它是如何工作的呢？下面的例子给出了一个线索。**

```
Output:#<Proc:0x00007fcbab852568@closures.rb:11>#<Proc:0x00007fcbab852568@closures.rb:11>#<Proc:0x00007fcbab852568@closures.rb:11>
```

正如你所看到的，它输出相同的 **Proc** 对象 3 次。但是当我们通过一个块时，为什么它是一个 proc？答案是，以'**&【T7]'开头的方法参数对传入的参数调用 **#to_proc** 。那么 proc 和 block 的区别是什么呢？主要区别在于，过程是对象，而块不是。这意味着 proc 可以保存到一个变量中并被传递，并且可以访问为 [**Proc** 类](https://docs.ruby-lang.org/en/2.6.0/Proc.html)定义的所有方法。下面是我们如何将 proc 与更新的 **#each** 方法一起使用。**

```
Output:123
```

注意，我们不需要在这里使用' **&** '，因为这个对象已经是一个 proc 了。但是，如果我们需要将一个过程传递给一个向块屈服的方法，该怎么办呢？

```
Output:123
```

在这个上下文中，' **&** '将一个过程转换成一个块，该块被传递到方法中。差别很微妙，但很重要。为方法参数(方法定义中使用的占位符变量)加前缀时，'**&【T21]'对传入的参数调用 **#to_proc** 。但是，在为一个参数(方法调用期间传入的内容)加前缀时，' **&** '会将参数转换成一个块，并将其传入一个方法。**

我们还没有讨论的最后一种闭包是 lambda。从技术上讲，lambda 是一种 proc，但是它有不同的规则。主要的区别是 arity，也就是关于参数如何传入的规则。如果传入的参数太少或太多，块和过程都不会引发错误，但 lambdas 会。例如:

```
Output:The parameter is nil because no argument was passed to yield.The parameter is nil because no argument was passed to call.**Traceback** (most recent call last):2: from closures.rb:27:in `<main>’1: from closures.rb:20:in `lambda_method’closures.rb:23:in `block in <main>’: **wrong number of arguments (given 0, expected 1) (ArgumentError)**
```

如您所见，block 和 proc 的行为类似。没有向 **yield** 或 **#call** 传递参数，但该参数刚刚被赋值为 **nil** 。然而，lambda 引发了一个 **ArgumentError** ，因为没有参数被传递给 **#call** ，但是它需要一个参数。

这三种类型的闭包都有一个共同点，那就是绑定。绑定是指闭包在本地代码环境中跟踪本地变量和其他工件的能力。因为块是在定义时被调用的，所以局部变量将具有它在生成块的方法被调用时的值。但是，如果变量在两次调用之间发生变化，procs 和 lambdas 将更新该值。

```
Output:2CalvinAlvinColoradoCalifornia
```

这个例子显示了当本地变量**名称**改变时，过程内部的**名称**的值也改变了。对于λ和局部变量**状态**也是如此。

不同类型闭包的最后一个重要区别是返回值。对于所有这三个，闭包的最后一行被隐式返回。然而，显式返回的功能不同。例如:

```
Output:This will get output because explicit returns in lambdas return to the calling method.
```

在第 11 行和第 18 行，文本没有得到输出，因为块或过程中的显式返回退出了调用方法。然而，第 3 行的文本确实得到了输出，因为 lambda 的显式返回返回到了调用方法。

总之，procs、blocks 和 lambdas 是 Ruby 中的三种闭包。它们有很多共同点，但是 lambdas 有不同的关于参数和显式返回的规则，而块不是对象。我们讨论了很多，但是我希望这篇文章让你对 Ruby 中的闭包和 arity 有了更好的理解。编码快乐！