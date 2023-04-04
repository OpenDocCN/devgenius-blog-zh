# 正则表达式括号:每种类型的例子

> 原文：<https://blog.devgenius.io/regex-parentheses-examples-of-every-type-aba8441be761?source=collection_archive---------2----------------------->

![](img/81fc2f2490ea642be355920693b79bf5.png)

我最近遇到了一个编码问题，这个问题包括字符串中的括号，我的目标是替换括号和括号内的所有内容。我的第一个想法是——想办法使用正则表达式！

我很快意识到，我对 regex 中的括号并没有自己想象的那么了解。所以，我想分享一下我在正则表达式中括号的不同用法中的发现会很不错。原来，有三种不同的类型！

首先，3 种不同类型的括号是**文字型**、**捕捉型**和**非捕捉型**。

如果您以前使用过正则表达式，您很可能至少熟悉文字括号，甚至可能熟悉捕获括号。但是，如果你和我一样，你可能没有听说过正则表达式中的非捕捉括号。这篇短文将讨论每一种括号，并将分解例子来加深我们的理解。

# 逐字的

这个听起来有点像，我们想要字面上匹配字符串中使用的括号。由于括号也用于捕获和非捕获组，我们必须用反斜杠来转义左括号。

假设给我们一本书的文本，我们想找到作者每次在括号里放东西的地方，包括括号本身。正则表达式可能类似于:

```
const literalRegex = /\([^()]+\)/g
```

然后使用 Lorem Ipsum 的摘录，在 3 个地方插入括号，我们可以用`.match()`方法测试我们的正则表达式，并检索所有使用的括号测试短语:

```
const book = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam ac purus dignissim, imperdiet dui sed, dignissim nunc. Sed faucibus turpis nec ultricies tempus. Integer scelerisque neque nisi (test1), at volutpat augue malesuada ut. Nam id venenatis urna. Suspendisse potenti. Aliquam id sem sem. Suspendisse mollis nulla eu ex tempor, et tincidunt risus condimentum. In non felis est. In ut tortor eget elit faucibus volutpat eget (test2) vulputate quam. Sed erat ex, consequat sed sapien vitae, porta pellentesque nulla. Etiam molestie libero sed lacus (test3) feugiat, hendrerit tempus eros interdum. Ut efficitur feugiat nunc, nec mattis risus ornare eget. Phasellus quis malesuada diam. Quisque."const allMatches = book.match(literalRegex)console.log(allMatches) //["(test1)", "(test2)", "(test3)"]
```

关于`literalRegex`如何工作的解释:

