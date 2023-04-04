# Swift 中“可编码”的本质

> 原文：<https://blog.devgenius.io/essential-of-codable-in-swift-18c83718d010?source=collection_archive---------0----------------------->

![](img/8ab15d357bd4165385fba2e1814b824a.png)

iOS 应用程序开发人员几乎每天都与`JSON`打交道，因为大多数 iOS 应用程序都与远程 API 通信。

我知道你们中的大多数人会和`JSONSerialization`一起处理`Encoding`，和`Decoding`一起处理`JSON`的数据。

我看到一些开发人员正在编写映射器方法来将他们的`JSON`映射到`Model`。

```
struct User {
  let id: Int
  let name: String

  init?(dictionary: [String: Any]) {
    guard let id = dictionary["id"] as? Int else { return nil }
    guard let name = dictionary["user_name"] as? String else { return nil }

    self.id = id
    self.name = name
  }
}
```

这不是一种将`JSON`映射到`Model`的便捷方式，而且还需要编写大量样板代码，这一点也不好玩！

**Swift 4** 引入`Codable`帮助你轻松完成`Encoding`和`Decoding.`

**可编码的**就是`typealias`，它是由`Encodable`和`Decodable`两个协议组成

```
public typealias Codable = Decodable & Encodable
```

**可解码**是一个协议，顾名思义就是要求`Decode`你的`JSON` 到你的`Model`。

```
public protocol Decodable {
    init(from decoder: Decoder) throws
}
```

**可编码**是一个协议，需要`Encode`你的`Model`到`JSON`。

```
public protocol Encodable {
    func encode(to encoder: Encoder) throws
}
```

在本教程中，我将向你展示如何使用`**Codable**`到`Encode`和`Decode`到`JSONs`和从`Model`来回。

我相信在这个故事的结尾，你会喜欢使用`Codable`(如果你还没有使用它的话)。因为它需要更少的样板代码，并提高了代码的可读性。

当**解码** `JSON`到`Model`时，我会经历一些我已经遇到的场景，这最终会帮助你在遇到这种情况时很好地处理它。

## 1.简单使用代码

在处理简单的`JSON`数据时，使用`Codable`是如此简单。

您只需确认`type`的`Codable`协议。你需要确保你的`JSON`中的`key`与你的`type.`中的`Property`名完全匹配

它将`Decode`你的`JSON`到`Country`，并打印下面的结果。

```
Country name = United States of America
Country capital = Washington, D.C.
Country sub region = Northern America
Country borders = ["CAN", "MEX"]
Country population = 323947000.0
```

## 2.在您的`Model`中重命名属性

很明显，我们不会总是喜欢相同的*属性*名称，因为我们在`JSON`中进行检索。

`Codable`提供了一种方便的方法，可以在您选择的模型中保存属性名，并将其从`JSON`映射到右侧`key`。

您的`type`需要实现`enum CodingKeys: String, CodingKey`,在这个枚举中，您可以提到正确的`key`来映射您重命名的属性，然后`Codable`就会为您完成。

它会将`altSpellings`键从`JSON`映射到`Country`的`alternateSpellings`。它将按如下方式打印。

```
Country alternate spellings: ["US", "USA", "United States of America"]
```

## 3.将 snake-case JSON 键转换为模型中的 camelCase 属性

有时我们会从远程 API 接收到`JSON`，其中包含写在`snake-case`中的`key`名称

Swift 编码惯例建议使用`camelCase`属性名。

所以我们需要将我们的 snake-case JSON 映射到 camelCase 属性名。我们可以用`enum CodingKeys: String, CodingKey`实现，如**场景#2 中所建议的。**

然而，使用`keyDecodingStrategy`有一种快速简单的方法可以实现这一点。

你只需要将你的`keyDecodingStrategy`设置为`convertFromSnakeCase`，剩下的工作会自动完成。它会将你所有的`snake-case` JSON `key`转换成`camelCase`属性。

在本例中，`JSON`中的`calling_codes`将映射到`Country`中的`callingCodes`，并按如下所示打印输出。

```
Country calling codes = ["1"]
```

