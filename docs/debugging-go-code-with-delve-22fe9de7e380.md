# 用 Delve 调试 Go 代码

> 原文：<https://blog.devgenius.io/debugging-go-code-with-delve-22fe9de7e380?source=collection_archive---------1----------------------->

> 天才是百分之一的天赋加上百分之九十九的努力
> 
> *“e = MC”*
> 
> *—阿尔伯特·爱因斯坦*

在软件开发领域，报价可能会稍有变化:

> *“软件开发就是 1%编程，99%调试。”*
> 
> *“错误=更多代码^ 2”*
> 
> *—某资深开发者，可能是*

调试是所有开发人员都必须经历的事情，它不在乎你的专业知识。这是一个令人沮丧的过程。犯错是人之常情，错误绝对会潜入你的程序。我不知道为什么，但是错误就像你的代码一样是你的后代，所以你有责任处理它们。

# 我们都喜欢打印调试…

如果你是计算机科学专业的本科生，那么你可能有过一个不愉快的经历，那就是某个项目似乎没有按照你想要的方式运行。这也适用于其他开发人员。我的代码不工作，我不明白为什么。

我不知道为什么会这样，但是开发人员普遍倾向于打印调试。在代码中您认为有错误的部分，您应该打印输入和输出变量。

对于修复简单的错误，打印调试通常更快。调试最重要的部分是检查您是否向函数提供了正确的输入，以及函数是否产生了想要的输出。打印语句可以很容易地处理这个问题。然而，有一些讨厌的 bug 往往出现在一个函数中，但却来自一个遥远的函数。还有一些情况下，你可能想检查调用栈，跟踪变量的生命周期，等等。

相信我，你会遇到这样的情况，你觉得你在浪费大量的时间来打印报表。你的代码看起来会很丑，你会迷失在自己的代码中。那些时候，你希望你知道如何使用调试器。

如果你不相信，试着这样想。知道如何使用调试器很好。我认为，因为不知道如何使用调试器而使用打印调试是一种糟糕的做法。像任何技能一样，不知道不应该是你不使用/学习它的主要原因。那就像叫外卖，因为你不知道怎么做饭。或者因为不知道如何使用健身器材而不健身。或者因为不理解一个概念而不学习……你懂了。

# 使用 Delve 调试器

有一个流行的调试器叫做 **gdb** 可以调试 Go 代码。然而，Go 文档并不推荐它，而是向您指出一个更好的选择: **Delve** 。

Delve 就像一个工具箱，里面有很多工具可以帮助你消灭那些讨厌的 bug。可以用诱饵站就不用徒手抓蟑螂了。Delve 为你提供杀虫剂、诱饵站、火把等。Delve 是专门用来抓 Go bugs 的，相当好用。

首先，让我们安装它。如果您运行的是 Go v1.16 或更高版本(您可能应该这样做)，您可以使用下面的代码行:

```
$ go install github.com/go-delve/delve/cmd/dlv@latest
```

确保通过运行以下命令安装了它:

```
$ dlv version
```

Delve 用于调试主包和测试。不幸的是，除了`main`之外，它在调试包方面相当有限，因为 Delve 需要程序的工作可执行文件，这需要一个`main`包。像我一样，如果你正在编写一个没有`main`包的库，你需要为你的代码编写测试，然后调试它。

# 回顾我最常用的命令，并附有示例代码

这是我们将在这个例子中使用的代码。

```
package mainimport (
    "fmt"
    "math"
)func main() {
    n1 := []float64{0.1, 0.2, 0.3, 0.4, 0.5}
    n2 := []float64{math.NaN(), 0.2, 0.3, 0.4, 0.5} mean1 := calcMean(n1)
    mean2 := calcMean(n2) fmt.Println("mean1:", mean1)
    fmt.Println("mean2:", mean2)
}func calcMean(nums []float64) float64 {
    mean := 0.0
    for _, num := range nums {
        mean += num
    }
    mean /= float64(len(nums)) return mean
}
```

