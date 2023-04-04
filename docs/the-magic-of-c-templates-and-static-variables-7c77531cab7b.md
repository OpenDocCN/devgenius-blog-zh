# C++模板和静态变量的魔力

> 原文：<https://blog.devgenius.io/the-magic-of-c-templates-and-static-variables-7c77531cab7b?source=collection_archive---------0----------------------->

# 释放函数模板的力量

泛型编程越来越受欢迎，Golang 最终计划在其 1.18 版本中使用泛型，因为它是 Go 开发者多年来最渴望的特性。通过允许您的算法跨数据类型使用，您可以最大限度地减少代码重复并节省时间。

C++以其编译时优化而闻名(*不幸的是，这也是为什么 C++程序需要一段时间来编译！*)。泛型的编译时检查至关重要，难道 C++和泛型不是完美的一对吗？

![](img/13f9d3b409727aefbe608bc902833abd.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[安舒 A](https://unsplash.com/@anshu18?utm_source=medium&utm_medium=referral) 的照片

在 C++中，这种功能是通过使用 ***模板*** 来提供的。模板和泛型有细微的区别，模板提供了额外的功能，这将在后面讨论。

在这篇文章中，我想重点讨论一下函数模板的使用:静态变量。

假设你有一个算法类。对于那个类，你有几个子类:贪婪、递归和暴力。对于每一个，您可能有一个类似的静态变量，例如一个列表列表，其中包含算法的每个输入的每种可能的类型。现在，您需要测试所有类型组合是否正常工作。你怎么能这样做？

嗯，静态变量属于派生类，**而不是**基本算法类，因为它只在贪婪算法、递归算法和蛮力算法中共享，而不是在所有算法中共享。这就是模板派上用场的地方！请参见下面的示例代码:

```
template <class ChosenAlgorithm>
std::vector<std::vector<Type>>
GetValidTypes()
{
  std::vector<std::vector<Type>> type_combinations = ChosenAlgorithm::supported_types_;
  std::vector<std::vector<Type>> valid_combinations;
  for(auto type_list: supported_types){
    if(ChosenAlgorithm::AreValidTypes(type_list))
      valid_combinations.emplace_back(type_list); return valid_combinations;}
```

如果你仔细观察，你会注意到我们不仅使用了静态成员变量 supported_types_ 而且还使用了静态成员函数 AreValidTypes。有什么神奇的*(还有危险！)*关于模板是没有强制多态性的。您可以用任何类调用 GetValidTypes，编译器将检查该类是否具有使用模板所需的所有成员。

```
auto types = GetValidTypes<Greedy>();
```

额外的好处是，您可以让*模板参数*成为函数的一个这样的参数，以确定您正在使用哪个类！然后，您不必手动指定模板参数。

```
template <class ChosenAlgorithm>
std::vector<std::vector<Type>>
GetValidTypes(ChosenAlgorithm algo)
{
  ...
}
...//Call elsewhere
Recursive a = new Recursive();
auto types = GetValidTypes(a);
```

# 模板与泛型

C++中没有泛型，不像 Java 这样的语言有泛型类型。C++使用模板来实现泛型编程。模板和一般的泛型有什么不同？

在 C++中，编译器为您使用的每组唯一的模板参数生成一个单独的模板。这可能导致代码膨胀。同时，**编译器只生成需要的成员和函数**，链接器尽可能减少代码重用。这种复制粘贴方法会导致可靠的编译时检查，因此如果您在模板中使用的任何类缺少所需的函数或变量，您都会收到一个错误。

有了这种编译时检查，在使用模板时就不那么需要类限制了！另外，这种**检查意味着如果你创建多个模板，编译器会找到与所需函数/变量**匹配的模板。

这意味着你*可以重载你的模板*并且*编译器会计算出哪个*有意义！例如，假设我们编写了下面两个模板。如果您用一个没有 AreValidTypes 的类调用 GetValidTypes()，编译器将使用第二个模板。

```
template <class ChosenAlgorithm>
std::vector<std::vector<Type>>
GetValidTypes()
{
  std::vector<std::vector<Type>> type_combinations = ChosenAlgorithm::supported_types_;
  std::vector<std::vector<Type>> valid_combinations;
  for(auto type_list: supported_types){
    if(ChosenAlgorithm::AreValidTypes(type_list))
      valid_combinations.emplace_back(type_list); return valid_combinations;}template <class ChosenAlgorithm>
std::vector<std::vector<Type>>
GetValidTypes()
{
  return ChosenAlgorithm::supported_types_;
}
```

你可以看到危险，因为你通常想要明确。然而，这也创造了简化的机会。

C++ **模板为大量代码重用**打开了大门，在可能的情况下优化我们的程序，使其保持简洁。这篇文章有望打开 C++模板的使用之门，从类模板到模板特殊化等等*还有更多有待发现！*

![](img/ed564ca36c4df573b16e423155aaaf26.png)

[Hennie Stander](https://unsplash.com/@henniestander?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照