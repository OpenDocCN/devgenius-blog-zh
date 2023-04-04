# Go 中任意例程的超时

> 原文：<https://blog.devgenius.io/timeout-for-arbitrary-routines-in-go-88dc1f52866e?source=collection_archive---------9----------------------->

在某些情况下，我们需要从库或外部 API 中调用一些其他已有的函数，用于您正在开发的功能或特性。如果您希望这些调用并发运行，并且对您自己的代码无害，您是如何管理这些调用的？

让我们看一下一些关注点和问题。

# 基本并发

从基础开始，你有两个你的特性需要的函数，分别需要 5s 和 2s 来执行

```
func F1() float64 {
   time.Sleep(5 * time.**Second**)
   return 80.3
}func F2() int {
   time.Sleep(2 * time.**Second**)
   return 45
}func main() {
   //Call ExternalTask1
   //Call ExternalTask2
}
```

我们需要它们并发运行，我们可以通过使用 **go** 关键字让它们在 goroutine 中运行，因为它们是已存在的外部函数/API(如 RestAPI 或 RPC…)，我们不能保证它们每次都能正确运行，一个常见的问题可能是这些函数花费太长时间返回一些预期值，这导致您自己的特性将被这些函数阻塞。为了避免这种情况，我们使用一个上限超时，如果执行时间超过超时值，我们可以认为是一个失败，并根据您的特性提前返回或放弃。

从简单 go routine 实现开始，创建 2 个 go routine 和 2 个通道来接收结果。

```
func main() {
   r1 := make(chan float64)
   r2 := make(chan int) e, result1, result2 := func() (e error, res1 float64, res2 int) {go func(r chan<- float64) {
         r <- F1()
      }(r1)go func(r chan<- int) {
         r <- F2()
      }(r2) res1 = <-r1
      res2 = <-r2
      return }() if e != nil {
      fmt.Println(e)
   }
   fmt.Printf("Got %f and %d\n", result1, result2)
}
```

在最好的情况下，您将获得良好的输出结果

```
Got 80.300000 and 45
```

上面的代码有什么问题？
这里
res1 = < -r1
从通道接收基本上会被阻塞，直到有发送者向通道推送某些东西，反之亦然。因此，即使 F2 只需要 2 秒来完成他们的工作，但 F2 的结果从来不会很快出来，因为它被等待 F1 的结果所阻挡。在代码中添加一些日志来证明这一点

```
func main() { r1 := make(chan float64)
   r2 := make(chan int) e, result1, result2 := func(wg *sync.WaitGroup) (e error, res1 float64, res2 int) {
      log.Print("Start")go func(r chan<- float64) {
         r <- F1()
         log.Print("Finish F1")
      }(r1)
      go func(r chan<- int) {
         r <- F2()
         log.Print("Finish F2")
      }(r2) res1 = <-r1
      log.Print("Received from F1")
      res2 = <-r2
      log.Print("Received from F2")
      return }(&wg) if e != nil {
      fmt.Println(e)
   }
   log.Printf("Got %f and %d\n", result1, result2)
}
```

得到

```
2022/09/19 08:33:59 Start
2022/09/19 08:34:04 Finish F1
2022/09/19 08:34:04 Received from F1
2022/09/19 08:34:04 Received from F2
2022/09/19 08:34:04 Got 80.300000 and 45
2022/09/19 08:34:04 Finish F2
Exiting.
```

我们几乎同时并在开始 5s 后收到 F1 和 F2 的结果，这正是 F1 执行所花费的时间。如果 F1 不返回或花太长时间响应，导致 F2 结果不能被正确接收，甚至阻塞我们的功能，这当然会有问题。

怎样才能同时在多个频道上听而不互相屏蔽？有了它，无论任务完成得多快，它的结果也会被更快地接收到。那就是**选择**关键字来玩。

```
A "select" statement chooses which of a set of possible [send](https://go.dev/ref/spec#Send_statements) or [receive](https://go.dev/ref/spec#Receive_operator) operations will proceed. It looks similar to a ["switch"](https://go.dev/ref/spec#Switch_statements) statement but with the cases all referring to communication operations.
[https://go.dev/ref/spec#Select_statements](https://go.dev/ref/spec#Select_statements)
```

