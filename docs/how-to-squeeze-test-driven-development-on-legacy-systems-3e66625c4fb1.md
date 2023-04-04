# 如何在遗留系统上挤压测试驱动开发

> 原文：<https://blog.devgenius.io/how-to-squeeze-test-driven-development-on-legacy-systems-3e66625c4fb1?source=collection_archive---------3----------------------->

## 我们都喜欢 T.D.D .我们知道它的好处，我们已经阅读了一千篇关于如何使用这种技术构建系统的教程。但是这对于当前的遗留系统是不可行的

![](img/96fc9189a7cb7eaa21c52fa2b64529d5.png)

安妮·尼加德在 [Unsplash](https://unsplash.com/s/photos/old-junk-car) 上拍摄的照片

# 什么是 TDD？

[测试驱动开发](https://en.wikipedia.org/wiki/Test-driven_development) (TDD)是一个软件开发过程，依赖于重复*非常短的*开发周期。

我们将需求转化为非常具体的测试用例。

我们改进软件，以便通过所有测试。

这与合并未被证明符合需求的功能相反。

由 Kent Beck 于 2003 年创建(也是 [xUnit 测试框架](https://en.wikipedia.org/wiki/XUnit)测试系统的作者)。

# 周期

1)添加一个测试(*必须失败*)

仅仅基于行为。忘记了意外实现的一切。

2)运行所有测试。新的测试肯定会失败。其他的都应该会过去。

3)编写尽可能简单的解决方案，使测试通过。程序员不得编写超出测试功能范围的代码。( [K.I.S.S.](https://es.wikipedia.org/wiki/Keep_It_Simple) 和[Y . a . g . n . I .](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it)设计原则)

如果所有测试都通过，则重新开始该过程或…

4)(可选地)进行重构(当代码很糟糕时)。

不要同时做 1 和 4。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) 

# 设计优势

*   可测试性和更好的类接口
*   更简单的设计(接吻、YAGNI、避免镀金、[假装成功](https://en.wikipedia.org/wiki/Fake_it_till_you_make_it)、[快速失败](https://codeburst.io/fail-fast-3f3f036032b0))
*   故障隔离(较少使用调试器或日志记录)。
*   合同设计。
*   模块化
*   自下而上的建筑。
*   正常用例与异常(替代用例)分离。
*   完整的分支覆盖(我们不能在没有测试覆盖的情况下添加代码)。
*   即时反馈/心理奖励。
*   小步渐进法。
*   基于维特根斯坦的增量范例学习思想和认知行为疗法。
*   延迟实现问题和过早优化。

[](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) [## 代码味道 20 —过早优化

### 提前计划需要一个水晶球，这是任何一个开发者都没有的。

medium.com](https://medium.com/dev-genius/code-smell-20-premature-optimization-60409b7f90ec) 

# 要求

测试必须在*全环境控制*下进行。

没有全局变量，没有[单例变量](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243)，[，没有设置](https://medium.com/dev-genius/code-smell-29-settings-configs-39188e5b8316)，没有[数据库](https://medium.com/dev-genius/code-smell-31-accidental-methods-on-business-objects-2ff9dc48a783)，没有缓存，没有外部 API 调用，完全没有副作用。

TDD 可以检测出[联轴器](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04)的问题。

解决这些问题会使代码更加清晰，只关注业务逻辑，封装实现决策。

我们必须使用*测试替身* : [模拟](https://medium.com/dev-genius/code-smell-30-mocking-business-885299c32d6f)、存根、假对象、间谍、代理、虚拟对象等来处理耦合问题。

[](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) [## 耦合:唯一的软件设计问题

### 对我们软件的所有故障进行根本原因分析，会发现一个有多种伪装的单一罪魁祸首。

codeburst.io](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) 

# 在现有系统上工作

根据流行的说法，我们不能在现有系统上使用 TDD。这不是真的。我们来举个例子。

[](https://medium.com/dev-genius/how-to-decouple-a-legacy-system-b4ea10db2642) [## 如何分离遗留系统

### 改进遗留代码的练习

medium.com](https://medium.com/dev-genius/how-to-decouple-a-legacy-system-b4ea10db2642) 

# 真实世界的例子

*   我们有一个售票系统需要展示几个艺术家在新冠肺炎疫情表演现场直播。
*   用户可以根据 React 应用程序上的预输入选择器来搜索艺术家。
*   系统在大量并发的后端系统上执行数据库查询。
*   我们需要删除匹配部分艺术家姓名的冗余的 SQL 查询。
*   [*像 SQL 操作符*](https://www.w3schools.com/sql/sql_like.asp) 在关系系统上开销很大，不允许我们改变后端架构。

# 问题是

我们需要简化多余的搜索。仅此而已。

```
SELECT * FROM ARTISTS
WHERE ((artist.fullname LIKE '%Arcade Fire%')
OR (artist.fullname LIKE '%Radiohead.%') 
OR (artist.fullname LIKE '%Radiohead%') 
OR (artist.fullname LIKE '%Sigur Ros%') 
OR (artist.fullname LIKE '%Sigur%')))
…
```

系统将只执行:

```
SELECT * FROM ARTISTS
WHERE ((artist.fullname LIKE '%Arcade Fire%')
OR (artist.fullname LIKE '%Radiohead%') 
OR (artist.fullname LIKE '%Sigur%')))
…
```

因为这部分对数据库来说是多余的和昂贵的。

```
OR (artist.fullname LIKE '%Radiohead.%') 
OR (artist.fullname LIKE '%Sigur Ros%')
```

# 让我们开始工作吧

> 总是从最简单的问题开始

1)添加一个测试(空案例)

注意:

*   LikePatternSimplifier 类尚未创建。
*   没有定义函数 **simplify()** 。
*   我们根据定义顺序对测试进行编号。
*   第一次测试是最简单的，也是僵尸方法论的*零案例*。

[](https://medium.com/dev-genius/how-i-survived-the-zombie-apocalypse-19905db22043) [## 我是如何在僵尸启示录中幸存的

### 选择优秀的测试用例非常困难。除非你召唤亡灵。

medium.com](https://medium.com/dev-genius/how-i-survived-the-zombie-apocalypse-19905db22043) 

> 测试失败(不出所料)。让我们创建类和函数。

3)编写尽可能简单的解决方案，使测试通过。

注意:

*   第一种解决方案总是硬编码的。

> 继续另一个小案例

1)添加一个测试(简单表达式)

3)也是两种情况下最简单的解决方案。

在这两种情况下都很管用。

我们正在逐步解决问题，并遵循分而治之的原则。

> 继续另一个(不那么简单的)例子:

1.  添加一个测试(两个独立的表达式)。

代码无需更改即可正常工作。这是一个好的测试吗？

我们将在更高级的文章中讨论它。

我们继续吧。

> 继续做一个有吸引力的商业案例。

1.  添加测试(一个表达式包含另一个表达式)。

> 让我们成功吧。

3)为所有已经编写的案例添加最简单的解决方案。

这是一个丑陋的算法解决方案，但一旦我们变得更加自信，我们会通过重构来改进它。

我们不能再伪装了。我们需要*做成*。

丑陋，不具表演性，不明确且复杂。

我们不在乎。我们需要获得信心并在该领域学习。

幸运的是，我们很快就会有时间找到更好的解决方案。

> 继续另一个案例。

1)添加一个测试(左表达式包含右表达式)

…我们是绿色的，所以我们遵守业务规则，说明订单条款不相关。(可交换性质)。

我们将它显式化，这样任何聪明的重构都无法破坏它！

> 继续另一个(不那么简单的)案例。

1.  添加一个测试(大写与 MYSQL 引擎无关，但是我们的用户可能没有意识到这一点)。

2)我们进行了测试，新的坏了。让我们修理它！

3)所有已编写案例的最简单解决方案(包含新案例)。

…使用丑陋的改进解决方案，测试再次变得一片绿色。

代码有味道，我们有几个测试用例。我们需要一个更好的解决方案。

4)让我们用一个更有效、更易读的方法来重构这个解决方案。

