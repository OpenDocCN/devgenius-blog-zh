# 在 Golang 中如何打印出一个变量的类型？

> 原文：<https://blog.devgenius.io/how-do-you-print-out-the-type-of-a-variable-in-golang-f3dd9c837ed0?source=collection_archive---------3----------------------->

![](img/046695538713a479d1635d82a3cd3071.png)

Anton Maksimov juvnsky 在 [Unsplash](https://unsplash.com/photos/bPMO9-1KEOM) 上拍摄的原始照片

当你编程的时候，有时候你想打印出变量的类型。

根据您使用的 IDE(集成开发环境),您可能会将鼠标悬停在变量上，或者以其他方式找出变量的类型，但是假设您想知道如何在不使用 IDE 提供的方式的情况下打印出来。

如何实现这一点的关键在于 Golang 中的`fmt`包，在其文档中描述为:

> 软件包 fmt 实现了格式化的 I/O，其功能类似于 C 语言的 printf 和 scanf。“动词”格式源于 C 语言，但更简单。

格式动词都以`%`符号为前缀，为了打印出变量的类型，您将使用带有`T`的百分号。迷茫？这里有一个例子:

```
num := 5
fmt.Printf("%T\n", num)
```

上面会打印出`num`的类型；在这种情况下，`num`的类型为`int`，其值为`5`。

我们如何实现这一点是因为我们使用了`fmt`包的`Printf()`函数，并且我们使用了那个神奇的动词`%T`来打印第二个参数(也称为 parameter)`num`的类型。

为了描述这里的一切，第一个参数中的字符串中的`\n`是一个换行符。这叫做转义序列，有多个；甲骨文的文档在这里有更详细的介绍[。](https://docs.oracle.com/javase/tutorial/java/data/characters.html)

但是我们跑题了:使用`fmt.Printf()`然后使用`%T`打印出给定变量的类型，就可以了。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)