代码真的很简单！`main`函数定义了两个切片`n1`和`n2`，并调用`calcMean`计算每个平均值。这段代码看起来很好，编译器也没有抱怨。如果有任何语法错误，我们甚至不能编译我们的代码。然而，这段代码实际上有一个小问题。你能猜出它可能是什么吗？

好吧，让我们运行代码，看看会发生什么。

```
$ go run main.go
mean1: 0.3
mean2: NaN
```

有意思。`mean1`看起来还行，但是`mean2`看起来有点怪。我们想要一个数字，而不是一个`NaN`值。为什么会这样？让我们使用调试器来弄清楚到底发生了什么。

`cd`到包含您的`main.go`文件的目录中，然后运行:

```
$ dlv debug
```

如果您正在调试测试，您可以`cd`进入保存您的`*_test.go`文件的目录，然后运行这个:

```
$ dlv test
```

据我所知，Delve 没有 GUI 前端，所以你需要使用命令行来使用 Delve。这很好，因为 Delve 有一个友好的 CLI，你实际上可以理解。

```
$ dlv debug
Type 'help' for list of commands.
(dlv)
```

你会看到我们已经进入了 Delve 界面。现在我们可以运行特定于 Delve 的命令来帮助我们修复这个 bug。

```
(dlv) break main.go:8
Breakpoint 1 set at 0x496732 for main.main() ./main.go:8
```

第一个命令是`break`。它会在您的代码中创建一个*断点*。断点是代码中可以停止执行的点。在这种情况下，我在文件`main.go`的第 8 行设置了一个断点。在可疑代码之前设置断点很有用。通常，它是一个接受输入并返回输出的函数。要转到断点，我们可以使用以下命令:

```
(dlv) continue
> main.main() ./main.go:8 (hits goroutine(1):1 total:1) (PC: 0x496732)
     3: import (
     4:         "fmt"
     5:         "math"
     6: )
     7:
=>   8: func main() {
     9:         n1 := []float64{0.1, 0.2, 0.3, 0.4, 0.5}
    10:         n2 := []float64{math.NaN(), 0.2, 0.3, 0.4, 0.5}
    11:
    12:         mean1 := calcMean(n1)
    13:         mean2 := calcMean(n2)
```

`continue`将继续遍历代码，直到到达下一个断点。如果没有设置断点，`continue`将只是运行整个程序，结束运行。在这里，continue 将把我们带到第 8 行。注意，断点实际上不会运行指定的行。现在让我们一行一行来。

```
(dlv) next
> main.main() ./main.go:9 (PC: 0x496749)
     4:         "fmt"
     5:         "math"
     6: )
     7:
     8: func main() {
=>   9:         n1 := []float64{0.1, 0.2, 0.3, 0.4, 0.5}
    10:         n2 := []float64{math.NaN(), 0.2, 0.3, 0.4, 0.5}
    11:
    12:         mean1 := calcMean(n1)
    13:         mean2 := calcMean(n2)
    14:
(dlv) next
> main.main() ./main.go:10 (PC: 0x4967fa)
     5:         "math"
     6: )
     7:
     8: func main() {
     9:         n1 := []float64{0.1, 0.2, 0.3, 0.4, 0.5}
=>  10:         n2 := []float64{math.NaN(), 0.2, 0.3, 0.4, 0.5}
    11:
    12:         mean1 := calcMean(n1)
    13:         mean2 := calcMean(n2)
    14:
    15:         fmt.Println("mean1:", mean1)
```

我们使用`next`命令转到下一行。这里，我们遍历第 9 行和第 10 行，在那里我们定义了`n1`和`n2`。让我们看看这些片段在调试器中是如何表示的。

```
(dlv) print n1
[]float64 len: 5, cap: 5, [0.1,0.2,0.3,0.4,0.5]
(dlv) print n2
Command failed: could not find symbol value for n2
```

