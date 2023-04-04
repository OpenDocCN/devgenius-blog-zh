# 没有银弹

> 原文：<https://blog.devgenius.io/no-silver-bullet-65cba1775f9b?source=collection_archive---------3----------------------->

## “没有灵丹妙药”这句话在业内广为使用。在本文中，我们将重温弗雷德·布鲁克斯的经典论文。

从最初的出版到现在已经过去了将近 35 年，几乎没有什么变化。

在论文中， [Fred Brooks](https://en.wikipedia.org/wiki/Fred_Brooks) 预测(到目前为止我们知道的是正确的)软件开发中固有的问题本质上是困难的，我们现在不会找到神奇的解决方案，将来也不会。

# 问题是

这篇论文是基于亚里士多德关于**本质**和**偶然**的概念。

这篇文章的标题来源于银弹隐喻，它有两个意思:

第一个迹象表明，我们面对的是一个无法用常规和已知武器消灭的怪物。

第二个意思告诉我们，一个敌人曾经是我们的盟友，但后来成为我们最大的恐惧。

狼人在我们的项目中成长。

![](img/7534ac2f948d3735951e8583f868b43c.png)

狼人以一种意想不到的方式变形。

在我们有一个朋友的地方，现在有一个怪物需要被打败。

这不是一个外来的外来威胁，而是我们的一个盟友，突然变成了我们最可怕的噩梦。

![](img/911834a7cdb30c40d0a4d4380161e39a.png)

照片由 [NeONBRAND](https://unsplash.com/@neonbrand) 在 [Unsplash](https://unsplash.com/s/photos/monster) 上拍摄

我们发现自己面对的是一个消耗时间和资源的怪物，交付日期的失败和遥遥无期。

我们需要银弹来控制怪兽。

# 作者

弗雷德里克·布鲁克斯是《人月神话》一书的作者。

这篇论文汇集了 Brooks 开发迄今为止最大的软件项目的经验:OS/360 操作系统。

在这本书里，我们可以找到现在被称为[布鲁克斯定律](https://en.wikipedia.org/wiki/Brooks%27s_law)的著名论点:

> 一个女人需要九个月才能生下一个孩子，而九个女人不可能在一个月内生下一个孩子

上述法律的一个推论是:

> 给一个后期的软件项目增加人力会使它变得更晚

这篇论文直面泰勒的软件开发思想，与彼得·诺尔提出的观点一致。

[](https://medium.com/dev-genius/programming-as-theory-building-9e8cb6f2cd73) [## 作为理论构建的编程

### 在软件中构建模型和解决方案不仅仅是编程。我们将回顾彼得·诺尔的经典论文。

medium.com](https://medium.com/dev-genius/programming-as-theory-building-9e8cb6f2cd73) 

布鲁克斯因其工作在 1999 年获得了图灵奖。

# 论文

Brooks 认为，没有任何现在(1986 年)或未来的技术发展(至少到文章发表之日)，将能够降低软件项目的成本和规划。

如[摩尔定律](https://en.wikipedia.org/wiki/Moore%27s_law)所述，这种改进是通过硬件系统实现的。

# 基本问题

从历史上看，我们一直想将软件工程归类为一个更熟悉和众所周知的隐喻，类似于桥梁或建筑物的建造。土木工程是可预测的，熟悉几千年的历史和经验。

计算机科学家想要认同这样的工程，如果我们考虑软件开发的非常不同的现实，我们寻找**并不真正存在的**相似性。

软件中有一些特殊和独特的属性，使其与所有已知的工程技术有本质的不同:

根据 Brooks 的说法，我们担心偶然的错误(比如编译或代码语法错误)，而不是处理本质问题(概念软件建模错误)。

[](https://codeburst.io/what-is-software-9a78c1172cf9) [## 软件有什么问题？

### 软件正在吞噬世界。我们这些工作、生活和热爱软件的人通常不会停下来思考它的…

codeburst.io](https://codeburst.io/what-is-software-9a78c1172cf9) 

# 复杂性

软件中建模的实体本质上将比任何其他人类构造复杂几个数量级。

> 具有 300 个布尔配置的系统具有比宇宙中的原子数量(10 个^ 80)更多的测试组合(2 个^ 300)。

![](img/d8cbc490b33b71428956eb3d7ca576ff.png)

照片由[杰瑞米·托马斯](https://unsplash.com/@jeremythomasphoto)在 [Unsplash](https://unsplash.com/s/photos/universe) 上拍摄

这种无法控制的状态爆炸使得系统很难测试，影响了可靠性。

大量的变量使得开发过程对人类来说很难处理。

正如彼得·诺尔列举的那样，软件建设的复杂性产生了管理、培训和知识转移问题，导致软件与开发它的人有着内在的联系。

每个软件开发的独创性导致每个新问题都是不同的，我们无法像在其他行业中那样找到重用重复组件的好工具，在其他行业中，新模型可以用标准件来构建。

复用一直是软件开发中的一个难点。然而，我们花了很多时间重新发明轮子，抛弃库，创造新的语言，重新发明异常处理，重新发现匿名函数或函数式编程的优点，不变性等等。

[](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) [## 变种人的邪恶力量

### 变异就是进化。它是由查尔斯·达尔文爵士提出的，我们在软件行业中使用它。但是有些事情是…

codeburst.io](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) ![](img/1dbfdf7f0be3459ad0bc63f487967514.png)

由 serjan midili 在 Unsplash 上拍摄的照片

软件组件之间的依赖比机器、物理结构或科学概念之间的依赖要复杂得多。

这些组件之间的关系是独特而复杂的，由于各部分之间的耦合，使得整个系统的发展极其困难。

[](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) [## 耦合:唯一的软件设计问题

### 对我们软件的所有故障进行根本原因分析，会发现一个有多种伪装的单一罪魁祸首。

codeburst.io](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) 

> 软件系统的复杂性是一个基本问题

软件组件之间的关系从来都不是线性的，因此，它们的组合成了指数关系。

数学家和物理学家花了几个世纪的时间来寻找简单的解释，以统一显然孤立的概念，如[万物理论](https://en.wikipedia.org/wiki/Theory_of_everything)。

![](img/836165e6838c8b94bc839f115c742c7a.png)

照片由[张阳](https://unsplash.com/@iamchang)在 Unsplash 上拍摄

# 一致

除了软件开发的本质复杂性之外，由于我们职业的不成熟，我们通过制造比必要复杂得多的模型来增加一层偶然的复杂性。

既然我们无法避免本质复杂性，软件工程师的唯一任务应该是消除所有偶然的复杂性。

[](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) [## 唯一的软件设计原则

### 如果我们在一个单一的规则上建立我们的整个范式，我们可以保持它的简单并做出优秀的模型。

codeburst.io](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) 

# 可变性

没有人会在建造了一座 60 层的高楼后要求土木工程师改变地下室的地基。然而，这是一个计算机工程师经常接受的任务，因为我们的产品更具可塑性，即使是在生产环境中。

汽车生产线很难对已经售出的汽车进行[产品召回](https://en.wikipedia.org/wiki/Product_recall)。

但是，我们会定期对我们的系统进行更改。软件是否如预期的那样工作，执行进化维护，以及它是否有缺陷，它是否需要适应新的环境、法律、技术变化等。

软件的寿命比最初估计的要长得多，这就是为什么世界上许多开发人员负责维护这些“遗留”系统。

[](https://medium.com/dev-genius/how-to-decouple-a-legacy-system-b4ea10db2642) [## 如何分离遗留系统

### 改进遗留代码的练习

medium.com](https://medium.com/dev-genius/how-to-decouple-a-legacy-system-b4ea10db2642) 

# 看不见

软件是无形的。几十年来，人们试图像建筑师一样用计划来设计软件。所有这些尝试都失败了。

> 软件设计存在于代码中，而不是图中。图不会执行，它们没有错误，也没有人维护它们。我们需要扔掉它们。

软件没有空间或几何表示。由于大量的变化轴，我们无法想象它或使它可见。

就像单元测试一样，只需执行[等高线](https://en.wikipedia.org/wiki/Contour_line)就能观察到一些孤立的方面。

然而，我们永远无法覆盖它的全部，跟随数据流，或者看到对象之间的所有交互。

# 失败的尝试

布鲁克斯列举了到 1986 年已经失败的可能的灵丹妙药:

![](img/ce979e9fca3e424eb4dc9c49c9662ac3.png)

照片由 Unsplash 上的 [engin akyurt](https://unsplash.com/@enginakyurt) 拍摄

## 高级语言

高级语言在 20 世纪 70 年代兴起，是当时的希望。Brooks 正确地指出，这些语言通过远离机器、机器状态、寄存器、磁盘等，消除了一些偶然的复杂性。

在像 2020 年这样的悲伤时期，看似一线希望仍然远离现实，像 Go 或 C #这样的语言优先于好的模型获得几个处理周期，用短期承诺诱惑**过早优化**的粉丝。

[](https://mcsee.medium.com/code-smell-20-premature-optimization-60409b7f90ec) [## 代码味道 20 —过早优化

### 提前计划需要一个水晶球，这是任何一个开发者都没有的。

mcsee.medium.com](https://mcsee.medium.com/code-smell-20-premature-optimization-60409b7f90ec) 

## 分时系统

布鲁克斯列举了 70 年代分时度假机的突破。我们目前拥有几乎无限的虚拟化、负载平衡器和每个人都可以使用的可伸缩性。但这并不意味着我们已经找到了银弹。

## 统一程序环境

共享库和操作系统，如新生的 Unix，保证了重用的便利性。但今天我们知道，尽管这是常态，但他们并没有成功地击败我们内心的狼。

## 面向对象编程

面向对象编程在 20 世纪 70 年代是减少意外复杂性的希望之火。布鲁克斯准确地指出，他们对本质的复杂性无能为力。

今天，我们看到一些概念是在 20 世纪 70 年代发现的，但被 90 年代的面向对象语言(如 Java、Python 或 PHP)遗忘了:封装、多态和不变性的好处就是这样被重新发现的。

[](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3) [## 裸体模特第一部分:模特

### 可靠的数据结构及其有争议的(写)访问。

medium.com](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3) 

## 人工智能

人工智能有许多充满希望和富有成效的夏天，中间夹杂着黑暗的中世纪冬天。

Brooks 列出了现在已经不存在的专家系统，这些专家系统带有基于预编程规则的推理机。这些系统的现代版本是*机器学习*，其中规则是“学习”的，而不是使用监督或无监督学习来固定的。

但今天的人工智能无法战胜本质的困难。视觉和机器人技术有了显著的进步，像 GPT3 这样的奇迹，它设法产生良好的代码来解决低本质复杂性的问题。

## 自动程序设计

与人工智能相关的是自动编程。今天有了更多有趣的分支，比如前面提到的竞争性编程或代码生成。

[](https://medium.com/dev-genius/most-programmers-are-losing-our-jobs-very-soon-cd5c0770bbec) [## (大多数)程序员很快就会失业

### 你是一个声明式程序员还是一个“接近机器”的程序员？

medium.com](https://medium.com/dev-genius/most-programmers-are-losing-our-jobs-very-soon-cd5c0770bbec) 

## 图形程序设计

布鲁克斯强烈批评那些试图用为硬件建造蓝图的方式来开发软件的人。

除了图形软件设计工具令人沮丧的失败之外，布鲁克斯没有预料到的是，设计电路现在也是一个本质上复杂的问题。

## 程序验证

正式的软件验证总是难以扩展。

这在 1986 年和现在都是有效的。

没有机制使得正式验证高度复杂的软件变得可行，我们也没有看到学术出版物表明这种趋势有所改变。

## 工具和环境

Brooks 列出的编程环境或 ide 在这四十年里有了惊人的发展，帮助开发人员构建越来越复杂的软件。

不用说，单靠它们无法消除本质上的复杂性。

# 1986 年的承诺

![](img/ae19f59f0dde78bc0471252ae32dc192.png)

布鲁克斯列出了未来可能的选择:

1.  构建和重用软件，而不是创建软件来降低开发成本
2.  快速原型
3.  迭代和增量开发
4.  更好的设计

目前，我们知道我们必须注意清单上的所有项目，这将有助于我们提高质量，并在行业中大量使用。

不幸的是，它们都不是我们渴望的银弹，我们不断做出糟糕的设计:

[](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) [## 辛格尔顿:万恶之源

### 允许的全局变量和假定的内存节省

codeburst.io](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) [](https://codeburst.io/null-the-billion-dollar-mistake-c2918c92f7e0) [## Null:十亿美元的错误

### 他不是我们的朋友。它不会简化生活，也不会让我们更有效率。只是更懒。是时候停止了…

codeburst.io](https://codeburst.io/null-the-billion-dollar-mistake-c2918c92f7e0) 

# 1996 年更新

在周年纪念版中，布鲁克斯澄清了偶然一词的用法，将其从不幸中分离出来，并使其更接近于机会的概念。

同时，它重新审视了购买软件包而不是生产软件包的重要性，并强调了面向对象编程的进步，尽管注意到它并没有产生数量级的优势。

作者回顾了对软件重用的低估，而不是对相同问题的解决方案的系统构建。

# 结论

大约 40 年前，布鲁克斯向我们展示了，我们不应该低估软件行业中本质上的难题。

这种说法与当时盛行的实证主义相反，类似于数学在 20 世纪初因哥德尔的不完全性定理而遭受的打击。

一旦认识到不可逾越的限制，我们将不得不在幸运的是我们仍有能力改进的方面努力。

这一系列文章的部分目标是为软件设计的辩论和讨论提供空间。

[](https://medium.com/@mcsee/object-design-checklist-47c63d351352) [## 目标设计清单

### 这是已经发表的软件设计文章的索引。

medium.com](https://medium.com/@mcsee/object-design-checklist-47c63d351352) 

我们期待对这篇文章的评论和建议！

这篇文章还有西班牙语版本[点击这里](https://medium.com/dise%C3%B1o-de-software/no-hay-balas-de-plata-b16449eb79c5)。