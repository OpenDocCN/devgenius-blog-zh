# 有经验的 Java 开发人员避免这 5 个可选的实践，以减少问题

> 原文：<https://blog.devgenius.io/experienced-java-developers-avoid-these-5-optional-practices-for-fewer-issues-ce78ac10d9c?source=collection_archive---------7----------------------->

## 要避免的 5 个习惯，并在未来以更干净的代码结束

![](img/d59ae5e4a95210ba5754e9fd185c808c.png)

照片由[马体·米罗什尼琴科](https://www.pexels.com/@tima-miroshnichenko?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[佩克斯](https://www.pexels.com/photo/man-in-red-and-blue-plaid-button-up-shirt-using-silver-macbook-5702300/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

“我的同事不知道可选的存在。”——[Reddit 上的一名 Java 开发者。](https://www.reddit.com/r/java/comments/t1ev7h/comment/hyfmdt0/?utm_source=share&utm_medium=web2x&context=3)

我们使用可选项已经 8 年了。尽管如此，人们对可选择的用法仍有争议。

我们应该用 of 还是`ofNullable`搭配`orElseThrow`？我们是不是用错了`ofNullable`？`Optional` `flatMap`的用例是什么？

让我们深入了解你应该避免使用`Optional`的 5 种做法。

# 1.不要用可选的验证不变量

验证不应该用`Optional`来完成。

他们想要空的`Optional`，而不是验证异常。这不是这个工具的用例。您应该事先进行验证并抛出异常。

将`Optional`仅作为缺席指示器。

```
1 Optional<Foo> getMeFoo(String fooId) {
2    var fooIdValid = validateFooId(fooId);
3    if(!fooIdValid) { throw new FooIdInvalidException();}
4    
5    return fooService.getFooById(fooID);
}
```

验证部分不返回空的`Optional`。只有当`Optional`不在`fooService`中时，才返回它。

***如果你在第 2 行返回空的*** `***Optional***` ***，你会有歧义。***

一个值显示两种情况。这肯定不是`Optional`最初打算做的事情。

如果你不想在没有这个值的情况下继续，你可以抛出一个异常。这看起来是这样的:

```
Foo getMeFoo(String fooId) throws IllegalStateException {
    var fooIdValid = validateFooId(fooId);
    if(!fooIdValid) { throw new FooIdInvalidException();} return fooService.getFooById(fooID)
       .orElseThrow(() ->  new IllegalStateException());}
}
```

***另一个例子你可能看到的是*** `***Optional***` ***上的空异常。***

这不是很好的实践，因为您使用空的`Optional`来`swallow`错误。这段代码会产生无声的错误，将来可能会让您感到困惑。

[来源](https://stackoverflow.com/a/42993601/5999670)

***空抓怎么办？*** 对于这个场景， ***我们可以返回一个空的可选，因为它会正确地标记非问题。***

`0`下面的场景可能意味着两件事——***错误发生了或者零是有意义的。***

用一个空的`Optional`我们得到了唯一的意义，那就是缺席值。

# 2.`flatMap`对嵌套`Optional`帮助很大

大多数开发人员使用地图，这在大多数情况下没问题。但是如果你需要将可选的映射到另一个可选的，你会有一个不必要的包装器。

[来源](https://www.tabnine.com/code/java/methods/java.util.Optional/flatMap?snippet=5ce70b3ee5946700042de71a)

什么是好的平面图候选？

如果映射器函数的输入和输出都是`Optional`。例如,`Optional`通过地图绘制器并在途中变平。最后，我们得到一个单独的`Optional`，没有包装器。

***另一个用例是将多个操作排序在一起。***

让我们有基于单个参数计算的多个操作。它们相互依赖，但结果是可选的。

```
Optional<Integer> calcAs(int a) {return Optional.of(a+1);}jshell> Optional.of(1).flatMap(a -> calcAs(a).flatMap(b -> calcAs(b)))
$6 ==> Optional[3]jshell> Optional.of(1).map(a -> calcAs(a).map(b -> calcAs(b)))
$7 ==> Optional[Optional[Optional[3]]]
```

这就是你会得到的差别。

# 3.不要混淆`orElseThrow`和`ofNullable`

***使用***`***ofNullable***`*`***orElseThrow***`***是毫无意义的。你可以做一个简单的空值检查。****

*正如在前面的例子中所看到的，您可以使用`orElseThrow`获得可选和解析。你不要把`ofNullable`和`orElseThrow`连起来。*

*出现此问题是因为开发人员将 optional 用作空检查。可选的如果值为空，将抛出 NPE。这并不意味着您应该使用它来代替空检查。*

****如果你能避开*** `***orElseThrow***` ***那也是个不错的练习。****

*然后你会把期权仅仅看作是进一步映射的工具。并且避免`value!=null`可选特质。更多的价值在于映射和功能特性的可选启用。*

```
*getMeFoo("123").map(Foo::getXProp).orElse(DEFAULT_VALUE)*
```

# *4.不要对记录字段使用选项*

****可选意为返回值。而不是作为对象字段。****

*Optional 不可序列化，因此在记录中使用没有意义。记录是数据载体，所以我们不应该添加不可序列化的选项。*

*有些人会将 optional 混淆为记录中的一个例外。但事实并非如此。*

*即使在未来，可选将保持可选。Valhalla 不允许使用可选字段。未来可选将是一个基于价值的类，但仍然有可选的特质。*

*只有基本选项是可序列化的。但我们将等待瓦尔哈拉，看看这将如何工作。毕竟，他们需要满足“像类一样的代码(可选)，像 int 一样工作”的承诺*

****我们能不能把一个可选的序列化？****

*Nicolai 检查了可选的的[序列化代理。即便如此，我们需要编写自己的序列化，而不是 java 支持的现成东西。](https://nipafx.dev/serialize-java-optional/)*

*你可以用芭乐可选的来代替。与 Java 的可选相比，这个可选是可序列化的。即便如此，番石榴乡亲们还是会再次[把你指向官方 Java 的可选](https://guava.dev/releases/19.0/api/docs/com/google/common/base/Optional.html#:~:text=However%2C%20we%20do%20gently%20recommend%20that%20you%20prefer%20the%20new%2C%20standard%20Java%20class%20whenever%20possible.)。*

*重点是避免记录中的`Optional`字段。这与将 null 作为记录的字段是一样的。这不是`Optional`用例。*

# *5.在 orElseGet 中获取`Optional`值*

**[***如何将***](https://stackoverflow.com/questions/72535838/how-to-optain-an-optional-with-orelseget)`[***Optional***](https://stackoverflow.com/questions/72535838/how-to-optain-an-optional-with-orelseget)`[](https://stackoverflow.com/questions/72535838/how-to-optain-an-optional-with-orelseget)*`[***orElseGet***](https://stackoverflow.com/questions/72535838/how-to-optain-an-optional-with-orelseget)`***？******

**以下是不可能的:**

**[来源](https://stackoverflow.com/questions/72535838/how-to-optain-an-optional-with-orelseget)**

**目前对于`orElseGet`,我们可以添加一个返回值类型的供应商。所以不支持添加`Optional`供应商。**

**我们可以用`[Optional#or](https://docs.oracle.com/en/java/javase/17/docs/api//java.base/java/util/Optional.html#or(java.util.function.Supplier)`来代替。这个操作符使我们能够返回可选值，而不是某个值。尽管如此，这个操作符只适用于 Java 9 和更高版本。对于较低版本，您需要将该值映射到 Optional。**

**[来源](https://stackoverflow.com/a/72535896/5999670)**

*****从之前的解决方案中我们可以看出什么？*****

**较新的 Java 版本解决了很多可选的问题。Java 9 & 10 增加了很多可选的新方法，所以升级会解决很多问题。**

**如果你需要的话，Guava Optional 为较低的 Java 版本提供了`[or](https://guava.dev/releases/snapshot-jre/api/docs/com/google/common/base/Optional.html#or(com.google.common.base.Optional)` [方法](https://guava.dev/releases/snapshot-jre/api/docs/com/google/common/base/Optional.html#or(com.google.common.base.Optional)。尽管如此，要注意的是，Guava Optional 和 Guava Optional 是不可互换的。如果没有番石榴可选，您需要以下产品:**

```
**thisOptional.isPresent() ? thisOptional : secondChoice**
```

*****你知道导致代码更简洁的可选做法吗？或者导致更少的问题？*****

**请在评论中告诉我们。**