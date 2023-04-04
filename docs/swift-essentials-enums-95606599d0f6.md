# Swift 基础|枚举

> 原文：<https://blog.devgenius.io/swift-essentials-enums-95606599d0f6?source=collection_archive---------13----------------------->

## 在 Swift 中创建您自己的数据类型

![](img/df2247a7bbf83a5ecbf8d459fda50273.png)

凯利·西克玛在 [Unsplash](https://unsplash.com/s/photos/build?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

答几天前，我谈到了 Swift 数据类型。今天，我将谈论 Enums，这是一种在 Swift 中创建自己的数据类型的简单方法。如果你没有看过我关于数据类型的帖子，我强烈建议你在阅读本文之前先看看。

 [## Swift Essentials(常量/变量/数据类型/类型别名)

### 常数/变量

/数据类型/ Typealias)常量/变量 medium.com](https://medium.com/@elizabethyu_21451/swift-essentials-constants-variables-data-types-typealias-47a1c1e68611) 

枚举可以被设计成适合特定的情况。例如，如果您只想要表示方向的值，您可以创建一个名为 directions 的枚举，其中只包含值“北”、“东”、“西”和“南”

```
enum Directions{
    case north 
    case east
    case west 
    case south
}
```

另一种初始化枚举的方法如下。

```
enum Directions{
    case north, east, west, south 
}
```

为了演示如何使用枚举，请看下面的例子。

```
**var** myDir = Directions.south // var myDir:Directions = .south switch myDir{
    case .north: 
       print("move north")     case .east: 
       print("move east") case .south 
       print("move east") case .west 
       print("move east")}
```

变量 myDir 属于方向类型。如果您之前已经初始化了该类型，则可以访问这些值，而不必提及该类型。上面的例子使用了变量 myDir，并确定了它包含的方向值，因此它将打印“move south ”,因为 myDir 的值是 south。

# **原始值**

你也可以增加你的枚举值。一种方法是添加用户定义的枚举。

```
enum Directions: String {
    case north = "Canada"
    case east = "New York"
    case west = "California"
    case south = "Mexico"
}
```

要访问这些特定的值，只需使用。原始值属性

```
print(Directions.north) --> north
print(Directions.north.rawValue) --> "Canada"
```

# **关联值**

向枚举添加值的另一种方法是让用户定义值。

```
enum Directions{
    case north(String)
    case east(String)
    case west(String)
    case south(String)
}
```

当用关联值初始化枚举时，必须指示关联值，否则将引发错误。

```
let myDir = Directions.north("Canada") 
print(myDir) --> north("Canada")
```

# CaseIterable

根据苹果的[文档](https://developer.apple.com/documentation/swift/caseiterable)，CaseIterable 通常用于访问一个枚举内部的所有值。如果您希望您的枚举是 CaseIterable 的，您必须将您的枚举定义为 CaseIterable。比如说。

```
enum Directions : CaseIterable{
    case north, east, west, south 
}
```

使用. allCases 访问这些值。

```
let allVals:[Directions] = Directions.allCases.map({$0}).joined(separator:",")
```

这将创建一个包含方向枚举中所有值的数组。

# 可比较的

可比值允许您将变量与、≤、≥、==、！=.整数是可比较的数据类型。通过将枚举声明为可比较的，可以使枚举具有可比较性。比如说。

```
enum Directions:Comparable{
    case north 
    case east
    case west 
    case south
}
```

更靠近枚举顶部的值比它们下面的值小。

```
print(Directions.north < Directions.south)
```

上面的代码输出 true，因为。北比高。枚举中的南方也就是说。北方比南方小。

以下是今天博客的简短总结。

*   枚举是具有特定值的用户定义的数据类型。
*   用户定义的值通过。原始值
*   关联值由用户在枚举首次初始化时定义
*   通过将枚举声明为 CaseIterable 或 Comparable，枚举可以是 CaseIterable 或 Comparable。

感谢您的阅读！