我们使用`print`命令来查看变量的细节。然而，我们看到了一些奇怪的东西。`print n1`有效，但`print n2`无效。这是因为当我们在某一行时，该行不会被执行，直到我们移动到下一行。`n1`在第 9 行定义，而`n2`在第 10 行定义。我们目前在第 10 行，这意味着第 9 行已经执行，但是第 10 行还没有执行。要解决这个问题，我们只需要转到下一行。

```
(dlv) next
> main.main() ./main.go:12 (PC: 0x4968a5)
     7:
     8: func main() {
     9:         n1 := []float64{0.1, 0.2, 0.3, 0.4, 0.5}
    10:         n2 := []float64{math.NaN(), 0.2, 0.3, 0.4, 0.5}
    11:
=>  12:         mean1 := calcMean(n1)
    13:         mean2 := calcMean(n2)
    14:
    15:         fmt.Println("mean1:", mean1)
    16:         fmt.Println("mean2:", mean2)
    17: }
(dlv) print n1
[]float64 len: 5, cap: 5, [0.1,0.2,0.3,0.4,0.5]
(dlv) print n2
[]float64 len: 5, cap: 5, [NaN,0.2,0.3,0.4,0.5]
```

你现在可以看到`print n2`是如何按预期工作的了。

我们现在在第 12 行，这是使用调试器区别于打印调试的地方。Delve 有一个特性，你可以*进入*一个函数。顾名思义，单步执行函数允许我们一步一步地进入函数并检查函数。我会告诉你这意味着什么:

```
(dlv) step
> main.calcMean() ./main.go:19 (PC: 0x496ac0)
    14:
    15:         fmt.Println("mean1:", mean1)
    16:         fmt.Println("mean2:", mean2)
    17: }
    18:
=>  19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
    21:         for _, num := range nums {
    22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
```

使用`step`进入一个功能。我们可以看到我们的范围是如何从`main`函数变成`calcMean`函数的。

```
(dlv) args
nums = []float64 len: 5, cap: 5, [...]
~r0 = 0.000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004074232364696
```

使用`args`将显示功能参数。`nums`是`calcMean`的输入。`~r0`表示函数的返回值。`r`代表返回，`0`代表第 0 个返回值。如果我们有多个返回值，我们会有类似于`r0, r1, r2...`的东西。`~`代表一个未命名的值。我们知道`calcMean`将返回一个`float64`，但是我们没有为它指定一个名称。

我们也可以使用`print`命令检查功能输入。

```
(dlv) print nums
[]float64 len: 5, cap: 5, [0.1,0.2,0.3,0.4,0.5]
```

所以我们知道输入没有问题。我们继续吧。我们希望跟踪`mean`变量的值。为此，使用`display`命令。

```
(dlv) display -a mean
0: mean = error could not find symbol value for mean
(dlv) next
> main.calcMean() ./main.go:20 (PC: 0x496ae5)
    15:         fmt.Println("mean1:", mean1)
    16:         fmt.Println("mean2:", mean2)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
=>  20:         mean := 0.0
    21:         for _, num := range nums {
    22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
0: mean = error could not find symbol value for mean
(dlv) next
> main.calcMean() ./main.go:21 (PC: 0x496aeb)
    16:         fmt.Println("mean2:", mean2)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
=>  21:         for _, num := range nums {
    22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
0: mean = 0
```

使用`-a`标志将变量添加到我们的显示列表中。如果我们移到下一行，您可以看到`mean`的值是如何显示在底部的。

有一个小错误，说我们如何`could not find symbol value`。这是因为`mean`还没有定义。当我们在第 21 行并且已经定义了`mean`时，你可以看到这个错误是如何消失的。

