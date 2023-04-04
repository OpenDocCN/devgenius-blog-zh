# 用 Python 构建一个 Instagram Bot

> 原文：<https://blog.devgenius.io/make-an-instagram-bot-with-python-a0c8d5fd2092?source=collection_archive---------0----------------------->

![](img/ec5bd780abd4ad87f2bcfde8ac2a31bc.png)

学分 [Instavest](https://medium.com/u/fc3dd7691c15?source=post_page-----a0c8d5fd2092--------------------------------)

Instagram 是目前最有用的社交媒体应用。由于其受欢迎程度，每秒钟都会存储大量数据。你听说过 Instagram 吗？它们都可以帮助你在 Instagram 上接触到更多的观众，获得更多的关注者，获得更多的喜欢，而你几乎不用动动手指就可以在 insta gram 上实现自动化等等。我们将使用 Instagram API 和 python 编程语言构建同类型的 bot。我们将使用`open-source module` InstaPy 来创建机器人。

## 在本教程中，您将学习:

1.  Instagram 机器人如何工作
2.  Instapy 功能
3.  如何用 InstaPy 搭建 Instagram bot

# 安装:

您应该在操作系统中安装 python 的最新版本，如果您已经安装了 python 的最新版本，那么在 Mac Os 和 Linux 的命令提示符和终端中运行以下命令。

`pip install instapy`

# Instapy 的基础知识:

在这一基本部分，我们将介绍如何使用`instapy`模块登录 Instagram。

第一步总是导入模块，然后在第 7 行实现了 Instapy()类，我在其中传递了凭证参数。On next line session.login()方法将使用 Instagram API 并登录。运行该程序后，您会看到一个浏览器窗口打开，可以通过编程方式登录 Instagram。

# Instapy 的特点:

`Instapy`有了大量可用的特性，我们将在本文中讨论一些特性。功能列表如下所示。

1.  评论
2.  跟随
3.  不跟随
4.  发布相册
5.  发布视频
6.  喜欢

# 比如:

`InstaPy`有一个喜欢的方法，可以用来喜欢 Instagram 上的照片和其他东西。`Instapy`有 3 种方法喜欢 Instagram 上的东西。

## 比如按标签

Like by Tag 方法将字符串参数作为参数。参数将采用列表形式。还有第二个论点，就是帖子点赞的界限。机器人将在 Instagram 上搜索这些标签，就像他们在结果窗口上发现的帖子一样。

## 按位置分类:

该模块有一个名为 like_by_location()的方法。它有两个参数，一个是字符串位置参数，另一个是你要发送的帖子数量。

## 比如通过饲料

Like by Feed 方法用于在你自己的 Feed 帖子上执行 Like。Like_by_feed()方法有 4 个参数。帖子的数量，随机化参数持有真值和假值，随机跳过帖子以在您的 feed 上被喜欢，取消关注参数持有真值和假值，取消关注被考虑的帖子的作者，交互参数也持有真值和假值，访问某个帖子的作者个人资料页面并喜欢他的给定数量的图片，然后返回 feed。

# 正在评论:

# 以下:

InstaPy 有一个 follow 方法，可以用来关注 Instagram 上的用户。由于 like 方法分为子方法，Follow 方法也分为许多 follow features 方法。我们将在本文中逐一讨论。

## 后跟标签

跟随标签功能根据标签跟随用户，但不喜欢标签。当您想要关注特定兴趣的用户时，此功能非常有用。该方法将两个参数作为参数，字符串列表(具有字符串形式的标签)和数量参数。

## 后面是一个列表

正如您从名称中看到的，它遵循将传递给该方法的用户列表。它采用 4 个参数作为参数，字符串列表的用户，时间， `sleep_delay`和互动，它只关注一个用户一次(如果不关注)将有助于精确定位`sleep_delay`用于定义休息时间后，一些良好的关注平均 10 次关注。如果需要从所选账户/账户组的关注者那里获得关注反馈，这种方法非常有用。

## 追随他人的追随者

`Instapy`给了我们一个特性，我们的机器人可以跟随我们用户的追随者，没错，这个方法名为`follow_user_followers()`，它以 4 个参数作为参数。追随者的名字，数量，随机化和互动的字符串列表。随机参数将随机跟随用户追随者。

## 关注用户照片的喜欢者

这将跟踪给定用户列表中喜欢照片的人。它需要 5 个参数。`photo_grab_amount`是我将从用户的个人资料中获取多少照片并分析谁喜欢它，而`follow_likers_per_photo`是每张照片有多少人关注。

## 关注用户照片的评论者

这将跟踪对给定用户列表的照片发表评论的人。这需要 6 个参数。`daysold`只会从不到几天前的照片上获取评论。他们会限制要分析的照片数量

# 取消跟踪:

我们已经讨论了很多在 Instagram 上关注用户的方法，现在是时候讨论我们的机器人如何取消关注用户了。检查下面的代码，我们使用了带 5 个参数的`unfollow_user`方法。

## 取消关注那些不关注你的用户:

在这种方法中，机器人将取消关注那些不关注你的用户，这对于取消关注那些你关注但他们不关注你的用户是有用的。`Unfollow_after`参数将命令我们的机器人取消跟随那个在我们分配给`unfollow_after`参数的特定时间没有跟随你的特定用户。

## 不管有没有人关注你，只要不关注就行了:

此方法用于取消关注用户，用户是否关注您并不重要。该方法采用 5 个参数作为参数。`Unfollow_after`该参数用于告诉机器人在几分钟或几小时后取消关注该用户。

# InstaPy 的重要功能:

到目前为止，我们讨论了每个 Instagram 机器人的基本功能。但是`instapy`并不局限于那些功能，它们还有一些额外的有用的功能。下面是我们将要讨论的一些额外特性的列表。

1.  智能标签
2.  忽略用户
3.  忽略限制
4.  不包括朋友

## 智能标签

智能标签方法基于`[https://displaypurposes.com](https://displaypurposes.com)`排名生成智能标签，被禁止的和垃圾的标签被过滤掉，你可以使用那个会话来喜欢和评论文章。

## 忽略用户

这种方法将完全忽略用户喜欢他们的图像，关注他们，甚至评论他们的图像/视频。这需要一个 list 参数，它有一个字符串格式的用户名列表。

## 忽略限制

这将忽略对带有将被传递给方法`set_ignore_if_contains()`的单词的图像/视频的喜欢或评论。该方法将检查图像/视频是否有任何类似的文字，它将忽略该图像/视频。

## 不包括朋友

这种方法会忽略在 Instagram 上取消关注和评论你的亲密/好朋友，但机器人仍然会喜欢他们的图像/视频。该方法采用字符串格式的用户名列表。

# 结论

到目前为止，我们已经介绍了`Instapy` 的大部分功能，但是 Instagram bot 的旅程并没有在这里结束，你可以在`instaPy`的官方文档中了解新功能和剩余功能。这篇文章只是帮助你理解`InstaPy module`的工作方式，甚至你可以修改这些方法来构建你的定制 Instagram bot。我希望你将来会发现这篇文章有用。请随意分享您的回答。

如果你有兴趣，可以看看我的其他文章。学点新东西！。

[](https://python.plainenglish.io/22-useful-snippets-to-code-like-a-pro-in-python-1d0dcaacac69) [## 22 个有用的代码片段，像专业人员一样用 Python 编程

### 获取元音，寻找字谜，排序字典，n 倍字符串，字节大小，过滤等。你会学到更多…

python .平原英语. io](https://python.plainenglish.io/22-useful-snippets-to-code-like-a-pro-in-python-1d0dcaacac69) [](https://javascript.plainenglish.io/32-useful-snippets-to-code-like-a-pro-in-javascript-c8b5be11752f) [## 32 个有用的 JavaScript 代码片段

### 在这篇文章中，我们将看看 getAngaram，RemoveDupli，maxN，minN，getDate，数组方法，大写，currenturl 和…

javascript.plainenglish.io](https://javascript.plainenglish.io/32-useful-snippets-to-code-like-a-pro-in-javascript-c8b5be11752f) [](https://python.plainenglish.io/how-to-convert-text-to-speech-in-python-c9aeaaf8d2c1) [## 如何在 Python 中将文本转换成语音

### 使用 pyttsx3 库和 gtts 在 Python 中将文本转换为语音的指南

python .平原英语. io](https://python.plainenglish.io/how-to-convert-text-to-speech-in-python-c9aeaaf8d2c1) [](https://levelup.gitconnected.com/a-beginners-guide-to-natural-language-processing-in-python-using-nltk-6e4692b825d4) [## 使用 NLTK 的 Python 自然语言处理初学者指南

### 自然语言处理是人工智能的一个分支，它帮助计算机理解自然语言

levelup.gitconnected.com](https://levelup.gitconnected.com/a-beginners-guide-to-natural-language-processing-in-python-using-nltk-6e4692b825d4) [](https://python.plainenglish.io/best-ways-to-earn-1000-in-a-month-with-programming-fbb70cf553da) [## 通过编程一个月赚 1000 美元的 7 种方法

### 在这篇文章中，如果你是一名程序员，我们将看到一个月内在网上赚 1000 美元的最好方法。嗯，你有…

python .平原英语. io](https://python.plainenglish.io/best-ways-to-earn-1000-in-a-month-with-programming-fbb70cf553da) [](https://python.plainenglish.io/how-to-read-and-write-to-json-file-in-python-ef35460aaeb5) [## 如何在 Python 中读写 JSON 文件

### JSON 代表 JavaScript 对象符号。JSON 是一种用于存储和传输数据的轻量级格式，即使它是…

python .平原英语. io](https://python.plainenglish.io/how-to-read-and-write-to-json-file-in-python-ef35460aaeb5) [](https://javascript.plainenglish.io/9-must-know-array-methods-to-boost-your-javascript-skills-4ae1822f7751) [## 提高 JavaScript 技能的 9 个必须知道的数组方法

### JavaScript 有一些内置的数组方法，可以在不同的情况下使用。了解这些将会提高你的…

javascript.plainenglish.io](https://javascript.plainenglish.io/9-must-know-array-methods-to-boost-your-javascript-skills-4ae1822f7751) [](https://python.plainenglish.io/build-your-first-simple-django-web-app-with-python-9a4452157089) [## 用 Python 构建第一个简单的 Django Web 应用程序

### Django 是一个用于快速 web 应用程序设计的 Python 开源 web 开发框架。今天，我们将介绍姜戈和…

python .平原英语. io](https://python.plainenglish.io/build-your-first-simple-django-web-app-with-python-9a4452157089) [](https://levelup.gitconnected.com/master-object-oriented-programming-oop-in-python-3-c69a1e8a6d3d) [## 掌握 Python 的面向对象编程(OOP)

### 通过掌握面向对象编程(OOP ),学习用 Python 编写更简洁、更模块化的代码。

levelup.gitconnected.com](https://levelup.gitconnected.com/master-object-oriented-programming-oop-in-python-3-c69a1e8a6d3d) [](https://levelup.gitconnected.com/pyqt5-tutorial-learn-gui-programming-with-python-and-pyqt5-df4225d2e3b8) [## PyQt5 教程:用 Python 和 PyQt5 学习 GUI 编程

### Pyqt5 是图形用户界面小部件工具包。它是最强大和最流行的 python 接口之一…

levelup.gitconnected.com](https://levelup.gitconnected.com/pyqt5-tutorial-learn-gui-programming-with-python-and-pyqt5-df4225d2e3b8) [](https://levelup.gitconnected.com/python-pandas-tutorial-a-complete-introduction-for-beginners-add7013095c2) [## Python 熊猫教程:初学者完全入门

### 在本分步教程中，您将了解如何开始使用 Pandas 和 Python 探索数据集。

levelup.gitconnected.com](https://levelup.gitconnected.com/python-pandas-tutorial-a-complete-introduction-for-beginners-add7013095c2) [](https://levelup.gitconnected.com/how-to-work-with-a-pdf-in-python-a1e0c1d127a4) [## 使用 Python 阅读和编辑 PDF 文档

### 在本文中，我们将了解如何使用 python pdf 模块来读写 pdf 文件。PyPDF2 是一个…

levelup.gitconnected.com](https://levelup.gitconnected.com/how-to-work-with-a-pdf-in-python-a1e0c1d127a4) [](https://levelup.gitconnected.com/build-a-desktop-app-with-python-4a847e3b596c) [## 用 Tkinter 和 Python 构建桌面应用程序

### 在本文中，我们将学习如何使用 python 和 Tkinter 模块开发现代桌面应用程序。一个…

levelup.gitconnected.com](https://levelup.gitconnected.com/build-a-desktop-app-with-python-4a847e3b596c) [](https://levelup.gitconnected.com/a-beginners-guide-to-tesseract-ocr-using-pytesseract-23036f5b2211) [## 使用 Pytesseract 的 Tesseract OCR 初学者指南

### 光学字符识别或光学字符阅读器(OCR)是电子或机械转换的图像…

levelup.gitconnected.com](https://levelup.gitconnected.com/a-beginners-guide-to-tesseract-ocr-using-pytesseract-23036f5b2211) [](/how-to-make-a-chrome-extension-f37bdfb6edb3) [## 如何制作一个 Chrome 扩展

### 使用分步指南构建您的第一个 chrome 扩展

blog.devgenius.io](/how-to-make-a-chrome-extension-f37bdfb6edb3) [](https://python.plainenglish.io/how-to-read-and-write-excel-files-in-python-3da9825e4955) [## 如何在 Python 中读写 Excel 文件

### 使用 Openpyxl 模块在 Python 中读写 Excel 文件。

python .平原英语. io](https://python.plainenglish.io/how-to-read-and-write-excel-files-in-python-3da9825e4955)