# 如何在不破坏应用程序的情况下，以编程方式强制更改应用程序语言

> 原文：<https://blog.devgenius.io/how-to-force-change-app-language-programmatically-without-breaking-your-app-1c73be9608e0?source=collection_archive---------0----------------------->

## 鉴于你的应用程序支持多种本地化，当我们在 iOS 13 和 14 中强制设置`UserDefaults` 键“AppleLanguage”及其副作用时，我们也应该小心

![](img/9101bf8c673254a066689609eca1ceff.png)

照片由[格雷格·罗森克](https://unsplash.com/@greg_rosenke?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 设置应用程序语言的方法数量

1.  设置`UserDefaults`键**苹果语言**(单数)
2.  设置`UserDefaults`键 **AppleLanguages** (复数) **(Apple PreferredLanguages 数组)**
3.  创建自定义`AppLanguageManager`

# 安装

在我们开始研究设置应用程序语言的每种方法之前，我们需要一些助手函数，这些函数在所有这些方法中都是相同的。

> 注意:这个扩展只是为了这个演示，在 iOS 社区中你会发现很多不错的助手扩展

*   **捆绑扩展**

# 1.设置`UserDefaults`键**苹果语言**

```
let lang = "th"
let defaults = UserDefaults.standard
defaults.set(lang, forKey: "AppleLanguage")
defaults.synchronize()
Bundle.setLanguage(lang)
// Restart your app by setting window rootViewcontroller
```

# 优势

1.  没有副作用。
2.  你可以在你的应用程序中添加功能，让用户选择应用程序语言，而不是改变系统语言，然后看到结果，这对用户来说是一个不错的 UX。

# 不足之处

1.  每次启动应用程序时，您必须设置最后一个应用程序语言代码以保存语言设置，为此，您必须将语言代码保存在另一个`UserDefaults`键中。
2.  您的应用程序不会改变或检测到操作系统或系统语言的变化，但让我们从用户的角度考虑这是不太可能发生的。

# 2.设置`UserDefaults`键“**苹果语言** s”(首选语言)

还有其他几种类似的方法来强制设置应用程序语言，让我们考虑一下 basic one。此外，本文的目的是了解这样做的副作用。

当你使用下面这段代码时，你的应用语言会发生变化，在这种情况下是泰语。

```
let lang = "th"
let defaults = UserDefaults.standard
defaults.set([lang], forKey: "AppleLanguages")
defaults.synchronize()
Bundle.setLanguage(lang)
// Restart your app by setting window rootViewcontroller
```

# 优势

1.  您没有在应用启动时设置语言代码
2.  你不需要像我们在前一个键中那样在其他键中保存语言代码
3.  你可以在你的应用程序中添加功能，让用户选择应用程序语言，而不是改变系统语言，然后看到结果，这对用户来说是一个不错的 UX。

# 不足之处

以下是一些主要问题

1.  您的应用程序不会改变或检测到操作系统或系统语言的变化，但让我们从用户的角度考虑这是不太可能发生的。
2.  严重的日期格式副作用。(

**这一点很重要，假设你的应用程序使用`DateFormatter`将日期从字符串转换过来，这就出现了一个大问题，因为现在事情开始变得奇怪了**

**假设您将应用程序语言设置为" **th** "( **泰语**)，当前系统日历设置为"**公历"**"，现在您正在尝试一个简单的任务，用`DateFormatter`从字符串中获取日期，让我们看看输出**

> **注意:你会发现泰国的用户同时使用“公历”和“佛教历”**

```
let format = DateFormatter()
print(format.date(from: "2020-11-22"))// Output
1477-11-22 // Weird
```

**我不确定为什么会给出错误的日历，因为这个苹果 API 的结果因操作系统而异。但这是促使我思考深入这个问题的主要原因。**

**以下是打印以下属性时的输出**

1.  **`Locale.current`它给你一个 **th** 作为标识符**
2.  **`Calendar.current.dictionary?[“identifier”]`它给了你**佛教**，即使你用的是**公历**日历**

**现在你可能会说，我们可以设置`DateFormatter`的区域设置、时区和其他属性，并获得所需的输出，但从用户的角度来看，使用系统区域设置、日历和其他属性，而不是硬编码这些属性，这是一个很好的实践，而且如果你的应用程序针对使用不止一个日历的国家进行了本地化，这也是一个巨大的副作用。**

# **3.创建自定义`AppLanguageManager`**

**为此，我们需要以下东西**

1.  **`AppLanguageManager`维护当前语言和包路径的类。**
2.  **在`UserDefaults`中设置语言代码，以保留上次保存的语言，当应用程序重启或重新启动时，我们会显示更新的语言**
3.  **获取当前语言本地化字符串的帮助函数**

# **我们开始吧**

1.  **下面是`AppLanguageManager`代码，它将保持当前语言和包路径，它也有助于在`UserDefaults`中设置语言代码，以保留上次保存的语言。**

**2.获取本地化字符串的助手`String`扩展**

```
extension String {
        fileprivate func localized(bundle: Bundle = AppLanguageManager.shared.bundle, tableName: String = "Localizable") -> String {
        return NSLocalizedString(self,
                                 tableName: tableName,
                                 bundle: bundle,
                                 comment: "")
    }
}
```

**3.助手`Localizable` propertyWrapper 和`Strings` enum**

```
@propertyWrapper
struct Localizable {
 private let key: String
 var wrappedValue: String {
      return key.localized()
 } init(wrappedValue: String) {
   key = wrappedValue
 }
}enum Strings {                         
  @Localizable static var featureOneTitle = "featureOneTitle"                               
  @Localizable static var featureTwoTitle = "featureTwoTitle"                       }
```

**这一切帮助我们消除了对 AppleLanguage `UserDefaults`键的依赖，并且更加灵活和可伸缩，没有任何副作用。**

# **最终产量和用途**

```
print("\(Strings.featureOneTitle)")
// Output in current app language set in this case english
featureOneTitle
```

# ****结论****

**我相信，如果我们试图用黑客来解决问题，总会有不利的一面。**

**设置`UserDefaults`**“apple language”**和**apple language**键对我来说感觉不太对，在最近的 iOS 版本中也有自己的缺点。**

**此外，如果您使用`UserDefaults` **“苹果语言”**和“**苹果语言**”键在您的应用程序中实现了语言更改功能，如果您发现除了**日期**之外的任何其他问题，请在本文回复部分告诉我。**

**感谢您的阅读。希望这篇文章能帮助你制作自己的定制语言管理器。**