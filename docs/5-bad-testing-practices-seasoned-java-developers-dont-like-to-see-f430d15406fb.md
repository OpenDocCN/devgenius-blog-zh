# 经验丰富的 Java 开发人员不喜欢看到的 5 种不良测试实践

> 原文：<https://blog.devgenius.io/5-bad-testing-practices-seasoned-java-developers-dont-like-to-see-f430d15406fb?source=collection_archive---------5----------------------->

## 下面是你应该知道的创建简洁和编写良好的测试的方法

![](img/213d7fc01d98af5346b621685580096b.png)

照片由来自[佩克斯](https://www.pexels.com/photo/woman-in-orange-long-sleeve-shirt-standing-beside-woman-in-black-long-sleeve-shirt-5439487/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[马体·米罗什尼琴科](https://www.pexels.com/@tima-miroshnichenko?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

***你每天都会看到糟糕的考试实践。***

***开发者追逐虚荣的指标。代码覆盖率之类的对测试没有任何好处。你可以在这里阅读在这个故事中什么是更好的衡量标准。***

***测试发现 bug。好的测试能及早发现 bug，而坏的测试会让它们飞来飞去。这就是为什么创建好的测试很重要。***

*让我们看看有哪些不好的测试实践，以及如何修复它们。*

# 1.你不用存根

你只在测试中使用模拟。

虽然这些服务的目的很好，但要测试行为，它们并不是万能的。

假设您需要测试一个只用于生产的服务。

所以你想测试 S3 的上传流程。您不想为您的单元测试 ping 实际的服务。但是你也想测试失败和成功的流程。

您需要模拟客户端的每个方法。此外，您需要为每个模拟方法添加一个示例响应。

你会在测试中得到很多这样的陈述。这段代码是一个例子，不是一个实际的测试。

```
when(s3UploadClient.upload(any())).thenReturn(s3response);
when(s3UploadClient.upload(new Failure())).thenThrow(new NotStoredException())
```

在你模拟出他们之后，你需要测试他们的行为。而不是文件是否被实际上传。

***本期用存根修复。***

***存根可以根据输入提供想要的答案。***

有了存根，你可以很容易地改变、失败或抛出异常。你将永远依赖于客户端的真实界面。这样更容易维护[和*写在最后*和](https://blog.cleancoder.com/uncle-bob/2014/05/14/TheLittleMocker.html)。

S3 上传流仅适用于更高的环境。例如，您的机器没有上传权限。理由很充分。所以树桩来帮忙。 ***存根可以不去外部服务测试上传流量。***

存根服务创建全面的测试。

模拟在测试库中有自己的位置，但是存根在上面一个级别。正如鲍勃 [*所说*](https://blog.cleancoder.com/uncle-bob/2014/05/14/TheLittleMocker.html) 模仿是一种存根。所以为了更多的控制，使用存根，而不是模仿。

# 2.你有古怪的测试

你运行了 20 次测试，但是有两次失败了。*这是我对古怪测试的粗略定义。*

当然，您可以重试，直到成功。但这掩盖了问题，没有解决原因。

原因可能是环境、副作用或遗漏清理。

你可以看到很多解决这个问题的方法。

*   重试，直到薄片测试通过
*   将测试标记为进行中的工作
*   删除测试(尽管不推荐)
*   有指数级的后退
*   运行几次，测试失败时调试；如果远程调试可以做到这一点

你会发现最有效的方法是使用假货。*例如，测试环境。*

> 假对象实际上有工作的实现。假货走了一些捷径，使它们不适合生产。内存数据库中的一个[就是一个很好的例子。—梅萨罗什](https://martinfowler.com/bliki/InMemoryTestDatabase.html)

你启动环境，测试它，清理它，然后关闭它。这种短暂环境的一个很好的例子是测试容器。

***测试容器将独立运行你的测试。隔离有助于解决问题，但也消除了副作用。***

# 3.你的测试应该是一个代码描述

> 如果你的代码丢失了，你可以从测试中获取代码——我的一个前辈

***不要追逐虚荣的度量，比如代码覆盖率。***

您可能会遇到对测试没有影响的情况。并利用漏洞进行测试以提高代码覆盖率。

***用测试描述代码。***

你上的每一堂课都必须有一个好的描述性测试。如此具有描述性，以至于您可以从测试中重新创建代码。这是我在测试中唯一喜欢看到的指标。

# 4.您的测试没有响应变化

***当你改变被测代码时，测试仍然通过。这意味着变化对测试没有任何影响。***

如果这发生在你的测试中，那意味着测试有漏洞。 路径没有覆盖好，或者根本没有覆盖。因为变化没有让你的测试失败，所以你的测试并不好。

*为什么会这样？*

***大多数开发人员测试是为了满足虚荣心指标。*** 等代码覆盖率。代码覆盖率并不能保护你不被改变，因为它是可以被欺骗的。

代码覆盖 [*可游戏化*](https://www.youtube.com/watch?v=BK-DzCCFOR0&ab_channel=DATAMINER) :

*   故意地
*   偶然

Nicolas [*提议*](https://blog.frankel.ch/introduction-to-mutation-testing/) [*使用这个库*](http://pitest.org/) 进行突变测试。

你今天能做些什么来纠正这种行为呢？

稍微改变一下代码，看看测试会发生什么。此外，检查遗漏的断言，并添加好的断言。

遵循好的测试模式。例如，遵循 [*僵尸*](/how-i-survived-the-zombie-apocalypse-19905db22043) 模式:

*   z-零
*   一个
*   M —许多(或更复杂)
*   B —边界行为
*   I —接口定义
*   e——锻炼特殊行为
*   简单的场景，简单的解决方案

# 5.你不用假货

在前一篇技巧文章中，我们提到了假货。*什么是假货？*

> Fakes 是它们所模仿的接口的完全内存实现。— [来源](https://nedbatchelder.com/blog/201206/tldw_stop_mocking_start_testing.html)

例如， [*测试*](https://github.com/mockito/mockito/wiki/Using-Spies-(and-Fakes)) `[*HttpRequest*](https://github.com/mockito/mockito/wiki/Using-Spies-(and-Fakes))`的几种方法。

因为有一个保存属性的映射，所以测试可以获取不同的属性键。你需要嘲笑每一个人。这时候假货就开始发挥作用了。

你可以伪造一些测试方法。剩下的就不要说了。这里有一个例子。

[*来源*](https://github.com/mockito/mockito/wiki/Using-Spies-(and-Fakes))

***假货大多用于集成测试，因为他们在使用商业行为。最好自己做，因为不需要像 Mockito 这样的额外框架。***

下面用 Mockito 实现一个 [*。*](http://blog.tremblay.pro/2017/09/mocks.html)

```
when(authorizer.authorize(any(), any())).thenAnswer(invocationOnMock -> "Bob".equals(invocationOnMock.getArgument(0)));
```

*那么存根和假货有什么区别呢？* [*存根会返回固定答案*](http://blog.tremblay.pro/2017/09/mocks.html) ，而且总是同一个答案。

```
when(mock.authorize(any(), any()).thenReturn(true)
```

*你知道哪些糟糕的测试实践？请在评论中告诉我。*