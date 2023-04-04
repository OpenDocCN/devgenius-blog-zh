# 通过在初始化结构时指定字段名来编写更好的 Golang 代码

> 原文：<https://blog.devgenius.io/write-better-golang-code-by-specifying-field-names-when-initializing-structs-519ffa33cccd?source=collection_archive---------3----------------------->

![](img/17950b8dad15c3d263b86a3bd70a9b0f.png)

由[坎贝尔](https://unsplash.com/@campful)在 [Unsplash](https://unsplash.com/photos/3ZUsNJhi_Ik) 上拍摄的原始照片

优步有一个很棒的 [Go 风格指南，它详细介绍了公司编写 Golang 代码的注意事项，当你和/或你的团队也在编写 Go 时，它会是一个有用的资源。](https://github.com/uber-go/guide/blob/master/style.md)

我认为特别有用的一点是，在初始化结构时指定字段名。

那到底是什么样子的呢？

当你初始化结构时，你不必指定字段名。可能是这样的:

```
c := Car{"Toyota", "Camry", 2018, true}
```

现在，如果我们了解汽车，我们可以猜测这些领域是什么，但当内部细节不是很清楚时，这可能有点模糊。这不仅适用于代码的作者和维护者，尤其适用于接触代码的新开发人员。

还有，那里的`true`到底是什么意思？你对汽车了解多少并不重要——不管它指的是什么都不清楚。

那么，优步在其风格指南中建议的是实际指定字段名:

```
c := Car {
    Make: "Toyota",
    Model: "Camry",
    Year: 2018,
    Available: true
}
```

这样，你在结构体中初始化什么就很清楚了。

顺便说一下，优步的围棋风格指南提到，“这现在由`[go vet](https://golang.org/cmd/vet/)`强制执行。”

> Go 包 cmd 有更多的文档:
> 
> Vet 检查 Go 源代码并报告可疑的构造，比如参数与格式字符串不一致的 Printf 调用。Vet 使用试探法，不能保证所有的报告都是真正的问题，但是它可以发现编译器没有发现的错误。
> 
> Vet 通常通过 go 命令调用。该命令检查当前目录中的包:
> 
> 去兽医那里

但是不管怎样，只要记住指定这些字段名就行了！

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)