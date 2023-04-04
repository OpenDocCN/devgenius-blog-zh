# 用 Goroutines 比较慢？？

> 原文：<https://blog.devgenius.io/using-goroutines-is-slower-ba99c2fadce9?source=collection_archive---------4----------------------->

啊，戈鲁丁斯。Go 编程语言最重要的特性之一。一旦你理解了 goroutines 的语法和并发性背后的理论，你会觉得好像获得了一种超能力。一把锤子，如果你愿意的话。我们很兴奋能让一切同时进行。我肯定有罪。我是说，为什么不呢，对吧？并发性解决了代码阻塞的问题，所以尽可能让所有事情都并发会加快速度，对吗？

有时候，太多太多，并不是所有的事情都是你可以钉的钉子。

# 但是首先，介绍一下 Go 中的并发性

阅读这篇文章，你可能至少有一些编写并发 Go 代码的经验。但是为了以防万一，我将快速解释并发性和 goroutines。

随着我们越来越擅长编程和构建更大的项目，我们不可避免地会遇到障碍。有一项工作至少需要几秒钟。也许那份工作就是给你的用户发一封电子邮件。可能是读取解析 CSV 或者 JSON。也许只是一个傻乎乎的复杂计算。如果你的程序一次只为一个或两个用户服务，那就更好了。然而，想象一下必须发送一百万封电子邮件或者必须解析一百万个 JSON 流对象。你的服务会被这些缓慢的操作屏蔽掉，人们使用起来会有很恐怖的体验。

我们如何解决这个问题？一个人想了一段时间这个问题，决定做好晚饭后再多想想。他想要一只美味多汁的烤鸡。他开始腌制鸡肉，并把它放进冰箱，让它静置 30 分钟。当鸡肉放在冰箱里的时候，他开始切一些生菜和洋葱做沙拉…然后它来了。这是他需要做的！让阻塞代码先运行，同时运行代码的其他部分！他可以在鸡肉腌好之后检查一下，然后再烤！

上面的故事是对并发程序如何工作的一种过于简单化的描述。Go 使用 goroutines 来委派这些任务。主 goroutine 负责运行`main`函数，工作 go routine 各自处理并发运行的代码部分。

现在，并发与并行有点不同，并行是一个类似的概念。然而，并行就像让两个厨师同时做不同的菜，而并发就像让一个厨师处理不同的任务。是的，这可能会令人困惑。我认为这种困惑源于我们把每一个 goroutine 都当成了独立的物体。为了简化，我们称他们为*工人 T2，但他们实际上并不是一起工作的独立工人。它们仅仅是由一个厨师完成的*道独立工序*。他们是 *goroutines* ，不是 *goworkers* (懂了吗？协程和同事？是吗？…好吧，我停下来)。*

# 超级有效！

我们将在实验中使用下面的片段。

```
package mainimport (
    "encoding/csv"
    "fmt"
    "os"
)func main() {
    db := map[string][][]string{
        "AgeDataset-V1.csv": nil,
        "neo_v2.csv":        nil,
        "nba.csv":           nil,
        "airquality.csv":    nil,
        "titanic.csv":       nil,
    }
    for file := range db {
        db[file] = ReadCsv(file)
    }
}func ReadCsv(filepath string) [][]string {
    f, err := os.Open(filepath)
    if err != nil {
        fmt.Println(err)
    }
    defer f.Close() csvr := csv.NewReader(f)
    rows, err := csvr.ReadAll()
    if err != nil {
        fmt.Println(err)
    } return rows
}
```

`ReadCsv`是一个非常简单的代码，用来读取 CSV 文件。它接受文件的路径，并以`[][]string`的格式返回文件中的数据。`main`函数包含一个`db`对象，它是一个映射，将文件路径匹配到里面的数据。这个`main`函数是串行的，因为在循环内部的操作完成之前，for 循环不会进入下一次迭代。

下面是`main`函数的一个并发版本:

```
func main() {
    db := map[string][][]string{
        "AgeDataset-V1.csv": nil,
        "neo_v2.csv":        nil,
        "nba.csv":           nil,
        "airquality.csv":    nil,
        "titanic.csv":       nil,
    } var wg sync.WaitGroup wg.Add(5)
    for file := range db {
        go func(file string) {
            defer wg.Done()
            db[file] = ReadCsv(file)
        }(file)
    }
    wg.Wait()
}
```

我们首先创建`wg`，一个 waitgroup，它检查有多少 goroutines 仍在工作。对于每个文件，我们启动一个运行`ReadCsv`的 goroutine，并在完成后从`wg`中减去 1。我们在最后等待，直到每一次例行公事都结束。

我们的基准代码非常简单:

```
func BenchmarkMain(b *testing.B) {
    for i := 0; i < b.N; i++ {
        main()
    }
}
```

以下是基准测试结果:

```
// serial
$ go test -bench=Main -benchtime=10s
goos: linux
goarch: amd64
pkg: example.com/slowconc
cpu: Intel(R) Core(TM) i7-7700K CPU @ 4.20GHz
BenchmarkMain-8               18         609132683 ns/op
PASS
ok      example.com/slowconc    11.633s// concurrent
$ go test -bench=Main -benchtime=10s
goos: linux
goarch: amd64
pkg: example.com/slowconc
cpu: Intel(R) Core(TM) i7-7700K CPU @ 4.20GHz
BenchmarkMain-8               20         559101265 ns/op
PASS
ok      example.com/slowconc    11.781s
```