> 让我们测试客户要求的第一个生产场景。

1)添加两个不相关的冗余前缀

而且很管用！

> 我们在遗留代码中注入它:

以前

让我们注射它。

![](img/cc7bb9afc98a189f9cbb0a812277756c.png)

# 到底发生了什么

到目前为止，我们在孤立的场景中工作。

软件开发是一项团队活动。

质量保证工程师发现了其他可能的好处。

模式可能在字符串的中间。

客户同意添加此功能。

> 让我们考虑一下这些案例。

新案件被打破，因为它们没有被前一个案件所代表。我们一直在修复它们。

4)让我们更改解决方案，以涵盖所有以前的案例和新的案例。

# 结束是开始

*   TDD 在所有阶段都有效。
*   使用 CI/CD codefix 投入生产。
*   大团圆结局。

![](img/0b0bf015130b96ff71a1d73f9cdd304b.png)

一旦我们提交了*智能* SQL 简化器，不好的事情发生了。

这是经过错误处理后的实际 SQL:

```
SELECT * FROM artists WHERE (())
```

此 SQL 生成被误认为是空条件。

> 所以我们会用 TDD 的方式来解决。

我们隔离缺陷，并将其添加为一个破损的 TDD 案例。

当然，它失败了，因为以前的实现带来了一个空的解决方案(和一个客户投诉)。

