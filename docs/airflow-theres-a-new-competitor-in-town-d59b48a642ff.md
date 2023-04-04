# 气流，镇上有了新的竞争对手。

> 原文：<https://blog.devgenius.io/airflow-theres-a-new-competitor-in-town-d59b48a642ff?source=collection_archive---------2----------------------->

## 使用 Prefect 让您的数据管道编排面向未来。

![](img/e54faf68ee3a8417597ebfd42eb8cb1c.png)

Boris Stefanik 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我刚刚在一家伟大的公司开始了一个新的职位，我们正在重新评估我们的选择。为了提供一些背景，我们在 Azure 中，并且正在使用 Azure 数据工厂。我们对这种设置不太满意，因为我们没有大量的历史数据，并且监控显示有故障，例如管道在实际发生故障时是成功的。我们的选择？气流还是提督。

现在，气流也很糟糕。作为曾经使用过它的人，设置 airflow 服务器不是一件容易的事情，dag 必须与服务器同步，然后才能测试它是否工作。Dag 不是以 pythonic 方式设置的，并且不允许在任务之间轻松传输数据，除非您设置了 XCOM。最后，我们的代码被推向气流。

这让我们感觉很好..和提督一起。哦，天哪，这完全改变了我对工作流管理的看法。

让我们回顾一下选择提督的四大理由

1.  **简约**
2.  **测试能力**
3.  **他们多样化的基础设施模式**
4.  **开源社区**
5.  **免费(绝大部分)**

1.  **简单性**

任务和流程的设置类似于 Python 中的设置。你也可以添加装饰者。我从他们的 GitHub 页面上取了这个例子来展示它的简单性。

```
from prefect import task, Flow, Parameter

@task(log_stdout=True)
def say_hello(name):
    print("Hello, {}!".format(name))

with Flow("My First Flow") as flow:
    name = Parameter('name')
    say_hello(name)

flow.run(name='world') # "Hello, world!"
flow.run(name='Marvin') # "Hello, Marvin!"
```

**2。测试能力**

他们绝对相信测试。他们的平台有大量针对他们自己平台的测试，无论你是否想知道他们在他们的流程或任务上运行什么测试(更多细节见[这里](https://github.com/PrefectHQ/prefect/tree/master/tests))。

但这还不是全部。您可以轻松地将您自己的测试添加到任务和流程中。

```
***from*** prefect ***import*** Task, Flow

***class*** **Extract**(Task):
    ***def*** **run**(self):
        ***return*** 10

***class*** **Load**(Task):
    ***def*** **run**(self, x):
        ***print***(x)

flow **=** Flow("testing-example")

e **=** Extract()
l **=** Load()

flow.add_edge(upstream_task**=**e, downstream_task**=**l, key**=**"x")#Added this test for the examplestate **=** flow.run()
***assert*** state.result[e].result **==** 10
```

需要更多例子吗？这是测试文件。

他们还集成了数据构建工具(dbt)、远大前程等。这些库允许您轻松地测试已经创建的代码。

**3。不同的基础设施模式**

无论基础设施是运行在完美的云还是完美的核心，代码都不会离开您的环境。

[他们的文档](https://docs.prefect.io/orchestration/faq/dataflow.html#how-is-data-persisted)也明确谈到了在什么情况下，Prefect 保存数据，并为 [Actium Health](https://www.prefect.io/why-prefect/case-studies/) 提供了案例研究。

**4。开源社区**

你将有一个开源的、有回应的社区来问问题。我个人已经查看了他们的 Slack 频道寻求建议，也查看了他们的 Github 和涵盖多种场景的文章。

提督有 200 多个投稿人。但是如果您缺少一个，可以随意添加代码并提供一个集成？请随意创建集成，它们也可用于 PRs 的新功能。

**5。免费(绝大部分)**

Prefect 最棒的地方在于，它允许你每月免费使用该服务多达 20，000 次。没错。这意味着你作为一个开发者或小企业可以免费使用这项服务。

**参考文献**

1.  [https://github.com/prefecthq/prefect](https://github.com/prefecthq/prefect)
2.  [https://www.prefect.io/pricing/](https://www.prefect.io/pricing/)
3.  [https://medium . com/the-prefect-blog/the-prefect-hybrid-model-1b 70 c 7 FD 296](https://medium.com/the-prefect-blog/the-prefect-hybrid-model-1b70c7fd296)
4.  [https://medium . com/geek culture/prefect-can-be-perfect-a 318 b 9 B1 ad 6 e](https://medium.com/geekculture/prefect-could-be-perfect-a318b9b1ad6e)

# **TL；博士**

Prefect 是一个优秀的工作流管理和编排工具，它鼓励测试和可靠性。

你还在等什么？让我们一起创造完美的流量。