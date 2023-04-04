# 名字到底是什么？第二部分:康复

> 原文：<https://blog.devgenius.io/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1?source=collection_archive---------6----------------------->

## 我们都同意:好名声永远是最重要的。让我们找到他们。

*我们用名字来编程，不管语言是高级还是低级，不管是命令式的、函数式的还是面向对象的。到处都是名字。但是我们继续滥用它们。在第二部分，我们将看到如何改变一些习惯。*

# 搜索仍在进行中

在上一篇文章中，我们介绍了寻找好名字的各种定义和技术。在这份报告中，我们将试图指出目前在命名方面存在的一些问题，以便改进我们的实践。

[](https://medium.com/dev-genius/what-exactly-is-a-name-part-i-the-quest-b812a4b1e0bf) [## 名字到底是什么？—第一部分:探索

### 我们都同意:好名声永远是最重要的。让我们找到他们。

medium.com](https://medium.com/dev-genius/what-exactly-is-a-name-part-i-the-quest-b812a4b1e0bf) 

## 我们不需要帮助

所有对象都有帮助，没有“不支持”的对象。

> 在现实世界中，没有帮手。

我们有一个单一的设计规则。如果一个概念在现实世界中不存在，并且我们不能向领域专家解释它，那么这个对象一定不存在。

![](img/1c84e115c34a282e2dda048e992a8e2f.png)

[大 Dodzy](https://unsplash.com/@bigdodzy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/baywatch?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

> 规则 9:帮手不存在

[](https://mcsee.medium.com/code-smell-22-helpers-1d5057137ddb) [## 代码气味 22 —助手

### 你需要帮助吗？你要给谁打电话？

mcsee.medium.com](https://mcsee.medium.com/code-smell-22-helpers-1d5057137ddb) 

## 我们不需要老板

所有物体生来平等。我们设计的对象会有[社会平等](https://en.wikipedia.org/wiki/Social_equality)。没有经理。有不同责任的对象。

在现实世界中，没有经理(除非我们正在模拟工作角色)。

> 规则 10:经理是不存在的。

![](img/108d1ba61a6f971c9bb60ea0fa7e0ff9.png)

照片由 Amy Hirschi 在 [Unsplash](https://unsplash.com/s/photos/boss?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 我们不需要说“物体”

物体无所不在。通过说 Object…来命名一个类是一种类似上面提到的代码味道。

> 规则 11:物体不存在。他们都是。

## [你们所有的基地都是属于我们的](https://es.wikipedia.org/wiki/All_your_base_are_belong_to_us)

除非我们正在建模一个空间或军事系统，否则我们不应该用 Base 来命名我们的类。

*Base* 代表现实世界抽象概念的缺失，以及我们通过继承重用代码的机会。又一个代码味道。

这违背了设计原则，即我们更喜欢对象的(动态)组合，而不是类的(静态)继承。

> 规则 12:基本对象不存在。

这些名字都很不好。有[个幽默网站](https://projects.haykranen.nl/java/)自动生成。

## 我们不需要访问器

正如我们在以前的文章中看到的，拥有*设置器*和*获取器*会导致封装冲突和责任的错误分配。我们应该对所有的 *getXXX()* 或者 *setXXX()* 函数持怀疑态度。

我们通常不会在现实世界的领域实体中发现这些职责。业务模型中没有真正的 *set()* 和 *get()* 职责。

[](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3) [## 裸体模特第一部分:模特

### 旧的可靠数据结构及其有争议的(写)访问。

medium.com](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3) 

在将责任与属性匹配的偶然情况下，我们将以相同的方式调用函数，但不使用 *get()* 前缀。

[](https://medium.com/dev-genius/nude-models-part-ii-getters-b039e5ad3427) [## 裸体模特第二部分:吸气剂

### 旧的可靠数据结构及其有争议的(读)访问。

medium.com](https://medium.com/dev-genius/nude-models-part-ii-getters-b039e5ad3427) 

> 规则 13:没有 setXXX 或 getXXX 方法。

## 不要问我是谁

以 *isXXXX()* 开头的函数通常是可实现的。

它们需要一个类型(避免双重分派模式)，生成耦合，后面总是跟一个 if。

作为一般规则，我们应该将布尔的使用限制在真实世界中存在这种布尔的情况下。

作为推论，思考映射器，不信任 *isXXX()* 方法。

> 规则 14:没有 isXXX()方法。

## 如果它闻起来像界面，那它就是界面

诸如 iterable、serializable 等名称。宣扬对象的责任。它们将是优秀的接口名称，因此我们不应该用它们来命名类。

> 规则 15:名称…能够为接口保留(因此它们不能被实例化)

## 池塘里的鸭子

鸭子总是出现在软件开发中。

还有[橡皮鸭调试](https://en.wikipedia.org/wiki/Rubber_duck_debugging)技术。这就是[鸭子打字](https://en.wikipedia.org/wiki/Duck_typing)技术

![](img/63f57fe4f2c402bac10314c059bdd17f.png)

照片由 [Jordan M. Lomibao](https://unsplash.com/@jlcruz_photography?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/rubber-duck?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

> 当我看到一只像鸭子一样走路的鸟，它像鸭子一样游泳，听起来像鸭子，我就把那只鸟叫做鸭子。
> 
> 詹姆斯·惠特科姆·赖利

这种技术表明，我们通过对象的职责和它们的上下文角色来了解对象。一个推论是，我们应该根据这种责任来命名物体。

> 规则 16:观察行为后使用名字。

## 命名模式

已知设计模式的最大好处是统一了一种通用语言。

我们都知道什么是*装饰*、什么是*策略*、什么是*适配器*或者什么是*门面*。我们知道您永远不应该使用*单线*:

[](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) [## 单身:万恶之源

### 允许的全局变量和假定的内存节省

codeburst.io](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) 

> 规则 17:使用模式名来实现概念。

## 如果它是抽象的，它就有一个抽象的名字。

我们的语言非常丰富，有许多单词来模拟概念。如果我们想使用亚里士多德的分类技术对概念进行分组，我们将使用这些名称给类命名。

这是展示继承的经典例子:

*{车、船、飞机}* 的普通超类不应是**抽象车**或**底盘车**，既不是**底盘船**也不是**可移动**。

应为:**车辆**。

> 规则 18:必须发现抽象的名字。不是发明的。
> 
> 推论 18:不要用抽象这个词作为名字的一部分。

## 责任是最好的名字

命名一个类的最好规则是在双射中找到它的对应物。如果这很难，我们应该根据他们的责任来理解这个概念。

让我们看一个例子:

如果可以遍历，可以添加，也可以删除，它不是**数组帮助器**，不是**数组管理器**，不是**基数组**，更不是**数组对象**。

例如:分组时:*数组*、*集合*、*链表*、多组、*堆栈*、*队列*等。我们可以推导出最能描述它们的词。

其主要职责是**收集**物品，因此我们有一个**系列**。

我们只有在了解了它的许多子类之后，才能使用利斯科夫替换原理(L 代表固体)来学习这个。

> 规则 19:为了命名概念，我们必须知道它们的协议。

## 我们说同一种语言

作为一名在大学任教的以西班牙语为母语的人，学生们经常问我们，他们是应该用西班牙语(商业语言)还是英语(编程语言的基础语言)来命名。

如果我们要与函数的多态性保持一致，就必须始终使用**相同的**语言。

为了使 *foreach()迭代器*对于所有可迭代对象都是多态的，它必须有英文名称。

如果我们用一个 **recorrer()** 函数*(西班牙语中的遍历)*创建一个对象，我们将失去多态性，我们将不得不耦合一个 If 规则。

> 20:代码必须用英语编写。

# 规则摘要

*   名称必须是声明性的，而不是实现性的。
*   名称必须与上下文相关。
*   不要混淆类型和角色。
*   不要使用 setters 或 getters。
*   不要使用不存在的术语，如经理，助手，基地。
*   不要使用过于笼统的术语，如 Object。
*   在分配姓名之前分配责任。
*   有疑问的时候，写上不好的名字。
*   避免评论。

这一系列文章的部分目标是为软件设计的辩论和讨论提供空间。

[](https://medium.com/@mcsee/object-design-checklist-47c63d351352) [## 目标设计清单

### 这是已经发表的软件设计文章的索引。

medium.com](https://medium.com/@mcsee/object-design-checklist-47c63d351352) 

我们期待着对这篇文章的评论和建议。

本文还有西班牙语版本[点击这里](https://medium.com/puntotech/qu%C3%A9-es-exactamente-un-nombre-parte-2-las-malas-costumbres-d85cb08c8ca7)。

你可以在推特上联系我。