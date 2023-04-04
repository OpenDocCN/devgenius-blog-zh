# 掌握正则表达式[Regex]

> 原文：<https://blog.devgenius.io/getting-to-grips-with-regular-expressions-regex-eaaa25a3344b?source=collection_archive---------12----------------------->

![](img/f6c0398c4b10901a930efd197794bdbe.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Elisa Ventur](https://unsplash.com/@elisa_ventur?utm_source=medium&utm_medium=referral) 拍摄的照片

当我第一次尝试使用正则表达式(从这里开始称为 Regex)时，我可能看起来就像上面的图片一样…也许笔记本电脑也因为沮丧而戏剧性地关闭了几次！

学习 Regex 就像玩拼图游戏，每个部分看起来都一样。我理解每一个组件，但是把它们组合在一起需要大量的练习和耐心。然而，正则表达式构成了处理用户输入的基础部分，所以别无选择，只能继续战斗。

在这篇文章中，我将我所学到的东西进行了归类，试图帮助其他正在经历相同学习曲线的人…我还包括了一些帮助练习和测试你的知识的链接([如果你好奇就跳过去)](#5d65)。

# 什么是正则表达式？

正则表达式允许在各种输入上执行文本验证，而不必显式地指定这些输入…我知道这个定义听起来令人困惑，但我保证，通过一个例子，它会更有意义！

以一个要求用户输入电子邮件地址的表单为例。为了帮助理解输入是否有效，我们可以检查它是否包含一个“@”符号。然而，检查每个可能的电子邮件组合是不可能的，这就是正则表达式的用武之地。

# 使用正则表达式的实用性

在进入所涉及的特定表达式之前，让我们回顾一下如何在代码中应用正则表达式的一个非常基本的例子。

首先，用户在表单中输入内容，然后存储该值。然后将这个值与我们指定的正则表达式模式进行比较。如果输入符合标准，则验证成功，否则验证失败并被拒绝。

用代码表示，这看起来像下面的 if 逻辑(其中`$input`是用户输入，`$pattern`是我们定义的正则表达式。

```
//PHP syntax used here but logic applies to all coding languagesif ($input === $pattern) {
     return true; 
   } else {
     return false;
   }
```

# 创建表达式

对于下面的所有例子，变量`$pattern`将用于编写的正则表达式，而`$input`将用于我们想要验证的字符串。注释将指示输入是否匹配。

![](img/2b9a1146cc98e145edbc62f475eb7984.png)

照片由 [Antoine Dautry](https://unsplash.com/@antoine1003?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## **【aeiou】:匹配括号内的内容**

```
$pattern = "[bcd]";
$input1 = "cat";  //Matches the pattern 
$input2 = "mat";  //Does not match the pattern
```

## [A-Z]:A 和 Z 之间的所有字母

```
$pattern = "[A-Z]";
$input1 = "Cat";  //Matches the pattern
$input2 = "mat";  //Doesn't match (regex is case sensitive)
```

## [^b-g]:不是字母 b 到 g

```
$pattern = "[^t]";
$input1 = "abc";  //Matches the pattern 
$input2 = "ttt";  //Does not match the pattern
```

## \d:查找一个数字

```
$pattern = "[\d]";
$input1 = "f1nd";  //Matches the pattern 
$input2 = "find";  //Does not match the pattern
```

## \s:查找空白

```
$pattern = "[\s]";
$input1 = "cat and dog";  //Matches the pattern 
$input2 = "catanddog";  //Does not match the pattern
```

## \w:查找单词字符

```
$pattern = "[\w]";
$input1 = "cat!";  //Matches the pattern 
$input2 = "@@@";  //Does not match the pattern
```

## n* : 0 或 1+次

```
$pattern = "colo[u]r";
$input1 = "color";  //Matches the pattern 
$input2 = "colour";  //Also matches the pattern
```

## n+ : 1 次或更多次

```
$pattern = "[o]+";
$input1 = "cal";  //Doesn't match
$input2 = "col";  //Matches the pattern
$input3 = "cooool";  //Matches the pattern
```

## n？:0 或 1 次

```
$pattern = "c[a]?t";
$input1 = "cat";  //Matches the pattern 
$input2 = "caat";  //Does not match the pattern
```

## n{x}:出现 n 次

```
$pattern = "spe{2}d";
$input1 = "speed";  //Matches the pattern 
$input2 = "speeed";  //Does not match the pattern
```

## n{x，y}:发生在 x 和 y 时间之间，包括 x 和 y 时间

```
$pattern = "com{1,2}a";
$input1 = "coma";  //Matches the pattern 
$input2 = "comma";  //Matches the pattern
$input3 = "commmma"; //Does not match the pattern
```

## xxx$:表达式必须以要搜索的短语结尾

```
$pattern = "dogs$";
$input1 = "I like dogs";  //Matches the pattern 
$input2 = "I like dogs and cats";  //Does not match the pattern
```

## ^xxx:条目必须以被搜索的短语开始

```
$pattern = "^They";
$input1 = "They like dogs";  //Matches the pattern 
$input2 = "I like dogs";  //Does not match the pattern
```

## (x|y):输入可以包含 x 或 y

```
$pattern = "(cat|dog)";
$input1 = "I like cats";  //Matches the pattern 
$input2 = "I like dogs";  //Also matches the pattern
$input3 = "I like cows"; //Doesn't match the pattern
```

# 组合

上面的每个表达式都可以组合起来，让我们进一步指定我们想要查找的内容。例如，如果我们希望某人指定一个电子邮件地址，这将需要几个相关的标准:

1.  各种长度的字母、数字或“_”符号的选择
2.  “@”符号
3.  选择更多字母来表示电子邮件主机提供商
4.  一个“.”符号
5.  表示域的字符集合

```
$pattern = "[\w]*@[a-zA-z]*\.[a-zA-Z]*";
$input1 = "simply_stef@mail.com";  //Matches the pattern 
$input2 = "simply.mail.com";  //Does not match the pattern
```

*(我发现像上面那样分解表达式标准是理解发生了什么的最简单的方法，因为它创建的长表达式可能难以阅读)*

# 练习你的技能

虽然最初很难理解，但是理解正则表达式的最好方法是练习。如果你和我一样喜欢解决迷你谜题的挑战，并且通过谜题比看书学得更好，那么下面的链接可能会让你感兴趣。

**regexon**

本网站提供了上述所有规则的演练，并在每页底部提供了一个互动练习来测试您的理解:

 [## RegexOne —学习正则表达式—第 1 课:简介和基础知识

### 正则表达式在从文本中提取信息时非常有用，比如代码、日志文件、电子表格或…

regexone.com](https://regexone.com/) 

**黑客等级**

这个网站是解决编码挑战和强化技能的好方法。每完成一项挑战，你就会获得积分，并提高你在网站上的排名。虽然排名因素不是我的个人动机，但很高兴看到分数随着你解决更困难的问题而增加。每个挑战都有简单、中等或困难的等级，因此您可以根据自己所处的水平定制内容。

[](https://www.hackerrank.com/domains/regex) [## 解决正则表达式代码挑战

### 加入 1600 多万开发人员的行列，在 HackerRank 上解决代码挑战，这是为…

www.hackerrank.com](https://www.hackerrank.com/domains/regex) 

**Regex 纵横字谜**

这个网站是一个简单的概念，但当我在拼图中前进时，它给我带来了很多挫败感和满足感。这个想法是，您需要在纵横字谜中输入文本，以满足它周围的每个正则表达式。他们开始时很容易，但以后会变得更高级:

[](https://regexcrossword.com/) [## 正则表达式纵横字谜

### 一种使用正则表达式的纵横字谜游戏。完成难题挑战获得成就。适用于…的简单教程

regexcrossword.com](https://regexcrossword.com/) 

我希望这个概述是有用的，并希望您查看这些资源来挑战您的知识。自信地使用正则表达式是能够处理用户输入和执行数据验证的一个关键部分，所以花时间好好学习它绝对是值得的。(这个话题我还在继续学习，所以请留下评论，让我知道我是否错过了什么！)

> 如果你喜欢这篇文章并想阅读更多，一定要查看我的类似文章。考虑成为一个媒体成员，以获得无限的接触最好的想法和作家。
> 
> [**如果你通过这个链接加入 Medium，我会从你的费用中收取很少的一部分——而且不会花你任何额外的钱！提前感谢。**](https://medium.com/@simply_stef/membership) **💰**
> 
> *感谢阅读！*