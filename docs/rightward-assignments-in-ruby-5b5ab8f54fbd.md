# 拼音向右赋值

> 原文：<https://blog.devgenius.io/rightward-assignments-in-ruby-5b5ab8f54fbd?source=collection_archive---------5----------------------->

![](img/bdcc006a28e706d167c7eba042a2a31c.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[尼克·费因斯](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral)拍摄

最新版本的 ruby 实验性地增加了一个奇怪的新特性:“向右赋值操作符”

通常在 ruby 程序中，你用传统的方法给变量赋值:

```
variable_name = variable_value
```

但是这个新的实验性特性，符合 Ruby 的形式，为您提供了另一种可能的方法来分配变量。传统的命名操作是“向左”，而新的命名操作是“向右”

```
variable_value => variable_name
```

这似乎是受统计编程语言 R 的可选向右赋值的启发:

```
variable_value -> variable_name
```

我想这对于喜欢代码在视觉上朝那个方向流动的人来说很好，但是我不喜欢。它向后看，结果使代码不那么清晰。

更令人困惑的是，它使用了“Hash Rocket”语法，所以在第一次读取时，我很自然地将其解析为一个键-值对，而不是一个甚至不在 Hash 中的值-键对。

因为这是实验性的，它将只基于用户的反馈，我不能想象任何人会在他们的 Ruby 实践中采用这种方法。

对于那些因为某种原因而错过了 Ruby 代码中这个很少使用的功能的 R 爱好者来说，欢呼吧！你会在家的。

[](https://blog.saeloun.com/2020/08/31/ruby-adds-experimental-rightward-assignment.html) [## Ruby 增加了对向右赋值的实验性支持

### 这篇博文讨论了 ruby 对向右赋值的支持。从历史上看，所有早期的编程…

blog.saeloun.com](https://blog.saeloun.com/2020/08/31/ruby-adds-experimental-rightward-assignment.html) [](https://github.com/ruby/ruby/commit/1b2d351b216661e03d497dfdce216e0d51474664#diff-8312ad0561ef661716b48d09478362f3) [## 向右-由 ASSOC ruby/ruby@1b2d351 分配

### Ruby 编程语言[镜像]。在 GitHub 上创建一个帐户，为 ruby/ruby 开发做贡献。

github.com](https://github.com/ruby/ruby/commit/1b2d351b216661e03d497dfdce216e0d51474664#diff-8312ad0561ef661716b48d09478362f3)  [## r:赋值运算符

### 给名称赋值。x x value ->> x x = value 有三个不同的赋值操作符:其中两个有…

stuff.mit.edu](https://stuff.mit.edu/afs/athena/software/r/current/lib/R/library/base/html/assignOps.html)