从 Golang 规范中可以推断出一些著名的东西:
*它只精确地评估一次，对于更多的评估，我们可以将它放在循环中或使用 continue
*如果一个或多个通道可以继续，那么可以通过统一的伪随机选择
来选择一个可以继续的通道* Select 只有`nil`个通道，并且永远没有默认的 case 阻塞(如果我们不够小心，这将是一个严重的问题)

好，让我们使用选择

```
for i := 0; i < 2; i++ {
   select {
   case res1 = <-r1:
      {
         log.Print("Received from F1")
      }
   case res2 = <-r2:
      {
         log.Print("Received from F2")
      }
   }
}
```

现在输出

```
2022/09/19 09:15:23 Start
2022/09/19 09:15:25 Finish F2
2022/09/19 09:15:25 Received from F2
2022/09/19 09:15:28 Finish F1
2022/09/19 09:15:28 Received from F1
2022/09/19 09:15:28 Got 80.300000 and 45
Exiting.
```

看起来，我们已经在 F1 结果之前得到了 F2 结果。在这里，我们使用 2 次迭代的循环来从 2 个通道中检索结果，但是如果其中一个通道从不返回结果，我们仍然会遇到阻塞问题。我们将在这篇文章的下一部分解决这个问题。现在，让我们稍微改进一下这个工具。
在现实中，我们不需要预先确定我们能从 goroutines 中得到多少结果，或者有时我们不能，我们能做的另一种方法是，只是倾听它们，直到你的所有任务完成。通过使用 WaitGroup 和一个单独的通道来指示所有任务是否完成，有一种方法可以确定所有任务何时完成。

```
func main() { var wg sync.WaitGroup
   c := make(chan struct{})
   r1 := make(chan float64)
   r2 := make(chan int)
   wg.Add(2) e, result1, result2 := func(wg *sync.WaitGroup) (e error, res1 float64, res2 int) {
      log.Print("Start") go func() {
         defer close(c)
         wg.Wait()
      }() go func(wg *sync.WaitGroup, r chan<- float64) {
         defer wg.Done()
         r <- F1()
         log.Print("Finish F1")
      }(wg, r1) go func(wg *sync.WaitGroup, r chan<- int) {
         defer wg.Done()
         r <- F2()
         log.Print("Finish F2")
      }(wg, r2) for {
         select {
         case res1 = <-r1:
            {
               log.Print("Received from F1")
            }
         case res2 = <-r2:
            {
               log.Print("Received from F2")
            }
         case <-c:
            return
         }
      } }(&wg) if e != nil {
      fmt.Println(e)
   }
   log.Printf("Got %f and %d\n", result1, result2)
}
```

通过向等待组添加 2，我们确保了工作组。Wait()将被阻塞，直到 wg 内部的内部计数器再次达到 0，每次调用 wg。Done()将使计数器减 1。请参阅信号量模式了解这一想法。
当 wg。Wait()完成了它的调用，意味着所有的任务都完成了作业，我们可以简单地关闭通道 c，这样，通道 c 本身就会产生一个事件，并表示选择语句，从而打破无限循环。

结果会和以前一样

```
2022/09/19 09:52:57 Start
2022/09/19 09:52:59 Received from F2
2022/09/19 09:52:59 Finish F2
2022/09/19 09:53:02 Received from F1
2022/09/19 09:53:02 Finish F1
2022/09/19 09:53:02 Got 80.300000 and 45
Exiting.
```

一切都好了。如果一个或两个任务都没有使用超时返回，我们将继续修复该问题。

# 添加超时

添加超时非常简单，time pkg 支持 us 函数，为此，这将返回一个通道，事件将在某个时间后发送到该通道，这里我设置为 3 秒。剩下的工作就是等待这一事件的到来。

```
func main() {
   timeout := 3 * time.**Second** var wg sync.WaitGroup
   c := make(chan struct{})
   r1 := make(chan float64)
   r2 := make(chan int)
   wg.Add(2) e, result1, result2 := func(wg *sync.WaitGroup, timeout time.Duration) (e error, res1 float64, res2 int) {
      timerT := time.After(timeout)
      log.Print("Start") go func() {
         defer close(c)
         wg.Wait()
      }() go func(wg *sync.WaitGroup, r chan<- float64) {
         defer wg.Done()
         r <- F1()
         log.Print("Finish F1")
      }(wg, r1) go func(wg *sync.WaitGroup, r chan<- int) {
         defer wg.Done()
         r <- F2()
         log.Print("Finish F2")
      }(wg, r2) for {
         select {
         case res1 = <-r1:
            {
               log.Print("Received from F1")
            }
         case res2 = <-r2:
            {
               log.Print("Received from F2")
            }
         case <-c:
            return
         case <-timerT:
            {
               e = errors.New("Got Timeout")
               return
            } }
      } }(&wg, timeout) if e != nil {
      log.Println(e)
   }
   log.Printf("Got %f and %d\n", result1, result2)
}
```

