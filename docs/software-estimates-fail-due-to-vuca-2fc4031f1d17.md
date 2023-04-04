# 由于 VUCA，软件评估失败

> 原文：<https://blog.devgenius.io/software-estimates-fail-due-to-vuca-2fc4031f1d17?source=collection_archive---------6----------------------->

## 估计与实际不符，这种差距从何而来？

![](img/4f5e91c578802dd46524e22b30811280.png)

罗伯特·古利在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在项目结束时，事后诸葛亮是一件好事，但这也是一个学习和提高的好机会。软件评估很少与现实相符，因此，它们无法在工作开始前设定正确的预期。为了更好地进行评估，我们需要理解评估方法的差异和不可预测性的原因。

**现实不可预测**实际上是缺乏预测能力还是更多？尤其是软件项目，看起来比其他工程领域更难预测，比如盖房子。

2000 多年前，中国的一位将军孙子写了一本关于如何在战斗中取胜的手册，叫做《孙子兵法》。这本书是一套非常简短和简明的说明，涵盖了如何管理小型和大型军队，军队运行成本的问题，以及如何操纵它。关键是了解你的敌人、战略、战术和你自己。第二章涵盖了影响战争的问题和影响成功的因素。关键信息是打一场战争的总成本。这些成本可以归因于军队的规模和战争的持续时间。

让我们穿越时间来到 1987 年，美国陆军战争学院使用 VUCA 和 T21 的概念进行领导力培训。

VUCA 代表着:

*   **波动性**——快速的变化或方向的突然和出乎意料的改变。
*   不确定性——缺乏对未来的可预测性。
*   **复杂性**——众多的部分及其关系，让人难以理解。
*   含糊不清——缺乏明确的精确性和清晰性。

因此，2000 年后，军方仍在编写手册，提供处理不稳定情况的建议，这些情况会影响战斗的预测结果。他们给出的分析是明智和直接的，并且简单地映射到软件项目上。

将此与软件编码和项目进行比较，可以识别出关键领域，这些领域是规模、复杂性、评估的不确定性以及缺乏明确的需求。

*   易变的——由于项目通常被延长，客户倾向于改变主意或要求更多的东西，奇怪的敏捷方法进一步增加了这种易变，设定了频繁变化是好的预期。
*   **不确定**——一个只进化到小、中、大 t 恤或故事点和规划扑克的基本水平的估算过程。由于任务被认为是新的，我们的经验有限，我们有问题估计。
*   **复杂**——软件模块的巨大数量和规模，以及高度互联的模块和微服务。通常很少被记录，随着突破性的变化而快速发展。
*   **含糊不清** —需求不清晰，不明确或缺少要素。

这个模型帮助我们理解了软件评估的错误所在

> 估计=现实+/- VUCA(波动性+不确定性+复杂性+模糊性)

因此，如果军事和商业世界已经分析了这些领域很长一段时间，也许他们有一个解决方案或软件工程世界的一些提示？

