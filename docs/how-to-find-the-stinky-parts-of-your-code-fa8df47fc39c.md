# 如何找到你的代码中有问题的部分

> 原文：<https://blog.devgenius.io/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c?source=collection_archive---------3----------------------->

## 代码很难闻。让我们看看如何改变香味。

![](img/0ee24a785aeb3f6ca26d8c90da6abaf6.png)

> TL；DR:代码中难闻气味的汇编。

在这个系列中，我们将会看到一些使我们怀疑开发质量的症状和情况。

我们将提出可能的解决方案。

这些气味中的大部分只是可能出问题的线索。它们不是严格的规则。

# 代码气味

[](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [## 代码气味 01——贫血模型

### 你的对象是一堆没有行为的公共属性。

medium.com](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [](https://medium.com/dev-genius/code-smell-02-constants-and-magic-numbers-d6c320eef90b) [## 代码气味 02——常数和幻数

### 一种方法用大量的数字进行计算，而不描述它们的语义。

medium.com](https://medium.com/dev-genius/code-smell-02-constants-and-magic-numbers-d6c320eef90b) [](https://medium.com/dev-genius/code-smell-03-functions-are-too-long-accea7eb4ae9) [## 代码气味 03 —函数太长

### 人类过了 10 线就烦了。

medium.com](https://medium.com/dev-genius/code-smell-03-functions-are-too-long-accea7eb4ae9) [](https://medium.com/dev-genius/code-smell-04-string-abusers-ec67005d9d5f) [## 代码气味 04 —字符串滥用者

### 太多的解析、分解、正则表达式、strcomp、strpos 和字符串操作函数。

medium.com](https://medium.com/dev-genius/code-smell-04-string-abusers-ec67005d9d5f) [](https://medium.com/dev-genius/code-smell-05-comment-abusers-feec3aeb926) [## 代码气味 05 —评论滥用者

### 代码有很多注释。注释与实现联系在一起，很难维护。

medium.com](https://medium.com/dev-genius/code-smell-05-comment-abusers-feec3aeb926) [](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) [## 代码气味 06——太聪明的程序员

### 难以阅读的代码。没有语义的名字很棘手。有时使用语言的意外复杂性。

medium.com](https://medium.com/dev-genius/code-smell-06-too-clever-programmer-bffec35daf0b) [](https://medium.com/dev-genius/code-smell-07-boolean-variables-260bef4d1146) [## 代码气味 07 —布尔变量

### 使用布尔变量作为标志，暴露了意外的实现，并用 Ifs 污染了代码。

medium.com](https://medium.com/dev-genius/code-smell-07-boolean-variables-260bef4d1146) [](https://mcsee.medium.com/code-smell-08-long-chains-of-collaborations-68aa0207ddd0) [## 代码气味 08——长长的合作链

### 使长链产生耦合和涟漪效应。任何连锁变化都会破坏代码。

mcsee.medium.com](https://mcsee.medium.com/code-smell-08-long-chains-of-collaborations-68aa0207ddd0) [](https://mcsee.medium.com/code-smell-09-dead-code-9fb2488d9f5f) [## 代码气味 09 —死代码

### 不再使用或需要的代码。

mcsee.medium.com](https://mcsee.medium.com/code-smell-09-dead-code-9fb2488d9f5f) [](https://medium.com/dev-genius/code-smell-10-too-many-arguments-b44064610789) [## 代码味道 10 —参数太多

### 对象或函数需要太多的参数才能工作

medium.com](https://medium.com/dev-genius/code-smell-10-too-many-arguments-b44064610789) [](https://mcsee.medium.com/code-smell-11-subclassification-for-code-reuse-2e2f5b564204) [## 代码味道 11 —代码重用的子类化

### 代码重用是好的。但是子类化会产生静态耦合。

mcsee.medium.com](https://mcsee.medium.com/code-smell-11-subclassification-for-code-reuse-2e2f5b564204) [](https://mcsee.medium.com/code-smell-12-null-64fbd7792a7c) [## 代码气味 12 —空

### Null 正在用作不同的标志。它可以提示缺席、未定义的值、错误等。多重语义导致…

mcsee.medium.com](https://mcsee.medium.com/code-smell-12-null-64fbd7792a7c) [](https://mcsee.medium.com/code-smell-13-empty-constructors-8fa705fb3f33) [## 代码味道 13 —空构造函数

### 不完整的对象会导致很多问题。

mcsee.medium.com](https://mcsee.medium.com/code-smell-13-empty-constructors-8fa705fb3f33) [](https://mcsee.medium.com/code-smell-14-god-objects-b84b75b702) [## 代码气味 14 —上帝物品

### 知道得太多或做得太多的对象。

mcsee.medium.com](https://mcsee.medium.com/code-smell-14-god-objects-b84b75b702) [](https://mcsee.medium.com/code-smell-15-missed-preconditions-84349973e003) [## 代码气味 15 —错过前提条件

### 断言、前置条件、后置条件和不变量是我们避免无效对象的盟友。避开他们会导致…

mcsee.medium.com](https://mcsee.medium.com/code-smell-15-missed-preconditions-84349973e003) [](https://mcsee.medium.com/code-smell-16-ripple-effect-c5a2db81f37e) [## 代码气味 16 —连锁反应

### 微小的变化会产生意想不到的问题。

mcsee.medium.com](https://mcsee.medium.com/code-smell-16-ripple-effect-c5a2db81f37e) [](https://mcsee.medium.com/code-smell-17-global-functions-d32b8796d284) [## 代码味道 17 —全局函数

### 受到面向对象编程的阻碍，许多混合语言支持它。开发者滥用它们。

mcsee.medium.com](https://mcsee.medium.com/code-smell-17-global-functions-d32b8796d284) [](https://mcsee.medium.com/code-smell-18-static-functions-68d35c15e76) [## 代码味道 18 —静态函数

### 又一个全球接入加上懒惰。

mcsee.medium.com](https://mcsee.medium.com/code-smell-18-static-functions-68d35c15e76) [](https://mcsee.medium.com/code-smell-19-optional-arguments-c0714855dbbb) [## 代码味道 19 —可选参数

### 伪装成友好的快捷方式是另一种耦合气味。

mcsee.medium.com](https://mcsee.medium.com/code-smell-19-optional-arguments-c0714855dbbb) [](https://mcsee.medium.com/code-smell-20-premature-optimization-60409b7f90ec) [## 代码味道 20 —过早优化

### 提前计划需要一个水晶球，这是任何一个开发者都没有的。

mcsee.medium.com](https://mcsee.medium.com/code-smell-20-premature-optimization-60409b7f90ec) [](https://mcsee.medium.com/code-smell-21-anonymous-functions-abusers-e6f1e3db6a3f) [## 代码气味 21 —匿名函数滥用者

### 函数，lambdas，闭包。所以高阶，非声明，和热。

mcsee.medium.com](https://mcsee.medium.com/code-smell-21-anonymous-functions-abusers-e6f1e3db6a3f) [](https://medium.com/dev-genius/code-smell-22-helpers-1d5057137ddb) [## 代码气味 22 —助手

### 你需要帮助吗？你要给谁打电话？

medium.com](https://medium.com/dev-genius/code-smell-22-helpers-1d5057137ddb) [](https://mcsee.medium.com/code-smell-23-instance-type-checking-83a1a36a6336) [## 代码味道 23 —实例类型检查

### 你检查过你在和谁说话吗？

mcsee.medium.com](https://mcsee.medium.com/code-smell-23-instance-type-checking-83a1a36a6336) [](https://mcsee.medium.com/code-smell-24-boolean-coercions-1594cc1be008) [## 代码气味 24 —布尔强制

### 布尔应该只有真和假

mcsee.medium.com](https://mcsee.medium.com/code-smell-24-boolean-coercions-1594cc1be008) [](https://mcsee.medium.com/code-smell-25-pattern-abusers-bc0204b277b7) [## 代码气味 25 —模式滥用者

### 图案很赞。权力越大，责任越大

mcsee.medium.com](https://mcsee.medium.com/code-smell-25-pattern-abusers-bc0204b277b7) [](https://mcsee.medium.com/code-smell-26-exceptions-polluting-9246aca40234) [## 气味代码 26 —污染例外

### 有许多不同的例外是非常好的。您的代码是声明性的和健壮的。还是没有？

mcsee.medium.com](https://mcsee.medium.com/code-smell-26-exceptions-polluting-9246aca40234) [](https://mcsee.medium.com/code-smell-27-associative-arrays-77c36284eb5e) [## 代码味道 27 —关联数组

### [键，值]，魔术，快速，可塑性和错误修剪。

mcsee.medium.com](https://mcsee.medium.com/code-smell-27-associative-arrays-77c36284eb5e) [](https://mcsee.medium.com/code-smell-28-setters-5b0e764049aa) [## 气味代码 28 —设定器

### 初级程序员做的第一个练习。ide、教程和高级开发人员不断教他们这种反模式。

mcsee.medium.com](https://mcsee.medium.com/code-smell-28-setters-5b0e764049aa) [](https://mcsee.medium.com/code-smell-29-settings-configs-39188e5b8316) [## 代码气味 29 —设置/配置

### 在控制板中改变系统行为是客户的梦想。软件工程师的噩梦。

mcsee.medium.com](https://mcsee.medium.com/code-smell-29-settings-configs-39188e5b8316) [](https://mcsee.medium.com/code-smell-30-mocking-business-885299c32d6f) [## 代码气味 30——嘲弄商业

### 在测试行为时，嘲笑是一个很好的帮助。像许多其他工具一样，我们正在滥用它们。

mcsee.medium.com](https://mcsee.medium.com/code-smell-30-mocking-business-885299c32d6f) [](https://medium.com/dev-genius/code-smell-31-accidental-methods-on-business-objects-2ff9dc48a783) [## 代码气味 31——业务对象上的意外方法

### 向对象添加持久性、序列化、显示、导入、导出代码会增加其协议并带来…

medium.com](https://medium.com/dev-genius/code-smell-31-accidental-methods-on-business-objects-2ff9dc48a783) [](https://mcsee.medium.com/code-smell-32-singletons-d6d7b0a9f251) [## 代码气味 32 —单件

### 世界上最常用和最著名的设计模式正在给我们带来巨大的伤害。

mcsee.medium.com](https://mcsee.medium.com/code-smell-32-singletons-d6d7b0a9f251) [](https://mcsee.medium.com/code-smell-33-abbreviations-ba5149c93a68) [## 代码气味 33 —缩写

### 缩写是非常重要的，这样我们看起来聪明，节省记忆和思维空间

mcsee.medium.com](https://mcsee.medium.com/code-smell-33-abbreviations-ba5149c93a68) [](https://mcsee.medium.com/code-smell-34-too-many-attributes-2df68c7db040) [## 代码味道 34 —属性太多

### 一个类定义了具有许多属性的对象。

mcsee.medium.com](https://mcsee.medium.com/code-smell-34-too-many-attributes-2df68c7db040) [](https://mcsee.medium.com/code-smell-35-state-as-attributes-ea30a2c97e8e) [## 代码气味 35 —作为属性的状态

### 当一个对象改变它的状态时，最好的解决方法是改变它的属性，不是吗？

mcsee.medium.com](https://mcsee.medium.com/code-smell-35-state-as-attributes-ea30a2c97e8e) [](https://mcsee.medium.com/code-smell-36-switch-case-elseif-else-if-statements-73f0215abf2b) [## 代码气味 36 — Switch/case/elseif/else/if 语句

### 第一节编程课:控制结构。高级开发人员的教训:避免它们。

mcsee.medium.com](https://mcsee.medium.com/code-smell-36-switch-case-elseif-else-if-statements-73f0215abf2b) [](https://mcsee.medium.com/code-smell-38-abstract-names-c2306d0846ca) [## 代码气味 38 -抽象名称

### 工具命名无意义的名字打破映射和双射到现实世界的实体。选择有意义的名字。查找…

mcsee.medium.com](https://mcsee.medium.com/code-smell-38-abstract-names-c2306d0846ca) [](https://medium.com/dev-genius/code-smell-39-new-date-7a2571a81ecb) [## 代码气味 39 —新日期()

### 70s 第一教程:getCurrentDate()。小菜一碟。我们处在 20 年代，时代不再全球化。

medium.com](https://medium.com/dev-genius/code-smell-39-new-date-7a2571a81ecb) [](https://medium.com/dev-genius/code-smell-40-dtos-ca35f5d8f7c9) [## 代码气味 40—dto

### dto 被广泛使用并“解决”了实际问题，是吗？

medium.com](https://medium.com/dev-genius/code-smell-40-dtos-ca35f5d8f7c9) [](https://mcsee.medium.com/code-smell-41-regular-expression-abusers-ae2976eef322) [## 代码气味 41 —正则表达式滥用者

### 正则表达式是一个很棒的工具，我们应该小心使用它们，不要显得太聪明。

mcsee.medium.com](https://mcsee.medium.com/code-smell-41-regular-expression-abusers-ae2976eef322) [](https://mcsee.medium.com/code-smell-42-warnings-strict-mode-off-d7b467be34d7) [## 代码气味 42 —警告/严格模式关闭

### 编译器和警告灯可以帮助你。不要忽视他们。

mcsee.medium.com](https://mcsee.medium.com/code-smell-42-warnings-strict-mode-off-d7b467be34d7) [](https://medium.com/dev-genius/code-smell-43-concrete-classes-subclassified-7cccf1545ab5) [## 代码气味 43——具体分类

### 遗产。具体的类。重用。奇妙的混合。

medium.com](https://medium.com/dev-genius/code-smell-43-concrete-classes-subclassified-7cccf1545ab5) [](https://medium.com/dev-genius/code-smell-44-magic-corrections-2d9aa37f4f3f) [## 代码气味 44 —魔法修正

### 编译器比我们聪明。在周五晚上的制作部署中，他们背叛了我们。

medium.com](https://medium.com/dev-genius/code-smell-44-magic-corrections-2d9aa37f4f3f) [](https://mcsee.medium.com/code-smell-45-not-polymorphic-1a2259e001d) [## 代码气味 45 —非多态

### 如果方法做同样的事情，它们应该是可互换的。

mcsee.medium.com](https://mcsee.medium.com/code-smell-45-not-polymorphic-1a2259e001d) [](https://mcsee.medium.com/code-smell-46-repeated-code-433d3feb516d) [## 代码气味 46 —重复代码

### 干是我们的口头禅。老师告诉我们要消除重复。我们需要超越。

mcsee.medium.com](https://mcsee.medium.com/code-smell-46-repeated-code-433d3feb516d) [](https://mcsee.medium.com/code-smell-47-diagrams-b8bc7702a085) [## 代码气味 47 —图表

### 图表不是代码。它们不可能是代码气味。它们只是图表气味。

mcsee.medium.com](https://mcsee.medium.com/code-smell-47-diagrams-b8bc7702a085) [](https://mcsee.medium.com/code-smell-48-code-without-standards-60c9e0905627) [## 代码气味 48 —没有标准的代码

### 从事个人项目很容易。除非你几个月后再去找它。与许多其他开发人员合作…

mcsee.medium.com](https://mcsee.medium.com/code-smell-48-code-without-standards-60c9e0905627) [](https://mcsee.medium.com/code-smell-49-caches-d2e64373b838) [## 代码气味 49 —缓存

### 储藏处很性感。他们是一夜情。我们需要在长期关系中避开它们。

mcsee.medium.com](https://mcsee.medium.com/code-smell-49-caches-d2e64373b838) [](https://mcsee.medium.com/code-smell-50-object-keys-c2599b569403) [## 代码气味 50 —对象键

### 主键，id，引用。我们添加到对象中的第一个属性。它们在现实世界中并不存在。

mcsee.medium.com](https://mcsee.medium.com/code-smell-50-object-keys-c2599b569403) [](https://mcsee.medium.com/code-smell-51-double-negatives-993b6160f3e1) [## 气味代码 51 —双重否定

### 不是操作员是我们的朋友。不是不是操作员不是我们的朋友

mcsee.medium.com](https://mcsee.medium.com/code-smell-51-double-negatives-993b6160f3e1) [](https://mcsee.medium.com/code-smell-52-fragile-tests-b917de33fd53) [## 代码气味 52 —易碎测试

### 测试是我们的安全网。如果我们不信任他们的正直，我们将处于极大的危险之中。

mcsee.medium.com](https://mcsee.medium.com/code-smell-52-fragile-tests-b917de33fd53) [](https://mcsee.medium.com/code-smell-53-explicit-iteration-d481d8a7e5d7) [## 代码味道 53——显式迭代

### 我们在学校学过循环。但是枚举器和迭代器是下一代。

mcsee.medium.com](https://mcsee.medium.com/code-smell-53-explicit-iteration-d481d8a7e5d7) [](https://mcsee.medium.com/code-smell-54-anchor-boats-190cb9ef6bdc) [## 代码气味 54 —锚船

### 代码在那里。以防万一。我们可能很快就需要它。

mcsee.medium.com](https://mcsee.medium.com/code-smell-54-anchor-boats-190cb9ef6bdc) [](https://medium.com/dev-genius/code-smell-55-object-orgy-529c754a2fac) [## 代码气味 55——对象狂欢

### 如果你把你的对象看作数据持有者，你将违反它们的封装，但是你不应该，因为在现实生活中，你…

medium.com](https://medium.com/dev-genius/code-smell-55-object-orgy-529c754a2fac) [](https://medium.com/dev-genius/code-smell-56-preprocessors-89d8beb6655b) [## 代码气味 56 —预处理器

### 我们希望我们的代码在不同的环境和操作系统下表现不同，所以在编译时做出决定…

medium.com](https://medium.com/dev-genius/code-smell-56-preprocessors-89d8beb6655b) [](https://medium.com/dev-genius/code-smell-57-versioned-functions-d548b7f1b481) [## 代码味道 57 —版本化函数

### sort，sortOld，sort20210117，workingSort，把它们都有了太好了。以防万一

medium.com](https://medium.com/dev-genius/code-smell-57-versioned-functions-d548b7f1b481) [](https://medium.com/dev-genius/code-smell-58-yo-yo-problem-1e092d3c69ff) [## 代码气味 58 —溜溜球问题

### 寻找一个具体的方法实现？来来回回，上上下下。

medium.com](https://medium.com/dev-genius/code-smell-58-yo-yo-problem-1e092d3c69ff) [](https://mcsee.medium.com/code-smell-59-basic-do-functions-595ce8e6efa5) [## 代码气味 59 —基本/执行功能

### sort，doSort，basicSort，doBasicSort，primitiveSort，superBasicPrimitiveSort，谁做真正的工作？

mcsee.medium.com](https://mcsee.medium.com/code-smell-59-basic-do-functions-595ce8e6efa5) [](https://mcsee.medium.com/code-smell-60-global-classes-3ae3bb2582ac) [## 代码气味 60 —全局类

### 上课很方便。我们可以随时调用它们。这样好吗？

mcsee.medium.com](https://mcsee.medium.com/code-smell-60-global-classes-3ae3bb2582ac) [](https://mcsee.medium.com/code-smell-61-coupling-to-classes-8c52b8e33c95) [## 代码气味 61——耦合到类

### 上课很方便。我们可以随时调用它们。这样好吗？

mcsee.medium.com](https://mcsee.medium.com/code-smell-61-coupling-to-classes-8c52b8e33c95) [](https://mcsee.medium.com/code-smell-62-flag-variables-c864eac65a84) [## 代码气味 62 —标志变量

### 旗帜表明发生了什么。除非他们的名字太普通了。

mcsee.medium.com](https://mcsee.medium.com/code-smell-62-flag-variables-c864eac65a84) [](https://mcsee.medium.com/code-smell-63-feature-envy-ac1f93cf8dce) [## 代码气味 63 —特征羡慕

### 如果你的方法是嫉妒和不信任授权，你应该开始这样做。

mcsee.medium.com](https://mcsee.medium.com/code-smell-63-feature-envy-ac1f93cf8dce) [](https://mcsee.medium.com/code-smell-64-inappropriate-intimacy-f1b064984094) [## 代码气味 64——不适当的亲密关系

### 两个纠结于爱情的阶层。

mcsee.medium.com](https://mcsee.medium.com/code-smell-64-inappropriate-intimacy-f1b064984094) [](https://mcsee.medium.com/code-smell-65-variables-named-after-types-1911f4b2ea29) [## 代码味道 65 —以类型命名的变量

### 名字应该总是表明角色。

mcsee.medium.com](https://mcsee.medium.com/code-smell-65-variables-named-after-types-1911f4b2ea29) [](https://mcsee.medium.com/code-smell-66-shotgun-surgery-d97eb688622) [## 气味代码 66——猎枪手术

### 只说一次

mcsee.medium.com](https://mcsee.medium.com/code-smell-66-shotgun-surgery-d97eb688622) [](https://medium.com/dev-genius/code-smell-67-middle-man-cb07d8307d1c) [## 气味代码 67——中间人

### 让我们打破德米特里定律。

medium.com](https://medium.com/dev-genius/code-smell-67-middle-man-cb07d8307d1c) [](https://mcsee.medium.com/code-smell-68-getters-68571a0f7fa8) [## 代码气味 68 —吸气剂

### 得到的东西是广泛而安全的。但这是一种非常糟糕的做法。

mcsee.medium.com](https://mcsee.medium.com/code-smell-68-getters-68571a0f7fa8) [](https://mcsee.medium.com/code-smell-69-big-bang-javascript-ridiculous-castings-870f20469322) [## 代码味道 69 —大爆炸(JavaScript 荒谬的铸件)

mcsee.medium.com](https://mcsee.medium.com/code-smell-69-big-bang-javascript-ridiculous-castings-870f20469322) [](/code-smell-70-anemic-model-generators-99aca77391fc) [## 代码气味 70 —贫血模型发电机

### TL；大卫:不要创造贫血的物体。更不用说自动化工具了。

blog.devgenius.io](/code-smell-70-anemic-model-generators-99aca77391fc) [](https://mcsee.medium.com/code-smell-71-magic-floats-disguised-as-decimals-31271b957ac5) [## 代码气味 71 —伪装成小数的魔法浮动

mcsee.medium.com](https://mcsee.medium.com/code-smell-71-magic-floats-disguised-as-decimals-31271b957ac5) [](/code-smell-72-return-codes-1b869fb53f54) [## 代码气味 72 —返回代码

blog.devgenius.io](/code-smell-72-return-codes-1b869fb53f54) [](https://mcsee.medium.com/code-smell-73-exceptions-for-expected-cases-ea870197d5fc) [## 代码气味 73 —预期情况的例外

mcsee.medium.com](https://mcsee.medium.com/code-smell-73-exceptions-for-expected-cases-ea870197d5fc) [](https://mcsee.medium.com/code-smell-74-empty-lines-66d8d41d83e4) [## 代码气味 74 —空行

mcsee.medium.com](https://mcsee.medium.com/code-smell-74-empty-lines-66d8d41d83e4) [](https://mcsee.medium.com/code-smell-75-comments-inside-a-method-c911f8729ee1) [## 代码味道 75 —方法内部的注释

### 注释通常是一种代码味道。将它们插入到方法中需要紧急重构。

mcsee.medium.com](https://mcsee.medium.com/code-smell-75-comments-inside-a-method-c911f8729ee1) [](https://maximilianocontieri.com/code-smell-76-generic-assertions) [## 代码味道 76 -通用断言

### 您的浏览器不支持音频元素。不要做弱测试来制造虚假的覆盖感。TL；博士…

maximilianocontieri.com](https://maximilianocontieri.com/code-smell-76-generic-assertions) [](/code-smell-77-timestamps-cb3d4cdbb1f2) [## 代码气味 77 —时间戳

### 时间戳被广泛使用。他们有一个中央发行机构，不会回去，是吗？

blog.devgenius.io](/code-smell-77-timestamps-cb3d4cdbb1f2) [](/code-smell-78-callback-hell-aeb4a3010270) [## 代码气味 78 —回调地狱

### 将算法作为一系列嵌套回调来处理并不明智。

blog.devgenius.io](/code-smell-78-callback-hell-aeb4a3010270) [](https://mcsee.medium.com/code-smell-79-theresult-3c536d926f06) [## 气味代码 79 —结果

### 如果一个名字已经被使用，我们总是可以在它前面加上“the”。

mcsee.medium.com](https://mcsee.medium.com/code-smell-79-theresult-3c536d926f06) [](/code-smell-80-nested-try-catch-32aaab32100c) [## 代码气味 80 —嵌套的尝试/捕捉

### 例外是区分快乐之路和困难之路的好方法。但是我们倾向于使我们的解决方案过于复杂。

blog.devgenius.io](/code-smell-80-nested-try-catch-32aaab32100c) [](/code-smell-82-tests-violating-encapsulation-aa10f5e02445) [## 代码气味 82 —违反封装的测试

### 对象工作良好，并实现业务目标。但是我们需要测试它们。让我们打破他们。

blog.devgenius.io](/code-smell-82-tests-violating-encapsulation-aa10f5e02445) [](/code-smell-83-variables-reassignment-6d3d4f4aa3b) [## 代码气味 83 —变量重新分配

### 变量重用是我们在大块代码中看到的。

blog.devgenius.io](/code-smell-83-variables-reassignment-6d3d4f4aa3b) [](/code-smell-84-max-min-javascript-37470a3111db) [## 代码气味 84 —最大值

### 有些功能没有按预期运行。可悲的是，大多数程序员接受了它们。

blog.devgenius.io](/code-smell-84-max-min-javascript-37470a3111db) [](https://mcsee.medium.com/code-smell-85-and-functions-7f9cb0ad0b2d) [## 代码气味 85 —和功能

### 不要执行超过要求的内容。

mcsee.medium.com](https://mcsee.medium.com/code-smell-85-and-functions-7f9cb0ad0b2d) [](https://mcsee.medium.com/code-smell-86-mutable-const-arrays-97b1a7d72b6c) [## 代码气味 86 —可变常量数组

### Const 声明某个东西是常量。会变异吗？

mcsee.medium.com](https://mcsee.medium.com/code-smell-86-mutable-const-arrays-97b1a7d72b6c) [](https://mcsee.medium.com/code-smell-87-inconsistent-parameters-sorting-46e91d48bae0) [## 代码气味 87 —不一致的参数排序

### 与您使用的参数保持一致。代码是散文。

mcsee.medium.com](https://mcsee.medium.com/code-smell-87-inconsistent-parameters-sorting-46e91d48bae0) [](https://mcsee.medium.com/code-smell-88-lazy-initialization-48e1b95a949e) [## 代码气味 88 —惰性初始化

### 又一个不成熟的优化模式

mcsee.medium.com](https://mcsee.medium.com/code-smell-88-lazy-initialization-48e1b95a949e) [](https://mcsee.medium.com/code-smell-89-math-feature-envy-2c7e3c61f7d1) [## 代码气味 89 —数学特征羡慕

### 一个类为另一个类计算公式。

mcsee.medium.com](https://mcsee.medium.com/code-smell-89-math-feature-envy-2c7e3c61f7d1) [](https://mcsee.medium.com/code-smell-90-implementative-callback-events-df1d2b371087) [## 代码气味 90 —实现性回调事件

### 在创建事件时，我们应该将触发器与操作分离。

mcsee.medium.com](https://mcsee.medium.com/code-smell-90-implementative-callback-events-df1d2b371087) [](https://mcsee.medium.com/code-smell-91-test-asserts-without-description-fcf1ea2bcbf9) [## 代码气味 91 —没有描述的测试断言

### 我们是 xUnit 的忠实粉丝。但是我们不太关心程序员。

mcsee.medium.com](https://mcsee.medium.com/code-smell-91-test-asserts-without-description-fcf1ea2bcbf9) [](/code-smell-92-isolated-subclasses-names-3b9ac28922f1) [## 代码气味 92 —独立的子类名称

### 如果你的类是全局的，使用完全限定的名字

blog.devgenius.io](/code-smell-92-isolated-subclasses-names-3b9ac28922f1) [](https://mcsee.medium.com/code-smell-93-send-me-anything-c790248b0c4c) [## 气味代码 93——发送任何信息给我

### 可以接收许多不同的(而不是多态的)参数的神奇函数

mcsee.medium.com](https://mcsee.medium.com/code-smell-93-send-me-anything-c790248b0c4c) [](https://mcsee.medium.com/code-smell-95-premature-classification-3cc6ad27c27a) [## 气味代码 95 —过早分类

### 我们过于一般化了。在我们看到足够多的具体化之前，我们不应该创造抽象。

mcsee.medium.com](https://mcsee.medium.com/code-smell-95-premature-classification-3cc6ad27c27a) [](https://mcsee.medium.com/code-smell-96-my-objects-69fe32c670ad) [## 代码气味 96 —我的物品

### 你没有自己的物品。

mcsee.medium.com](https://mcsee.medium.com/code-smell-96-my-objects-69fe32c670ad) [](/code-smell-97-error-messages-without-empathy-952c320976d8) [## 代码气味 97——没有同理心的错误消息

### 我们应该特别注意用户(和我们自己)的错误描述。

blog.devgenius.io](/code-smell-97-error-messages-without-empathy-952c320976d8) [](/code-smell-98-speling-mistakes-680978dace1c) [## 代码气味 98 —拼写错误

### 拼写和可读性对人类非常重要，对机器不重要。

blog.devgenius.io](/code-smell-98-speling-mistakes-680978dace1c) [](/code-smell-99-first-second-7006b0822a61) [## 气味代码 99 —第一秒

### 我们有多少次看到懒惰的参数名？

blog.devgenius.io](/code-smell-99-first-second-7006b0822a61) [](/code-smell-100-goto-f89a31f5c902) [## 代码气味 100 —转到

### 50 年前，后藤被认为是有害的

blog.devgenius.io](/code-smell-100-goto-f89a31f5c902) [](/code-smell-101-comparison-against-booleans-958b112a560) [## 代码气味 101 —与布尔比较

### 当与布尔比较时，我们执行魔法转换并得到意想不到的结果

blog.devgenius.io](/code-smell-101-comparison-against-booleans-958b112a560) [](/code-smell-102-arrow-code-3e9301e83269) [## 代码气味 102 —箭头代码

### 嵌套的 if 和 Elses 很难阅读和测试

blog.devgenius.io](/code-smell-102-arrow-code-3e9301e83269) [](/code-smell-103-double-encapsulation-32c5d0978988) [## 代码气味 103 —双重封装

### 调用我们自己的访问器方法似乎是一个很好的封装想法。但事实并非如此。

blog.devgenius.io](/code-smell-103-double-encapsulation-32c5d0978988) [](/code-smell-104-assert-true-ec6a9ef6dafd) [## 代码气味 104 —断言为真

### 针对布尔值进行断言会增加错误跟踪的难度。

blog.devgenius.io](/code-smell-104-assert-true-ec6a9ef6dafd) [](/code-smell-105-comedian-methods-d96d356693bd) [## 代码气味 105 —喜剧演员的方法

### 使用专业和有意义的名字

blog.devgenius.io](/code-smell-105-comedian-methods-d96d356693bd) [](/code-smell-106-production-dependent-code-f74dbbfde0cf) [## 代码气味 106 —依赖于生产的代码

### 不要为生产环境添加 IFs 检查。

blog.devgenius.io](/code-smell-106-production-dependent-code-f74dbbfde0cf) [](/code-smell-107-variables-reuse-222a15b1a819) [## 代码味道 107 —变量重用

### 重用变量使得作用域和边界更难遵循

blog.devgenius.io](/code-smell-107-variables-reuse-222a15b1a819) [](/code-smell-108-float-assertions-ce806da449bb) [## 代码气味 108 —浮点断言

### 断言两个浮点数是相同的是一个非常困难的问题

blog.devgenius.io](/code-smell-108-float-assertions-ce806da449bb) [](/code-smell-109-automatic-properties-8d7b3a956aff) [## 代码气味 109 —自动属性

### 如果把 4 种代码气味结合在一起会怎么样？

blog.devgenius.io](/code-smell-109-automatic-properties-8d7b3a956aff) [](/code-smell-110-switches-with-defaults-7884aa11b1d0) [## 代码气味 110 —带默认值的开关

### 默认的意思是‘我们还不知道的一切’。我们无法预见未来。

blog.devgenius.io](/code-smell-110-switches-with-defaults-7884aa11b1d0) [](/code-smell-111-modifying-collections-while-traversing-b453783ab983) [## 代码味道 111 —遍历时修改集合

### 在遍历时更改集合可能会导致意外错误

blog.devgenius.io](/code-smell-111-modifying-collections-while-traversing-b453783ab983) [](/code-smell-112-testing-private-methods-ef812f521a36) [## 代码气味 112 —测试私有方法

### 如果你从事单元测试，你迟早会面临这个困境

blog.devgenius.io](/code-smell-112-testing-private-methods-ef812f521a36) [](https://mcsee.medium.com/code-smell-113-data-naming-1255bf528465) [## 代码味道 113 —数据命名

### 使用实体域名来建模实体域对象。

mcsee.medium.com](https://mcsee.medium.com/code-smell-113-data-naming-1255bf528465) [](https://mcsee.medium.com/code-smell-114-empty-class-66041acd0c43) [## 代码气味 114 —空类

### 你遇到过没有行为的班级吗？上课是他们的行为。

mcsee.medium.com](https://mcsee.medium.com/code-smell-114-empty-class-66041acd0c43) [](https://mcsee.medium.com/code-smell-115-return-true-7d457c1237af) [## 代码气味 115 —返回真

### 布尔是自然的代码气味。返回并转换它们有时是一个错误

mcsee.medium.com](https://mcsee.medium.com/code-smell-115-return-true-7d457c1237af) [](https://mcsee.medium.com/code-smell-116-variables-declared-with-var-1c9ecf42bb9f) [## 代码味道 116 —用“var”声明的变量

### Var，Let，Const:都一样，不是吗？

mcsee.medium.com](https://mcsee.medium.com/code-smell-116-variables-declared-with-var-1c9ecf42bb9f) [](https://mcsee.medium.com/code-smell-117-unrealistic-data-145ae3de5ff8) [## 代码味道 117 —不切实际的数据

### 程序员很懒，很少尝试从真实的业务领域中学习

mcsee.medium.com](https://mcsee.medium.com/code-smell-117-unrealistic-data-145ae3de5ff8) [](https://mcsee.medium.com/code-smell-118-return-false-966202352d73) [## 代码气味 118 —返回 False

### 检查布尔条件以返回布尔值是不方便的

mcsee.medium.com](https://mcsee.medium.com/code-smell-118-return-false-966202352d73) [](https://mcsee.medium.com/code-smell-119-stairs-code-5e0a865b3d2a) [## 气味代码 119 —楼梯代码

### 嵌套布尔条件表达了一个业务规则。不是如果

mcsee.medium.com](https://mcsee.medium.com/code-smell-119-stairs-code-5e0a865b3d2a) [](https://mcsee.medium.com/code-smell-120-sequential-ids-512e29ad31a0) [## 代码气味 120 —顺序 id

### 大多数 id 都是代码气味。顺序 id 也是一个漏洞

mcsee.medium.com](https://mcsee.medium.com/code-smell-120-sequential-ids-512e29ad31a0) [](https://mcsee.medium.com/code-smell-121-string-validations-ecc7d9d04b73) [## 代码气味 121 —字符串验证

### 您需要验证字符串。所以你根本不需要绳子

mcsee.medium.com](https://mcsee.medium.com/code-smell-121-string-validations-ecc7d9d04b73) [](https://mcsee.medium.com/code-smell-122-primitive-obsession-ad1f821f7158) [## 代码气味 122——原始痴迷

### 物品就在那里等待挑选。即使是最小的。

mcsee.medium.com](https://mcsee.medium.com/code-smell-122-primitive-obsession-ad1f821f7158) [](https://mcsee.medium.com/code-smell-123-mixed-what-and-how-330f48036f3d) [## 代码气味 123 —混合了“什么”和“如何”

### 我们喜欢观察时钟的内部齿轮，但我们需要开始关注指针。

mcsee.medium.com](https://mcsee.medium.com/code-smell-123-mixed-what-and-how-330f48036f3d) [](https://mcsee.medium.com/code-smell-124-divergent-change-48825bcc74d0) [## 代码气味 124 —不同的变化

### 你在课堂上改变了一些东西。你在同一个类中改变一些不相关的东西

mcsee.medium.com](https://mcsee.medium.com/code-smell-124-divergent-change-48825bcc74d0) [](https://mcsee.medium.com/code-smell-125-is-a-relationship-3de263ab437d) [## 代码气味 125 —“是-是”关系

### 我们在学校学过，遗传代表一种“是-是”的关系。它不是。

mcsee.medium.com](https://mcsee.medium.com/code-smell-125-is-a-relationship-3de263ab437d) [](https://mcsee.medium.com/code-smell-126-fake-null-object-25f4bac926fd) [## 代码气味 126 —假的空对象

### 空对象是十亿美元错误的绝佳替代品。有时候我们不需要它们

mcsee.medium.com](https://mcsee.medium.com/code-smell-126-fake-null-object-25f4bac926fd) [](https://mcsee.medium.com/code-smell-127-mutable-constants-a43e26c60d5b) [## 代码气味 127 —可变常量

### 你声明一个常数。但是你可以让它变异。

mcsee.medium.com](https://mcsee.medium.com/code-smell-127-mutable-constants-a43e26c60d5b) [](https://mcsee.medium.com/code-smell-128-non-english-coding-3ce32d1f6ca6) [## 代码气味 128 —非英语编码

### 使用本地语言是一个好主意，因为域名命名更容易。不

mcsee.medium.com](https://mcsee.medium.com/code-smell-128-non-english-coding-3ce32d1f6ca6) [](https://mcsee.medium.com/code-smell-129-structural-optimizations-cf16b371acf0) [## 代码味道 129 —结构优化

### 我们喜欢通过猜测不真实的场景来提高时间和空间的复杂性

mcsee.medium.com](https://mcsee.medium.com/code-smell-129-structural-optimizations-cf16b371acf0) [](https://mcsee.medium.com/code-smell-130-addressimpl-89f5209870a7) [## 代码气味 130 —地址

### 很高兴看到一个实现接口的类。更好的是理解它做什么

mcsee.medium.com](https://mcsee.medium.com/code-smell-130-addressimpl-89f5209870a7) [](https://mcsee.medium.com/code-smell-131-zero-argument-constructor-b48b518aa9e4) [## 代码气味 131 —零参数构造函数

### 不带参数创建的对象通常是可变的和不稳定的

mcsee.medium.com](https://mcsee.medium.com/code-smell-131-zero-argument-constructor-b48b518aa9e4) [](https://mcsee.medium.com/code-smell-132-exception-try-too-broad-6e245f910f9b) [## 代码气味 132 —异常尝试范围太广

### 例外很方便。但是应该尽可能窄

mcsee.medium.com](https://mcsee.medium.com/code-smell-132-exception-try-too-broad-6e245f910f9b) [](https://levelup.gitconnected.com/code-smell-133-hardcoded-if-conditions-8cc7398fbe90) [## 代码气味 133 —硬编码 IF 条件

### 硬编码没问题。在短时间内

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-133-hardcoded-if-conditions-8cc7398fbe90) [](https://levelup.gitconnected.com/code-smell-134-specialized-business-collections-ce63d851792b) [## 代码气味 134 —专业商务系列

### 如果它走路像鸭子，叫声像鸭子，那么它一定是一只鸭子

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-134-specialized-business-collections-ce63d851792b) [](https://levelup.gitconnected.com/code-smell-135-interfaces-with-just-one-realization-9609f377e3e2) [## 代码气味 135 —只与一个实现接口

### 通用和预见未来是好的。

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-135-interfaces-with-just-one-realization-9609f377e3e2) [](https://levelup.gitconnected.com/code-smell-136-classes-with-just-one-subclass-9406951f9e16) [## 代码气味 136 —只有一个子类的类

### 通用和预见未来是好的(再次)。

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-136-classes-with-just-one-subclass-9406951f9e16) [](https://levelup.gitconnected.com/code-smell-137-inheritance-tree-too-deep-541590a56bd5) [## 代码味道 137 —继承树太深

### 又一个糟糕的代码重用症状

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-137-inheritance-tree-too-deep-541590a56bd5) [](https://levelup.gitconnected.com/code-smell-138-packages-dependency-af3825b14438) [## 代码味道 138 —包依赖性

### 行业趋势是尽可能避免编写代码。但这不是免费的

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-138-packages-dependency-af3825b14438) [](https://levelup.gitconnected.com/code-smell-139-business-code-in-the-user-interface-83b0941b657a) [## 代码味道 139 —用户界面中的业务代码

### 验证是否应该在接口上？

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-139-business-code-in-the-user-interface-83b0941b657a) [](https://medium.com/javarevisited/code-smell-140-short-circuit-evaluation-8afa1015ae11) [## 代码气味 140 —短路评估

### 我们在最初的编程课程中学习短路。我们需要记住为什么。

medium.com](https://medium.com/javarevisited/code-smell-140-short-circuit-evaluation-8afa1015ae11) [](https://levelup.gitconnected.com/code-smell-141-iengine-avehicle-implcar-a23ff338299f) [## 气味代码 141 —发动机、车辆、汽车

### 你在野外见过工程师吗？

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-141-iengine-avehicle-implcar-a23ff338299f) [](https://levelup.gitconnected.com/code-smell-142-queries-in-constructors-c61e359567b1) [## 代码味道 142 —构造函数中的查询

### 在域对象中访问数据库是一种代码味道。在构造函数中做这件事是一个双重问题

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-142-queries-in-constructors-c61e359567b1) [](https://levelup.gitconnected.com/code-smell-143-data-clumps-86cfe2f36a19) [## 代码气味 143 —数据块

### 有些物体总是在一起。我们为什么要分开他们？

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-143-data-clumps-86cfe2f36a19) [](https://levelup.gitconnected.com/code-smell-144-fungible-objects-c5b014137f73) [## 代码气味 144 —可替换对象

### 我们听说了很多关于非功能性测试的事情。现在我们掌握了可替代的概念

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-144-fungible-objects-c5b014137f73) [](https://mcsee.medium.com/code-smell-145-short-circuit-hack-606ffb351c2d) [## 代码气味 145 —短路黑客

### 不要使用布尔计算作为可读性的捷径

mcsee.medium.com](https://mcsee.medium.com/code-smell-145-short-circuit-hack-606ffb351c2d) [](https://levelup.gitconnected.com/code-smell-146-getter-comments-ebada3aa5d08) [## 代码气味 146 — Getter 注释

### 评论是一种代码味道。Getters 是另一种代码味道。你猜怎么着？

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-146-getter-comments-ebada3aa5d08) [](https://medium.com/javarevisited/code-smell-147-too-many-methods-5da19bc3427b) [## 代码味道 147 —方法太多

### Util 类非常适合收集协议

medium.com](https://medium.com/javarevisited/code-smell-147-too-many-methods-5da19bc3427b) [](https://levelup.gitconnected.com/code-smell-148-todos-5a23d024ad70) [## 气味代码 148 — ToDos

### 我们为未来的自己购买债务。是回报的时候了

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-148-todos-5a23d024ad70) [](https://levelup.gitconnected.com/code-smell-149-optional-chaining-b8830d7206ae) [## 代码气味 149 —可选链接

### 我们的代码更加健壮和易读。但是我们把空藏在地毯下

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-149-optional-chaining-b8830d7206ae) [](https://levelup.gitconnected.com/code-smell-150-equal-comparison-4a3d1eed823d) [## 代码气味 150 —同等比较

### 每个开发人员都平等地比较属性。他们错了。

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-150-equal-comparison-4a3d1eed823d) [](https://levelup.gitconnected.com/code-smell-151-commented-code-3a6feaeedb93) [## 代码气味 151 —注释代码

### 新手不敢拆代码。许多老年人也是如此。

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-151-commented-code-3a6feaeedb93) [](https://levelup.gitconnected.com/code-smell-152-logical-comment-750eadc84c09) [## 代码气味 152 —逻辑注释

### 暂时的攻击可能是永久的

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-152-logical-comment-750eadc84c09) [](https://levelup.gitconnected.com/code-smell-153-too-long-names-64d586d6cd31) [## 代码气味 153 —太长的名字

### 名称应该长且具有描述性。多久了？

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-153-too-long-names-64d586d6cd31) [](https://levelup.gitconnected.com/code-smell-154-too-many-variables-2d1c0eec8ac2) [## 代码气味 154 —变量太多

### 您调试代码，看到太多的变量声明和活动

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-154-too-many-variables-2d1c0eec8ac2) [](https://levelup.gitconnected.com/code-smell-155-multiple-promises-67cccd8795c) [## 代码气味 155 —多个承诺

### 你有承诺。你需要等待。等他们都来

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-155-multiple-promises-67cccd8795c) [](https://levelup.gitconnected.com/code-smell-156-implicit-else-2e5c96e64c0c) [## 代码气味 156 —隐式 Else

### 我们在第一个编程日学习 if/else。然后我们忘记其他的

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-156-implicit-else-2e5c96e64c0c) [](https://levelup.gitconnected.com/code-smell-157-balance-at-0-eefc9bf7c061) [## 代码气味 157 —平衡值为 0

### 今天我在钱包里期待一笔付款。余额为 0。我慌了。

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-157-balance-at-0-eefc9bf7c061) [](https://mcsee.medium.com/code-smell-158-variables-not-variable-2d7ccae484e5) [## 代码气味 158 —变量不是变量

### 你给一个变量赋值并使用它，但从不改变它

mcsee.medium.com](https://mcsee.medium.com/code-smell-158-variables-not-variable-2d7ccae484e5) [](https://levelup.gitconnected.com/code-smell-159-mixed-case-acb9f2b8616) [## 代码气味 159 —混合 _ 案例

### 严肃的开发是由许多不同的人完成的。我们必须开始达成一致。

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-159-mixed-case-acb9f2b8616) [](https://mcsee.medium.com/code-smell-160-invalid-id-9999-bbc9aaba7464) [## 代码气味 160 —无效 Id = 9999

### 对于无效 id 来说，Maxint 是一个非常好的数字。我们永远也到不了。

mcsee.medium.com](https://mcsee.medium.com/code-smell-160-invalid-id-9999-bbc9aaba7464) [](https://levelup.gitconnected.com/code-smell-161-abstract-final-undefined-classes-3b3acaefb29f) [## 代码气味 161 —抽象/最终/未定义的类

### 你的类是抽象的，最终的或未定义的

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-161-abstract-final-undefined-classes-3b3acaefb29f) [](https://mcsee.medium.com/code-smell-162-too-many-parentheses-49f624076da7) [## 代码气味 162 —括号太多

### 括号是免费的。不是吗？

mcsee.medium.com](https://mcsee.medium.com/code-smell-162-too-many-parentheses-49f624076da7) [](https://levelup.gitconnected.com/code-smell-163-collection-in-name-9b1a92d57920) [## 气味代码 163——名义上的收藏

### 你见过顾客收藏吗？

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-163-collection-in-name-9b1a92d57920) [](https://levelup.gitconnected.com/code-smell-164-mixed-indentations-a328947b9a7e) [## 代码气味 164 —混合压痕

### 制表符与空格。最重要的计算机问题

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-164-mixed-indentations-a328947b9a7e) [](https://levelup.gitconnected.com/code-smell-165-empty-exception-blocks-ad13df47ffc9) [## 代码气味 165 —空异常块

### 接下来是我在第一份工作中学到的第一件事

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-165-empty-exception-blocks-ad13df47ffc9) [](https://levelup.gitconnected.com/code-smell-166-low-level-errors-on-user-interface-ed9b57ee7951) [## 代码气味 166 —用户界面上的低级错误

### 致命错误:未捕获的错误:在/var/www/html/query-line . PHP:78 堆栈跟踪中找不到类“logs _ queries _ web:# 0…

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-166-low-level-errors-on-user-interface-ed9b57ee7951) [](https://mcsee.medium.com/code-smell-167-hashing-comparison-dcc321453af1) [## 代码气味 167 —哈希比较

### 哈希保证两个对象是不同的。并不是说它们是一样的

mcsee.medium.com](https://mcsee.medium.com/code-smell-167-hashing-comparison-dcc321453af1) [](https://levelup.gitconnected.com/code-smell-168-undocumented-decisions-c63205d35dd0) [## 代码气味 168 —未记录的决策

### 我们需要做一些改变。我们需要弄清楚为什么

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-168-undocumented-decisions-c63205d35dd0) [](https://levelup.gitconnected.com/code-smell-169-glued-methods-eeca156171df) [## 代码气味 169 —粘合方法

### 不要同时做两件或两件以上的事情。

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-169-glued-methods-eeca156171df) [](https://levelup.gitconnected.com/code-smell-170-refactor-with-functional-changes-5569a110902d) [## 代码味道 170 —通过功能变化进行重构

### 发展是伟大的。重构很神奇。不要同时做

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-170-refactor-with-functional-changes-5569a110902d) [](https://mcsee.medium.com/code-smell-171-plural-classes-564b2d7a40fe) [## 代码气味 171 —复数类

### 课程是我的宝贝

mcsee.medium.com](https://mcsee.medium.com/code-smell-171-plural-classes-564b2d7a40fe) [](https://levelup.gitconnected.com/code-smell-172-default-argument-values-not-last-484d6efaee72) [## 代码气味 172 —默认参数值不持久

### 应该对函数签名进行错误删除

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-172-default-argument-values-not-last-484d6efaee72) [](https://levelup.gitconnected.com/code-smell-174-broken-windows-ee74f2af2ff2) [## 气味代码 174 —破碎的窗户

### 离开露营地时，一定要比你发现时干净。“如果你发现地上一片狼藉，你会不顾一切地清理干净…

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-174-broken-windows-ee74f2af2ff2) [](https://levelup.gitconnected.com/code-smell-174-class-name-in-attributes-f45bbccde106) [## 代码气味 174 —属性中的类名

### 名字中的冗余是一种不好的气味。名字应该是上下文相关的

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-174-class-name-in-attributes-f45bbccde106) [](https://levelup.gitconnected.com/code-smell-175-changes-without-coverage-bd04eded060b) [## 代码味道 175 —没有覆盖的更改

### 如果您的合并请求没有测试变更，那么您还没有完成您的工作

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-175-changes-without-coverage-bd04eded060b) [](https://levelup.gitconnected.com/code-smell-176-changes-in-essence-433898f2e77c) [## 代码气味 176 —本质上的变化

### 变异是好事。事情变了

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-176-changes-in-essence-433898f2e77c) [](https://levelup.gitconnected.com/code-smell-177-missing-small-objects-747833a30446) [## 代码气味 177 —丢失小物件

### 我们到处都能看到小的原始数据

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-177-missing-small-objects-747833a30446) [](https://levelup.gitconnected.com/code-smell-178-subsets-violation-3b3b9cb15d85) [## 代码气味 178 —违反子集

### 不可见的物体有规则，我们需要在一个点上执行

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-178-subsets-violation-3b3b9cb15d85) [](https://levelup.gitconnected.com/code-smell-179-known-bugs-e7e60d17d99f) [## 代码气味 179 —已知错误

### 每个软件都有一个已知缺陷列表。为什么？

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-179-known-bugs-e7e60d17d99f) [](https://levelup.gitconnected.com/code-smell-180-bitwise-optimizations-cfa684aa47b1) [## 代码气味 180 —逐位优化

### 按位运算符更快。避免这些微优化

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-180-bitwise-optimizations-cfa684aa47b1) [](https://levelup.gitconnected.com/code-smell-181-nested-classes-5388b8cabc69) [## 代码味道 181 —嵌套类

### 嵌套类或伪私有类似乎非常适合隐藏实现细节。

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-181-nested-classes-5388b8cabc69) [](https://levelup.gitconnected.com/code-smell-182-over-generalization-4743e4f2ea90) [## 代码气味 182 —过度概括

### 我们是重构粉丝。有时候我们需要停下来思考。

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-182-over-generalization-4743e4f2ea90) [](https://levelup.gitconnected.com/code-smell-183-obsolete-comments-db946fffc766) [## 代码气味 183 —过时的注释

### 评论是一种代码味道。过时的注释是一种真正的危险，没有人维护那些不能执行的东西。

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-183-obsolete-comments-db946fffc766) [](https://levelup.gitconnected.com/code-smell-184-exception-arrow-code-ae357f8d9ae2) [## 代码气味 184 —异常箭头代码

### 箭头代码是一种代码气味。污染是另一个例外。这是一个致命的组合。

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-184-exception-arrow-code-ae357f8d9ae2) [](https://levelup.gitconnected.com/code-smell-185-evil-regular-expressions-942c187d821b) [## 代码气味 185 —邪恶的正则表达式

### 正则表达式是一种代码味道。有时也是一个弱点

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-185-evil-regular-expressions-942c187d821b) [](https://levelup.gitconnected.com/code-smell-186-hardcoded-business-conditions-e0a22e6e8e68) [## 代码气味 186 —硬编码的业务条件

### 你是 FTX，你的代码允许特殊情况

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-186-hardcoded-business-conditions-e0a22e6e8e68) [](https://levelup.gitconnected.com/%E3%84%A58%C9%A9-ll%C7%9D%C9%AFs-%C7%9Dpo%C9%94-sp%C9%B9%C9%90%CA%8D%CA%9E%C9%94%C9%90q-%C7%9Dsl%C7%9D-%E2%85%8Ei-beab7a040d66) [## ㄥ8ɩllǝɯsǝpoɔ—spɹɐʍʞɔɐq ǝslǝ/ⅎi

### 我们在 if 条件之后读到的第一件事是 IF

levelup.gitconnected.com](https://levelup.gitconnected.com/%E3%84%A58%C9%A9-ll%C7%9D%C9%AFs-%C7%9Dpo%C9%94-sp%C9%B9%C9%90%CA%8D%CA%9E%C9%94%C9%90q-%C7%9Dsl%C7%9D-%E2%85%8Ei-beab7a040d66) [](https://levelup.gitconnected.com/code-smell-188-redundant-parameter-names-9ba6a81a99b) [## 代码气味 188 —冗余参数名

### 使用上下文名称和本地名称

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-188-redundant-parameter-names-9ba6a81a99b) [](https://levelup.gitconnected.com/code-smell-189-not-sanitized-input-f94d51aebacb) [## 代码气味 189 -未净化的输入

### 坏演员在那里。我们需要非常小心他们的投入。

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-189-not-sanitized-input-f94d51aebacb) [](https://levelup.gitconnected.com/code-smell-190-unnecessary-properties-b93a1ca474b1) [## 代码气味 190-不必要的属性

### 不要把数据当成属性。他们只是需要支持你的行为

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-190-unnecessary-properties-b93a1ca474b1) [](https://levelup.gitconnected.com/code-smell-191-misplaced-responsibility-585aeeaa3206) [## 代码气味 191 —错误的责任

### 你对错误的对象负有明确的责任

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-191-misplaced-responsibility-585aeeaa3206) [](https://levelup.gitconnected.com/code-smell-192-optional-attributes-8e6e05ba5146) [## 代码气味 192 —可选属性

### 你需要建模一些可选的东西。你试过收藏吗？

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-192-optional-attributes-8e6e05ba5146) [](https://levelup.gitconnected.com/code-smell-193-switch-instead-of-formula-14aaad750bcf) [## 代码气味 193 -开关而不是公式

### 声明式代码和更短的代码哪个更好？

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-193-switch-instead-of-formula-14aaad750bcf) [](https://levelup.gitconnected.com/code-smell-194-missing-interval-9b92541357d) [## 代码气味 194 —缺少间隔

### 开始日期应小于结束日期

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-194-missing-interval-9b92541357d) [](https://levelup.gitconnected.com/code-smell-195-yoda-conditions-209612304218) [## 气味代码 195 —尤达状况

### 最好是把期望值放在最后，如果你想写条件的话。

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-195-yoda-conditions-209612304218) [](https://levelup.gitconnected.com/code-smell-196-javascript-array-constructors-762507beb67f) [## 代码味道 196 - Javascript 数组构造函数

### 数组创建是同质的、可预测的，还是一种黑客行为？

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-196-javascript-array-constructors-762507beb67f) [](https://levelup.gitconnected.com/code-smell-197-gratuitous-context-39c82eb0a5cb) [## 代码气味 197 -无端上下文

### 这是标记“你的”类和对象的好方法

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-197-gratuitous-context-39c82eb0a5cb) [](https://levelup.gitconnected.com/code-smell-198-hidden-assumptions-3e5198bbf3e5) [## 代码气味 198 —隐藏的假设

### 软件是关于合同的，模糊的合同是一场噩梦

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-198-hidden-assumptions-3e5198bbf3e5) [](https://levelup.gitconnected.com/code-smell-199-gratuitous-booleans-a1a5a16d9919) [## 代码气味 199 —无偿布尔

### 要么返回要么根本不返回的代码

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-199-gratuitous-booleans-a1a5a16d9919) [](https://levelup.gitconnected.com/code-smell-200-poltergeist-393117966a9c) [## 代码气味 200 —恶作剧鬼

### 神秘地出现又消失的物体

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-200-poltergeist-393117966a9c) [](https://levelup.gitconnected.com/code-smell-201-nested-ternaries-abe2f38c883c) [## 代码气味 201 -嵌套 Ternaries

### 箭头代码、嵌套条件、开关、else 等等

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-201-nested-ternaries-abe2f38c883c) [](https://levelup.gitconnected.com/code-smell-202-god-constant-class-3871e53b52cb) [## 代码气味 202 -神常数类

### 常数应该放在一起，以便容易找到它们

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-202-god-constant-class-3871e53b52cb) [](https://levelup.gitconnected.com/code-smell-203-irrelevant-test-information-1a70ae62b06f) [## 代码气味 203 -无关的测试信息

### 无关的数据会分散读者的注意力

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-203-irrelevant-test-information-1a70ae62b06f) [](https://levelup.gitconnected.com/code-smell-204-tests-depending-on-dates-7dc923c2b82) [## 代码气味 204 -根据日期进行测试

### 断言未来发生的事情是一个好主意

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-204-tests-depending-on-dates-7dc923c2b82) 

…还会有更多。

![](img/0b79edb972b70d42ff0eabd8c202ea6d.png)

> 气味是代码中的某些结构，它们暗示(有时它们强烈要求)重构的可能性。
> 
> 马丁·福勒

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

这一系列文章的部分目标是为软件设计的辩论和讨论提供空间。

[](https://mcsee.medium.com/object-design-checklist-47c63d351352) [## 目标设计清单

### 这是已经发表的软件设计文章的索引。

mcsee.medium.com](https://mcsee.medium.com/object-design-checklist-47c63d351352) 

我们期待着对这篇文章的评论和建议。