# 在 Go 中收听多个频道

> 原文：<https://blog.devgenius.io/listening-to-multiple-channels-in-go-11a1c6cd3a21?source=collection_archive---------0----------------------->

欢迎回到这个系列！今天我们来看看同时收听多个频道的方法。之前的指南帮助您开始使用 Go 中的并发性。尽管简单的方法通常是最好的方法，但是您可能一直在尝试实现更复杂的行为。阅读本指南后，您将能够使您的并发代码更加灵活。

# 选择关键字

我们可以使用`select`关键字同时收听多个 goroutines。

```
package mainimport (
    "fmt"
    "time"
)func main() {
    c1 := make(chan string)
    c2 := make(chan string) go func() {
        time.Sleep(1 * time.Second)
        c1 <- time.Now().String()
    }() go func() {
        time.Sleep(2 * time.Second)
        c2 <- time.Now().String()
    }() for i := 0; i < 2; i++ {
        select {
        case res1 := <-c1:
            fmt.Println("from c1:", res1)
        case res2 := <-c2:
            fmt.Println("from c2:", res2)
        }
    }
}from c1: 2022-09-04 14:30:39.4469184 -0400 EDT m=+1.000172801
from c2: 2022-09-04 14:30:40.4472699 -0400 EDT m=+2.000524401
```

上面的代码展示了`select`关键字是如何工作的。

*   我们首先创建两个频道`c1`和`c2`来听。
*   然后我们生成两个 goroutines，每个都将当前时间发送给`c1`和`c2`。
*   在 for 循环中，我们创建了一个`select`语句并定义了两种情况:第一种情况是当我们可以从`c1`接收时，第二种情况是当我们可以从`c2`接收时。

您可以看到`select`语句在设计上与`switch`语句非常相似。两者都定义了不同的情况，并在遇到特定情况时运行各自的代码。另外，我们可以看到`select`语句正在阻塞。也就是说，它会一直等到满足其中一种情况。

我们对循环迭代两次，因为只有两个 goroutines 要听。更确切地说，每个 goroutine 都是一个“一劳永逸”的 goroutine，这意味着它们在返回之前只向一个频道发送一次。因此，这段代码中始终最多有两条消息，我们只需要选择两次。

# 如果我们不知道工作什么时候会结束呢？

有时候我们不知道有多少工作。在这种情况下，将`select`语句放在 while 循环中。

```
package mainimport (
    "fmt"
    "math/rand"
    "time"
)func main() {
    c1 := make(chan string)
    rand.Seed(time.Now().UnixNano()) for i := 0; i < rand.Intn(10); i++ {
        go func() {
            time.Sleep(1 * time.Second)
            c1 <- time.Now().String()
        }()
    } for {
        select {
        case res1 := <-c1:
            fmt.Println("from c1:", res1)
        }
    }
}
```

因为我们让随机数量的 goroutines 运行，所以我们不知道有多少个作业。幸运的是，底部封装 select 语句的 for 循环将捕获每个输出。让我们看看运行这段代码会发生什么。

```
from c1: 2022-09-04 14:48:47.5145341 -0400 EDT m=+1.000257801
from c1: 2022-09-04 14:48:47.5146126 -0400 EDT m=+1.000336201
from c1: 2022-09-04 14:48:47.5146364 -0400 EDT m=+1.000359901
fatal error: all goroutines are asleep - deadlock!goroutine 1 [chan receive]:
main.main()
        /home/jacob/blog/testing/listening-to-multiple-channels-in-go/main.go:22 +0x128
exit status 2
```

嗯，select 语句按预期收到了三次，但是程序由于死锁而出错。为什么会这样呢？

请记住，在没有任何条件的情况下，Go 中的 for 循环将永远运行。这意味着 select 语句将永远尝试接收。但是，要运行的作业数量有限。即使没有更多的作业，select 语句仍将尝试接收。

还记得在本系列的第一篇文章中，我说过如果你在发送者没有准备好的时候试图从一个无缓冲的通道接收，你的程序将会陷入死锁吗？这正是我们例子中的情况。

