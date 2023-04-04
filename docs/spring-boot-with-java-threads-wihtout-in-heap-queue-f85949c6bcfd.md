# 堆队列中没有 Spring Boot 线程

> 原文：<https://blog.devgenius.io/spring-boot-with-java-threads-wihtout-in-heap-queue-f85949c6bcfd?source=collection_archive---------12----------------------->

![](img/b067a2baa756f9694f82976b14ebb411.png)

在我早期的开发生涯中，我从来不需要任何线程，主要是我的开发项目是小而快速的小过程，不需要任何线程操作。

但是进了大公司之后，我最需要！

首先，我开始用小线程，你知道像做两个工作在同一时间的事情。像这样的线程使用；

```
new Thread(() -> {
    //*TODO* }).start();
```

这是最简单的用例。打开一个新线程，发送一些 todo 并启动它。它走自己的路，你走自己的路。

但是如果你想做一项复杂的工作，那就有点困难了。

假设您有一个包含 100k 个对象的列表，需要尽可能快地处理和完成。在这种情况下，我们需要更有效地使用线程。

第一种方法是:

```
Executor executor = Executors.*newFixedThreadPool*(10);
executor.execute(() -> {
    //*TODO* });

while (true){
    if(...) ...
}
```

创建一个包含 10 个线程的线程池，同时处理 10 个数据。(在这种情况下，当您发送所有 100k 个对象时，它存储在一个无限大小的队列中，等待线程获取数据并处理它。这可能会导致堆大小增加。当你使用这种方法时，你需要小心。)

发送完所有的对象列表后，你可以等待并检查是否所有的对象都被处理并完成*(通过并发 hashmap，你可以存储对象状态并在 while 循环中检查它们是否完成)*。如果不需要等待和检查，可以忽略 while 循环。

如果我的对象很小，我的最大列表数是 100k，有时我会使用这种方法。但是有更有效的方法可以做到这一点。因为在这种情况下，您无法控制队列大小，这会导致堆问题。

还有第二种方法，我主要在我们的大数据项目中使用这种方法。

![](img/f35c3185df81d8f8df7d25bc921d4c25.png)

有了 Spring Boot，我们的工作变得更加容易，我喜欢和豆子一起工作。

首先，您需要创建一个线程池 bean

```
@Bean
public ThreadPoolTaskExecutor taskExecutor() {
    ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
    executor.setCorePoolSize(50);
    executor.setMaxPoolSize(50);
    executor.setQueueCapacity(10);
    executor.setThreadNamePrefix("Thread-");
    executor.initialize();
    return executor;
}
```

我建议不要使用超过*你的服务器的核心大小，如果你想你可以增加一倍，但要小心 cpu 的使用。*

如您所见，我的线程池容量是 10，仅仅是 10。所以在你发送了 60 个对象之后*(如果线程没有完成)*我的队列将会满了，它不能再处理更多的对象了。

如果你试图发送它，它会得到一个异常，你的数据将会丢失。我们如何防止这种情况发生？

在我们的项目中，我们使用 RabbitMQ *(它可以是任何消息队列，但 rabbit 是我最喜欢使用的)*来发送和接收数百万的数据。所以在我的兔子处理器方法中，我使用了这个。

```
while(true) {
    try {
        taskExecutor.submit(() -> {
            //TODO
        });
        break;
    } catch (TaskRejectedException e) {
        try {
            Thread.*sleep*(50);
        } catch (InterruptedException ex) {
            ex.printStackTrace();
        }
    }
}
```

在这个部分中，我的数据还没有提交给线程，如果线程正在工作，线程池已满，就会出现异常。

为了防止你的数据丢失并等待线程处理，它处于一个无限的 *(kinda)* while 循环*(最后有一点 50 毫秒的等待时间，以防止你的应用程序每毫秒试图发送数据，并防止太多的 cpu 使用，通常处理时间可以约 150 毫秒。在这种情况下，我们的 while 循环 3 次)*。当我的 one 数据在循环中等待时，我的其他 99k 对象在兔子队列中等待被应用程序使用。

这样，我们的应用程序堆将保持稳定，不会因为消耗线程池队列中的所有对象而随时间增加。