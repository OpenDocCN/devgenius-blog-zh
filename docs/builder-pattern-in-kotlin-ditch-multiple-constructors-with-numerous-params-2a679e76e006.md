# Kotlin 中的构建器模式:丢弃带有大量参数的多个构造器

> 原文：<https://blog.devgenius.io/builder-pattern-in-kotlin-ditch-multiple-constructors-with-numerous-params-2a679e76e006?source=collection_archive---------4----------------------->

![](img/2e64d55579d055ae178d7ec7cef51baa.png)

Kotlin 默认参数使我们从多个构造函数重载中解脱出来，但是仍然有一个带有许多可选参数(尤其是同一类型)的构造函数会降低代码的可读性和可用性。我们可以做得更好:)

*构建器模式*正是可以提供良好的可读性和使用多个任意参数或**嵌套对象作为参数**来创建对象的**选项。**

构造对象的过程是通用的，因此它可用于创建同一对象的不同表示。这种类型的设计模式属于创建模式，因为它提供了一种创建对象的方法。现在让我们深入研究一下实现。

> 在我的 Github 上查看更多 Kotlin 中的设计模式—[https://Github . com/AnastasiaRainMaker/kot Lin-Design-Pattern-Examples/tree/master/app/src/main/Java/com/heb/Design _ Pattern _ practice](https://github.com/AnastasiaRainMaker/Kotlin-Design-Pattern-Examples/tree/master/app/src/main/java/com/heb/design_pattern_practice)

**游戏计划**

让我们从一个简短的游戏计划开始。第一步是创建一个 POJO (Plain old Java object)类，其中包含一个 Builder 类实现。对象类本身将具有分配了默认值的私有属性字段。生成器实现并不要求使用默认参数，这只是在代码中提供了空安全。POJO 类将接受一个构建器实例作为构造函数参数。

在下面的例子中，您将很快看到 *Vacation* 实例在创建时接受了 *VacationBuilder* 类实例。

对于构建器类本身，它还将包含与 POJO 字段的匹配，以便以后在创建时可以将它们分配给 POJO 对象。

因此，让我们来看看示例代码。

正如您在示例中所看到的， *Vacation* 是我们上面讨论的 POJO 类，而 *VacationBuilder* 是 Builder 模式的实现。所有将被链接的方法都返回构建器类型— *VacationBuilder。*和方法 *build()* 返回我们的 POJO 对象— *休假*。

**一句警告的话**

我见过其他使用 Kotlin 的 *apply { }* 的实现，我想请你在这种情况下不要这样做。因为 *apply {}* 和其他更高阶的函数在幕后创建了一个匿名的内部类——你应该尽量少用它们。所以在链接调用中返回它自己( *this* )将允许我们拥有这个漂亮的链接流，而没有 *apply {}* 可能导致的开销。

让我们把它插上

实现了 POJO 和构建器之后，我们就可以开始使用它了。就像下面我们得到我们的*假期*对象创建的*。*我们通过 *Vacation* 类获得对 *VacationBuilder* 的引用。在创建它的一个实例之后，我们可以链接所有必要的调用来设置所需的参数。最后“构建”我们的对象。

瞧，我们可以通过构建器模式用漂亮的表达性代码构建复杂的对象。这种模式是基本的创造模式之一，我相信每个人都应该理解它，并在需要时利用它提供的简单性。许多主流库使用它来创建对象——翻新、Android sdk(即 StringBuilder)等。