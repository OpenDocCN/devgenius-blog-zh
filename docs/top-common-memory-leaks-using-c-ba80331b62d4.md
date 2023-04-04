# 使用 C#时常见的内存泄漏

> 原文：<https://blog.devgenius.io/top-common-memory-leaks-using-c-ba80331b62d4?source=collection_archive---------1----------------------->

![](img/e3f5fbb1e9c4268e4d60360b89a79b10.png)

尽管有 GC 的存在，但还是非常容易造成内存泄漏。并不是垃圾收集器工作不好，只是在托管语言中导致内存泄漏的方式太多了。

在本文中，我们将讨论内存泄漏的最常见原因。NET 程序。所有的例子都是用 C#写的。

我们从这里开始:

# 1.losure

当匿名方法通过闭包捕获一个类的成员时，就会发生泄漏。

示例:

在这段代码中，`_id`字段在匿名方法中被捕获，并且实例也被引用。这意味着当`Worker`存在并在 Do 方法中引用作业委托时，它也将引用`Closure`的一个实例。

解决方法很简单——使用局部变量代替类成员:

通过使用局部变量的值，不会捕获任何内容，并且可以防止潜在的内存泄漏。

# 2.事件

。NET 事件会导致内存泄漏。当您订阅事件时，事件触发对象包含对您的类的引用。如果订阅是通过匿名方法进行的，则不会发生这种情况。
举例:

如果`eventManager`对象比`EventLeak` 类存活的时间长，那么就存在内存泄漏。`EventLeak` 的任何实例都被`eventManager` 引用，不会被 GC 收集。

如何避免:

1.  取消订阅该活动。
2.  使用弱处理程序模式。
3.  使用匿名函数订阅，不捕获任何成员。

# 3.静态

静态对象位于 GC 根目录，永远不会被收集器收集。这种行为会导致内存泄漏。静态对象和它们引用的任何东西都不会被垃圾回收。

示例:

任何`Static`的实例将永远留在内存中，导致内存泄漏。

# 4.隐藏物

如果你缓存所有的东西，你最终会耗尽内存。

示例:

这段代码可以保存你的数据库，但是会占用你所有的可用内存。

解决这个问题的一些方法:

1.  删除一段时间没有使用的缓存
2.  限制缓存大小
3.  使用`WeakReference`来保存缓存的对象。( [WekReference](https://docs.microsoft.com/en-us/dotnet/api/system.weakreference?view=net-5.0) )

# 5.线

如果你创建了一个无限运行的线程，它什么也不做，却引用了对象，这就是内存泄漏。这很容易发生的一个例子是使用`Timer`。

示例:

如果你不停止定时器，它将在一个单独的线程中运行，引用一个`Threads`的实例，阻止它被收集。

# 6.IDisposable

IDisposable 模式的不正确使用会导致内存泄漏。
如何正确调用 Dispose:

你可以做的一件事是在 C#中使用`using`语句:

这适用于`IDisposable`类，由编译器翻译成:

这非常有用，因为即使抛出了异常，`Dispose`仍然会被调用。

您可以做的另一件事是利用[处置模式](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-dispose)。这里有一个如何实现它的例子:

这种模式确保了即使没有调用`Dispose`，当实例被垃圾收集时，它最终也会被调用。另一方面，如果`Dispose` *被*调用，那么终结器被取消。抑制终结器很重要，因为终结器[开销很大](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/destructors)并且会导致性能问题。

# 7.WPF 绑定

WPF 绑定会导致内存泄漏。经验法则是总是绑定到一个`DependencyObject`或一个`INotifyPropertyChanged`对象。当你不这么做的时候，WPF 会从一个静态变量创建一个对你的绑定源(ViewModel)的强引用，导致内存泄漏([解释](https://stackoverflow.com/a/18543350/1229063))。

这里有一个例子:

```
<UserControl x:Class="WpfApp.MyControl"      ae mq" href="http://schemas.microsoft.com/winfx/2006/xaml/presentation" rel="noopener ugc nofollow" target="_blank">http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="[http://schemas.microsoft.com/winfx/2006/xaml](http://schemas.microsoft.com/winfx/2006/xaml)">
    <TextBlock Text="{Binding Text}"></TextBlock>
</UserControl>
```

这个视图模型将永远留在内存中:

然而这种视图模型不会导致内存泄漏:

是否调用`PropertyChanged`并不重要，重要的是这个类是从`INotifyPropertyChanged`派生出来的。这告诉 WPF 基础设施不要创建一个强有力的参考。

当绑定模式为单向或双向时，会发生内存泄漏。如果绑定是一次性的或单向的，那就不是问题。

当您绑定到集合时，会出现另一个 WPF 内存泄漏问题。如果这个集合没有实现`INotifyCollectionChanged`，那么就会出现内存泄漏。您可以通过使用实现该接口的`ObservableCollection`来避免这个问题。

# 8.非托管内存

非托管内存是不由垃圾回收器管理的内存。你需要自己释放内存。

示例:

在上面的方法中，我们使用了`Marshal.AllocHGlobal`，它分配一个非托管内存的缓冲区([文档](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.marshal.allochglobal?redirectedfrom=MSDN&view=netframework-4.7.2#System_Runtime_InteropServices_Marshal_AllocHGlobal_System_Int32_))。如果不使用`Marshal.FreeHGlobal`手动释放句柄，缓冲内存将被进程的内存堆占用，从而导致内存泄漏。

为了处理这样的问题，您可以添加一个`Dispose`方法来释放任何非托管资源，如下所示:

非托管内存泄漏会导致[内存碎片](https://stackoverflow.com/questions/3770457/what-is-memory-fragmentation)。

现在您知道了如何避免最常见的内存泄漏问题。

感谢阅读！