# Swift 核心概念:类型和变化

> 原文：<https://blog.devgenius.io/swift-core-concepts-types-mutation-b36d296c0b88?source=collection_archive---------6----------------------->

让我们回到基础，介绍 Swift 中的类型和突变。

![](img/d93fbe48cac944728b74c004abaddf40.png)

亨利·阿斯克罗夫特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## Swift 中的类型

类型是一组属性和我们可以对其执行的一组操作。当我们尝试使用 Swift 创建应用程序时，我们可以使用标准型或定制型。Swift 类型系统执行一项称为类型检查的活动，它将验证类型的属性和操作，以确保在我们运行程序之前应用程序的行为正确。通过类型检查，我们可以对编译器进行一些优化。随着时间的推移，我们将变得更加以类型为中心，作为一名 iOS 工程师，我个人认为理解这一点非常重要。

我们将讨论 Swift 中的基本类型。Swift 中的基本类型包括基于名称的类型，如类、结构、枚举或协议，以及复合类型，如函数或元组。Swift 中的每种类型都有其独特的能力，我们可以使用它来解决许多不同的问题。

**造型用类型**

```
Struct Point {
  var x: Double
  var y: Double
}Class Point {
  var x: Double
  var y: Double init(x: Double, y: Double) {
    self.x = x
    self.y = y
  }
}
```

让我们来配置一下类和结构之间的区别:

## 初始化程序

如果使用 struct，则不需要定义初始值设定项。编译器将为每个属性赋予默认值。在幕后，编译器定义了一个叫做**的基于成员的初始化器**。当你想从结构点创建一个实例，比如上面的例子，你有四个选择。首先，您可以在不为点属性赋值的情况下进行初始化。第二，您可以初始化 x 或 y 并为其赋值。第三，您可以初始化所有属性并为其赋值。而如果你使用类，那么你需要定义一个初始化式，除非你把所有的属性都变成存储属性或者计算属性。

## 值与参考值

在我看来，这是 classes 和 struct 最重要的区别。从结构创建的实例是值类型，从类创建的实例是引用类型。如果我从 Point struct 创建了两个实例， **A** 和 **B** ，那么当我从 **A** 实例进行一些值更改时，就不可能影响到 **B** 实例，反之亦然。但是在课堂上，复制语义有一个重要的区别。

```
var aStruct = Point(x: 0.0, y: 0.0)
var bStruct = Astruct
aStruct.x += 10.0
print(BStruct) // not affected let aClass = Point(x: 0.0, y: 0.0)
let bClass = A
aClass.x += 10.0
print(B) // affected
```

从上面的例子中，我们知道当我们试图对一个类实例进行更改时，它也会影响到 b 类实例。发生这种情况是因为它是引用类型，这意味着两个实例都指向底层内存中的同一个对象。作为一名 iOS 开发人员，了解类和结构之间的区别非常重要，尤其是在复制值语义方面。

## 变化

它们之间的另一个区别是范围突变。你可能想知道为什么我们在上面的例子中用 var 声明一个 struct 实例，用 let 声明一个 class 实例。使用 let 是为了防止我们从实例中改变值。我们可以将 x 值加 10，因为我们使用 var 来声明这个结构。如果您使用 let 关键字并试图改变该值，您将收到一个错误。但是为什么我们使用 let 关键字来存储来自类的实例，而我们仍然可以从类中改变 x 值呢？答案是因为类的作用域突变只应用于它自己的引用，而不是底层属性。

## 堆与栈

是的，这与内存分配有关，请继续阅读，因为理解这一点非常重要:】。作为高层，struct 和 enumeration 使用堆栈，而 classes 使用堆。每个执行线程都有自己的堆栈，我们可以在一个时钟周期内只使用一条指令进行加减操作。修改 stack it 非常简单，我们只关心最顶层的元素，我们不需要并发锁来做这件事。

那希普呢？堆由多个线程共享，我们需要执行一些并发锁来防止堆碎片化，如果我们分配和释放一个不同大小的块，就会发生这种情况。来自类的实例被分配给堆内存。它还包含关于保存了多少引用的信息，实际上这是由 ARC 处理的(我们将在另一篇文章中讨论)。如果引用总数为 0，那么对象将从堆中释放。

## 列举

Enum 是一种值类型，我们可以定义一组有限的状态。例如，我想声明一个枚举来描述视图控制器中的视图状态。

```
enum ViewState {
  case initial
  case loading
  case empty
  case error
  case populated
}var state: ViewState = .initial {
   didSet {
      // reload view
   }
}func fetchData() {
   state = .request
   NetworkService.shared.fetchData { [weak self] response in
     self?.data = response.data
     self?.state = (response.data.isEmpty) ? .empty : .populated
   }, onFailure: { [weak self] in
     self?.state = .error
   }
}
```

我经常使用枚举驱动来管理应用程序中 UI 状态。从上面的代码中，我用创建了一个变量来保存视图状态值。例如，如果我们想获取数据，状态从初始变为请求。然后了解这些信息，我们可以呈现一些活动指示器或一些加载视图。当网络服务完成获取数据时，我们还可以检查，如果数据为空，那么我们可以将状态更改为空，否则我们可以将状态更改为已填充。但是，如果发生了不好的事情，网络服务会告诉我们一个错误，我们也可以改变状态为错误。我们还可以为特定情况定义一个带有关联值的枚举。

```
enum Shape {
  case point(Point)
  case circle(radius: Int, center: Point)
  case rectangle
}
```

## 原始可表示的

实际上有一个基本的概念，也许你已经在使用它了，但是你不知道。ViewState 枚举基本上是原始可表示的，您可以创建并获取原始值。您还可以定义符合某些类型(如 Int、String 甚至 Character)枚举。这也意味着该类型是等价的、可散列的和可免费编码的。

```
enum MovieSection: String {
  case topRated = "Top Rated"
  case upcoming = "Upcoming"
  case nowPlaying = "Now Playing"
}let section: MovieSection = .topRated
print(section) // "Top Rated"
```

## 从这里去哪里

恭喜你，你现在已经向掌握 swift 编程语言迈出了第一步:】。我个人认为，作为一名 iOS 工程师，了解这个话题非常重要。理解这一点会让我们成为更好的软件工程师。你可以一直跟随最新的技术，但不要忘记在编程中创造一个坚实的基本技能。还有一些其他重要的主题，如内存管理、协议、泛型，甚至是并发性，我计划定期为 iOS 中的一些基础主题写一篇文章。