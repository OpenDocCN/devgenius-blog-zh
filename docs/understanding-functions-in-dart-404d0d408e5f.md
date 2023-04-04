# 了解 Dart 中的函数

> 原文：<https://blog.devgenius.io/understanding-functions-in-dart-404d0d408e5f?source=collection_archive---------9----------------------->

## 熟悉 Dart 功能的分步指南

![](img/704f9d49f7a224e2f7292dbe58c762cd.png)

照片由[摄影师](https://unsplash.com/@ffstop?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

[**Dart**](https://dart.dev/) 是谷歌开发的一种编程语言，用于构建不同平台的应用。Dart 主要用于开发移动、桌面和 web 应用程序。Dart 被认为是一种面向对象的编程语言。它可以编译成本机代码或 JavaScript。

在本文中，我将介绍我们可以在 Dart 编程语言中使用的基本函数。

# 安装镖

在开始之前，我们先在电脑上安装 Dart。可以在 Windows、Linux 和 Mac 操作系统上使用 Dart SDK 安装 Dart。前往以下链接，并在您的计算机上安装 Dart SDK。

[](https://dart.dev/get-dart) [## 获取 Dart SDK

### 本页描述如何下载 Dart SDK。Dart SDK 拥有您需要的库和命令行工具…

dart.dev](https://dart.dev/get-dart) 

在本教程中，我使用的是 Mac 电脑，所以我将按照每个步骤在 MAC OS 上安装 Dart。Dart 可以使用**自制软件**轻松安装在 MAC OS 上。

[](https://brew.sh/) [## 公司自产自用

### macOS(或 Linux)缺失的软件包管理器。

brew.sh](https://brew.sh/) 

家酿是一个软件包管理器，我们可以用它来安装几个软件包到 Mac OS。

在 Mac 电脑上成功安装了 Homebrew 后，打开终端并运行以下命令在您的电脑上安装 Dart SDK。

```
$ brew tap dart-lang/dart$ brew install dart
```

如果您使用的是除 Mac OS 之外的任何其他操作系统，请参考 Dart 安装指南，该指南解释了安装 Dart SDK 的分步指南。

至此完成后，在终端上运行命令`dart --version`来验证是否正确安装了 Dart。

# 功能

要测试和运行 Dart 代码，您可以直接在浏览器中使用在线 **DartPad** 。

[](https://dartpad.dev/?null_safety=true) [## 镖靶

### 编辑描述

镖靶](https://dartpad.dev/?null_safety=true) 

基本上，Dart 被认为是一种静态类型语言，它有四种原始数据类型，分别是`String`、`int`、`double`和`bool`。

```
ex:String = 'Hello World' 
// all strings fall here and they need to be surrounded by single quotes.int = 2 
// all positive & negative whole numbers fall here.double = 3.14
// numbers with decimal values fall under this data type.bool = true
// true & false come under this data type.
```

在 Dart 编程语言中，您主要可以找到三种类型的函数。让我们一次检查一个。

# 1.不返回值的函数

这种类型的函数使用 void 作为返回类型。这意味着这些函数不返回函数之外的任何值。

让我们看一个例子。

```
void main() {
  greet();
}void greet(){
  print('Have a good day!');
}
```

在这个例子中， **greet()** 是我们用来打印一些字符串值的 Dart 函数。所以在函数名(“greet”)之前，我们需要使用`void`关键字，这表明这个函数不返回函数之外的任何值。

当我们调用 greet()函数时，它将运行其中的代码块并退出函数。*(同样，您可以在没有 void 关键字的情况下使用相同的函数，它在 Dart 中工作得很好。)*

```
void main() {
  greet();
}greet(){
  print('Have a good day!');
}// output: Have a good day!
```

# 2.返回值的函数

当你需要从你的函数返回一个值给另一个函数时，你可以使用这种类型的函数。

让我们看一个例子。

```
void main() {
  int result = sum();
  print(result);
}int sum(){
  return 5 + 3;
}// output: 8
```

这里我们有一个函数叫做 **sum()** 。它将计算 5 和 3 的和，然后将结果返回给主函数。在主函数中，我们可以打印结果。

在这个例子中，我们从名为 **sum()** 的函数向主函数返回一个整数，并将其存储在名为 **result** 的变量中。因此，我们将函数的返回类型设置为一个 **int** 。

同样，您可以使用其他数据类型作为函数返回类型。

```
ex 1:void main() {
  String result = msg();
  print(result);
}String msg(){
  return 'Hi, ' + 'Howdy?';
}// output: Hi, Howdy? ex 2:void main() {
  double result = multiply();
  print(result);
}double multiply(){
  return 3.14 * 2;
}// output: 6.28 ex 3:void main() {
  bool result = isTrue();
  print(result);
}bool isTrue(){
  return true;
}// output: true
```

此外，您可以将值用作上述函数类型的函数参数。

让我们看一个简单的例子。*(这里我不打算深入研究 Dart 函数，因为这是一个初学者友好的教程。)*

```
ex 1:void main() {
  int result = add(4, 7);
  print(result);
}int add(int n1, int n2){  //here, int is optional before n1 & n2
  return n1 + n2;
}// output: 11 ex 2: 

void main() {
  msg(greeting: 'Hi,', day: 'Good Morning!');
}void msg({String day, String greeting}){
  print('$greeting $day');
}// output: Hi, Good Morning!
```

同样，您可以在 Dart 编程语言中使用高级函数。

# 3.箭头功能

这与我们之前使用的函数类型相同。唯一的区别是我们在这类函数中使用的语法。当常规函数中只有一行代码块时，我们主要在 Dart 中使用箭头函数。

让我们看一个例子。

```
void main() {
  int result = add(5, 6);
  print(result);
}// regular function
int add(n1, n2){
  return n1 + n2;
}// arrow function
int add(n1, n2) => n1 + n2;//output: 11
```

# 结论

**恭喜你！**您已经成功完成了熟悉 Dart 功能的分步指南。对于那些想学习 Dart 编程语言的人来说，这实际上是一个初学者友好的教程。

感谢您的阅读。