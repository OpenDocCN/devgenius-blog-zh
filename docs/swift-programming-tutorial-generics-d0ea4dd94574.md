# Swift 编程教程:泛型

> 原文：<https://blog.devgenius.io/swift-programming-tutorial-generics-d0ea4dd94574?source=collection_archive---------6----------------------->

## 灵活编码的基本工具

![](img/c01c62cc9a3e8c1167c861aebc4d42b5.png)

凯利·西克玛在 unsplash.com 拍摄的照片

wift 对泛型的支持允许你编写可以用于多种类型的代码，同时仍然保持类型安全。通过使用泛型，您可以创建灵活且可重用的函数和类型，这意味着它们可以很容易地适应不同的类型，而无需为每种类型编写单独的代码。泛型还允许您编写可应用于多种类型的抽象代码，从而帮助您避免重复。这可以使您的代码更简洁、更易读，因为它清楚地表达了代码的意图，而不需要重复。

本文是我的 [Swift 编程教程](https://arc-sosangyo.medium.com/list/introduction-to-swift-programming-tutorials-0a558ad90722)系列的一部分。

您可以通过在函数、类或其他类型的名称后添加所需类型的占位符来使用泛型，写为`<GenericName>`。下面是一个语法示例:

```
func printDataType<T>(data: T) {}
```

> **注意:**在定义泛型类型时，字母“T”通常被用作占位符，但是它可以被任何其他有效的标识符替换。

## 在函数中使用泛型

从一个非常简单的使用函数的例子开始，这就是泛型的创建方式。

```
func printDataType<T>(data: T) {
    print(data, "is", type(of: data))
}

printDataType(data: "Hello World")
printDataType(data: 45.0000087)
```

如果你好奇，这里是输出:

```
Hello World is String
45.0000087 is Double
```

## 使用具有类型约束的泛型

再举个例子怎么样。让我们创建一个比较两个数字并返回较小数字的函数。下面是没有使用泛型的代码:

```
func showLowerNumber(firstNum: Int, secondNum: Int) {
    if firstNum < secondNum {
        print("First number is lower")
    } else {
        print("Second number is lower")
    }
}

showLowerNumber(firstNum: 34, secondNum: 89)
```

上面显示的函数用于显示哪个数字更小。但是，它只接受整数数据类型[。如果我们想使用其他数值数据类型，比如 double 或 float，我们可以利用泛型修改函数来处理这些数据类型。像这样编辑上面的代码:](https://medium.com/@arc-sosangyo/swift-programming-tutorial-variables-670ceea20bd1)

```
func showLowerNumber<AnyNum: Comparable>(firstNum: AnyNum, secondNum: AnyNum) {
    if firstNum < secondNum {
        print("First number is lower")
    } else {
        print("Second number is lower")
    }
}

showLowerNumber(firstNum: 0.067, secondNum: 0.0001)
```

以下是输出结果:

```
Second number is lower
```

> [Comparable](https://developer.apple.com/documentation/swift/comparable) 是一个协议，允许使用关系[操作符](https://medium.com/@arc-sosangyo/swift-programming-tutorial-basic-operators-72dc48116b1c)来比较特定类型的实例。

## 在类中使用泛型

泛型也可以用在类中，因为它们允许创建可重用的对象。这里有一个例子:

```
class ShowDataType<T> {

    var data: T

    init(data: T) {
        self.data = data
    }

    func printDataType() {
        print(data, "is", type(of: data))
    }

}

var showAny = ShowDataType(data: [1, "Hi"])
showAny.printDataType()

var showString = ShowDataType<String>(data: "Hello World")
showString.printDataType()

var showInt = ShowDataType<Int>(data: 45)
showInt.printDataType()
```

以下是输出结果:

```
[1, "Hi"] is Array<Any>
Hello World is String
45 is Int
```

在提供的例子中，没有为`showAny` 变量指定数据类型，但是为`showString`和`showInt` 变量都指定了数据类型。

## 在 SwiftUI 这样的结构中使用泛型

由于 [SwiftUI](https://medium.com/@arc-sosangyo/list/swiftui-tutorial-03734e631240) 使用 struct，您将基本上使用泛型来存储视图，作为可重用 SwiftUI 视图的属性。这里有一个例子:

```
struct BlueCirclePlaceholder<Content: View>: View {

    var content: () -> Content

    var body: some View {
        content()
            .frame(maxWidth: .infinity)
            .frame(height: 50)
            .background(Color.blue)
            .clipShape(Circle())
    }

}
```

在上面的代码中，我们将`Content`类型声明为泛型，并指定它符合作为类型约束的`View`协议。

我们现在可以这样使用新视图:

```
struct ContentView: View {

    var body: some View {
        VStack {
            BlueCirclePlaceholder {
                Text("AS")
                    .font(.system(size: 30))
                    .padding()
            }
        }
        .background(Color.white)
    }

}
```

总的来说，泛型帮助您创建适应性强的函数和类型，它们可以处理不同的数据类型，而不需要重复，从而提高代码的效率和可维护性。

愿法典与你同在，

-电弧