```
(dlv) next
> main.calcMean() ./main.go:22 (PC: 0x496b44)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
    21:         for _, num := range nums {
=>  22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
    27: }
0: mean = 0
(dlv) next
> main.calcMean() ./main.go:21 (PC: 0x496b56)
    16:         fmt.Println("mean2:", mean2)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
=>  21:         for _, num := range nums {
    22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
0: mean = 0.1
(dlv) next
> main.calcMean() ./main.go:22 (PC: 0x496b44)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
    21:         for _, num := range nums {
=>  22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
    27: }
0: mean = 0.1
(dlv) next
> main.calcMean() ./main.go:21 (PC: 0x496b56)
    16:         fmt.Println("mean2:", mean2)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
=>  21:         for _, num := range nums {
    22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
0: mean = 0.30000000000000004
(dlv) next
> main.calcMean() ./main.go:22 (PC: 0x496b44)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
    21:         for _, num := range nums {
=>  22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
    27: }
0: mean = 0.30000000000000004
(dlv) next
> main.calcMean() ./main.go:21 (PC: 0x496b56)
    16:         fmt.Println("mean2:", mean2)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
=>  21:         for _, num := range nums {
    22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
0: mean = 0.6000000000000001
(dlv) next
> main.calcMean() ./main.go:22 (PC: 0x496b44)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
    21:         for _, num := range nums {
=>  22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
    27: }
0: mean = 0.6000000000000001
(dlv) next
> main.calcMean() ./main.go:21 (PC: 0x496b56)
    16:         fmt.Println("mean2:", mean2)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
=>  21:         for _, num := range nums {
    22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
0: mean = 1
(dlv) next
> main.calcMean() ./main.go:22 (PC: 0x496b44)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
    21:         for _, num := range nums {
=>  22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
    27: }
0: mean = 1
(dlv) next
> main.calcMean() ./main.go:21 (PC: 0x496b56)
    16:         fmt.Println("mean2:", mean2)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
=>  21:         for _, num := range nums {
    22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
0: mean = 1.5
(dlv) next
> main.calcMean() ./main.go:24 (PC: 0x496b65)
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
    21:         for _, num := range nums {
    22:                 mean += num
    23:         }
=>  24:         mean /= float64(len(nums))
    25:
    26:         return mean
    27: }
0: mean = 1.5
```

我为冗长的输出道歉，但是我真的想向您展示`mean`在每次迭代中是如何变化的。你可以看到平均值在增加。

```
(dlv) next
> main.calcMean() ./main.go:26 (PC: 0x496b87)
    21:         for _, num := range nums {
    22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
=>  26:         return mean
    27: }
0: mean = 0.3
```

`mean`除以后等于 0.3，因此对于`n1`的输入，该函数按预期工作。让我们看看当我们输入`n2`时会发生什么。

```
(dlv) next
> main.main() ./main.go:12 (PC: 0x4968c5)
Values returned:
        ~r0: 0.3 7:
     8: func main() {
     9:         n1 := []float64{0.1, 0.2, 0.3, 0.4, 0.5}
    10:         n2 := []float64{math.NaN(), 0.2, 0.3, 0.4, 0.5}
    11:
=>  12:         mean1 := calcMean(n1)
    13:         mean2 := calcMean(n2)
    14:
    15:         fmt.Println("mean1:", mean1)
    16:         fmt.Println("mean2:", mean2)
    17: }
0: mean = error could not find symbol value for mean
(dlv) display -d 0
(dlv)
```

当我们到达返回线时，我们就离开了范围。我们需要从显示列表中删除`mean`，因为我们不再跟踪它了。运行`display -d`并使用列表中变量的 id 跟随它。在我们的例子中，它是 0。

```
(dlv) next
> main.main() ./main.go:13 (PC: 0x4968cb)
     8: func main() {
     9:         n1 := []float64{0.1, 0.2, 0.3, 0.4, 0.5}
    10:         n2 := []float64{math.NaN(), 0.2, 0.3, 0.4, 0.5}
    11:
    12:         mean1 := calcMean(n1)
=>  13:         mean2 := calcMean(n2)
    14:
    15:         fmt.Println("mean1:", mean1)
    16:         fmt.Println("mean2:", mean2)
    17: }
    18:
(dlv) step
> main.calcMean() ./main.go:19 (PC: 0x496ac0)
    14:
    15:         fmt.Println("mean1:", mean1)
    16:         fmt.Println("mean2:", mean2)
    17: }
    18:
=>  19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
    21:         for _, num := range nums {
    22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
(dlv) print nums
[]float64 len: 5, cap: 5, [NaN,0.2,0.3,0.4,0.5]
```

