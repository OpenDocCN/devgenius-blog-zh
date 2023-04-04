# 国际奥委会形

> 原文：<https://blog.devgenius.io/ioc-kata-c4e99694412f?source=collection_archive---------2----------------------->

![](img/74d5838a3c0b3abf725d9a9c08a3694b.png)

如今，我们经常认为理所当然的工具之一是控制反转(IoC)容器。IoC 使得遵循依赖倒置原则(SOLID 中的“D ”)相对简单，所以开发人员经常混淆这两者并不奇怪，但它们不是一回事。IoC 容器只是实现该原则的一种方式，它是将我们大部分现代工作结合在一起的粘合剂。尽管如此，对大多数开发人员来说，这仍然是一个黑匣子，他们不知道幕后的魔力是如何发生的。我发现真正理解某些东西的最好方法是自己构建一个，所以在这篇文章中，我们将介绍如何创建一个全功能的 IoC 容器来处理最常见的情况。

我们将以编码形的形式建立一个 IoC，这种做法源于 90 年代中期的软件工艺运动。卡塔是简短的编码练习，旨在帮助强化和理解常见问题的解决方案，以便当它们出现在现实世界中时，您能够识别它们，并且已经熟悉了解决方案。您通过一系列小步骤编写了一个或多或少固定的解决方案，在这里添加一个特性，在那里重构，然后朝着一个常见业务问题的公认解决方案努力，或者实现一个简单的游戏。

对我来说，最有用的形总是那些能教会我一些东西的，或者它们本身就是有趣的。Guy Royse 的“ever craft Kata”不是我所说的热身运动，因为你可能会花很多时间来完成你的第一次练习。它不会产生一个你可以在商业场景中利用的成品，但是它*确实*帮助你开发了一种思考*在日常业务中出现的问题的方式，比如给现有的实体如用户或企业增加新的功能，而不在过程中关闭现有的系统。如果你从未完成过，我建议你在某个周末完成它。*

