# 有效的 Java:对于实例控制，优先使用枚举类型而不是 readResolve

> 原文：<https://blog.devgenius.io/effective-java-for-instance-control-prefer-enum-types-to-readresolve-4c30c71b765e?source=collection_archive---------9----------------------->

![](img/da3391d3a799c454d2da839685541fe8.png)

马丁·桑切斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在上一节中，我们讨论了在 Java 中创建单例对象的不同方法。我们讨论的方法之一遵循以下模式:

```
public class Elvis {
  public static final Elvis INSTANCE = new Elvis();
  private Elvis() { ... } public void leaveTheBuilding() { ... }
}
```

通过构造构造函数`private`，我们可以防止意外创建`Elvis`对象。这种模式的问题是，如果你把`implements Serializable`添加到类中，我们就会绕过私有构造函数。正如前面章节中提到的，序列化有效地引入了一个新的、系统提供的构造函数。

处理这个问题并收回对一个类产生的实例的一些控制的内置功能是`readResolve`函数。该功能允许用另一个实例代替由`readObject`创建的实例。如果你的类用正确的签名定义了一个`readResolve`函数，它将在`readObject`函数之后被调用。此方法返回的引用将代替新创建的对象返回。通常情况下，不会保留对新创建对象的引用，因此可以对其进行垃圾收集。

使用这个功能来确定我们上面的`Elvis`类可能最终看起来像下面这样:

```
private Object readResolve() {
  return INSTANCE;
}
```

在这种情况下，这非常简单，我们不需要对新创建的对象做任何事情，只需要返回一个真正的`Elvis`实例。因为没有使用来自序列化的数据，所以我们可以并且应该将所有实例字段声明为`transient`。如果你有对象引用类型的实例字段，那么你必须声明它们`transient`以避免可能的攻击，攻击者可以在对象被垃圾收集之前得到反序列化的对象，因此可以保留它，导致你的 singleton 不再是 singleton。

理解这种攻击的具体步骤并不是绝对必要的。鼓励感兴趣的读者阅读原始资料。我只想说，通过创建一个“stealer”类，可以导致与反序列化对象的循环依赖，从而避免垃圾收集。虽然不太可能受到攻击，但安全总比后悔好。

虽然将所有字段声明为`transient`是避免这个问题的一种方法，但也有其他方法可以实现。上一章中的另一个模式使用了单元素枚举类型来简化单例。这将大量的单例安全语义放在 JVM 上执行，并将您从这一负担中解放出来。我们的枚举类型示例如下:

```
public enum Elvis {
  INSTANCE; private String[] favoriteSongs = { "Hound Dog", "Heartbreak Hotel" }
  public void printFavorites() {
    System.out.println(Arrays.toString(favoriteSongs));
  }
}
```

当一个类的实例在编译时未知时，即使使用上面的模式也可能是必要的。

使用`readResolve`函数时，另一件要注意的事情是方法的可见性。如果你的班级是`final`，那么`readResolve`应该是`private`。如果你的课程不是期末考试，你会有更多的选择。如果设置为`private`，它不会对子类做任何实例控制。如果它是包私有的，它将只适用于存在于同一个包中的子类。最后，如果将它设为`protected`或`public`，并且子类没有覆盖它，那么该类的任何反序列化都将创建超类的一个实例，而不是子类，这可能会导致`ClassCastException`。

总之，在尝试对可序列化的类实施实例控制时，应该尽可能优先使用枚举类型单例。如果不可能使用枚举模式，那么在编写类的`readResolve`方法时需要仔细考虑。您应该确保该类的所有实例字段要么是原始的，要么被标记为瞬态的，以防止对您的实例控制机制的潜在攻击。

*更多内容尽在*[*blog . dev genius . io*](http://blog.devgenius.io)*。*