我们再次进入`calcMean`功能。我们打印`nums`并检查输入是否符合预期。

```
dlv) display -a mean
0: mean = error could not find symbol value for mean
(dlv) next
> main.calcMean() ./main.go:20 (PC: 0x496ae5)
    15:         fmt.Println("mean1:", mean1)
    16:         fmt.Println("mean2:", mean2)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
=>  20:         mean := 0.0
    21:         for _, num := range nums {
    22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
0: mean = error could not find symbol value for mean
(dlv) next
> main.calcMean() ./main.go:21 (PC: 0x496aeb)
    16:         fmt.Println("mean2:", mean2)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
=>  21:         for _, num := range nums {
    22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
0: mean = 0
```

我们使用`display`命令来跟踪`mean`。

```
(dlv) next
> main.calcMean() ./main.go:22 (PC: 0x496b44)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
    21:         for _, num := range nums {
=>  22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
    27: }
0: mean = 0
(dlv) next
> main.calcMean() ./main.go:21 (PC: 0x496b56)
    16:         fmt.Println("mean2:", mean2)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
=>  21:         for _, num := range nums {
    22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
0: mean = NaN(dlv) next
> main.calcMean() ./main.go:22 (PC: 0x496b44)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
    21:         for _, num := range nums {
=>  22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
    27: }
0: mean = NaN
(dlv) next
> main.calcMean() ./main.go:21 (PC: 0x496b56)
    16:         fmt.Println("mean2:", mean2)
    17: }
    18:
    19: func calcMean(nums []float64) float64 {
    20:         mean := 0.0
=>  21:         for _, num := range nums {
    22:                 mean += num
    23:         }
    24:         mean /= float64(len(nums))
    25:
    26:         return mean
0: mean = NaN
```

女士们先生们，这就是窃听器的位置。注意当我们添加第一个元素`nums`时`mean`是如何变成`NaN`的。从那时起，无论我们给它添加什么，`mean`都会保持`NaN`。显然，这是一种不受欢迎的行为。我们想忽略 NaN 值，在没有它们的情况下计算平均值。

使用`exit`退出调试器。

让我们回到我们的代码，将`calcMean`代码更新如下:

```
func calcMean(nums []float64) float64 {
    mean := 0.0
    nanCount := 0
    for _, num := range nums {
        if math.IsNaN(num) {
            nanCount++
            continue
        }
        mean += num
    }
    mean /= float64(len(nums) - nanCount) return mean
}
```

如果我们遇到一个 NaN 值，我们将`nanCount`加 1，并通过使用`continue`跳到下一次迭代。计算平均值时，我们用`nums`的和除以`nums`的长度减去 NaNs 的个数。

现在让我们看看我们的代码是如何运行的。

```
$ go run main.go
mean1: 0.3
mean2: 0.35
```

不错！我们成功地调试了代码。

# 结论

我们学会了如何像专家一样使用 Delve 调试我们的代码。当然，上面的例子可以通过在`calcMean`的循环中添加一个 print 语句来轻松调试。然而，当你在旅途中遇到讨厌的 bug 时，你会感谢自己读了这篇帖子，知道如何使用调试器。Delve 提供了更多的命令，你可以在他们的[文档](https://github.com/go-delve/delve/tree/master/Documentation/cli)中查看。上一次，我们学习了如何编写测试来防止错误。当错误出现时，我们现在知道如何使用调试器来消除它们。

感谢您的阅读！你也可以在 [Dev.to](https://dev.to/jpoly1219/debugging-go-code-with-delve-2l67) 和[我的个人网站](https://jpoly1219.github.io)上阅读这篇文章。