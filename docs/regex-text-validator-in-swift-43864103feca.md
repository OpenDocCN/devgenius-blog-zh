# Swift 中的正则表达式文本验证器

> 原文：<https://blog.devgenius.io/regex-text-validator-in-swift-43864103feca?source=collection_archive---------4----------------------->

## 在 Swift 中为您的项目创建一个可重用的正则表达式文本验证器管理器

![](img/8197b16988a90fb7c443de7813b1be1c.png)

几乎每个大中型应用程序都需要用户提供一些信息。通常，提供它们最直接的方式是通过任何类型的输入文本字段。但是我们如何保证用户输入的文本是正确的呢？也许我们用数字的形式询问用户的年龄，而他/她用一个简单的字符串发来了年龄信息……或者用户输入了完全错误的信息，比如“🥳”🤗👌".在这种情况下，我们需要验证我们的测试。

验证用户输入最常见的方式是使用一个**正则表达式**，也称为 regex。

让我们从维基百科的一些定义开始:

> 一个**正则表达式**(简称为 **regex** 或**regexp**；也称为**有理表达式**是一系列[字符](https://en.wikipedia.org/wiki/Character_(computing))，指定了[文本](https://en.wikipedia.org/wiki/String_(computer_science))中的 [*搜索模式*](https://en.wikipedia.org/wiki/Pattern_matching) 。通常这样的模式被[字符串搜索算法](https://en.wikipedia.org/wiki/String-searching_algorithm)用于对[字符串](https://en.wikipedia.org/wiki/String_(computer_science))的“查找”或“查找和替换”操作，或者用于输入验证。它是在[理论计算机科学](https://en.wikipedia.org/wiki/Theoretical_computer_science)和[形式语言](https://en.wikipedia.org/wiki/Formal_language)理论中发展起来的技术。

基本上，它是一个字符串，有自己的规则提供给函数，用来分析输入文本并检查它是否与 regex 本身的规则相匹配。

如果你想学习如何创建一个正则表达式，看一看我最近的一篇文章，在那里我向你展示了一个非常方便的在线工具。

该框架附带了一系列处理正则表达式的方法，因此验证文本非常容易。只需 3 个步骤即可完成:

*   用您想要应用的正则表达式模式创建一个`NSRegularExpression`对象。
*   用你想要验证的字符串文本创建一个`NSRange`……你甚至可以只验证你输入文本的一部分，也就是范围。
*   如果文本中有一个`firstMatch`，则使用正则表达式结果验证文本。

超级简单！

第一个简单的解决方案是为测试创建一个函数:

这个功能可以用…但是有点笨拙。为了验证一个文本，我们需要以类似的方式调用它:

```
**let** isValid = validate(text: “UserName”, with: #”^[A-Za-z]+$”#)
```

请注意，我使用了`#"text here"#`字符串文字符号。有了正则表达式，避免写转义字符就变得非常方便了。

在我们的项目中反复调用前面的函数不是很干净。我们可以做一些改进。

我首先想到的是把功能改造成一个`String Extension`。这很明显，因为我们需要验证的所有文本都是一个字符串。让我们现在就开始吧:

嗯，现在情况好多了。现在让我们来看看我们的调用函数:

```
**let** isValid = “UserName”.isValidWith(regex: #”^[A-Za-z]+$”#)
```

哇！更加清晰，可读性更强！

非常好。这可能足够了，但我们仍有改进的空间。这个解决方案很好，但是我认为这里有两个问题。

如果你正在做一个大项目，你将需要在不同的部分反复执行相同的验证。如果每次需要时都编写正则表达式模式，最终会出现问题和所谓的技术债务。如果你在你的应用的七个点中验证一个用户名，而业务部门决定改变验证规则，该怎么办？也许现在允许空格？我向你保证，你不会改变所有七个地方的正则表达式，你会发现一个错误…可能在一个月后的产品中被发现。可怕的事情！

第二个问题是可读性…你能告诉我这个正则表达式是干什么用的吗？

```
**let** isValid = “Username”.isValidWith(regex: #”^(?:[A-Z][AEIOU][AEIOUX]|[B-DF-HJ-NP-TV-Z]{2}[A-Z]){2}(?:[\dLMNP-V]{2}(?:[A-EHLMPR-T](?:[04LQ][1–9MNP-V]|[15MR][\dLMNP-V]|[26NS][0–8LMNP-U])|[DHPS][37PT][0L]|[ACELMRT][37PT][01LM]|[AC-EHLMPR-T][26NS][9V])|(?:[02468LNQSU][048LQU]|[13579MPRTV][26NS])B[26NS][9V])(?:[A-MZ][1–9MNP-V][\dLMNP-V]{2}|[A-M][0L](?:[1–9MNP-V][\dLMNP-V]|[0L][1–9MNP-V]))[A-Z]$”#)
```

如果答案是肯定的…那么，你不需要这篇文章，我很抱歉地说，你已经浪费了你生命中的三分钟😁…不要再读了😉。

幸运的是 Swift 给了我们一个非常干净的解决方案…我们可以使用`enum`案例！

让我们在`String Extension`内部构建一个完整的管理器:

如您所见，我已经创建了一些示例来展示这种方法。我们一行一行的分析吧。

*   第 4 行:我在`Foundation String`中创建了一个嵌套类型。看一看[这篇文章](https://medium.com/@alessandromanilii/nested-object-in-swift-4cda290bfa18)中的这个技巧。
*   第 5 行:我创建了一个`none`规则……以防万一，你永远不会知道。
*   第 8–9 行:为了给规则更多的灵活性，我在枚举中使用了一个`associated value`。在应用程序中，文本输入有不同的字符串限制是很常见的。有了这个小变量，我们就万事俱备了。
*   第 14 行:这里我创建了一个返回正则表达式代码的`private var pattern: String`。将它包装在一个`enum`中使得一切都非常可读。
*   第 40 行:我们之前的函数做了一点修改(见第 43 行)。

请记住在枚举的每种情况下添加小的注释，以便澄清一切。编写干净的代码通常是我们的主要目标。

现在让我们看看我们的代码可读性如何:

```
**let** isValid = “Username”.isValidWith(regexType: .minLetters(8))
**let** isValid = "appleseed@apple.com".isValidWith(regexType: .email)
```

不仅如此，现在正则表达式的所有模式都在代码的一个地方。都在一起，写一次。多神奇啊！

如果需要额外的正则表达式，只需在枚举中添加一个 case 即可。超级容易，超级快！

我希望你喜欢这篇文章，如果你喜欢，请鼓掌。
如果这篇文章对你有用，请随意[给我一杯咖啡](https://www.buymeacoffee.com/dy59tqxn794)，并允许我创造更多酷的内容和文章。

享受你的编码！