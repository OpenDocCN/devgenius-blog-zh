# 通过避免测试中的恐慌来编写更好的 Golang 代码

> 原文：<https://blog.devgenius.io/write-better-golang-code-by-avoiding-panic-in-tests-f6a7a22937e6?source=collection_archive---------1----------------------->

![](img/a21edf6ca9c6a945e6112941b7077500.png)

由 [Jasmin Sessler](https://unsplash.com/@jasmin_sessler) 在 [Unsplash](https://unsplash.com/photos/egqR_zUd4NI) 上拍摄的原始照片

当您编写 Golang 代码(特别是本文的测试)时，您会希望尽量避免使用`panic()`函数。这是一个很好的经验法则，它得到了优步特别好的 [Golang 风格指南的支持。](https://github.com/uber-go/guide/blob/master/style.md#error-types)

对于测试，您可能想要检查错误，在这种情况下，您可能会尝试使用`panic()`,如下面的代码块所示:

```
// func TestStrConversion(t *testing.T)

f, err := utils.ConvertStr("input", "output")
if err != nil {
  panic("err in ConvertStr")
}
```

不用太担心所有的功能是什么；这里需要注意的重要一点是，如果函数返回了一个错误，我们会生成代码`panic()`。然而，这正是我们想要避免的。《优步围棋风格指南》称:“恐慌是连锁故障的主要来源。如果发生错误，函数必须返回一个错误，并允许调用者决定如何处理它

因此，我们可以使用`t.Fatal`和/或`t.FailNow`，而不是使用`panic()`。这来自于`t *testing.T`的论证。

您可以编写的更好的 Golang 测试代码可以是这样的:

```
// func TestStrConversion(t *testing.T)

f, err := utils.ConvertStr("input", "output")
if err != nil {
  t.Fatal("err in ConvertStr")
}
```

那么什么是`t.Fatal`和`t.FailNow`？

## t.致命的

`t.Fatal`真的是两个东西的组合:`t.Log`和`t.FailNow`。这是`testing`包的 [Go 文档](https://pkg.go.dev/testing#B.Log)对`Log`的描述:

> Log 使用默认格式设置其参数的格式，类似于 Println，并将文本记录在错误日志中。对于测试，只有当测试失败或设置了-test.v 标志时，才会打印文本。对于基准测试，总是打印文本，以避免性能依赖于-test.v 标志的值。

接下来我们就来说说`t.FailNow`。

## t.故障排除

同样，让我们参考 [Go 文档](https://pkg.go.dev/testing#T.FailNow)中的`testing`包:

> FailNow 将函数标记为失败，并通过调用 runtime 来停止其执行。Goexit(然后运行当前 goroutine 中的所有延迟调用)。执行将在下一次测试或基准测试时继续。FailNow 必须从运行测试或基准函数的 goroutine 调用，而不是从测试期间创建的其他 go routine 调用。调用 FailNow 不会停止其他 goroutines。

所以，总而言之，你通常会希望在 Golang 代码中避免使用`panic()`，特别是对于本文，这也适用于你的 Go 测试。用`t.Fatal`或`t.FailNow`来代替，你就能写出更好的 Go 代码了。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)