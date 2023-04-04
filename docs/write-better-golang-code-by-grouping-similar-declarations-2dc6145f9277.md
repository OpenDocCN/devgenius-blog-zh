# 通过对相似的声明进行分组，编写更好、可读性更强的 Golang 代码

> 原文：<https://blog.devgenius.io/write-better-golang-code-by-grouping-similar-declarations-2dc6145f9277?source=collection_archive---------1----------------------->

![](img/262e9c209672abec3b57e9d5fb8d4ab7.png)

迈克尔·魏德曼在 [Unsplash](https://unsplash.com/photos/oGhTfu1UrOY) 上的原始照片；Golang 的 Gopher。

在本文中，我们将学习一个简单的技巧来编写更好的 go 代码。

当编写 Go 代码时，您希望确保您的方法是一致的，同时也鼓励可读性。这是一个平衡的行为，所以确保你和你的团队成员，如果适用的话，遵守某些风格指南是很重要的。

对 Go 代码的一个简单改进是将常量、变量甚至类型常量的声明组合在一起。我的意思是 Go 支持以下内容:

```
var (
   a = 5
   b = 10
)
```

在这个代码块中，我们声明了两个变量，`a`和`b`。这里的改进是我们将它们组合在一个声明中。您也可以用其他语言的声明看起来更熟悉的常用方式来声明它们:

```
var a = 5
var b = 10
```

从技术上来说，组合在一起并不能节省代码行(尽管它确实删除了关键字`var`的一个声明)，所以你可能会想为什么这样更好。真正的改进来自于您添加的推论，即该语法允许您将相关或相似的变量逻辑地分组在一起。想象一下，我们有两个变量叫做`revenue`和`expenses`，而不是`a`和`b`。它们很相似，所以我们可以把它们归为一类:

```
var (
   revenue = 1000000
   expenses = 50000000
)
```

撇开该业务的粗略数字不谈，变量的逻辑分组可以在代码中的其他变量声明中脱颖而出，如下例所示:

```
var (
   revenue = 1000000
   expenses = 50000000  
)var colorBuilding = red
var ownerName = "Mr. Anderson"
```

正如你所看到的，分组更清楚地表明那些变量与财务有关，而其余的与财务无关。这个例子有点傻，但希望它能说明问题。

请注意，您可以将这种分组语法应用于`const`值:

```
const (
   red = "#FF0000"
   green = "#008000"
)
```

您经常会在 Go 文件顶部的导入分组中看到这种结构。与前面的不同，这种分组并不是由一般的导入来安排的，但是语法至少是相同的，供您参考。让我们看看 Kelsey Hightower 的 [envconfig](https://github.com/kelseyhightower/envconfig) Go 存储库中的`[usage.go](https://raw.githubusercontent.com/kelseyhightower/envconfig/master/usage.go)`中的真实例子:

```
import (
	"encoding"
	"fmt"
	"io"
	"os"
	"reflect"
	"strconv"
	"strings"
	"text/tabwriter"
	"text/template"
)
```

说，当我们在做的时候，我们有一些在这篇文章中给出的建议的好例子！凯尔西*知道他在做什么吗，所以我很乐意把它作为你可以模仿的东西来分享:*

*我也非常推荐阅读有效围棋；[以下是他们对分组话题的看法](https://golang.org/doc/effective_go#commentary):*

> *Go 的声明语法允许对声明进行分组。单个文档注释可以引入一组相关的常量或变量。由于整个申报都已提交，这样的评论往往是敷衍了事。*
> 
> *//解析表达式失败时返回的错误代码。
> var (
> ErrInternal =错误。New("regexp:内部错误")
> ErrUnmatchedLpar = errors。new(" regexp:unmatched '(')"
> ErrUnmatchedRpar = errors。new(" regexp:unmatched ")' "
> ...
> )*
> 
> *分组还可以表示项目之间的关系，例如一组变量受互斥体保护的事实。*
> 
> *var (
> countLock 同步。互斥体
> 输入计数 uint32
> 输出计数 uint32
> 错误计数 uint32*

*关于能够注释一个声明块的说明确实是一个非常强大的功能，您应该利用它来使您的代码更加可读和可理解。*

*现在你有了:一个简单的风格技巧，你可以用它来使你的代码更易读，并且随着时间的推移可能更易维护。再说一次，如果你有一个团队，最好在这一点上达成一致，否则随着时间的推移，你会发现拉请求有些脱节。*

*[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)*