在阅读了由[柳文欢·艾尼](http://ayende.com/Blog/archive/2007/10/20/Building-an-IoC-container-in-15-lines-of-code.aspx)和[肯·埃戈齐](http://www.kenegozi.com/Blog/2008/01/17/its-my-turn-to-build-an-ioc-container-in-15-minutes-and-33-lines.aspx)所做的一些微 IoC 实现后，我写了我的“Itty Bitty IoC”。我看到了国际奥委会是如何在幕后运作的，并决定建立自己的国际奥委会。从那时起，我把它发展成了一个编码形，因为它真的很容易理解和使用。我现在想给你演示一下这个形。我们将一次构建一个特性，遵循测试驱动的开发模式，首先编写一个测试来演示我们想要的行为，然后在进入下一个之前实现那个特性。

# 第 0 步——设置 dojo

克隆 [GitHub repo](https://github.com/MelGrubb/IocKata) ，并确保您在“主”分支上。这里有一个用于 IoC 的空类，一个空的测试类，以及三个将在测试中使用的相互依赖的类 Foo、Bar 和 Baz。我将使用 NUnit，因为它是。Net 空间，但是您可以使用您最喜欢的测试库。存储库还包含每个已完成步骤的分支。

# 第一步—实例注册

第一种，也是最简单的一种注册，只不过是类型或接口的目录，以及这些类型的实例*。每当我们需要时，这将简单地把一个类的现有实例交还给我们。实质上，当我请求一个 IFoo 时，我希望 IoC 把我注册的 Foo 的具体实例还给我。*

第一个测试验证了两件事:一个实例既可以注册也可以解析，并且后续的注册会覆盖之前的注册(即，后进先出)。

```
using NUnit.Framework;namespace IocKata
{
    [TestFixture]
    public class Tests
    {
        [Test]
        public void Step1_InstanceRegistration()
        {
            var instance1 = new Foo(new Bar(new Baz()));
            IoC.Register<IFoo>(instance1); var instance2 = new Foo(new Bar(new Baz()));
            IoC.Register<IFoo>(instance2); var value = IoC.Resolve<IFoo>();
            Assert.AreSame(instance2, value);
        }
    }
}
```

为了实现这个功能，我们需要一些东西。我们需要某种容器来存储依赖项，这些是我们要注册的类型。我们可以用一个简单的字典做到这一点，使用类型作为键，使用实例作为值。我们还需要一个方法将实例*添加到这个字典中，并需要另一个方法来检索它们。*

```
using System;
using System.Collections.Generic;namespace IocKata
{
    public static class IoC
    {
        private static readonly Dictionary<Type, object> Dependencies = new Dictionary<Type, object>(); public static void Register<T>(T instance)
        {
            Dependencies[typeof(T)] = instance;
        } public static T Resolve<T>()
        {
            return (T) Dependencies[typeof(T)];
        }
    }
}
```

此时，测试应该通过了，我们准备进入下一步。

# 第二步—委托注册

下一种注册只是稍微复杂一点。我们不会向 IoC 提供要返回的实际对象，而是会传入一个在依赖关系解决后执行的函数，本质上是告诉 IoC *如何*构建类型。这些功能甚至可以利用 IoC 作为其逻辑的一部分。用法应该类似于下面的测试。

```
[Test]
public void Step2_DelegateRegistration()
{
    IoC.Register<IBaz>(() => new Baz());
    IoC.Register<IBar>(() => new Bar(IoC.Resolve<IBaz>()));
    IoC.Register<IFoo>(() => new Foo(IoC.Resolve<IBar>())); var value = IoC.Resolve<IFoo>();
    Assert.IsInstanceOf<Foo>(value);
    Assert.IsInstanceOf<Bar>(value.Bar);
    Assert.IsInstanceOf<Baz>(value.Bar.Baz);
}
```

我们可以将函数存储在现有的字典中，因为函数可以被视为对象，但我们需要一种方法来记住条目是表示实例还是函数，因此我们将修改字典来存储包含对象或函数的元组，以及一个新的枚举值，该值表示它是哪种依赖关系。我们将使用 C# 7 中引入的新的元组语法，使其在后面更具可读性，即使声明有点罗嗦。

```
private enum DependencyType
{
    Instance,
    Delegate,
}private static readonly Dictionary<Type, (object value, DependencyType dependencyType)> Dependencies
    = new Dictionary<Type, (object value, DependencyType dependencyType)>();
```

接下来，我们需要修改现有的实例注册方法，并添加新的委托注册函数。

```
public static void Register<T>(T instance)
{
    Dependencies[typeof(T)] = (instance, DependencyType.Instance);
}public static void Register<T>(Func<object> func)
{
    Dependencies[typeof(T)] = (func, DependencyType.Delegate);
}
```

最后，我们将扩展 Resolve 方法以涵盖新的注册类型

```
public static T Resolve<T>()
{
    var dependency = Dependencies[typeof(T)]; if (dependency.dependencyType == DependencyType.Instance)
    {
        return (T) dependency.value;
    }
    else
    {
        return (T) ((Func<object>) dependency.value).Invoke();
    }
}
```

# 第三步——单身

通过一个小的调整，我们可以将实例和委托解析结合在一起，以便将对象构造为单件。也就是说，我们将在第一次被请求时创建一个新的依赖实例，但是在以后的每次调用中返回相同的实例。下一个测试展示了我们想要的行为。

```
[Test]
public void Step3_SingletonDelegateRegistration()
{
    IoC.Register<IBaz>(() => new Baz(), isSingleton: false);
    Assert.AreNotSame(IoC.Resolve<IBaz>(), IoC.Resolve<IBaz>()); IoC.Register<IBaz>(() => new Baz(), isSingleton: true);
    Assert.AreSame(IoC.Resolve<IBaz>(), IoC.Resolve<IBaz>());
}
```

我们需要再次更新字典，这一次向元组添加第三个值来指示注册是否应该是单例的，并更新现有的 Register 方法。对于实例注册来说，isSingleton 值不会有任何区别，但是根据定义，我认为它们是单例的，所以我将在第一个 Register 方法中将它设置为 true。

```
private static readonly Dictionary<Type, (object value, DependencyType dependencyType, bool isSingleton)>
    Dependencies = new Dictionary<Type, (object value, DependencyType dependencyType, bool isSingleton)>();public static void Register<T>(T instance)
{
    Dependencies[typeof(T)] = (instance, DependencyType.Instance, true);
}public static void Register<T>(Func<object> func, bool isSingleton = false)
{
    Dependencies[typeof(T)] = (func, DependencyType.Delegate, isSingleton);
}
```

最后，我们将像往常一样更新 Resolve 方法来创建依赖实例，但是当它应该是单例时，将其重新注册为一个实例。

```
public static T Resolve<T>()
{
    var dependency = Dependencies[typeof(T)]; if (dependency.dependencyType == DependencyType.Instance)
    {
        return (T) dependency.value;
    }
    else
    {
        var value = (T) ((Func<object>) dependency.value).Invoke();
        if (dependency.isSingleton)
        {
            Dependencies[typeof(T)] = (value, DependencyType.Instance, true);
        } return value;
    }
}
```

现在这是一个可用的 IoC，除了你必须明确地告诉它如何构建所有东西。它还不知道如何自己解决任何问题…目前还不知道。

# 第四步—动态解析

一个真正的国际奥委会的标志之一是它有能力自己解决一些事情。只要一个类型的依赖项也被注册，就不需要给它一个函数来完成这项工作。它应该知道该做什么。它的用法应该是这样的。

```
[Test]
public void Step4_AutomaticResolution()
{
    IoC.Register<IBaz, Baz>();
    IoC.Register<IBar, Bar>(isSingleton: true);
    IoC.Register<IFoo, Foo>(); var foo = IoC.Resolve<IFoo>();
    Assert.IsInstanceOf<Foo>(foo);
    Assert.IsInstanceOf<Bar>(foo.Bar);
    Assert.IsInstanceOf<Baz>(foo.Bar.Baz); Assert.AreNotSame(foo, IoC.Resolve<IFoo>());
    Assert.AreSame(foo.Bar, IoC.Resolve<IBar>());
    Assert.AreNotSame(foo.Bar.Baz, IoC.Resolve<IBaz>());
}
```

您会注意到，在这个测试中，我们不再提供委托函数。我们只是告诉 IoC，当我们要求注册接口时，我们想要什么具体的类。由 IoC 选择一个构造函数并调用它，解决任何具体类的依赖关系。这比听起来容易。我们将创建第三个枚举条目来表示这种情况，并添加一个新的 Register 方法。

```
private enum DependencyType
{
    Instance,
    Delegate,
    Dynamic,
}public static void Register<T1, T2>(bool isSingleton = false)
{
    Dependencies[typeof(T1)] = (typeof(T2), DependencyType.Dynamic, isSingleton);
}
```

因为泛型是编译时的东西，所以我们不能使用现有的泛型 Resolve 方法在运行时构建构造函数参数。因此，我们首先需要将 Resolve 方法的主要逻辑提取到一个私有的、非泛型的版本中，该版本只返回对象，并从现有的泛型 Resolve 方法中调用它。这将允许新提取的方法回调到自身中，以在运行时解析构造函数参数。因为这篇文章已经够长了，我不会在这里深入讨论新的 Resolve 分支的所有细节，也不打算作为反思的教程，但是要点是这样的。找到“最贪婪”的构造函数，即具有最多参数的构造函数，通过回调 resolve 方法来解析这些参数，然后使用这些参数来调用您最初尝试构建的类型的构造函数。这是完整的版本。

```
public static T Resolve<T>()
{
    return (T) Resolve(typeof(T));
}private static object Resolve(Type type)
{
    var dependency = Dependencies[type]; if (dependency.dependencyType == DependencyType.Instance)
    {
        return dependency.value;
    }
    else if (dependency.dependencyType == DependencyType.Delegate)
    {
        var value = ((Func<object>) dependency.value).Invoke();
        if (dependency.isSingleton)
        {
            Dependencies[type] = (value, DependencyType.Instance, true);
        } return value;
    }
    else
    {
        var concreteType = (Type) dependency.value;
        var constructorInfo = concreteType.GetConstructors()
		    .OrderByDescending(o => (o.GetParameters().Length)).First();
        var parameterInfos = constructorInfo.GetParameters(); if (parameterInfos.Length == 0)
        {
            return Activator.CreateInstance((Type)dependency.value);
        }
        else
        {
            var parameters = new List<object>(parameterInfos.Length);
            foreach (ParameterInfo parameterInfo in parameterInfos)
            {
                parameters.Add(Resolve(parameterInfo.ParameterType));
            }
            var value = constructorInfo.Invoke(parameters.ToArray()); if (dependency.isSingleton)
            {
                Dependencies[type] = (value, DependencyType.Instance, true);
            } return value;
        }
    }
}
```

仅此而已。我们有一个不到 100 行的完整 IoC。如果您通过从 Resolve 方法中删除“else ”,并从带有单个指令的分支中删除括号，从而牺牲一些可读性，那么您可以使它变得更小。您可以看到我为什么将其命名为 IttyBittyIoC，但是您还可以做更多的事情来使它变得更好。

# 额外学分:

*   添加一个 InjectionConstructorAttribute 来手动标记希望 IoC 使用的构造函数。
*   添加基于约定的程序集扫描，以匹配具有相似名称的类的接口(例如 IFoo/Foo)。

您可以查看 [GitHub repo](https://github.com/MelGrubb/IocKata) 中的 Step5 和 Step6 分支来了解我是如何实现这些特性的，Step7 可以查看完全重构的、极简的 68 行版本。

## -梅尔·格拉布，AWH 软件开发团队负责人