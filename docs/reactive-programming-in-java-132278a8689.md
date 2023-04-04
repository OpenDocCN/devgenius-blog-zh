# Java 中的反应式编程

> 原文：<https://blog.devgenius.io/reactive-programming-in-java-132278a8689?source=collection_archive---------2----------------------->

**期货 vs CompletableFutures 期货 vs rx Java vs react Java 流 9…**

让我们深入探讨如何在同步请求处理期间执行“你的部分代码**异步**”

![](img/d9ca01ae092eaf21edaab52f29f368ec.png)

对于许多用例，当您必须执行一些计算密集型工作，或者进行网络调用或执行一些并行处理时，我们倾向于使用多线程。这里产生一些临时线程来处理较小的任务，并合并回主请求线程后处理。

一旦多线程出现，下一步就是如何处理并行处理过程中产生的数据。

Java 支持多种结构。我们将简要介绍一下

# 期货

Java5 (2004)中引入了期货。它们基本上是尚未完成的可执行操作结果的占位符。一旦操作完成，`Future`将包含该结果。

例如，操作可以是提交给 [ExecutorService](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ExecutorService.html) 的[可运行](https://docs.oracle.com/javase/8/docs/api/java/lang/Runnable.html)或[可调用](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Callable.html)实例。操作的提交者可以使用`Future`对象来检查操作[是否是 Done()](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Future.html#isDone--) ，或者等待它完成使用**blocking**[get()](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Future.html#get-long-java.util.concurrent.TimeUnit-)方法。

样本:

```
public static class MyAsyncExecutor implements Callable<Integer> {
    @Override
    public Integer call() throws Exception {
        Thread.sleep(1000);
        return 1;
    } 
}public static void main(String[] args) throws Exception{
    ExecutorService exec = Executors.newSingleThreadExecutor();
    Future<Integer> future = exec.submit(new MyAsyncExecutor()); System.out.println(future.isDone()); // False - a non blocking call, prints false, if processing is not finished; and moves on to next statement. System.out.println(future.get());    // Blocking call - waits until the task is done, then prints 1
}
```

# CompletableFutures

[CompletableFutures](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html) 在 Java 8 (2014)中引入。

事实上，它们是常规期货的进化，受谷歌的可列表期货的启发，是 T2 番石榴图书馆的一部分。它们是未来，也允许你把相关的任务串在一起。你可以用它们来告诉某个工作线程“去做某个任务 X，当你完成后，去做任务 Y，也就是依赖于 X 的结果”。使用 CompletableFutures，您可以对上一个操作的结果做一些事情，而不用实际阻塞一个线程来等待结果。

让我们看一个例子来理解这一点:

```
public static class MyAsyncExecutor implements Supplier<Integer> {
    @Override
    public Integer get() {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            //Suppress: do nothing
        }
        return 1;
    }
}/**
 * Simple function: adds 1 to result
 */
public static class PlusOne implements Function<Integer, Integer> {
    @Override
    public Integer apply(Integer x) {
        return x + 1;
    }
}public static void main(String[] args) throws Exception {
    ExecutorService exec = Executors.newSingleThreadExecutor();
    CompletableFuture<Integer> f = CompletableFuture.supplyAsync(new MyAsyncExecutor(), exec); System.out.println(f.isDone()); // False - a non blocking call, prints false, if processing is not finished; and moves on to next statement. CompletableFuture<Integer> f2 = f.thenApply(new PlusOne()); // Chaining another operation    System.out.println(f2.get()); // Blocking call - waits until the all chained operations are finished, then prints 2
}
```

# **RxJava**

[RxJava](https://github.com/ReactiveX/RxJava) 是网飞创建的[反应式编程](https://en.wikipedia.org/wiki/Reactive_programming)的库。乍一看，它似乎与 Java 8 的流相似。它确实是，除了更强大。

与期货类似，RxJava 可以用来链接一组同步或异步函数，以创建一个处理管道。

与单一操作的期货不同，RxJava 处理一个或多个项目的*流*，包括具有无限数量项目的永无止境的连续流。得益于难以置信的丰富的操作符集，它也更加灵活和强大。

Java 8 的流和 RxJava 的主要区别在于，Rx 有[背压](http://reactivex.io/documentation/operators/backpressure.html)支持，这允许它处理这样的情况，其中你的处理管道的不同部分在不同的线程、*中以不同的速率*操作*。为了使其可配置和更容易使用，库中添加了许多排列。RxJava 的主要缺点是，尽管有坚实的文档，但由于传统 Java8 涉及的范式转变，它是一个学习的**挑战库。***

由于这种复杂性，Rx 代码可能是调试的噩梦。尤其是在有多个线程的情况下，如果还涉及到背压，情况会变得更糟！

有很多关于 Rx 使用模式的教程。官方的[文档](http://reactivex.io/documentation/observable.html)和 [Javadoc](http://reactivex.io/RxJava/javadoc/) 也相当丰富。你也可以找到一些可靠的视频，比如来自网飞的[这个](https://www.youtube.com/watch?v=_t06LRX0DV0)，它给出了 Rx 的 101 以及如何开始。还简单谈了一下 Rx 和 Futures 的区别。

# **反应流**

[反应流](https://community.oracle.com/docs/DOC-1006738)是在 Java 9 (2017)中引入的

反应流，又名[流 API](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Flow.html) 是由各种[反应流](http://www.reactive-streams.org/)库实现的一组标准接口，如 [RxJava 2](https://github.com/ReactiveX/RxJava/wiki/Reactive-Streams) 、 [Akka 流](http://doc.akka.io/docs/akka-stream-and-http-experimental/1.0-M2/stream-design.html)和 [Vertx](http://vertx.io/) 。公共接口有助于标准化约定，并允许这些反应库互连，同时保留所有的核心。

当谈到 Java 中的并发处理时，经典的[Java Concurrency in Practice(jcip.net)](https://jcip.net/)总是我的首选参考。

快乐编码…

# Java #并发#多线程#Future #CompletableFuture #Rx