# Swift 中的下标

> 原文：<https://blog.devgenius.io/subscripts-in-swift-dbcdb65233b7?source=collection_archive---------9----------------------->

类、结构和枚举可以定义下标，下标是访问集合、列表或序列的成员元素的快捷方式。

![](img/ff96d4d74e69088e201179535bfb3ef0.png)

照片由 [Unsplash](https://unsplash.com/s/photos/syntatic-sugar-code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 [Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

我将解释下面列出的一些关于下标的问题。

*Swift 中的下标是什么？*
*Swift 中如何使用下标？
Swift 中的多维下标是什么？*

类、结构和枚举可以定义*下标*，这是访问集合、列表或序列的成员元素的快捷方式。

使用下标按索引设置和检索值，而不需要单独的设置和检索方法。例如，访问数组实例中的元素作为 someArray[index]，访问字典实例中的元素作为 someDictionary[key]。

您可以为单个类型定义多个下标，并根据传递给下标的索引值的类型来选择要使用的适当下标重载。

下标不限于单个维度，您可以使用多个输入参数来定义下标，以满足您的定制类型的需要。

让我们创建一个代码示例。我将创建一个负责用户首选项的 UserPreferences 类。

UserPreferences 类有两个方法和一个数组。其中之一就是保留给予偏好。另一个是通过首选项类型获取首选项值。

```
enum UserPreferencesType {
case username,email
}class UserPreferences {

  static let shared = UserPreferences() private var preferences: [UserPreferencesType: Any] = [:] func preference(for type: UserPreferencesType) -> Any? {
    preferences[type]
  } func preference(_ value: Any, for type: UserPreferencesType) {
    preferences[type] = value
  }}
```

让我们通过下面的调用保存一个用户偏好。

```
UserPreferences.shared.preference(“batikansosun”, for: .username)
UserPreferences.shared.preference(“[batikansosun@example.com](mailto:batikansosun@example.com)”, for: .email)
```

然后像下面的调用那样按首选项类型获取任何首选项值。

```
UserPreferences.shared.preference(for: .username) //batikansosun
UserPreferences.shared.preference(for: .email) //batikansosun@example.com
```

获取一个首选项值，并设置一个首选项值，这样可读性会更好一些。我们如何解决这个问题？
首先，我们可以像我下面提到的那样定义一个下标方法。

```
subscript(type: UserPreferencesType) -> Any? {
  get { preferences[type] }
  set { preferences[type] = newValue }
}
```

然后调用如下所示的方法。

```
UserPreferences.shared[.username] = “batikansosun”
UserPreferences.shared[.username] //batikansosun
```

我仍然认为我们的类需要更多的可读性。我将把共享实例移到下标方法中。所以我需要定义一个静态下标方法。

```
static subscript(type: UserPreferencesType) -> Any? {
  get { shared.preferences[type] }
  set { shared.preferences[type] = newValue }
}
```

静态前缀将使我免于使用共享实例。终于，我们班有了新的看法。

```
class UserPreferences {

  static let shared = UserPreferences() private var preferences: [UserPreferencesType: Any] = [:] func preference(for type: UserPreferencesType) -> Any? {
    preferences[type]
  } func preference(_ value: Any, for type: UserPreferencesType) {
    preferences[type] = value
  }

  static subscript(type: UserPreferencesType) -> Any? {
   get { shared.preferences[type] }
   set { shared.preferences[type] = newValue }
  }}
```

现在，按照下面的调用，通过首选项类型获取任何首选项值。

```
UserPreferences[.username] = “batikansosun”
UserPreferences[.username] //batikansosun
```

我认为这段代码是一件伟大的事情。你同意我的观点吗？

最后可能得出结论:
Swift 语言有很多特性，提供了很多编程的方式。下标就是其中之一。
下标提供句法糖。
下标减少开发时间。
下标提供了一目了然的理解代码。这样使事情更容易阅读或表达。

ref:[https://docs . swift . org/swift-book/language guide/subscripts . html](https://docs.swift.org/swift-book/LanguageGuide/Subscripts.html)