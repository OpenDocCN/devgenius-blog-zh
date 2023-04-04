# 动态时差通知

> 原文：<https://blog.devgenius.io/dynamic-slack-notifications-5fb3c1716689?source=collection_archive---------8----------------------->

![](img/c5f5eba916cc8741859413e79ebd9c73.png)

声明:这只是一个简短的帖子，有些人可能会觉得有用，其他人可能会称之为噱头。是什么东西:)

# **【问题】**

我与之交互的许多自动化或我创建的工具都以某种形式集成到 Slack 中。无论工具是由 Slack 触发还是自动化向 Slack 发送通知，Slack 都以某种形式存在。我见过的最常见的模式是，当任务开始时发送通知，当任务以“通过/失败”等状态结束时发送通知。这些通知通常被发送到一个专用通道，在大多数情况下已经足够了，但是如果我们想要更细粒度的通知呢？例如，一个部署过程由多个持续时间不同的任务组成。在这种情况下，我们可以在每个任务开始和结束时发送一个通知。这可能会很快失去控制，感觉就像我们在向频道发送垃圾邮件。单个部署流程可能会将多个通知发送到主通道或线程中。松弛溢出。

# **《解决方案》**

如果我们可以只发送一条消息，然后在任务完成时更新这条消息，会怎么样？我们可以，而且使用 Slack API 实际上非常简单。下面的例子可以从任何地方调用；从 CI/CD 管道，从 Lambda，从容器化工具…

重要的部分是初始消息模板和初始消息的时间戳。我们的模板包含每个任务状态的占位符。一个部署过程可能有 5 个任务；任务[1–5]。我们的模板将为每个任务准备一个占位符。使用 Slack API，我们发送初始消息并存储时间戳。现在，当每个任务开始、停止、暂停、失败时，我们可以用新的状态更新初始消息。

包含 3 项任务的流程的初始消息模板示例:

模板= " " "

将服务$service $version 部署到暂存:$ instance *

*步骤:*

1 .*构建可部署的服务* $(第 1 类。$instance.status)
2。*健全性检查可部署* $(第 2 类。$instance.status)
3。*部署服务* $(第 3 类。$instance.status)
" " "

当我们发送这个初始模板时，每个$instance.status 文件的内容可以包含一个表情符号或一个字符串，如“尚未开始”。重要的部分是我们在发送这个模板后存储时间戳(ts)值。

**curl-s-H " Content-type:application/JSON "--data ' { " channel ":" $ channel " '，" blocks":[{"type":"section "，" text":{"type":"mrkdwn "，" text ":" ' " $ template " ' " } }]'-H " Authorization:Bearer $ auth _ token "**[**https://slack.com/api/chat.postMessage**](https://slack.com/api/chat.postMessage)>【instance . response . JSON

' ts = $(cat $ instance . response . JSON | jq-r '。ts ')
echo " *]写时间戳:$ts 到。$instance.slack.ts 文件"
echo $ts >。$instance.slack.ts

此时，我们已经向 Slack 发送了一条消息，通知用户已经启动了一个流程。随着自动化流程的运行以及每个任务的开始、停止、暂停和失败，我们会更新任务状态，更新有效负载变量，并使用时间戳值重新发送它:

**curl-s-H " Content-type:application/JSON "--data ' { " ts ":" $ ts " '，" channel ":" $ channel " '，" blocks":[{"type":"section "，" text":{"type":"mrkdwn "，" text ":" " $ template " ' " } }]'-H " Authorization:Bearer $ auth _ token "**[**https://slack.com/api/chat.update**](https://slack.com/api/chat.update)

这给人一种单一动态时差消息的印象，该消息随着每项任务的完成而更新。这种方法的好处是，当一个任务失败时，用户现在可以确定任务失败的位置。这对于长期运行的流程来说也很棒，因为它显示了流程的实时进度。

# 摘要

就是这样。这可能是一个噱头，但我发现它很有用，我使用的工具的用户似乎也喜欢这种方法。如果你决定使用它并且需要一些帮助，就在 Twitter 上给我发消息:" [@](https://medium.com/u/619edccbd9b7?source=post_page-----5fb3c1716689--------------------------------) tomwillfixit "。感谢阅读。