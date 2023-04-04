# 爪哇，2021 年 3 月

> 原文：<https://blog.devgenius.io/state-of-java-march-2021-5656e2343803?source=collection_archive---------5----------------------->

![](img/82208dd53339460216802492451208e7.png)

由 [Carli Jeen](https://unsplash.com/@carlijeen?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

和往常一样，网上有很多关于 Java 是一种死亡语言的讨论。在我看来，Java 正在加速发展，最近，有这么多现有的东西正在发生，这是很长时间以来我第一次对 Java 感到兴奋。

# Java 16 + 17

Java 16 是 3 月份发布的。Java 16 不是 LTS(长期支持)版本，所以很多人会跳过它，因为他们只升级到 LTS 版本(这样做并不可耻)。

但是只有少数 jep 以 Java 17 为目标(这将是 9 月发布的 LTS 版本)。最大的一个是 [JEP 356:增强型伪随机数发生器](https://openjdk.java.net/jeps/356)。因此，16 是我们未来几年都将使用的一个非常好的预览。

# 16 场最佳

Java 16 有很多变化。你可以在这里找到最终名单[。](https://www.infoq.com/news/2021/03/java16-released/)

主要的两个是 [JEP 395:记录](https://openjdk.java.net/jeps/395)和 [JEP 394:实例](https://openjdk.java.net/jeps/394)的模式匹配。关于这两个特性已经有很长的文章了。

# 记录

# 模式匹配

# 项目织机

即将出现的 Java 的最新特性是 Project Loom。这是将轻量级并发引入 Java 的一次尝试。Loom 将引入一个名为 Fiber 的新类别。这个类的行为与 Thread 类非常相似。但是纤程的生命周期将由 Java 运行时管理，而不是由内核管理。在一个应用程序允许数百万个事务、会话等的世界里，每个用户一个线程是不可行的。纤维会是。

项目织机没有发布日期，但有几个原型建设，你可以玩。
[项目织机页面](https://wiki.openjdk.java.net/display/loom/Main)。

# 春季原生测试版

春原终于在 beta 阶段走出了 alpha 阶段。这允许您使用 GraalVM 将您的 Spring 应用程序编译成本机映像。让你的运行时间更短，更快(几乎即时启动)。
这些可以部署为独立的可执行文件(不需要 JVM)或容器映像，包含最少的操作系统层。你可以用 Spring Cloud Function，Kubernetes 等在无服务器上运行这些…

你可以在 [Spring 原生 GitHub 页面](https://github.com/spring-projects-experimental/spring-native)上找到更多。

如果你喜欢你所阅读的内容，你可以随时在[推特](https://twitter.com/pavel_polivka)上关注我以获得更多信息。

*原载于 2021 年 3 月 19 日*[*https://dev . to*](https://dev.to/pavel_polivka/state-of-java-march-2021-3cpk)*。*