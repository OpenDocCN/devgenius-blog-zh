# 让我们来谈谈月亮和单例设计模式。

> 原文：<https://blog.devgenius.io/lets-talk-about-the-moon-and-the-singleton-design-pattern-a73ad248b804?source=collection_archive---------2----------------------->

## 世界上没有一种面向对象的编程语言对许多对象(任何类)的创建加以限制。

但有时如果程序中存在任何类的多个实例(对象),这可能是不可行的。例如，假设编写了一个与硬件通信的类。现在想象一下，如果该类的多个实例正在运行，告诉硬件做某件事情，而另一件事情正在执行，会发生什么。对象 1 告诉执行任务 1，同时对象 2 与任务 2 一起到来。

你能想象那会有多混乱吗？这种情况下的解决方案是单例设计模式。它只允许任何类中存在一个对象。只有一个对象将用于与硬件通信。或者在另一种情况下，只使用一个对象来跟踪登录活动(日志记录)。

![](img/8fb454858f8cdb591eeb2a3416b005a6.png)

[摄影:兰蒂](https://unsplash.com/@photos_by_lanty?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上摄影

[如果地球有多个卫星(天然卫星)](https://www.businessinsider.in/video/a-city-in-china-wants-to-launch-an-artificial-moon-into-orbit-by-2020-heres-what-would-happen-if-earth-really-did-have-two-moons/articleshow/67474908.cms#:~:text=The%20combined%20pull%20of%20the,surface%20triggering%20tremendous%20volcanic%20activity.)会发生什么？这会导致大量的火山活动。嗯，这对人类来说是不可行的。因此，让我们编写一个 java 代码，这样在程序的任何一点都不会创建类 **NaturalSatellite** 的多个实例，并展示 singleton 设计模式的实际应用。

![](img/7404e5590ec4617ac8b704b072f8435b.png)

第一步是使构造函数成为类(NaturalSatellite)的私有函数，我们在这个类上应用了单例设计模式。因此，我们不能继续创建该类的对象，无论我们想要多少。

其次，我们写一个工厂方法(*static natural satellite get satellite()*)。这个方法返回给我们一个 NaturalSatellite 的实例。

第三，我们编写一个私有静态变量，该变量将在静态方法 getSatellite()中使用，以检查 NaturalSatellite 的任何实例是否已经存在，如果它确实返回现有实例，如果 NaturalSatellite 的任何实例不存在，则创建一个新实例。使用这种方法，我们无法创建 NaturalSatellite 的多个实例