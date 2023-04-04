# chance . js:faker js 的良好替代品

> 原文：<https://blog.devgenius.io/chance-js-a-good-replacement-of-fakerjs-660536891298?source=collection_archive---------8----------------------->

![](img/18016ff00bd042a39907a4ae2c12960e.png)

关于这个机会的一个小介绍…

机会是随机[【1】](https://chancejs.com/#true-random)字符串、数字等的极简生成器。为了帮助减少一些单调，特别是在编写自动化测试或者任何你需要随机的东西的时候。

Chance 是开源软件，在开发者和商业友好的麻省理工许可下发布

机会是加载在这个网站上，所以你可以打开你的浏览器和游戏控制台。

## 如何安装:

```
npm install chance
```

在终端中安装它之后，您可以使用 requirejs 加载它，如下所示

```
require(['Chance'], function(Chance) {
    // Instantiate
    var chance = new Chance();

    // Then use it:
    var my_random_integer = chance.integer();
  });
```

既然您已经知道如何安装和加载它，我们可以开始讨论在哪里以及如何使用它的基础知识。

通常，对于初学者来说，你会使用硬编码的值，但是如果你可以使它更容易被随机化，并且一次输入代码，你为什么要这样做呢？

## 以下是基本情况:

*   **人物**

它的用法因您希望角色如何显示而异，如下所示

```
chance.character();
=> 'v'
```

以下是利用机遇的其他方法

```
chance.character()
chance.character({ pool: 'abcde' })
chance.character({ alpha: true })
chance.character({ numeric: true })
chance.character({ casing: 'lower' })
chance.character({ symbols: true })
```

*   **布尔型**

boolean 有 2 个答案，要么是真，要么是假，下面是如何使它随机

```
chance.bool()
chance.bool({ likelihood: 30 })
```

默认的成功可能性(返回`true`)是 50%。可以选择指定百分比的可能性，在这种情况下，只有 30%的可能性为`true`，70%的可能性为`false`。

*   **浮动**

下面是使用浮动的方法

```
chance.floating()
chance.floating({ fixed: 7 })
chance.floating({ min: 0, max: 100 })
```

带 fixed 的那个用来设置随机数的小数点位数。而有 min & max 的是选择 0 到 100 之间的随机数函数。

*   **整数**

```
chance.integer()
chance.integer({ min: -20, max: 20 })
```

这些最小值和最大值包含在内，因此它们包含在范围内。这意味着

```
chance.integer({ min: -2, max: 2 })
```

将返回-2，-1，0，1 或 2。

```
// Specific case
-2 <= random number <= 2

// General case
min <= random number <= max
```

整数范围是 *-9007199254740991 到 9007199254740991*

> 9007199254740991 是 2⁵ — 1，是 JavaScript 中最大的数值

*   **字母**

```
chance.letter()
chance.letter({ casing: 'lower' })
```

就大小写而言，使用它的开发人员或测试人员很自然地想要一种特定类型的字母大小写，这样您就可以在大写或小写字母之间进行选择。

> 注意，我想把这个选项叫做 case 而不是 case，但不幸的是，这是 JavaScript 中的一个保留字，用于 switch 语句

*   **自然**

```
chance.natural()
chance.natural({ min: 1, max: 20 })
```

这些都是包含性的，所以包含在范围内。这意味着`chance.natural({min: 1, max: 3});`将返回 1、2 或 3。

自然范围是 *0 到 9007199254740991。*

*   **质数**

```
chance.prime()
chance.prime({ min: 1, max: 20 })
```

这些都是包含性的，所以包含在范围内。这意味着`chance.prime({min: 2, max: 5});`将返回 2、3 或 5。

主要范围是 0 到 10000。

*   **字符串**

```
chance.string()
chance.string({ length: 5 })
chance.string({ pool: 'abcde' })
chance.string({ alpha: true })
chance.string({ casing: 'lower' })
chance.string({ symbols: true })
```

所以之前是使用 chanceJS 的基础，现在会给你一些你最常用的实际用法。

*   **段**

```
chance.paragraph()
chance.paragraph({ sentences: 1 })
```

在段落中，你可以看到句子被设置为 1，这意味着在随机段落中你将只有一个句子，而不是多个。

*   **句子**

```
chance.sentence()
chance.sentence({ words: 5 })
```

就像句子中的段落一样，这将是句子中的字数限制。

*   **音节**

```
chance.syllable()
```

*   **字**

```
chance.word()
chance.word({ syllables: 3 })
chance.word({ length: 5 })
```

*   **年龄**

```
chance.age()
chance.age({ type: 'child' })
```

*   **生日**

```
chance.birthday()
chance.birthday({ string: true })
chance.birthday({ type: 'child' })
```

*   **名字**

```
chance.name()
chance.name({ middle: true })
chance.name({ middle_initial: true })
chance.name({ prefix: true })
chance.name({ nationality: 'en' })
```

*   **性别**

```
chance.gender()
```

*   **动物**

```
chance.animal()
chance.animal({type: 'zoo'})
```

其他类型有:`ocean`、`desert`、`grassland`、`forest`、`farm`和`pet`

*   **颜色**

```
chance.color()
chance.color({format: 'hex'})
chance.color({grayscale: true})
chance.color({casing: 'upper'})
```

*   **域**

```
chance.domain()
chance.domain({tld: 'com'})
```

*   **电子邮件**

```
chance.email()
chance.email({domain: "example.com"})
```

*   **地址**

```
chance.address()
chance.address({short_suffix: true})
```

*   **日期**

```
chance.date()
chance.date({string: true})
chance.date({string: true, american: false})
chance.date({year: 1983})
```

如果你想进一步探索，还有很多

[](https://chancejs.com/index.html) [## 机会

### 机会是随机字符串、数字等的最小化生成器。帮助减少一些单调，尤其是在…

chancejs.com](https://chancejs.com/index.html) 

> 我总是在尝试新事物，学习新事物。如果你再也学不到什么了，那就到此为止了。总是努力寻找可以学习的东西。