# 浅谈技术债务

> 原文：<https://blog.devgenius.io/a-brief-talk-about-technical-debt-fcc50ee45c97?source=collection_archive---------32----------------------->

![](img/f732afabb17529161a7e81cfd11f9451.png)

# [什么是技术债？](http://localhost:8080/#what-is-technical-debt)

“技术债务”出自[沃德·坎宁安](https://en.wikipedia.org/wiki/Ward_Cunningham)之口，他首次用技术复杂度比率作为负债，简称“技术债务”。

软件开发是一个非常复杂的项目，所以很多人把“软件开发”当成“软件工程”。软件旨在服务于各种行业(金融、医疗、购物等。).因此，我们的程序员可能不完全理解某个领域，以便在我们的专业知识下正确地控制它。最终，软件架构将不可避免地导致大量的“技术债务”。

虽然技术债是不可避免的，但事实上这是一个量的问题。

# [技术债务是如何产生的？](https://pitayan.com/posts/technical-debt/#how-did-technical-debt-arise)

我认为技术债主要有三类。

[**债务凭证**](https://pitayan.com/posts/technical-debt/#doucment-debts)

*   [1。要求债务](https://pitayan.com/posts/technical-debt/#1-requirements-debts)
*   [2。开发文件债务](https://pitayan.com/posts/technical-debt/#2-development-document-debts)
*   [3。测试文件债务](https://pitayan.com/posts/technical-debt/#3-test-document-debts)

[**计划债务**](https://pitayan.com/posts/technical-debt/#program-debts)

*   [1。结构性债务](https://pitayan.com/posts/technical-debt/#1-structural-debts)
*   [2。编码债务](https://pitayan.com/posts/technical-debt/#2-coding-debts)
*   [3。业务逻辑债务](https://pitayan.com/posts/technical-debt/#3-business-logic-debts)

[**管理债务**](https://pitayan.com/posts/technical-debt/#management-debts)

*   [1。到期债务](https://pitayan.com/posts/technical-debt/#1-deadline-debts)
*   [2。周转债务](https://pitayan.com/posts/technical-debt/#2-turnover-debts)
*   [3。协同负债](https://pitayan.com/posts/technical-debt/#3-coordinated-liabilities)
*   [4。成本债务](https://pitayan.com/posts/technical-debt/#4-cost-debts)

# [欠债](https://pitayan.com/posts/technical-debt/#doucment-debts)

## [1。需求债务](https://pitayan.com/posts/technical-debt/#1-requirements-debts)

> *不懂需求分析的软件开发人员不是好的软件开发人员。*

如果出现问题，开发人员必须提供及时的反馈，并与上级或客户沟通。如果有些需求无法完成，那么他/她必须对这些需求保持怀疑，并适当地否定它们。

除了私下抱怨，找到正确的方式反馈问题并知道如何有效沟通总是更好。这样我们就可以让客户或领导者了解技术上的困难。

反过来，如果开发人员不了解项目需求却盲目开发项目。这将导致业务规范和开发之间的不匹配，并且由于错误，这将花费更多的时间甚至金钱。

## [2。开发文件债务](https://pitayan.com/posts/technical-debt/#2-development-document-debts)

这通常发生在开发文档不完整，或者文档中的功能与开发中的源代码不一致的时候。

有以下两个原因:

1.  (*)该项目没有开发文件。没有编码规范文件、接口文件等相关技术文件。仅仅看代码而不看文档已经够难的了，即使语义变量名和函数名很容易快速理解。*
2.  *( ***普通*** )项目没有更新开发文档。前期有文档，后来文档一直没有更新过，因为有些需求几乎是临时新增的。这导致项目后期迭代时会产生大量冗余的源代码。*

*开发文档是项目最系统、最全面的反映。理解项目的功能模块比直接看源代码更容易。因此，为了对其他团队成员有所帮助，保持开发文档的更新非常非常重要。*

## *[3。测试文档债务](https://pitayan.com/posts/technical-debt/#3-test-document-debts)*

*当项目的测试覆盖率低，测试用例不足时，就会出现这种情况。*

*在大多数公司，为了控制人力成本，软件测试是由软件开发工程师而不是专业的测试工程师来完成的。这往往忽略了一些软件漏洞，并埋下了一些技术债务。*

> **旁观者清。**

*如果一个公司不重视检测，那么结果肯定是不合格的产品。*

# *[项目债务](https://pitayan.com/posts/technical-debt/#program-debts)*

## *[1。结构性债务](https://pitayan.com/posts/technical-debt/#1-structural-debts)*

*前期对项目结构评估不足，导致项目组织不合理，耦合度高。这使得后期很难扩展和维护。*

*随着业务需求的不断增加，项目很难继续进行。如果不小心处理，很容易出现错误和漏洞。后来，人们发现修改源代码是极其困难的，我们必须从头开始。*

## *[2。编码债务](https://pitayan.com/posts/technical-debt/#2-coding-debts)*

*低编码质量使得开发团队很难协同工作。软件产品迭代的时候，会有一堆技术债。而且软件产品漏洞百出，难以维护。*

*以下是一些常见的情况:*

1.  *命名约定:没有命名约定，但命名松散。*
2.  *代码复杂性:太多的条件语句/太复杂的流控制/太多的代码嵌套(或回调地狱)*
3.  *代码耦合:参数、类和接口是高度耦合的*
4.  *代码行:有大量未使用的代码。*

*一个好的、统一的编码规范给项目迭代和维护带来很多好处。并且也有利于重构，在一定程度上减少技术债务。当然每个团队都有自己的标准。以上只是参考指标，不是唯一指标。*

## *[3。业务逻辑债务](https://pitayan.com/posts/technical-debt/#3-business-logic-debts)*

*如果我们每次都以机会主义的方法修补漏洞或漏洞，而没有深入思考我们的业务逻辑或彻底了解漏洞的原因，这种“债务”很可能会发生。*

*为了简单起见，我们通过简单的过度条件判断来修复代码漏洞，或者强制修改数据库中的用户数据。*

*这种不成功的一次性计划是不可取的，同时造成更多的技术债务。*

# *[管理债务](https://pitayan.com/posts/technical-debt/#management-debts)*

## *[1。到期债务](https://pitayan.com/posts/technical-debt/#1-deadline-debts)*

*项目的期限也是技术债的原因之一。现在的项目，多是以项目做好之后快速拿钱为目标。*

*为了抢占市场份额，公司都想在短期内生产出产品，所以软件开发者必须只能使用一些旧的解决方案来加快开发速度。*

*开发出来的产品质量和之前相差不大，所以软件生命周期和之前一样短。*

## *[2。周转债务](https://pitayan.com/posts/technical-debt/#2-turnover-debts)*

*高流动性的团队使得项目开发变得困难或缓慢。更何况团队中开发人员的能力不同，各有风格。尽管代码风格是标准化的，但是每个人都以他们已经习惯的方式编程。*

*由于团队成员的失误，技术债务必须增加。*

## *[3。协调债务](https://pitayan.com/posts/technical-debt/#3-coordinated-debts)*

*我们需要让开发团队的人知道:“我是谁，我在哪里，我在做什么。”*

*有两件事值得注意:*

1.  *管理者必须充分了解自己的定位，做好自己该做的事情，不要过多干涉团队成员的工作，而是要监控团队成员的工作质量。*
2.  *管理者必须认真分配团队成员的发展任务，并明确划分任务和职责。避免任务重复。*

*良好的合作可以避免一些重复性的任务，减少软件冗余代码。另一方面，它将事半功倍地提高项目开发效率，并确保项目如期进行。*

## *[4。成本债务](https://pitayan.com/posts/technical-debt/#4-cost-debts)*

*硬件和软件环境决定了项目的成本负债。只有控制成本，我们才能获得更多的利益。聪明的管理者绝不会贪小便宜。着眼于眼前的利益会导致以后更大的成本问题。*

*需要钱的时候就花，不要吝啬。*

# *[技术债需要还吗？](https://pitayan.com/posts/technical-debt/#do-technical-debts-need-to-be-repaid)*

> **是的。当然可以。**

*我们无法避免技术债务。但是不还技术债的成本更高。*

*通过偿还技术债务，我们可以避免软件漏洞并改进软件功能。更重要的是，我们不需要让队友加班太多。*

*如果我们不这样做，我们当然不能支持大规模的项目需求。基于当前的业务逻辑重构源代码会有一定的技术风险。*

*是否还债由你决定。*

*总之，一个有经验的优秀软件工程师，绝不会轻易背负过多的技术债务。就算遇到技术债，不管多少，技术债还是可以还的。这样才会成为名副其实的不会被公司或者时代淘汰的软件工程师。*

**最初发表于*[*【https://pitayan.com】*](https://pitayan.com/posts/technical-debt/)*。**

*[](https://pitayan.com/posts/technical-debt/) [## 浅谈技术债——皮塔扬

### “技术债务”出自沃德·坎宁安之口，他首先用技术复杂性比率作为负债…

pitayan.com](https://pitayan.com/posts/technical-debt/)*