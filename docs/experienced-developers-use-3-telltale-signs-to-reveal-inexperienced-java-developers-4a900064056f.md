# 有经验的开发人员使用 3 个警告标志来揭露没有经验的 Java 开发人员

> 原文：<https://blog.devgenius.io/experienced-developers-use-3-telltale-signs-to-reveal-inexperienced-java-developers-4a900064056f?source=collection_archive---------4----------------------->

## 以及如何引导没有经验的开发者提高这 3 个特质

![](img/16c1000938b4c5024f326ac9cd6916c7.png)

[www.freepik.com 国家石油公司制作的商业照片](https://www.freepik.com/photos/business)

***你和一些没有经验的 Java 开发人员一起工作。*** 或者因为这个工作特点而辞职。

缺乏经验的特征是什么？你能做些什么来对抗这种行为？

你会学到一些克服这种无知的技巧。并引导没有经验的人走上正道。

# 不了解测试

没有经验的开发人员忽略了尽可能多的测试。或者他们尝试快乐之路，然后继续前进。

***他们不懂测试的原理。*** 比如集成或者单元测试的目的是什么。对他们来说，这是一些高级窃听他们更多的工作。

即使他们编写单元测试，测试也只是孤立地覆盖组件。他们看不到全局，错误也就溜走了。

***微服务集成测试的良好开端是***[***test containers***](https://www.testcontainers.org/)***。***

使用微服务架构，testcontainers 复制协作者。更好的是预先构建的容器。您从 testcontainers 中获得配置的 Postgres 测试容器。

对于 monolith 应用程序，您可以通过集成测试来测试重要的流程。例如，用户创建流程是集成测试中的一个重要流程。

集成测试应该覆盖与外部组件的集成。例如数据存储或 AWS S3。

关于其他测试的更多细节可以在本指南中找到:[微服务架构中的测试策略](https://martinfowler.com/articles/microservice-testing/#conclusion-summary)

预先做所有的测试是为了整块石头。大多数团队也将此引入微服务架构。

看看这个 Twitter 帖子，有个大概的印象。

集成测试在某种程度上是有价值的。但是测试并不止于此。

测试也发生在生产中。进行 canary 部署也是在生产环境中进行测试的一种方式。[由于实时流量使用，生产中的测试提供了高保真测试](https://blog.acolyer.org/2016/11/28/kraken-leveraging-live-traffic-tests-to-identify-and-resolve-resource-utilization-bottlenecks-in-large-scale-web-services/)。

这里有一份关于微服务测试的详尽指南。

[测试微服务，同样的方式|辛迪·斯里德哈兰| Medium](https://copyconstruct.medium.com/testing-microservices-the-sane-way-9bb31d158c16)

这些提示的结果是更快的反馈。

添加一个新的 Redis 实例，添加一个测试 Redis 实例。并通过客户端连接。您的集成测试失败。无需部署代码，您就可以在您的本地。要点是你的开发因为测试而更快。并不像大多数人认为的那样慢。

这些技巧本质上与 Java 开发人员无关。尽管如此，大多数团队在他们的项目中缺少测试部分。因此，期待这个问题:*“你如何一起测试*多个*微服务？”*面试时出现。

在企业环境中，你很可能不会太担心测试。 E2E 测试是在一个单独的团队中完成的。但在初创公司或类似的公司，测试是由开发人员完成的。所以忽视测试对你没有任何好处。

# 没有未来问题的预测

没有经验的开发者只关注今天的需求。 或者他们想更快地完成他们的转变。或者他们在假期前几个小时。

*而这种鲁莽行为的输出是什么？他们把代码焊接在一起工作，塞进更多的待办事项，然后继续前进。*

*为别人做更多的工作，并且麻烦地维护这个半生不熟的代码。*

有经验的开发人员在这种情况下会怎么做？打破变革的解决方案是什么？未知变化的解决方案是什么？

关注可扩展性而不是修改。变革应该经得起未来的考验。

没有经验的工作首先想到的选择。有经验的给出很多选项，讨论利弊，然后挑选一个最适合的。

***还有，对这个建议要半信半疑。*** 下面是[丹诺斯关于开闭原理](https://dannorth.net/2021/03/16/cupid-the-back-story/#open-closed-principle)的说法。

> 现在把代码改成别的就可以了。我们可以塑造它。代码是一种成本和债务。如果需要降低维护成本，重写更简单的代码。编写容易修改的简单代码，你有开放和封闭的代码，但是你需要它。

所以如果很难改变代码块，那就改变它，让它更灵活。 ***考虑未来的行动，而不仅仅是今天的需求。***

比如，如果不需要，就不要把所有东西都抽象化。或者在可以配置预先存在的库时使用定制的策略。 ***解决今天的需求，也让未来的改变更容易。***

*知道哪些是易变代码，哪些不是。围绕这些假设编写代码。*

如果你使用 Spring Security 并试图覆盖一些特性，你会遇到障碍。有时候你可能需要使用反射来注入你自己的 bean。质疑这是否是正确的方法。如果你正在开发一个标准，比如 OAuth2，那么这个过程就有了清晰的界限。

这里有一个这样的例子:

[请提供一种为 defaultoauth 2 authorizationrequestresolver 问题# 7808 spring-projects/spring-security GitHub 设置状态生成器的方法](https://github.com/spring-projects/spring-security/issues/7808)

状态生成器不应该是自定义的，因为 OAuth2 标准将它用于 CSRF。调查这个问题[你会看到标准禁止状态生成器](https://github.com/spring-projects/spring-security/issues/7808#issuecomment-574714946)。

*有经验的开发人员理解 ADR 的重要性。*

丹妮拉在她关于异步工作场所的故事中提到了 ADR。ADR 是异步工作和知识转移的一种工具。

更好的是，ADR 留在设计回购中，因此很容易回到决策上来。如果需要，你可以推翻这个决定，但是也要写下动机是什么。总比在没有任何“为什么”的情况下塞进更多的代码要好。

*与*以上所有*一样，代码更改应该是无事件的。*

如果需要，可以参考文档，或者[一些必要的图表](https://mermaid-js.github.io/mermaid/#/)。所以你的改变没有风险。因此，您的更改简单、灵活且易于删除。

# 任何可行的解决方案都会成为最佳实践

大多数没有经验的开发人员得到第一个解决方案，提交并推送代码。

工作做得很好。

他们错过的是分析。分析作为对这些问题的回答:

*   为什么这是这项工作的最佳工具？
*   这项功能最糟糕的结果会是什么？
*   这项任务的替代工具是什么？

***蒙昧节的获胜者必须是可选的。***

我见过太多关于可选的误用和无知。我想你也是。

误用 Optional 的代价是显而易见的。除了[引入样板代码](https://medium.com/javarevisited/experienced-developers-use-these-7-java-optional-tips-to-remove-code-clutter-6e8b1a639861)，[可选滥用也会影响性能](/experienced-developers-are-allergic-to-these-5-bad-optional-practices-ee1014940198)。

***对于随意误用或滥用，你能做些什么？***

我会指出代码审查中的错误。这是消除问题的时刻。

举个例子，我有一个项目，在这个项目中，供应商使用了`orElse`。而不是以供应商为论据的`orElseGet`。**这里的重点是** `**orElse**` **与供应商每次都被执行。**

假设你看到了这样的东西:

```
// ...Optional.of(getDocument()).orElse(getFallbackDocument());//...
```

你现在知道`orElseGet`和 orElse 是什么意思了。你知道`[orElse](https://stackoverflow.com/questions/33170109/difference-between-optional-orelse-and-optional-orelseget)`[和](https://stackoverflow.com/questions/33170109/difference-between-optional-orelse-and-optional-orelseget) `[orElseGet](https://stackoverflow.com/questions/33170109/difference-between-optional-orelse-and-optional-orelseget)` [的区别。](https://stackoverflow.com/questions/33170109/difference-between-optional-orelse-and-optional-orelseget)

这里的问题是如何评论这个问题。你可以这样写:

```
Please switch orElse to orElseGet.
```

这个评论不会给出太多的价值。*为什么作者要把* `*orElseGet*` *？有什么好处？你没有展示好处，所以显得有点无礼。*

那么你能写些什么来代替呢？

```
This will work but it could cause nasty bugs. The method within the orElse block will get executed even though the Optional is present. If there are side effects within the supplier or method in orElse, this could cause bugs.Let's see if there are other places where orElse+method exists so we can refactor.
```

提供由此误用引起的问题。你可以链接一下关于这个问题的 SO 帖子，比如。或者您可以使用 [JShell](https://docs.oracle.com/javase/9/jshell/introduction-jshell.htm) 并展示会发生什么。

建设性的、经过深思熟虑的评论会产生更有凝聚力的代码库。

***代码质量是重要的代码库特性。单凭这一点就可以成就或毁灭一个团队，因为糟糕的代码质量，我离开了一个项目。处理代码质量是很难的，因为期限和其他压力会接踵而至。***

没有人喜欢和不关心代码质量的人一起工作。所有团队成员都有糟糕代码质量的负担。