那么我们如何解决这个问题呢？我们可以结合使用前几篇文章中提到的概念:出口通道和等待组。

```
package mainimport (
    "fmt"
    "math/rand"
    "sync"
    "time"
)func main() {
    c1 := make(chan string)
    exit := make(chan struct{})
    rand.Seed(time.Now().UnixNano())
    var wg sync.WaitGroup go func() {
        numJob := rand.Intn(10)
        fmt.Println("number of jobs:", numJob)
        for i := 0; i < numJob; i++ {
            wg.Add(1)
            go func() {
                defer wg.Done()
                time.Sleep(1 * time.Second)
                c1 <- time.Now().String()
            }()
        }
        wg.Wait()
        close(exit)
    }() for {
        select {
        case res1 := <-c1:
            fmt.Println("from c1:", res1)
        case <-exit:
            return
        }
    }
}3
from c1: 2022-09-04 15:09:08.6936976 -0400 EDT m=+1.000287801
from c1: 2022-09-04 15:09:08.6937788 -0400 EDT m=+1.000369101
from c1: 2022-09-04 15:09:08.6937949 -0400 EDT m=+1.000385101
```

*   我们创建一个退出通道和一个等待组。
*   每次运行的作业数量是随机的。很多次，我们发射 goroutines。为了等待作业完成，我们将它们添加到`wg`。当一项工作完成时，我们从`wg`中减去一。
*   一旦所有工作完成，我们关闭出口通道。
*   我们将上述部分包装在一个 goroutine 中，因为我们希望所有部分都是非阻塞的。如果我们不把它包在 goroutine 中，`wg.Wait()`会等到任务完成。这会阻塞代码，不让底层的 for-select 语句运行。
*   此外，因为`c1`是一个无缓冲通道，等待所有 goroutines 将消息发送到`c1`将导致许多消息被发送到`c1`，而没有 for-select 语句来接收它们。这会导致死锁，因为发送方准备好了，而接收方却没有准备好。

# 如何使选择无阻塞

默认情况下,`select`语句是阻塞的。我们如何使它不阻塞？很简单——我们只需添加一个默认案例。

```
package mainimport (
    "fmt"
    "math/rand"
    "sync"
    "time"
)func main() {
    ashleyMsg := make(chan string)
    brianMsg := make(chan string)
    exit := make(chan struct{})
    rand.Seed(time.Now().UnixNano())
    var wg sync.WaitGroup go func() {
        numJob := rand.Intn(10)
        fmt.Println("number of jobs:", numJob)
        for i := 0; i < numJob; i++ {
            wg.Add(2)
            go func() {
                defer wg.Done()
                time.Sleep(time.Duration(rand.Intn(10)) * time.Millisecond)
                ashleyMsg <- "hi"
            }()
            go func() {
                defer wg.Done()
                time.Sleep(time.Duration(rand.Intn(10)) * time.Millisecond)
                brianMsg <- "what's up"
            }()
        }
        wg.Wait()
        close(exit)
    }() for {
        select {
        case res1 := <-ashleyMsg:
            fmt.Println("ashley:", res1)
        case res2 := <-brianMsg:
            fmt.Println("brian:", res2)
        case <-exit:
            fmt.Println("chat ended")
            return
        default:
            fmt.Println("...")
            time.Sleep(time.Millisecond)
        }
    }
}...
number of jobs: 4
brian: what's up
...
ashley: hi
...
...
brian: what's up
ashley: hi
ashley: hi
brian: what's up
...
...
ashley: hi
...
brian: what's up
...
chat ended
```

抛开蹩脚的对话，我们可以看看违约案例是如何运作的。我们可以在没有接收聊天信息的渠道时做一些事情，而不是等待聊天信息的到来。在这个例子中，我们只是打印出了椭圆，但是你可以做任何你想做的事情。

# 结论

这个帖子到此为止！现在，您可以同时收听多个频道，这在您开发个人项目时是一笔巨大的财富。感谢阅读，我们下次再见。

你也可以在 [Dev.to](https://dev.to/jpoly1219/listening-to-multiple-channels-in-go-152e) 和[我的个人网站](https://jpoly1219.github.io)上阅读这篇文章。