> `/` —打开或开始正则表达式。
> `\(` —转义单个左括号文字。
> `[^()]`—任何不是 ( `^`)左括号或右括号(`(`或`)`)的字符。括号代表一个字符类，当它与左括号(`[^`)后面的脱字符结合时，它对里面的字符求反——更多信息见[此处](https://www.regular-expressions.info/charclass.html)。
> + —直接出现在它前面的一个或多个字符(在这种情况下是上面的整个字符类)
> `\)` —转义一个右括号文字。
> `/` —关闭正则表达式。
> `g` —全局标志允许我们返回数组中的每一个匹配，而不仅仅是数组中的第一个匹配。

# 占领

这些括号用于将字符组合在一起，从而“捕获”这些组，以便它们可以通过反向引用重用，或者被赋予一个量词，如`+`或`*`。它们不与括号字面匹配，只与括号内的字符匹配，所以这些字符不需要用反斜杠进行转义，相反，我们将把它作为一个“元字符”来使用。

## 捕捉和使用量词

这里有一个例子，我们想在由所有的 A，T，C 和 G 组成的 DNA 序列中重复使用相同的模式。我们的目标是在每次使用“CAT”模式时返回一个数组。如果模式后面紧跟着完全相同的模式，它将延长放入数组的字符串:

```
const capturingRegex = /(CAT)+/g
```

再次使用`.match()`方法将返回一个数组，其中包含在字符串中找到的每个实例:

```
const dnaSequence = "GATCGATCATCATCATGGTATAGATGCTGATATGATCGCATCATTCGTAGTCGTGACCATCATCATCATCATCATGATGCGGATGTTATAGTAGTAGTCGGCGATGTAGCTGGATCGACATCATCATCATTGCTAGTCGTCATCATGCTAGTCGTAGCTGCATCATCTAGCATATACTTCGCCGCGTAATTATCGCCCATTT"const catMatches = dnaSequence.match(capturingRegex)console.log(catMatches) // ["CATCATCAT", "CATCAT", "CATCATCATCATCATCAT", "CATCATCATCAT", "CATCAT", "CATCAT", "CAT", "CAT"]
```

我不确定这个人的高“猫”数是怎么回事，但希望它证明了括号中模式的可重复性。

`capturingRegex`故障:

> `/` —打开或开始正则表达式。
> `(CAT)` —图案为“猫”的捕捉组。
> `+` —一个或多个直接出现在它前面的任何东西，在这种情况下，它是捕获组而不是单个字符。
> `/` —关闭正则表达式。
> g—全局标志允许我们返回数组中的每一个匹配项，而不仅仅是数组中的第一个匹配项。

## 捕获和使用反向引用

如果我们需要稍后在正则表达式中重用一个捕获组，而不是像上一个例子那样紧挨着原始模式，我们也可以使用反向引用。

在这里，我们将使用它来查找初始模式“TAG”或“TAT ”,然后，在“G”之后，再次查找在初始捕获组中找到的任何模式。换句话说，我们要寻找的模式要么是“TAGGTAG”，要么是“TATGTAT”:

```
const newCapturingRegex = /(TAG|TAT)G\1/gconst newSequence = "TAGGTAGCCCCCCCCCTATGTATCCCCCCCCCCCTAGGTAGCCCCTAGGTAGTAGGTAGTATGTATTATGTATCCCCCCCCCCCCCCTATGTATTATGTATTATGTATCCCCCCCCCTAGGTAGTAGGTAGCCC"const dnaMatches = newSequence.match(newCapturingRegex)console.log(dnaMatches) //["TAGGTAG", "TATGTAT", "TAGGTAG", "TAGGTAG", "TAGGTAG", "TATGTAT", "TATGTAT", "TATGTAT", "TATGTAT", "TATGTAT", "TAGGTAG", "TAGGTAG"]
```

`newCapturingRegex`故障:

> `/` —打开或开始正则表达式。
> `(TAG|TAT)` —与“TAG”或“TAT”匹配的捕获组。
> `G` —字母“G”的字符串文字匹配。
> `\1` —反向引用，引用第一个捕获组(本例中唯一的捕获组)。如果正则表达式中使用了另一个捕获组，那么可以使用递增的反向引用来重用它，就像这样— `\2`。
> `/` —关闭正则表达式。
> `g` —全局标志允许我们返回数组中的每个匹配项，而不仅仅是数组中的第一个匹配项。

## 从正则表达式中移除了 G 标志的匹配方法

如果我们从正则表达式的末尾移除`g`标志，我们得到的不是数组中的每一个匹配，而是一个不同类型的数组。正则表达式找到的第一个匹配是新数组中的索引`[0]`,之后的每个索引将是在捕获组中捕获的内容，这些捕获组按照在正则表达式中找到它们的顺序使用。

我们还可以使用点符号来查找匹配中第一个字符的索引(`.index`)，也可以查找初始字符串本身(`.input`)。让我们看一个例子:

```
const noFlagRegex = /(foo).*(bar)/const fooBarString = "A foo walked into a bar..."const fooBarMatch = fooBarString.match(noFlagRegex)console.log(fooBarMatch[0]) // "foo walked into a bar"
console.log(fooBarMatch[1]) // "foo"
console.log(fooBarMatch[2]) // "bar"
console.log(fooBarMatch.index) // 2
console.log(fooBarMatch.input) // "A foo walked into a bar..."
```

`noFlagRegex`的细目供感兴趣者参考:

> `/` —打开或开始正则表达式。
> `(foo)` —包含模式“foo”的捕获组。
> `.` —代表通配符的元字符，任何字符都可以。
> `*` —之前的 0 个或更多(在本例中为`.`)。
> `(bar)` —包含模式“foo”的捕获组。
> `/` —关闭正则表达式。

# 非捕获

这是我个人在研究它们之前一无所知的括号类型，我认为对于像我一样的其他正则表达式初学者来说也是如此。非捕获组本质上与捕获组做同样的事情，除了，听起来，我们不“捕获”括号之间的模式。

如果我们不添加`g`标志，而使用`.match`方法，我们将得到一个类似上面例子中的数组，但是我们没有得到索引`[1]`处的捕获组，等等，只是得到索引`[0]`处的完全匹配，并且我们仍然能够使用类似上面例子中的`.index`和`.input`。

非捕获组以`(?:`开始，右括号之前的冒号后面的内容被分组，但不会被捕获并存储在相应的数组中。让我们来看一个例子，在这个例子中，我们检查以确保电话联系人是这样的形式:mr/ms/mrs first last(XXX)XXX-xxxx，我们希望电话号码的区号是唯一的捕获组(在索引`[1]`中找到)，但是我们还需要将 Mr/ms/Mrs 分组在一起。

```
const contactRegex = /(?:mr|ms|mrs) \w+ \w+ \((\d{3})\) \d{3}-\d{4}/const contactInfo = "mr Tyler Funk (555) 888-7777"const contactMatch = contactInfo.match(contactRegex)console.log(contactMatch[0]) // "mr Tyler Funk (555) 888-7777"
console.log(contactMatch[1]) // 555
console.log(contactMatch.index) // 0 
console.log(contactMatch.input) // "mr Tyler Funk (555) 888-7777"
```

这个例子中的索引`[0]`和`.index`在这里并不那么有用，因为在这种情况下，完全匹配是整个字符串本身，这自然会成为匹配的第一个索引`0`。然而，我们没有捕获用于 mr/ms/mrs 的第一个组，而是捕获了索引`[1]`中电话号码的区号。

`contactRegex`的细目:

> `*/*` —打开或开始正则表达式。
> `(?:mr|ms|mrs)` —非捕获组，查找模式“mr”或“ms”或“mrs”，后跟一个空格。
> `\w+` —查找任何“单词”字符的元字符(`\w` —字母)，后跟查找其前面一个或多个任意字符的量词(`+`，以及一个空格。(名字和姓氏都用了两次)
> `\(` —左括号文字。
> `(\d{3})` —精确查找 3 ( `{3}`)个数字(`\d`)的捕获组。
> `\)` —右括号文字，后跟一个空格。
> `\d{3}` —同样，正好是 3 个数字(也称为数字字符)。
> `-` —连字符。
> `\d{}` —正好 4 个数字。
> `/` —关闭正则表达式。

# 结论

这就是正则表达式括号的主旨！我希望这在某种程度上对你有所启发，值得你花时间。以下是我用来研究这个话题的一些资源:

—一篇非常棒的文章，它帮助我理解了有 3 种不同的括号— [这里是](https://unbounded.systems/blog/3-kinds-of-parentheses-are-you-a-regex-master/)。阅读“但是等等，还有更多！”底部部分！

—一个优秀的常规正则表达式资源，但我发现一个特定的页面对于更好地理解特殊字符/元字符很有用— [这里是](https://www.regular-expressions.info/characters.html)。

祝大家黑客快乐！