# 我是如何在僵尸启示录中幸存的

> 原文：<https://blog.devgenius.io/how-i-survived-the-zombie-apocalypse-19905db22043?source=collection_archive---------0----------------------->

选择优秀的测试用例非常困难。除非你召唤亡灵。

![](img/988df7ad4a9edc175ba886edbe7381ec.png)

[约翰·利博特](https://unsplash.com/@yohannlibot)在 [Unsplash](https://unsplash.com/s/photos/zombies) 上的照片

# 问题是

对于大多数开发人员来说，软件测试是一门新的学科。

质量保证工程师有很好的用例创建方法。

此外，[测试驱动开发](https://en.wikipedia.org/wiki/Test-driven_development) (TDD)技术将开发人员推向了测试编写的世界。*(不测试世界因为 TDD 不是测试技术)*。

但是我们还没有准备好。

[](https://mcsee.medium.com/how-to-squeeze-test-driven-development-on-legacy-systems-3e66625c4fb1) [## 如何在遗留系统上挤压测试驱动开发

### 我们都喜欢 T.D.D .我们知道它的好处，我们已经阅读了一千篇关于如何使用它来构建系统的教程…

mcsee.medium.com](https://mcsee.medium.com/how-to-squeeze-test-driven-development-on-legacy-systems-3e66625c4fb1) 

# TDD 个人旅程

我在 2005 年遇到了 TDD。我爱上了 XUnit 的回归、领域发现和全面覆盖。

TDD 是一种不可思议的技术，但是决定测试什么是很麻烦的。

我用混乱的方式写了几年 TDD 测试。

我将总结我这些年学到的一些东西，希望能给新开发人员一些时间。

# 解决方案

## 学习区分测试用例与测试数据

大多数程序员喜欢数据和实例化，在处理抽象概念时有困难。

他们急于创建*测试数据*而忽略了选择*相关案例*的大局。

选择代表*测试用例*的*精确数据*应该是简单明了的。反之则不然。

[等价划分](http://diranieh.com/Testing/TestCaseDesign.htm)是一种挑选合适的大使*的技术*将所有可能的情况划分成有限数量的等价类。

您可以合理地假设，对每个类的代表值的测试等同于对任何其他值的测试。

## 给测试编号

如果你正在开发 TDD 方式，创建顺序显示了你是如何处理问题的。

第一种情况应该容易理解，耦合度非常低。

需要我们慢慢增加我们的测试复杂度，这样我们就可以逐渐处理它。早期绿灯是无价的反馈。

## 挑选僵尸代表

僵尸测试是以下词的首字母缩写:

z-零

一个

M —许多(或更复杂)

B —边界行为

I —接口定义

e——锻炼特殊行为

简单的场景，简单的解决方案

[](https://www.agilealliance.org/resources/sessions/test-driven-development-guided-by-zombies) [## 僵尸引导的测试驱动开发|敏捷联盟

### 你是否曾经很难找到从哪里开始测试驱动开发的方法？如果僵尸可以帮助你建立…

www.agilealliance.org](https://www.agilealliance.org/resources/sessions/test-driven-development-guided-by-zombies) 

## 例子

让我们创建一封电子邮件 *T.D.D. Way* 。

一次一个行为！

让我们从一个空的测试类开始:

```
TestCase subclass: #EmailMessageTest
    instanceVariableNames: ''
    classVariableNames: ''
    poolDictionaries: ''
    category: 'Zombies'
```

第一个测试将是我们的(Z)ero。因为我们正在进行 TDD，所以我们还没有定义 *EmailMessage* 类。

```
test01ZeroRecipients self deny: (EmailMessage forRecipients: Array new) isValid
```

名称 test01ZeroRecipients 由以下部分组成

*   前缀*测试*在许多测试框架中是强制性的。(*强制*)
*   连续顺序(逐步显示学习/测试程序)。(*可选*)
*   零/一/多来代表僵尸。(*可选*)
*   功能描述(*强制*)

我们开始否认(断言错误)我们的第一封电子邮件将是有效的。(因为没有收件人)。

(否认等于*断言，因为不是*而是为了更清楚地阅读和避免双重否定)。

我们进行第一次测试。

*EmailMessage* 类未定义

```
Object subclass: #EmailMessage
    instanceVariableNames: ''
    classVariableNames: ''
    poolDictionaries: ''
    category: 'TDD-Zombies'
```

*接收方:*(施工方)

```
forRecipients: recipients     ^self new initializeForRecipients: recipients
```

构造函数创建实例并用本质初始化(没有反模式设置器，没有从头开始的变化)。

[](https://medium.com/dev-genius/nude-models-part-ii-getters-b039e5ad3427) [## 裸体模特第二部分:吸气剂

### 可靠的数据结构及其有争议的(读)访问。

medium.com](https://medium.com/dev-genius/nude-models-part-ii-getters-b039e5ad3427) [](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) [## 变种人的邪恶力量

### 变异就是进化。它是由查尔斯·达尔文爵士提出的，我们在软件行业中使用它。但是有些事情是…

codeburst.io](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) 

*私人*初始值设定项

```
initializeForRecipients: emailRecipients     recipients := emailRecipients.
```

初始化器设置私有属性。

这是 *isValid* 的第一个(足够好的)实现。

```
isValid
     ^false
```

*isValid* 被硬编码为 *false* 。

这是 TDD 值之一。最简单的解决方案应该是硬编码的。

第一次测试通过！！！

让我们去海边吧。

```
test02OneRecipient self assert: (EmailMessage forRecipients: (Array with: 'john@example.com')) isValid.
```

测试不起作用，因为我们在上一步中将其硬编码为*假*。

我们改变*是有效的*实现。

```
isValid
     ^recipients notEmpty
```

这个条件比较一般。现在 test01 和 test02 工作了！

让我们向 test02 添加新协议。这应该是一个不同的测试案例，但我们将超载测试 02，所以我们坚持每个僵尸一个测试。

```
test02OneRecipient self assert: (EmailMessage forRecipients: (Array with: 'john@example.com')) isValid.
    self assert: (EmailMessage forRecipients: (Array with: 'john@example.com')) plain = 'to: john@example.com'.
```

*纯文本*消息将从电子邮件中返回纯文本。我们来实施吧。

```
plain
     ^'to: ' , recipients first.
```

我们将*连接到:*头和第一个接收者。硬编码。

(ZO)测试准备好了。让我们尽情享受吧。

```
test03ManyRecipients | message |
    message := (EmailMessage forRecipients:
 (Array with: 'john@example.com' with: 'jane@example.com' with: 'lucas@example.com')). self assert: message plain = 'to: john@example.com,jane@example.com,lucas@example.com'.
    self assert: message isValid
```

在这一点上*比*更难造假。这是一个提示，我们应该重构并制定一个通用的解决方案，而不是硬编码。

Many 由一个 3 元素集合表示。*有效*工作，但*普通*无效。

```
plain
     ^(recipients inject: 'to: ' into: [:plainText :address | plainText, address ,' ,' ]) allButLast
```

这个习语遍历 recipients(私有属性，没有 getters)，插入一个逗号(，)并删除最后一个( *allButLast* )。既然我们没有*爆炸*。

[](https://medium.com/dev-genius/nude-models-part-ii-getters-b039e5ad3427) [## 裸体模特第二部分:吸气剂

### 可靠的数据结构及其有争议的(读)访问。

medium.com](https://medium.com/dev-genius/nude-models-part-ii-getters-b039e5ad3427) 

3 次测试成功！(ZOM)

我们应该解决边界问题。

```
test04BoundaryRecipients | recipients |
    recipients := OrderedCollection new.
    1 to: 10 do: [:index | recipients add: ('john', index asString, '@example.com')]. self deny: (EmailMessage forRecipients: recipients) isValid
```

我们创建了 10 封不同的电子邮件[john1@example.com](mailto:john1@example.com)、[john2@example.com](mailto:john2@example.com)、…、[john10@example.com](mailto:john10@example.com)。

我们决定 10 是这个练习中接收者大小的任意上限。有 10 个收件人的邮件无效。

```
isValid
     ^recipients notEmpty and: [recipients size < 10]
```

该条件现在检查下限和上限。它等于:

```
isValid
     ^recipients size between: 1 and: 9
```

由于我们有全面的覆盖，我们可以随时改变实施！

僵尸进来了。该是检查界面的时候了。

```
test05InterfaceDefinition self should: [ EmailMessage forRecipients: 1 ] raise: Exception
```

我们试图创建一个带有整数的消息，这违反了我们的接口，因此它应该会引发一个异常。

```
forRecipients: recipients     recipients do: [:eachAddress | 
        eachAddress isString
            ifFalse: [ self error: (eachAddress , ' is not a valid recipient')]]. ^self new initializeForRecipients: recipients
```

我们在创建时验证地址。这里没有无效对象！

我是僵尸。例外行为怎么样？

```
test06ExceptionalBehavior self should: [EmailMessage forRecipients: (Array with: 'john@example.com' with: 'john@example.com')] raise: Exception
```

根据我们的域名，重复是无效的。让我们检查他们！

```
forRecipients: recipients recipients do: [:eachAddress | 
    eachAddress isString
        ifFalse: [ self error: (eachAddress , ' is not a valid recipient')]].
    (recipients asSet size = recipients size)
        ifFalse: [self error: 'Duplicates']. ^self new initializeForRecipients: recipients
```

我们检查重复映射我们的收件人到一个集合，并检查两个大小。不是最好的表演方式。

我们需要保持示例正常工作，避免过早优化。随着所有测试的运行，我们可以优化代码。

[](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) [## 代码味道 20 —过早优化

### 提前计划需要一个水晶球，这是任何一个开发者都没有的。

medium.com](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) 

可读性怎么样？方法太难看了。让我们做 TDD 第三步:重构。

```
forRecipients: recipients      self assertAllAddressesAreValid: recipients.
     self assertThereAreNoDuplicates: recipients.         
    ^self new initializeForRecipients: recipients
```

那更好！

最后一次测试！简单的情景。在这一小块没有必要。出于教学目的，我们可以添加一种冗余方式:

```
test07SimpleScenarios self assert: (EmailMessage forRecipients: (Array with: 'john@example.com')) plain =     'to: john@example.com'
```

仅此而已。我们用 TDD 建立了我们的模型，并且信心十足。全覆盖不镀金！。

*   没有空构造函数

[](https://medium.com/dev-genius/code-smell-13-empty-constructors-8fa705fb3f33) [## 代码味道 13 —空构造函数

### 不完整的对象会导致很多问题。

medium.com](https://medium.com/dev-genius/code-smell-13-empty-constructors-8fa705fb3f33) 

*   模型是可汇总的
*   没有 setters
*   没有吸气剂
*   没有偶然的如果

[](https://medium.com/dev-genius/how-to-get-rid-of-annoying-ifs-forever-317033474484) [## 如何永远摆脱烦人的 if

### 为什么我们学习编程的第一条指令应该是最后使用的。

medium.com](https://medium.com/dev-genius/how-to-get-rid-of-annoying-ifs-forever-317033474484) 

*   没有空值

[](https://medium.com/dev-genius/code-smell-12-null-64fbd7792a7c) [## 代码气味 12 —空

### 程序员使用 Null 作为不同的标志。它可以提示缺席、未定义的值、错误等。

medium.com](https://medium.com/dev-genius/code-smell-12-null-64fbd7792a7c) 

# 结论

有了这些提示，我们可以做非常基本的测试写作。

这对于 TDD 来说就足够了。

TDD 不会取代质量保证过程，QA 工程师会与开发人员一起工作，互相学习。

开发软件是一项人类的协作活动。

让我们一起欣赏吧！

这一系列文章的部分目标是为软件设计的辩论和讨论提供空间。

[](https://mcsee.medium.com/object-design-checklist-47c63d351352) [## 目标设计清单

### 这是已经发表的软件设计文章的索引。

mcsee.medium.com](https://mcsee.medium.com/object-design-checklist-47c63d351352) 

我们期待着对这篇文章的评论和建议。