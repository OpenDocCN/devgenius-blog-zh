# 有效的 Java:通过接口引用对象

> 原文：<https://blog.devgenius.io/effective-java-refer-to-objects-by-their-interfaces-7cc865dcf09?source=collection_archive---------3----------------------->

![](img/325dab7cbe9027c6edd8477b86aa3889.png)

照片由 [Namroud Gorguis](https://unsplash.com/@namroud?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

今天的主题是灵活性。当定义参数、返回类型或变量时，鼓励将它们定义为具体类型实现的接口。所以不要像这样:

```
LinkedHashSet<String> stringSet = new LinkedHashSet();
```

相反，我们应该实现如下内容:

```
Set<String> stringSet = new LinkedHashSet();
```

像这样编写我们的代码可以让我们改变变量的实现，只要它们符合相同的接口。因此，也许我们可以不使用第一个例子中定义的`LinkedHashSet`，而是将它改为`HashSet`，而不必更改任何其他代码。也就是说，我们确实需要知道代码是否依赖先前实现的属性(比如`LinkedHashSet`对`HashSet`的排序保证)。

我们可能决定改变实现的一些原因是什么？一些例子可能是:

*   性能改进
*   内存节省
*   期望的附加功能(如订购保证)
*   等等。

虽然在某些情况下，如果我们想改变变量的具体类型，可能会改变所有的类型声明，但这将更加痛苦，额外的成本最终可能会成为一个障碍，不值得进行更改。

让我们考虑一下我们可能不选择遵循这个建议或者不能遵循这个建议的一些原因。

如果我们使用的类型对其他实现没有任何期望，比如`String`、`BigInteger`等，那么在这些情况下我们选择不使用接口(事实上我们没有使用接口的选项，因为不存在接口)。

有些类的层次结构不是基于接口，而是基于继承。在这些情况下，替代方法通常是使用该类型的基类(通常是抽象类型)。这方面的一个例子是`ByteArrayOutputStream`到`OutputStream`。

最后一个想法是，即使存在一个接口，我们也选择不使用它，因为我们希望调用一个特定的函数，这个函数只在一个独特的具体类型上可用，而这个类型在接口上并不存在。这方面的一个例子是`Queue`接口中缺少的`PriorityQueue`的`comparator`接口。

这一章的重点是使用接口而不是具体的类，除此之外，这一章实际上是在讨论使用最通用的类型来完成工作。最终的泛型类型将是`Object`的类型，但是这种类型通常不会提供我们完成工作所需的方法，所以我们继续向下移动类的层次结构，直到我们找到足够好的方法来使用。我经常遇到的一个例子是将一个对象集合传递给一个方法并对其进行迭代。举例来说，我可以不采用类型为`List`的参数，而是采用`Iterable`，这让我的选择更加开放。在开发时，灵活性是一种超级力量。我们应该寻找机会尽可能增加我们的灵活性，这是我们实现这一目标的又一次机会。