得到

```
2022/09/19 12:10:08 Start
2022/09/19 12:10:10 Received from F2
2022/09/19 12:10:10 Finish F2
2022/09/19 12:10:11 Got Timeout
2022/09/19 12:10:11 Got 0.000000 and 45
```

听起来不错，对吧？我们在 3s 后超时，只收到 F2 的结果。

# 但是呢？

这是我们从一开始就想达到的预期结果，现在已经完成了。我们在提交代码和部署后关闭票证。
还有撞！有一天你的应用程序在没有明确原因的情况下崩溃了，为什么？
我们正在泄漏资源内存，并且随着时间的推移而增加。这是一个很难解决的问题，尤其是你离开了:D 公司

现在，首先添加一些调试看看这里

```
func main() {
   ... log.Println("Number of goroutine: ", runtime.NumGoroutine())
   for {
      select {
         ...
      }
   }
   ... log.Printf("Got %f and %d\n", result1, result2)
   log.Println("Number of goroutine: ", runtime.NumGoroutine())
   time.Sleep(6 * time.**Second**)
   log.Println("Number of goroutine: ", runtime.NumGoroutine())
}
```

检查我们创建的 goroutine 的数量以及完成后它们的情况

```
2022/09/19 12:24:01 Start
2022/09/19 12:24:01 Number of goroutine:  4
2022/09/19 12:24:03 Received from F2
2022/09/19 12:24:03 Finish F2
2022/09/19 12:24:04 Got Timeout
2022/09/19 12:24:04 Got 0.000000 and 45
2022/09/19 12:24:04 Number of goroutine:  3
2022/09/19 12:24:10 Number of goroutine:  3
Exiting.
```

注意，实际上我们的应用程序不会像 main func 那样退出。
我们看到，即使在中断上面的选择循环后再等待 6 秒钟，仍有 3 个 goroutines 存在。
让我们来识别这 4 种 goroutines 是什么？它们是 Main、F1、F2 和等待
当超时到达时，当然 F1 完成并退出。所以我们还有 Main，F1，等等。忽略主 goroutine，因为我们知道它还在运行。但是 F1 怎么样，等等

```
go func() {
   defer close(c)
   wg.Wait()
}()go func(wg *sync.WaitGroup, r chan<- float64) {
   defer wg.Done()
   r <- F1()
   log.Print("Finish F1")
}(wg, r1)
```

我们可以看到等待，因为工作组。Wait()将被阻止，直到 wg。在 F1 中完成调用，但为什么即使在 6s 后，F1 仍然不调用它。
原因是 F1 试图将其结果发送到 r 通道，但没有其他人监听它，因为由于超时，我们退出了下面的 select 语句，这将被永远阻止。当超时到达时，我们仍然退出循环，如何解决这个问题？有一个非常简单的修复方法，就是让通道 r 成为缓冲通道，这样可以避免在通道资源未达到限制时阻塞写操作。

```
var wg sync.WaitGroup
c := make(chan struct{}, 1)
r1 := make(chan float64, 1)
r2 := make(chan int, 1)
wg.Add(2)
```

再次运行，我们看到 F1 可以结束，goroutine 的数量为 1 (Main)

```
2022/09/19 12:54:57 Start
2022/09/19 12:54:57 Number of goroutine:  4
2022/09/19 12:54:59 Finish F2
2022/09/19 12:54:59 Received from F2
2022/09/19 12:55:00 Got Timeout
2022/09/19 12:55:00 Got 0.000000 and 45
2022/09/19 12:55:00 Number of goroutine:  3
2022/09/19 12:55:02 Finish F1
2022/09/19 12:55:04 Number of goroutine:  1
```

完整的代码在这里:【https://goplay.tools/snippet/Sm_vBastiKI 

编码快乐！