我们可以通过在 simplify 函数的开头做一个不区分大小写的去重器预处理器来解决这个问题:

要了解我们是否必须测试私有方法，请访问[shoulditestprivatemethods.com](https://hashnode.com/draft/shoulditestprivatemethods.com)

测试又变绿了。

不处理区分大小写的重复的算法再次工作。

> 让我们考虑不同的顺序。

> 违背我们的直觉，我们看到它失败了。

这是因为该单位带来的是“是”而不是“是”。

解决方案取决于产品所有者。我们可以:

1)归一化所有输出。

2)根据我们的 *SQL 引擎*对文本字段不区分大小写的特性来更改测试。

我们选择 2)

测试又变绿了

> 考虑到混合情况，我们增加了更多的测试。

# 错过的机会

测试使用了新的解决方案，并给出了预期的 SQL。

这个案子被提交给了同行评审。

其中一名评审员询问*不喜欢*比较，寻找改进机会。

我们征求了现场客户的同意。

如果用户选择不看*头*=>，就是选择不看*、【电台司令】*和*说话头*。

在 SQL 中:NOT LIKE“% head %”意味着 NOT LIKE“%电台司令%”，这在 AND 条件中是多余的。

我们的简化器已经意识到了这一点，所以我们在第二个地方注入，确信测试已经覆盖了这个场景。

# 结论

*   质量保证工程应该在纠正应该符合它的实现之前添加一个中断的集成测试。
*   在大型系统上实现需要特殊的技术来逐渐消除耦合。
*   TDD 影响了所有编写的代码。我们已经覆盖了所有的新代码。(17 个单元测试和 3 个 SQL 生成测试)。
*   我们对每一个新案例都充满信心，确保他们不会打破以前的案例。
*   我们应该[只使用方法对象/函数对象或反射来测试私有方法](http://shoulditestprivatemethods.com/)。
*   我们在开发、代码审查、QA 修正和产品缺陷生命周期中使用 TDD。
*   我们开发了一个并行的客户系统(测试)。他们将永远是我们第一个无处不在的用户。
*   解决方案尽可能简单。
*   很有可能在有大量代码的现有项目上进行 TDD。
*   TDD 不会取代或重叠 QA 流程和任务。
*   多个角色参与进来并增加了价值:开发人员、QA 工程师、客户和代码审查人员。
*   我们在成功之前一直在伪装。
*   TDD **并不能保证**一个好的设计。
*   我们应该**永远不要改变或优化**未覆盖的代码。
*   如果你喜欢这篇文章，请[让我知道](https://twitter.com/mcsee1)，这样我可以写更多关于遗留系统的 TDD。

> 遗留代码是所有没有测试的代码。

*迈克尔·费瑟*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

这一系列文章的部分目标是为软件设计的辩论和讨论提供空间。

[](https://mcsee.medium.com/object-design-checklist-47c63d351352) [## 目标设计清单

### 这是已经发表的软件设计文章的索引。

mcsee.medium.com](https://mcsee.medium.com/object-design-checklist-47c63d351352) 

我们期待着对这篇文章的评论和建议。

这篇文章也是西班牙文[这里](https://medium.com/dise%C3%B1o-de-software/c%C3%B3mo-exprimir-el-desarrollo-basado-en-pruebas-en-sistemas-heredados-bc763542d419)。