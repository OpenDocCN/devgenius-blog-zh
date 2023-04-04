# 分部类和方法

> 原文：<https://blog.devgenius.io/partial-classes-and-methods-fa6da5d97cb8?source=collection_archive---------14----------------------->

![](img/e33bde0f5792d54c0917368eba609de7.png)

在本文中，我将解释什么是分部类和分部方法，以及如何在 C#中实现它们。

在 C#中，用于标识类的标识符必须是唯一的；然而，当处理大型项目时，可能需要多个开发人员同时处理同一个类或方法。正因如此，微软引入了**关键字 partial** ，并在 C# 2.0 中加入了这个新特性。

关键字 partial 允许**将一个类定义**拆分成同一个名称空间内的不同部分**，这些部分将在应用程序编译时组合起来。**

要使用此功能，每个零件必须:

*   使用**部分**关键字。
*   在编译时可用。
*   拥有相同的访问修饰符( **public** ， **private** ， **protected** 和 **internal**
*   与其他部分位于相同的名称空间中。

同样值得一提的是:

*   如果有一部分被声明为**抽象**，那么整个类型都被认为是抽象的。
*   如果有一部分被声明为**密封的**，那么整个类型都被认为是密封的。
*   如果有一部分声明了一个基类，那么整个类型都会继承那个类(即使省略了基类的部分也会继承基类)。
*   在一个部分中声明的任何类、结构或接口成员对所有其他部分都可用。

例如，如果我们考虑以下部分:

**partialclassfirstfile . cs**

```
partial class Earth : Planet, IRotate
{
    public void CalcolateMass() { … }
}
```

**partialclassecondfile . cs**

```
partial class Earth : IRevolve
{
    public void CalcolateSpeed() { … }
}
```

编译结束后，等价的类是:

```
class Earth : Planet, IRotate, IRevolve
{
    public void CalcolateMass() { … }
    public void CalcolateSpeed() { … }
}
```

partial 关键字也可以应用于方法。它的行为与类非常相似，事实上，一个类可以包含方法的签名，另一个部分可以提供它的实现。这两部分将在编译时合并。

如果没有提供实现，编译器将通过在编译时移除方法和所有调用来优化代码。

提到分部方法很重要:

*   必须返回 **void** 。
*   在或 **ref** 中可以有**，但不能有 **out** 参数(必须分配 **out** 参数，因为可能无法实现分部方法，所以不允许使用这种参数)。**
*   是隐式**私有的**(因此不能是虚拟的)。
*   可以有**静态**和不安全**修改器**。
*   可以是泛型。

在下面的例子中，我们将在前面的代码中增加一些代码。

**partialclassfirstfile . cs**

```
partial class Earth : Planet, IRotate
{
    public void CalcolateMass() { … }
    public partial void CalcolateVolume() { }
    public partial void CalcolateForce() { }
}
```

**partialclassecondfile . cs**

```
partial class Earth : IRevolve
{
    public void CalcolateSpeed() { … }
    public partial void CalcolateVolume() { … }
}
```

在这种情况下，在 PartialClassFisrtFile.cs 文件中实现的类 Earth 只声明 CalcolateVolume 和 CalcolateForce 的方法签名。虽然 CalcolateVolume 方法是在 PartialClassSecondFile.cs 文件中的相关类中实现的，但另一个方法不是，这意味着一旦编译结束， *CalcolateForce* 方法将被移除，最终的类将是:

```
class Earth : Planet, IRotate, IRevolve 
{
    public void CalcolateMass() { … }
    public void CalcolateVolume() { … }
    public void CalcolateSpeed() { … }
}
```