# Swift:约束协议扩展

> 原文：<https://blog.devgenius.io/swift-constraining-protocol-extensions-e0152589b115?source=collection_archive---------29----------------------->

![](img/2cf9c368d01038999425fa1ebde192f9.png)

照片由[大卫·布鲁克·马丁](https://unsplash.com/@dbmartin00?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

想象一下，我们需要用一个特殊的函数来扩展`Array`，这个函数将删除所有的重复项。这可以通过引用`Element`作为其内部值来实现。为了检查相等性，数组`Element`必须符合`Equatable`，这可以作为约束添加。将`Element`约束到`Equatable`意味着这个方法只适用于带有`Equatable`元素的数组。

```
extension Array where Element: Equatable {
   func unique() -> [Element] {
     // implementation
   }
}
```

这看起来是一个好的开始，但是更合理的做法是将这个扩展给予其他集合类型，而不仅仅是`Array`。使用这种方法，其他类型可以从此方法中受益，而不是扩展一个具体的类型。

```
extension Collection where Element: Equatable {
   func unique() -> [Element] {
     // implementation
   }
}
```

这种方法的问题可能是速度不够快，或者没有您所寻求的性能。这可以用`Hashable`代替`Equatable`来解决。`Hashable`是`Equatable`的子协议，这意味着 Swift 将选择最具体的分机进行工作。通过在`Collection`上定义两种方法，Swift 将选择最专业的一种。

您可以将关联类型约束到具体类型，而不是将它们约束到协议。假设我们有带有`likesCount`属性的`Picture` struct，它跟踪关注者喜欢一张图片的次数。

现在`Collection`可以扩展得到总点赞数。因为我们想将`Element`约束到`Picture`(具体类型)，所以可以使用`==`操作符。

```
struct Picture: Hashable {
   let likesCount: UInt
}extension Collection where Element == Picture {
   var totalLikesCount: Int {
      // implementation
   }
}
```

有了这个约束，无论`Collection`类型如何(只要它包含`Picture` s)，您都可以获得总的赞数。决定延期可能会很棘手。通常你可以在`Array`类型上有一个具体的扩展，但是如果这不令人满意，你可以扩展更低级的类型(在这个例子中是`Collection`)。如果这对你的情况还不够，你可以延长`Sequence`。