您可以看到并发代码比串行代码快了大约 50 毫秒。在这些情况下，我们很高兴:并发确实使我们的代码运行得更快。让我们看看当这不发生时会发生什么。

# 不是很有效…

让我们看看什么时候使用 goroutines 适得其反。

```
func FindSum(list []int) int {
    sum := 0
    for _, number := range list {
        sum += number
    }
    return sum
}func FindSumConc(list []int) int {
    sum := 0
    var rwm sync.RWMutex var wg sync.WaitGroup
    wg.Add(len(list))
    for _, number := range list {
        go func(number int) {
            defer wg.Done()
            rwm.Lock()
            defer rwm.Unlock()
            sum += number
        }(number)
    }
    wg.Wait() return sum
}
```

这是我们将用来运行第二个实验的两个函数。这两个函数都返回列表中整数的和。并发版本使用互斥来防止数据竞争。这意味着当一个 goroutine 写入`sum`时，其他 go routine 无法写入。

我们将使用这个基准代码:

```
func BenchmarkFindSum(b *testing.B) {
    list := make([]int, 0)
    for i := 0; i < 1000000; i++ {
        list = append(list, rand.Intn(1000000))
    }
    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        FindSum(list)
    }
}func BenchmarkFindSumConc(b *testing.B) {
    list := make([]int, 0)
    for i := 0; i < 1000000; i++ {
        list = append(list, rand.Intn(1000000))
    }
    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        FindSumConc(list)
    }
}
```

结果如下:

```
$ go test -bench=FindSum -benchtime=10s
goos: linux
goarch: amd64
pkg: example.com/slowconc
cpu: Intel(R) Core(TM) i7-7700K CPU @ 4.20GHz
BenchmarkFindSum-8                 50497            243623 ns/op
BenchmarkFindSumConc-8                45         266713213 ns/op
PASS
ok      example.com/slowconc    27.144s
```

令人惊讶，不是吗？您会认为并发版本运行得更快。毕竟有一百万个整数要加。但是不行，并发版本要慢 1000 倍左右。

# 为什么会这样呢？

简单来说，就是因为两个问题的性质。软件开发人员遇到的瓶颈主要有两种类型:

*   **CPU 受限的作业。**这些工作依赖于你的 CPU 速度。像复杂的计算、破解加密和寻找圆周率的第 n 位数字这样的工作都是受 CPU 限制的。
*   与 IO 相关的工作。这些工作依赖于读取速度和写入速度。像从文件中读取数据和通过网络请求资源这样的工作是 IO 绑定的。

在我们的例子中，读取 CSV 文件的第一个例子是 IO 绑定的，因为 goroutines 必须调用操作系统来读取文件，并等待数据被使用。我们计算总和的第二个例子是 CPU 受限的，因为我们一直在计算，直到我们处理完列表中的最后一个整数。

在处理并发时，我们需要理解一个概念。当有多个 goroutine 运行时，我们管理 go routine 的调度程序需要决定运行哪个 go routine。记住，并发不是同时运行几个作业；这是关于任务之间的转换。这种变戏法的行为被称为**上下文切换。**

这里有一个问题:当我们从一个 goroutine 切换到另一个 goroutine 时，被换出的 go routine 在技术上暂时被暂停。更确切地说，它们进入两种状态之一:等待或可运行。等待状态意味着 goroutine 正在等待系统或网络的响应。可运行状态意味着 goroutine 需要关注，这样它就可以运行它正在做的任何事情。

让我们想想这个。当 goroutine 处于等待状态时，调度程序现在不必花时间担心它，因为 goroutine 无论如何都要等待响应。因此，短暂的停顿通常是好的。但是，如果 goroutine 处于运行状态，这意味着计算会暂停，直到调度程序将其换入。这里的短暂停顿会对性能造成毁灭性的影响。

如果你需要投掷和捕捉十个球，这是一个 IO 绑定的任务。你扔一个球，然后继续下一个球。当你转换的时候，你并没有放慢你的进度，因为当你扔出一个球的时候，这个球需要时间飞起来，然后再落下来。你基本上是在休息时间扔剩下的球。

然而，如果你需要清空十个球篮，这是一个 CPU 密集型的任务。唯一能让它更快的方法就是更快地扔掉球。从一个篮切换到另一个篮会阻止其他篮中发生任何清空。所以在这里，切换并不能帮助你更快地完成。如果有的话，切换到另一个篮子的行为将增加延迟，降低您的速度。

与 CPU 相关的作业相比，IO 相关的作业从并发设计中获益更多。我们也可以在我们的基准测试结果中看到这一点。

# 结论

希望这篇文章不会太混乱。我记得在尝试研究并发性时，我完全迷失了方向。如果您想知道为什么您的并发代码运行缓慢，请检查您的作业是受 CPU 限制还是受 IO 限制。如果受 CPU 限制，您可能需要利用并行性。或者更简单的说，做一些优化，提高你的算法的时间复杂度。

感谢阅读！你可以在[发展到](https://dev.to/jpoly1219/using-goroutines-is-slower-3b53)和[我的个人网站](https://jpoly1219.github.io)上阅读这篇文章。