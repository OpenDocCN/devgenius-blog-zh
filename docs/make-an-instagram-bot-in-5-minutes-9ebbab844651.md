# 5 分钟制作一个 Instagram 机器人

> 原文：<https://blog.devgenius.io/make-an-instagram-bot-in-5-minutes-9ebbab844651?source=collection_archive---------1----------------------->

## 使用你自己的机器人获得真正的追随者

![](img/e6aaf69a021483729b436b313bd9a8b6.png)

由[亚历山大·沙托夫](https://unsplash.com/@alexbemore?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

你为什么想要一个机器人？嗯，通过自动化来完成。许多人甚至为此付给公司一大笔钱。今天你将学习如何做同样的事情——免费——制作你自己的机器人！

在本教程中，你将学习如何用 [Python](https://realpython.com/learning-paths/python-basics-book/) 构建一个机器人，来**自动化**你的 Instagram 活动，以便用最少的手动输入获得更多的关注者和喜欢。在这个过程中，您将通过 [Selenium](https://realpython.com/modern-web-automation-with-python-and-selenium/) 了解浏览器自动化。

> ***重要:*** *确保在实现任何类型的自动化或抓取技术之前，检查* [*Instagram 的使用条款*](https://help.instagram.com/581066165581870) *。*

在本教程中，你将学习:

*   Instagram 机器人如何工作
*   如何用 [Selenium](https://www.selenium.dev/) 自动化浏览器

# Instagram 机器人如何工作

一个自动化脚本如何让你获得更多的关注者和喜欢？想想一个真实的人如何获得更多的关注者和喜欢。

他们通过在平台上的持续活跃来做到这一点。他们经常发帖，关注其他人，喜欢并评论其他人的帖子。机器人的工作方式完全相同:它们根据你设定的标准，在一致的基础上跟随、喜欢和评论。

1.  你给它你的凭证。
2.  您可以设置关注谁、留下什么评论以及喜欢哪种类型的帖子的标准。
3.  你的机器人打开浏览器，在地址栏输入`https://instagram.com`，用你的凭证登录，然后开始做你指示它做的事情。

接下来，您将构建 Instagram bot 的初始版本，它会自动登录到您的个人资料。

# 如何自动化浏览器

首先，[装硒](https://selenium-python.readthedocs.io/installation.html)。在安装过程中，确保你也安装了 [Firefox WebDriver](https://selenium-python.readthedocs.io/installation.html#drivers) 。这也意味着你需要在电脑上安装[火狐浏览器](https://www.mozilla.org/en-US/firefox/new/)。

现在，创建一个 Python 文件，并在其中编写以下代码，将其保存为 file.py:

打开终端，导航到您的文件并运行代码(python whatevername.py ),您将看到 Firefox 浏览器打开并引导您进入 Instagram 登录页面。下面是代码的逐行分解:

*   **线 1 和线 2** 导入`sleep`和`webdriver`。
*   **第 4 行**初始化火狐驱动，并设置为`browser`。
*   **第 6 行**在地址栏输入`https://www.instagram.com/`并点击回车。
*   **第 8 行**等待五秒钟，以便您可以看到结果。否则，它会立即关闭浏览器。
*   **第 10 行**关闭浏览器。

这是硒版的`Hello, World`。

# 安装机器人的步骤

> *打开你的终端，一次粘贴一段代码:*

# 使用

# 就是这样！！

现在，关于如何使用它的更多信息，只需阅读上面链接中的文档。

*注:*

你可以把这个机器人放在服务器上，让它全天候工作