## 4.将日期字符串从 JSON 转换为 Swift 日期

我们经常在`JSON`中收到各种记录的`date`，通常，同一应用程序的所有远程 API 都使用相同的`dateFormat`。

然而，我们需要在所有需要使用`Date`的地方将`date(String)`从`JSON`转换成 Swift `Date`。

相反，您可以使用`dateDecodingStrategy`轻松地将`date(String)`转换为`Date`，而只使用`decoding` `JSON`。

你只需要将`dateDecodingStrategy`设置为`.formatted(DateFormatter())` ，它会为你做休息。您可以将`dateFormat`设置为`DateFormatter`的实例，然后您可以将相同的`dateFormatter`用于`dateDecodingStrategy`。

在本例中，它会将“1776–07–04”映射到`let independanceDay: Date`。它将输出如下。

```
Country Independance Day = 1776-07-04
```

## 5.只使用必需的属性，忽略 JSON 中的 rest

经常发生的情况是，您只需要 3-4 个字段，而您会从`JSON`收到许多不必要的`keys`。

您可以通过不在您的`Model`中声明来忽略来自`JSON`的不需要的属性。

在上面的例子中，我们在模型中只声明了 2 个属性，忽略了 rest。它将输出如下。

```
Country name = United States of America
Country capital = Washington, D.C.
```

如果你使用`enum CodingKeys: String, CodingKey`来同时重命名一个或多个属性，那么你可以忽略不需要的属性，如下所示。

上面的例子将只使用 big JSON 中的 3 个属性，同时它将把`JSON`中的`altSpellings`重命名为`Country`中的`alternateSpellings`。它将输出如下。

```
Country name = United States of America
Country capital = Washington, D.C.
Country alternate spellings = ["US", "USA", "United States of America"]
```

## 6.处理可选财产

显而易见，远程 API 会给我们发送一些可选的属性。

我们可以使用`Codable`将相应的属性声明为`optional.`来轻松解决这个问题

在上面的例子中，`phoneNumber`是一个可选的属性，无论`phoneNumber`是否出现，它都能很好地`decodes`出来。它将输出如下。

```
Eric phone number = +1 12312312
Ronaldo phone number = Not Available
```

## 7.处理深度嵌套的 JSON

当使用复杂的远程 API 或任何公共的远程 API 时，您会得到嵌套很深的 JSON 作为响应，它有很多您不需要使用的属性。

嗯，而`decoding`比如`JSON`，你需要通过实现`Decodable`的 `init(from decoder: Decoder) throws` 方法来编写一些自定义实现。

在这个方法中，你可以从`decoder`中查询你感兴趣的`nestedContainer`，然后你可以从这个`nestedContainer`中查询你感兴趣的`property`。

在上面的例子中，我们用许多属性深度嵌套了 JSON，然而，我们只对`name`和`city`感兴趣。

虽然`name`很容易解码，但是`city`很难解码。

由于城市保存在`JSON`的最深处，我们需要从`decoder`查询`contactInformation`作为`nestedContainer`。然后，我们需要将`contactInformation`中的`mailingAddress`查询为`nestedContainer`。最后，我们可以从`contactInformation`查询`city`。

```
Employee name = Eric
Employee city = New York City
```

到目前为止，这就是我想分享给 Codable 的全部内容。

> **建议**:
> 
> 请在您的`Model`中仅声明基本属性。**不要**在你的`Model`中声明*非必要*属性，尽管它即将进入`JSON`。
> 
> 通过在你的`Model`中声明*非必要的*属性，你正在依赖那些你根本用不上的东西。
> 
> 如果您这样做了，如果远程 API 在 JSON 响应中更新/删除了那些不重要的属性，那么您将来会受到影响。这可以通过不在模型中声明来避免。

**提示:**你可以使用 [https://app.quicktype.io](https://app.quicktype.io) 将你的 JSON 转换成可编码模型。这将为您节省大量时间来声明所有属性和您的模型。

**我会不断更新这个故事，因为我遇到了使用 Codable 的不同场景和最佳实践。**

我希望你喜欢阅读这个故事，并发现它是有帮助的。如果您有任何问题/意见，请告诉我。