# Spring Boot 调度程序

> 原文：<https://blog.devgenius.io/spring-boot-scheduler-cd0e62075d9e?source=collection_archive---------6----------------------->

![](img/c4d4e765b1d8dc9a5bcda3bb5cfb9df0.png)

照片由[阿伦视觉](https://unsplash.com/@aronvisuals?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Spring Boot 调度器用于在未来重复或单次执行给定的任务。

*   为了调度任务`@Schedule`,在方法上使用了注释。
*   Spring boot 在内部使用`TaskScheduler`接口来执行带有`@Schedule`注释的方法。

## 调度程序入门

(1)启用调度程序

在您的应用程序类或配置类上添加`@EnableScheduling`注释。

启用调度程序

(2)创建调度任务的方法

1.  **固定费率调度器**

让我们创建一个每 5 秒运行一次并打印当前时间的调度程序。

输出

在固定速率调度的情况下，任务将在给定的时间执行。它不会等待前一个任务完成。

默认情况下，这些任务不会并行执行。所以如果你想并行执行它，你可以在方法上用`@Async`注释方法，在类上用`@EnableAsync`注释方法。

**2。具有固定延迟的调度器**

在固定延迟调度程序中，第二个任务仅在第一个任务完成后执行。两个任务之间的间隙将是`fixedDelay`持续时间。

这里任务执行需要 1 秒，两个任务之间的延迟是 3 秒。因此，每隔 4 秒就会执行下一个任务。

**4。初始延迟**

当您想要指定第一个任务执行的延迟时，您可以使用`intialDelay`属性来定义它。

这个属性可以和`fixedDelay`和`fixedRate`一起使用。

**5。用 Cron 表情**

您还可以使用 cron 表达式(* * * * *)来调度任务。在开始调度任务之前，让我们先理解 cron 表达式。

所以`* * * * *`是指`every month`的`every day`的`every hour`的`every minute`和`week`的`every day`。

示例:

*   `0 * * * *` —这意味着当分钟数为`0`(每小时一次)时，cron 将一直运行
*   `0 1 * * *` —这意味着 cron 将总是在 1 点钟运行。
*   `* 1 * * *` —这意味着当小时为 1 时，cron 将每分钟运行一次。所以`1:00`，`1:01`，...`1:59`。
*   `0 * * * * *` —这意味着 cron 将在每分钟运行一次。

在这里，我们创建了 cron 调度程序，它将在秒为 00 时执行，这意味着它将在每分钟执行一次。

**自定义配置**

默认情况下，spring boot 使用默认的一个线程池大小运行调度程序任务。如果您想更改配置，那么您可以使用`SchedulingConfigurer`界面。