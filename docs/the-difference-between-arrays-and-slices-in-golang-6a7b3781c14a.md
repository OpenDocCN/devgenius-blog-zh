# Golang 中数组和切片的区别

> 原文：<https://blog.devgenius.io/the-difference-between-arrays-and-slices-in-golang-6a7b3781c14a?source=collection_archive---------0----------------------->

## 这篇文章将讨论 Go 中数组和切片的主要区别

![](img/91c6d2393103a3018cb880076072e20a.png)

萨法尔·萨法罗夫在 [Unsplash](https://unsplash.com/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

在 **Golang** 中，定义数组和切片可能看起来很相似，但是你需要知道它们之间的一些差异。此外，您应该知道在 Go 代码中什么时候使用数组，什么时候使用切片。这篇文章的目的是让你对 Go 编程语言中数组和切片的区别有一个基本的了解。

另外，在你运行任何用 Go 编写的代码之前，你需要下载并安装 Go 到你的本地机器*(你可以通过下面的链接下载 Go 到你的本地机器)*

[](https://golang.org/doc/install) [## 下载并安装

### 按照此处描述的步骤快速下载并安装 Go。关于安装的其他内容，您可能会感兴趣…

golang.org](https://golang.org/doc/install) 

# 围棋中的数组

Golang 中的数组定义如下。

```
arr1 := [3]int {7,9,4}// array size is 3
```

这是在 Golang 中声明数组并赋值的最简单的方法。但是这个方法只能在函数内部初始化变量的时候使用。除此之外，您还可以遵循另外两种方法在 Go 代码中声明数组。

```
ex 1:
var arr1 [3]int = [3]int {2,3} ex 2:
var arr1 = [3]int {1,7,9}
```

当你声明一个数组的时候，你需要清楚的定义它的大小，当你初始化数组的时候，你可以赋值到那个大小。
一旦您定义并设置了数组值，您可以使用**“fmt”**轻松打印它们。

```
ex:
fmt.Println(arr1)
```

当您需要更改数组值时，可以使用相关的索引并相应地更改其值。

```
ex:
arr1[2] = 6
fmt.Println(arr1)
```

# 围棋中的切片

您可以使用我们在 Golang 中声明数组时使用的相同方法来声明切片。但不同之处在于，在切片中，您不必提及该特定切片的**大小**。但是在数组中，我们必须在声明时定义它的大小。

```
cities := []string {"London", "NYC", "Colombo", "Tokyo"}ORvar cities = []string {"London", "NYC", "Colombo", "Tokyo"}ORvar cities []string = []string {"London", "NYC", "Colombo", "Tokyo"}
```

当您在 Go 代码中声明切片时，可以使用上面列表中的任何方法。此外，您可以使用下面的**“fmt”**轻松打印切片。

```
fmt.Println(cities)
```

由于切片没有特定的大小，所以除了更改其现有值之外，我们还可以向切片追加值。但是在数组中，我们不能追加值，我们能做的就是改变它现有的值。
您可以使用两种不同的方法**将值**追加到切片。如果您想要将值追加到原始切片，可以按如下方式进行。

```
cities = append(cities, "LA")
fmt.Println(cities)
```

这将把 **LA** 添加到原始切片的末尾并更新它。然后，当您打印原始切片时，它将与其他城市名称一起输出 LA。
但是在切片中，你可以使用内置的 **append()** 函数来添加值，它将创建另一个切片并追加值，而不会覆盖原来的切片。

```
ex:cities := []string {"London", "NYC", "Colombo", "Tokyo"}
fmt.Println(cities)addCity := append(cities, "Auckland")fmt.Println(addCity)
fmt.Println(cities)
```

在上面的例子中，我创建了一个名为 **addCity** 的独立变量，并使用 append 函数来添加值。当您打印 **addCity** 变量时，它将输出城市以及**奥克兰**，但不会覆盖原始切片。您可以通过再次打印原始切片来验证它。

```
// output
[London NYC Colombo Tokyo]
[London NYC Colombo Tokyo Auckland]
[London NYC Colombo Tokyo]
```

此外，您可以直接在 **fmt 中使用该附加功能。Println()** 它也将执行与上面相同的操作。

```
 fmt.Println(append(cities, “Auckland”))
```

感谢您的阅读！