# 瓦尔哈拉如何重写可选

> 原文：<https://blog.devgenius.io/how-valhalla-rewrites-optional-a8b4008c81af?source=collection_archive---------5----------------------->

## Valhalla 进入 Java 后，Optional 会发生什么变化

![](img/f71aac8444bca88262bebde5bc3f03b5.png)

照片由[尼克拉斯·哈曼](https://unsplash.com/@niklas_hamann?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/lack?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

*“瓦尔哈拉将变可选。”*

***确定，但有什么变化？***

你会在社区里发现这些，但是没有具体的证据表明发生了什么变化。

*我们需要抛弃可选吗？在当前代码使用可选的情况下，这怎么可能呢？Optional 仍然可以为空吗？*

让我们深入研究并回答这些问题。

# 手工特殊化的可选将毫无意义

Valhalla 的目标之一是改进泛型。

你可以使用基本类型来代替装箱类型。这将比目前的方法带来更多的好处。对泛型使用装箱类型会影响性能。其中包括装箱转换、堆分配和内存间接寻址。

即使在今天，你也可以看到泛型对原语的需求。Eclipse 集合在它们的地图中使用原语。并且从它们的类型到原生 Java 的转换不是现成的。

你可以在[中查看他们对映射类型](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/impl/map/mutable/primitive/ByteIntHashMap.html)的实现。还有转换到原生的问题`Map<byte, String>`。更多关于这个问题的以下 SO 问题。

[](https://stackoverflow.com/questions/71349112/how-convert-adapt-byteobjecthashmap-to-a-jdk-map) [## 如何将 ByteObjectHashMap 转换/调整为 JDK 地图？

### 目前，实现这一点最简单的方法是使用 forEachKeyValue 将 ByteObjectMap 的内容复制到一个映射中…

stackoverflow.com](https://stackoverflow.com/questions/71349112/how-convert-adapt-byteobjecthashmap-to-a-jdk-map) 

*改进的仿制药如何影响 Optional？*

我们可以毫无问题地完成下面的转换。

*   `OptionalDouble`->-`Optional<double>`
*   `OptionalInt -> Optional<int>`

所有的手工特殊化选项将变得毫无意义。您可以使用它们的专用泛型来代替 primitive。

这将意味着手工专业课可能会被完全取消。`OptionalInt`、`OptionalDouble`等等。即便如此，为了保持向后兼容性，我会说它们会继续存在。

所有这些变化都考虑到了扁平化。

参考类型和`Optional`将启用扁平化。使用基元类型也将实现这一点，并增加空安全性。

[手工专用选项都是基于价值的类别。](https://openjdk.org/jeps/390#:~:text=The%20%22optional%22%20classes%20in%20java.util%3A%20Optional%2C%20OptionalInt%2C%20OptionalLong%2C%20and%20OptionalDouble%3B)

因此，这带来的变化是，你将被剥夺身份操作。参照性仍然存在。所以这使得实例仍然可以为空。

此外，使用原语可以消除对盒子的需求。意思是`OptionalInt`从`int -> Integer`增加了无用的转换。有了瓦尔哈拉，这将被移除。

***为什么可选实例仍然可以为空？***

主要是为了不破坏现有的代码。即使如此，对于将来的代码，仍然可以选择将 Optional 声明为不可为 will。

# 旧的可选保持可空，Valhalla 可选可能不可空

让我们先来看看值类和原始类。

***有些值类可能是原始的。基本实例必须有默认值，并且不能为空。***

所以即使`LocalDate`可能是原始的，也没有好的默认值候选。您可以设置一些值，但是这仍然会增加不确定性。

这让我想起了 proto 消息的默认值。例如，proto 消息中的 int 字段用默认值 0 填充。和今天的 Java 一样。但是对于其他复合值，默认值为 null。

我们知道`Optional`是一个基于价值的类。尽管如此，原始类型的`Optional`也是有可能的。

*那么如果* `*Optional*` *变成数值类会有什么变化呢？*

有一点可以肯定`Optional`不可能是原语类。作为引用类型有很多用法，客户希望这种用法永远存在。

所以我们需要参考投影。 ***这到底是什么意思？*** 一个`Optional`实例将是原始的，但作为引用传递。

这个实现可以使这成为可能。

```
sealed interface Optional permits Optional.val {}primitive class Optional.val implements Optional {...}
```

这意味着我们将不能拥有`Optional.val`的可空实例。但是执行的结果可能是一个空值，因为这将退回到`Optional`。

```
Optional v = findCustomer(); // could be nullable, in existing API even after Valhalla
Optional.val v1 = Optional.of(customer); // not-nullable
```

在瓦尔哈拉登陆后，`Optional`的未来实例可能是`Optional.val`。这与 Valhalla 的口号非常吻合: ***“代码像类一样工作就像 int。”*** 。

`Optional`将成为引用类型。对于原始类型的`Optional`，你可以使用`Optional.val`。

有些人会认为`[Optional.val](https://mail.openjdk.org/pipermail/valhalla-dev/2021-April/008876.html)`[是一个怪异的符号](https://mail.openjdk.org/pipermail/valhalla-dev/2021-April/008876.html)。毕竟，我们可以将`Optional`用于引用类型，将`optional`用于原始类型的类。即便如此，这也会引起连锁反应，而且不会带来任何额外的好处。

***这种设计有什么好？***

*   *不破坏现有代码*
*   *所有可选实例都变成 ref-default，但也可以接受 val-default*
*   *此更改不会破坏现有代码，因为所有 ref-default 字段*
*   *在堆上可展平，因为值类被剥离了身份*
*   *新的可选实例将* `*Optional.val*` *默认为*

# `ofNullable`会怎么样？

将`null`传递给`Optional#of`会抛出`NullPointerException`。这就是为什么我们有`Optional#ofNullable`。

当瓦尔哈拉登陆时，大多数人会求助于使用`Optional`来包装原语。至少这将比包装盒装实例带来更多的好处。

你宁愿用`Optional<int>`而不是`OptionalInt`或`Optional<Integer>`。

所以有了原语，`ofNullable`就没有意义了。本质上，`Optional#of`和`Optional#ofNullable`都会抛出`NullPointerException`。

对于基本类型的选项，使用`ofNullable`应该有一些区别。

即使这将导致编译时问题，它也会令人困惑。`Optional#ofNullable`不会接受`null`的价值观。

当我们得到新的泛型时，可能有一种方法来推断原语`Optional`不可空。

所有这些改进应该会使`Optional`更快，更容易使用。如果我们去掉身份运算，我们会得到平坦。如果我们移除引用，我们将移除堆分配，并移除可空性。

# 参考

[更新瓦尔哈拉状态文档](https://mail.openjdk.org/pipermail/valhalla-spec-experts/2021-December/001747.html)
[新候选 JEP: 401:原始对象(预览)](https://mail.openjdk.org/pipermail/valhalla-dev/2021-April/008872.html)
[引用偏向原始类堆分配](https://mail.openjdk.org/pipermail/valhalla-dev/2021-September/009536.html)