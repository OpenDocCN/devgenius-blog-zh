# 科特林代表团简介

> 原文：<https://blog.devgenius.io/a-gentle-introduction-to-delegation-in-kotlin-a1c4f19ec589?source=collection_archive---------2----------------------->

![](img/078bbe3ac2649784a226c3934ba05956.png)

[Saj Shafique](https://unsplash.com/@saj_shafique?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/forward?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

在面向对象编程中，委托是一种面向对象的设计模式，是继承的替代方案。在委托中，一个对象通过将请求委托给另一个对象来处理请求。另一个做实际工作的对象叫做委托。

由于委托是一种设计模式，这意味着我们可以用任何面向对象的语言来实现它。一些面向对象的语言如 Kotlin 本身就支持它，这意味着没有样板代码。

# 一个例子

让我们通过一个简单的例子来了解委托的基本概念。首先，我们定义了`Software`接口。

然后，我们为刚刚定义的接口定义两个实现。

现在我们定义一个名为`WordProcessor`的类，它实现了`Software`接口。

`WordProcessor`接收一个`Software`的实例，当它的`getLicense`被调用时，它简单地将调用委托/转发给`software`。

就是这样！现在你知道什么是委托了，但是还有更多。

# 班级委派

你刚才看到的例子是一个类委托的例子。在类委托中，类`A`将其部分或全部职责委托给类`B`。

因为 Kotlin 本身支持类委托，所以让我们以一种更优雅的方式实现前面的例子。

上面的`WordProcessor`类具有与我们之前定义的完全相同的功能，但是没有任何样板代码。Kotlin 编译器将为我们生成样板文件！

我们使用`by`关键字让 Kotlin 编译器做两件事:

1.  为`WordProcessor`生成`Software`接口的所有方法。
2.  将生成的方法的调用委托给`software`对应的方法。

将`WordProcessor`的字节码反编译成 Java，给出如下代码。

Kotlin 编译器已经完成了它的工作！

现在该谈谈另一种类型的委托了，这种委托被称为财产委托。

# 财产委托

在讨论财产委托之前，让我们看看为什么我们需要它。

## 一个例子

考虑下面的类。

假设我们想要为`User`的每个属性实现以下逻辑:

*   打印"<property name="">是读取"属性的值时，</property>
*   打印<property name="">是在给属性赋值时写的。</property>

所以当运行下面的代码时:

我们希望得到下面的输出:

```
firstName is written
secondName is written
firstName is read
secondName is read
```

一种简单的方法是在读/写`User`的任何属性时手动添加`println`语句。

上面的代码可以工作，但是你猜怎么着。它根本不可扩展，我们在重复同样的逻辑。如果我们可以一次定义逻辑并使用它，而不是一遍又一遍地重复相同的逻辑，那将是理想的。这就是财产代表可以帮忙的地方。

## 定义属性委托

属性委托只不过是一个具有非常特殊的`getValue`和`setValue`方法的类。

让我们定义一个委托，它实现了我们前面手动实现的相同逻辑。

注意，为了让下面的代码工作，我们需要将下面的依赖项添加到我们的`build.gradle`文件中(当然，假设我们使用的是 Gradle！)

```
*implementation*("org.jetbrains.kotlin:kotlin-reflect:<insert version here>")
```

当我们读取属性的值时，就会执行`getValue`方法。它是一个运算符方法，必须有以下确切的参数:

*   `thisRef`必须是`Any?`类型。它是包含委托属性的类的实例。如果委托属性没有在类中定义(如在`main`函数中定义)`thisRef`将为空。
*   `property`必须是类型`KProperty<*>`或其子类型。是房产本身。

`getValue`的返回类型必须与属性(或其子类型)的类型相同。

`setValue`是我们给属性赋值时执行的方法。它是一个运算符方法，必须有以下参数。

*   `thisRef`与上述目的完全相同。
*   `property`与上述用途完全相同。
*   `value`是分配给属性的值。

现在我们可以使用刚刚定义的委托了。

运行上述代码会生成以下输出。

```
firstName is written
secondName is written
firstName is read
secondName is read
```

这里，我们再次使用关键字`by`进行属性委托。`by`关键字告诉 Kotlin 编译器两件事:

1.  当访问比方说`firstName`的值时，不调用默认的`firstName`的 getter，而是调用`Delegate.getValue`。
2.  赋值给 say `firstName`时，不调用`firstName`的默认 setter，而是调用`Delegate.setValue`。

如果我们将`User`的字节码反编译成 Java，我们将得到下面的代码，它证实了这样一个事实，即当设置/获取`firstName` / `lastName`的值时，是`Delegate`处理这项工作。

请注意，我们刚刚实现的委托是一个非常简单且不太有用的委托。发挥你的想象力，想想我们可以通过财产委托实现的事情。

就是这个！现在你对 Kotlin 的委托有了基本的了解。

# 参考

[https://kotlinlang.org/docs/delegation.html](https://kotlinlang.org/docs/delegation.html)

[https://kotlinlang.org/docs/delegated-properties.html](https://kotlinlang.org/docs/delegated-properties.html)

[https://www.baeldung.com/kotlin/delegation-pattern](https://www.baeldung.com/kotlin/delegation-pattern)