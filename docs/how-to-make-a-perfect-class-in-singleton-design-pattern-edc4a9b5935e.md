# 如何在单例设计模式下创建一个完美的类

> 原文：<https://blog.devgenius.io/how-to-make-a-perfect-class-in-singleton-design-pattern-edc4a9b5935e?source=collection_archive---------1----------------------->

## 有三种方法可以破坏你的单例类，你必须知道如何防止它。

![](img/b24adacf18474afe3e394b3d09c797af.png)

在 [Unsplash](https://unsplash.com/s/photos/broken-walls?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [Kiwihug](https://unsplash.com/@kiwihug?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

如果你在一个面向对象的系统中工作，那么你必须使用 T4 设计模式中的一种，正如四人组(g of)在他们的书《设计模式——可重用面向对象软件的元素》(1994 年)中提出的。单例设计模式就是我们在应用程序中广泛使用的一种设计模式。单体设计模式属于创造性设计模式家族。

# 为什么首先要使用单例模式？

Singleton 类背后的主要思想是只创建一个对象，不允许进一步创建对象，从而控制对象创建过程。当您想要控制数据库连接、缓存、日志记录等资源时，这很方便。

例如，可以使用 Singleton 代替类的单个实例，因为日志类通常需要被项目中的每个类反复使用。如果每个类都使用这个日志类，依赖注入就变得很麻烦。

# 单一模式&打破它们的方法

使用单例模式的全部目的是创建一个对象，并防止进一步的对象创建。理想情况下，创建单例类的最简单方法是将构造函数设为私有，并创建一个公共静态方法来提供创建新实例的单一入口点。

但是，存在一些漏洞，通过这些漏洞您可以创建多个实例。如果发生这种情况，那么使用单例模式的整个目的就失败了。

有三种方法可以打破单一模式。

# 使用反射 API

反射 API 用于在运行时检查或修改方法、类和接口的行为。使用这种方法，您可以在运行时调用方法，而不考虑与它们一起使用的访问说明符。因此，使用反射 API，您可以创建额外的对象。

## 反射 API 的解决方案

有几种方法可以防止单例模式被破坏。其中一个解决方案是，如果实例已经存在，就在构造函数中抛出一个运行时异常。这将阻止您创建另一个实例。

另一个最佳解决方案是简单地使用枚举，因为 java 确保枚举值只被实例化一次。

# 使用可克隆的接口

如果 Singleton 类实现了 Cloneable 接口，那么 *clone()* 也会被覆盖。使用这个 *clone()，*您可以在 Singleton 类中创建对象的副本(或克隆)，这将打破 Singleton 模式。

## 可克隆界面的解决方案

为了防止使用 Cloneable 接口破坏单例模式，您需要覆盖 clone 方法并返回相同的实例，这将确保克隆的对象与原始对象相同。

# 使用反序列化

使用序列化，可以将字节流的对象保存到文件中。因此，当反序列化同一个类时，将创建该类的一个新实例，然后将其实例变量设置为已序列化的值。所以，如果类是单例的，那么使用反序列化，可以创建新的对象。

## 反序列化的解决方案

您可以通过覆盖 *readResolve()* 并返回同一个实例来防止破坏单例模式。

这是打破单例模式的三种方式，以及如何防止它的解决方案。单一模式是所有设计模式中最容易实现的，但是它也有自己的优缺点。尽管如此，Singleton 仍然是采访中讨论最多的设计模式之一。

如果你喜欢读这篇文章，你可能也会发现下面的文章值得你花时间去读。

[](https://python.plainenglish.io/design-patterns-that-every-software-developer-must-know-ac71f575e68) [## 每个软件开发人员都必须知道的设计模式

### 这些久经考验的解决方案提高了编程效率。

python .平原英语. io](https://python.plainenglish.io/design-patterns-that-every-software-developer-must-know-ac71f575e68) [](/features-that-every-developer-must-know-about-spring-boot-c1c0d7f1c0a8) [## 每个开发人员都必须知道的关于 Spring Boot 的特性

### 该框架通常被称为“类固醇上的弹簧”，的确如此。

blog.devgenius.io](/features-that-every-developer-must-know-about-spring-boot-c1c0d7f1c0a8)