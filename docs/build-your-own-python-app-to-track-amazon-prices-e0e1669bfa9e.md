# 构建自己的 Python 应用程序来跟踪亚马逊价格

> 原文：<https://blog.devgenius.io/build-your-own-python-app-to-track-amazon-prices-e0e1669bfa9e?source=collection_archive---------1----------------------->

## 我厌倦了自己检查物品。现在，这个应用程序为我做了这件事，并在找到我需要的东西时给我发消息。

![](img/b610605ac1f15f0e1f02f5f933a18fe2.png)

[亚历山大·奈特](https://unsplash.com/@agk42?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

随着时间的推移，我花在网上的时间越来越少。我总是在思考和寻找新的方法来自动化我所需要的东西。如果你和我一样，继续读下去。这个项目可能会帮助你为自己赢得更多的空闲时间。

> 注意:这也可以用于其他网站，不仅仅是亚马逊。

我喜欢在网上随意买东西。说实话，我都不记得上次去商场这种地方购物是什么时候了。当你不得不工作、去健身房、遛狗、在家应付一个生气的女朋友时，这可不是一件容易的事情。

**计划**📐

就像你去商店一样，你必须四处寻找合适的价格。在亚马逊上搜索商品时没有区别，甚至可能更糟，因为它们有数千个不同的价格和型号。为此，我决定开发一个应用程序来完成我的任务。我要做的就是给它喂我需要的任何东西。一旦它找到合适的价格，我希望这个应用程序让我知道给我发一封电子邮件作为通知。

> *我不会透露太多关于编码的细节。我假设您熟悉 Python。如果没有，看看* [*Python*](https://www.python.org/)

**App 代码**📝

**App 结构**🔬

*   **第 6 行** *frommail:* 这将是用来发送通知的邮件。
*   **第 7 行** *passwd:* 从您的 google gmail“应用程序密码”选项中生成的密码。
*   第 8 行 *tomail:* 这将是接收通知的邮件。
*   **第 10 行** *网址*:你想要的产品的链接。
*   **第 12 行** *表头*:添加你的浏览器信息 ex:Mozilla/5.0(Macintosh；英特尔 Mac OS X 9.11；rv:69.0)壁虎/20100101 火狐/68.0
*   **第 24 行** StrToFloat_price:你愿意支付的价格。
*   **第 31 行** *服务器* : smltplib。你的邮件的 STMP 服务。
*   **Line 51** *time.sleep* :这将是运行你的应用程序的时间。关于这项工作的更多信息，请点击这里

如你所见，这是一个简单的应用程序，不言自明。修改代码以满足您的所有需求。

在 Lightsail 中创建一个 Amazon Linux 实例💻

使用Lightsail 做一个 Linux 虚拟服务器全天运行这个 app。它还将为你提供从手机监控一切的商品，而不需要 PC。

1.  在[aws.amazon.com](https://aws.amazon.com)进入 **AWS 管理控制台** **> Lightsail** 。
2.  一旦在 Lightsail 中选择**创建实例**
3.  为您的实例选择**变更区域**和**可用区域**
4.  接下来选择**亚马逊 Linux** 蓝图选项。
5.  (可选)选择**更改 SSH 密钥对**来选择、创建或上传您想要用于 SSH 的密钥对到您的实例中。
6.  为您的**实例**选择一个名称
7.  最后一个**创建实例**。

连接到它，几分钟后，您的 Amazon Linux 实例就准备好了，您可以在 Lightsail 控制台中使用基于浏览器的 SSH 终端连接到它。可以使用 SSH 客户端比如 [PuTTy](https://www.putty.org/) 。使用此[链接](https://lightsail.aws.amazon.com/ls/docs/en_us/articles/lightsail-how-to-set-up-putty-to-connect-using-ssh)了解如何在您的实例中使用它。你将需要它来更新你的应用程序，以显示新的物品和价格。要使用手机连接，只需从 Android Play Store 或 Apple Store 下载任何 SSH 客户端应用程序。

**最后的想法和建议** 🗣

1.  使用 Gmail.com 时，记得先激活“不太安全的应用程序访问”和“创建应用程序密码”谷歌服务。每天发送邮件的上限是 2，000 封。

# 🛒:我们去购物吧！