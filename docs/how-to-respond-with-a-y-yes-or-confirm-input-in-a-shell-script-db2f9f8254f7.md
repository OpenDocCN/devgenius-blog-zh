# 如何在 shell 脚本中响应“Y”、“Yes”或“confirm”输入

> 原文：<https://blog.devgenius.io/how-to-respond-with-a-y-yes-or-confirm-input-in-a-shell-script-db2f9f8254f7?source=collection_archive---------3----------------------->

![](img/5c26e155b94b7b69c5bc2b27f6d4d572.png)

照片由[记者在](https://unsplash.com/@postebymach?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [Unsplash](https://unsplash.com/s/photos/yes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

在 shell 脚本中，可能会出现这样的用例:命令需要用户以“Y”、“yes”甚至“confirm”的形式输入。实际上，本文将为您提供在 shell 脚本中编写代码的能力，即模拟用户输入任何内容来手动确认某个步骤的方法。

因此，如果您在这里，很可能您正在测试一个 shell 脚本，该脚本卡在了一个期望用户输入内容的步骤上。这通常是一个大写的“Y”字符，因为它在脚本本身中，所以需要有一种方法让脚本将它打出来，以便它可以继续执行其余的指令。

出于本文的目的，假设这个命令是，想象一下，`command`。因此，非常基本的 shell 脚本如下所示:

```
*#!/bin/bash*command
```

`command`最终会要求用户键入“Y”确认，键入“n”拒绝。对于这个脚本，我们需要确认。

要做到这一点，非常简单:使用`echo`命令！如果你使用`echo`，你可以输出你放在它后面的任何东西。在这种情况下，我们想输出“Y”，所以我们要做的就是把它放在`command`之前，然后在后面放一个管道。它看起来像这样:

```
*#!/bin/bash*echo Y | command
```

就是这样！`command`将能够通过烦人的人工确认(或否认)。

除了“Y”之外，上面的`echo`建议适用于用户需要输入的任何字符串。

快乐脚本！

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)