商业世界已经跳到了 VUCA 身上，关于这个话题有各种各样的书籍。因此，在 2007 年，Robert Johansen 开发了 [VUCA 总理](https://www.axelos.com/news/blogs/november-2019/vuca-prime-the-answer-to-a-vuca-dynamic)，这是一个以积极行动应对 VUCA 问题的模型。

*   **愿景** —当变化围绕目标发生时，保持专注于目标。
*   **理解** —当存在不确定性时，进行实验和探索，以理解环境及其因素。
*   **清晰** —通过简化来应对不可预测的事项，清晰有助于决策和行动。
*   **敏捷性** -调整方法以获得所需的结果。

作为软件工程师，我们都可以立即看到敏捷被列为模型中的一个因素，这与我们部门从瀑布方法转移到敏捷方法的方法非常一致。

很容易说敏捷已经试图通过以下方式解决这个问题:

*   **V** 愿景——产品目标
*   **U** 理解——规划扑克和故事要点
*   拉里蒂——用户故事和用户故事提炼
*   快速冲刺和快速迭代

但是，由于我们仍然得到与现实不同的估计(我不想说它们是错误的)，所以感觉我们需要的不仅仅是 VUCA Prime，也就是敏捷来解决问题，尽管它无疑是有帮助的。

所以这里有一些关于减少 VUCA 对软件评估的影响的想法。

*   易变— **减少项目的持续时间**,这样就有更少的时间来考虑和请求变更。确保对提供项目核心目标的关键需求(即**产品目标**)有**明确的优先级**和某种**级别的可追溯性**。
*   不确定性——通过再次缩短项目持续时间和**只做我们在**中有经验的事情，缩小预测的范围。这意味着对事情说不，或者通过另一种方式做这些高风险或未知的领域。也许使用概念验证方法来处理它们，作为主项目的副业。这意味着任何评估可变性仅位于那些需要它们的区域。
*   复杂——保持事情简单。这意味着“**更少的移动部件**”和它们之间更简单的交互。在负面影响抵消任何好处之前，是否存在一个**微服务的最佳大小和数量**？
*   模糊性——我们是否应该**根据模糊程度对需求进行分级**,然后在评估后**对不清楚的需求应用更多的偶然性**?而不是推迟给出一个估计或不断要求更多的细节。

如果我们将重点放在利益相关者能够理解的领域，并且他们能够清楚地看到风险和未知领域，那么与利益相关者的对话将变得更加容易。

例如

```
 Traditional estimate for delivering product X with features A,B,C,DDesign 40 man days
Coding 20 man days
Test   40 man daysTOTAL 100 man days, delivered by 1 man duration 100 days
```

这种类型的评估很常见，不允许利益相关者/客户参与或做出任何明智的选择，因为没有给定的选择。在这个例子中，对客户来说唯一显而易见的选择是省去测试，以节省时间和金钱。

也许组织信息的新方法是:

```
Estimate showing VUCA for product X with features A,B,C,DFeature A 64 man days delivered by 4 men, duration 16 days (V)
Feature B 11 man days for B.1 which is fully defined
          5 - 20 man days for B.2 depends on requirements (A)Feature C 10 man days +/- 10 man days  as custom work (U)
Feature D 10 man days, 1 day design and code, 9 days test due (C)TOTAL 100 man days (plus potentially 25 days due to VUCA)Project could be delivered in 16 days if all teams work in parallel however 
- feature B could stretch the overall project to 31 days if feature B2 included (may be possible to reduce with more detail)
- feature C could stretch the overall project timeline to 20 days
```

在本例中:

特性 A 是团队舒适区内的任务，因此团队可以在 25%的时间内交付它。不仅减少了交付时间，还减少了请求任何变更的时间窗口，因此使项目更加稳定。

特性 B 依赖于需求的清晰性，有些是充分已知的，而特性 B2 有大量的可变性。

特征 C 是不可预测的，因为该任务以前没有做过，所以没有基础数据或经验可以推断。因此，给出的估计值有很大的差异。

特性 D 实现起来既快又容易，但是整个产品的整体复杂性不断增加，这就增加了大量的测试。

# 最后的想法

VUCA 似乎是一个很好的模型，它解释了为什么我们在软件估计和实际数字之间存在差异。使用 VUCA 分类法和 VUCA 质量法可以让我们将评估重新组织成一种格式，向客户展示不确定的领域。然后，这些评估使他们能够做出明智的决策，或者更清楚地了解成本和时间风险存在的地方。

# 进一步阅读

[](https://levelup.gitconnected.com/the-art-of-coding-waging-war-9589b313e25f) [## 编码的艺术——发动战争

### 第 2 章—规模、复杂性和不确定性

levelup.gitconnected.com](https://levelup.gitconnected.com/the-art-of-coding-waging-war-9589b313e25f) [](/how-not-to-communicate-an-estimate-fef77d5b40aa) [## 如何不传达一个估计！

### 敏捷评估及其导致的混乱

blog.devgenius.io](/how-not-to-communicate-an-estimate-fef77d5b40aa) [](https://medium.thirdwaveberlin.com/futures-thinking-vuca-our-approach-dc92b3065d13) [## 未来思维和 VUCA——我们的方法

### 试着向 20 世纪 90 年代的外交官解释一下这幅图景。很快就清楚了:现有的模型并不…

medium.thirdwaveberlin.com](https://medium.thirdwaveberlin.com/futures-thinking-vuca-our-approach-dc92b3065d13) 

# 关于作者的更多信息

**Greg** 是一名经验丰富的软件专业人士，也是[**outsource . dev**](https://outsource.dev/)**，**的首席技术官，他曾在多家公司工作过，现在热衷于帮助他人在软件开发、管理和外包方面取得成功。

如果你喜欢这篇文章，请鼓掌👏和**跟着**我。