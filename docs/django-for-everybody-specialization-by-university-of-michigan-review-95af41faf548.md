# Django for Everybody 由密歇根大学评论专业化

> 原文：<https://blog.devgenius.io/django-for-everybody-specialization-by-university-of-michigan-review-95af41faf548?source=collection_archive---------2----------------------->

![](img/e7cee23e862f975b958c531fa838c158.png)

达米安·扎莱斯基在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

从 Coursera 上的[Django for Everybody Specialization，从 HTML/CSS/JS 到 REST API 端点和服务器/客户端交互，可以学到对 Web 开发有很强的基础理解所需的一切。](https://www.coursera.org/specializations/django)

讲师查尔斯·拉塞尔·塞弗伦博士不仅教学生如何编写代码，还解释了代码的工作方式以及实现常见 web 开发代码的不同方法。

[](http://www.dr-chuck.com/) [## 查尔斯·r·塞弗伦博士主页

### 信息学院&nbsp 播客博客@drchuck Twitter 主题演讲人 Slideshare 简历和传记亚马逊作者页面…

www.dr-chuck.com](http://www.dr-chuck.com/) 

每门课程的知识都是复合的，也就是说，到最后一门课程结束时，你将对之前的每门课程都有很强的理解。该专业的教学大纲如下:

1.  Web 应用技术和 Django
2.  在 Django 中构建 Web 应用程序
3.  Django 特性和库
4.  在 Django 中使用 Javascript、JQuery 和 JSON

注意:Python 知识是 Django for Everybody 专门化中假定的，包括基本的面向对象编程概念。我个人一直在使用 [Python 来自动化](https://medium.com/dev-genius/google-it-automation-with-python-review-by-a-software-qa-engineer-421ba785e912)我的日常工作。我对 HTML/CSS/JS(通读 w3schools.com 文档)也有很好的理解，但对 Django 以及如何使用 Javascript/JQuery 向后端发出请求了解不多。

![](img/43079d100033173e6721eab552554fd9.png)

照片由 [Domenico Loia](https://unsplash.com/@domenicoloia?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# **1。Web 应用技术和 Django**

在第一周，您将学习浏览器和服务器如何工作，包括它们如何通过应用层通信(HTTP/Sockets)相互通信，它们在通信时做什么，以及如何使用原生 python 库构建一个。

学习者将在第二周在 PythonAnywhere 建立一个帐户。这个 web 服务将为每个人的 Django 专门化托管所有的 Django 项目。课程的其余部分涵盖了 HTML、CSS 和 SQL 的介绍性材料，这些都是 web 的关键语言。

HTML 是**超文本标记语言，**可以理解为一个网站的骨架。HTML 是使用尖括号符号(<标记>内容</标记>’)编写的，用于告诉浏览器文档的结构是什么，以及它应该如何为最终用户布局视图。

CSS 是**层叠样式表，**可以理解为 web 应用程序的样子。CSS 被用来使 HTML 更加美观，因为一般来说，企业/客户不喜欢“丑陋”的 HTML(回想一下 21 世纪初的网站)。

SQL 是**结构化查询语言**，用于检索存储在数据库中的数据(通常由后端服务器使用)。该课程涵盖基本语法以及 SQL 的历史。

总的来说，我真的很喜欢这门课，因为 Chuck 博士提供了每种底层技术的历史目的，包括每种技术如何发展成为 web 技术的标准。

![](img/6bdeead3013ed4f3639fc296604dd8dd.png)

照片由 [Florian Olivo](https://unsplash.com/@florianolv?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 2.在 Django 中构建 Web 应用程序

在介绍了基本的 web 技术(HTML/CSS/SQL)之后，Django for Everybody Specialization 的下一个课程将介绍 Django 的基本功能，这是一个用 Python 构建的 web 开发的开源框架，包括模型、视图和表单。

在模型部分，Chuck 博士介绍了模型视图控制器模型(MVC)，这是 web 开发的一种常见设计模式。然后，这些材料涵盖了如何在 Django 中创建模型，如何告诉 Django 将更改提交给数据库，以及如何使用 Django Shell 来测试数据库中的数据。

接下来，Chuck 博士将介绍如何在 Django 中实现控制器/视图。Django 中控制器的命名称为视图，而 Django 中的视图称为‘模板’(一开始会令人困惑，但随着项目的进展，它会变得更容易理解)。课程的这一部分教你如何为你的服务器设置 URL，以及使用视图改变 Django 服务器的 http 响应。

Django 中的模板本质上是 HTML，但是在发送到浏览器之前由 Django 编译。这意味着 Django 服务器将在把模板传递给浏览器之前读取模板(通常在视图对象中)。这允许扩展基本 HTML 来包含 **Django 模板语言** (DTL)，它允许你在 HTML 中定义变量和条件语句**。Django 的强大之处在于它将 URL 映射到视图对象的功能，视图对象将依次从模型中检索数据，并通过几行代码将数据作为参数传递给模板。**

下一节将介绍浏览器如何实现表单，服务器如何处理表单。互联网上无处不在的表单，无论是登录脸书，输入下一个亚马逊订单的发货信息，还是撰写这篇文章。

这个课程对 Django 来说是一个很好的介绍，因为它涵盖了 Django 应用程序的基本结构。真正精彩的是 Chuck 博士详细解释了实现模型(包括进行数据库迁移)、路线、视图和模板的工作流程，以及如何使用 Django 的通用视图简化工作流程。

![](img/ef6d2eb1231eb1fbab3645148574d2c0.png)

照片由[阿尔方斯·莫拉莱斯](https://unsplash.com/@alfonsmc10?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 3.Django 特性和库

在介绍了 Django 之后，Chuck 博士继续深入到更多的 Django 特性，包括 cookies 和会话如何工作，如何向现有项目添加用户和登录功能，如何使用 Django 表单简化表单创建，如何创建自己的行，以及介绍性的数据建模(一对多和多对多关系)。

在第一周，Chuck 博士讨论了服务器如何使用 Cookies 恢复与浏览器的会话的问题。Chuck 博士演示了如何在没有 Django 的情况下创建一个 cookie。本节中的专题有助于巩固之前的课程材料。

下一节将介绍如何创建用户和实现登录/注销视图。这一部分的代码实现相当简单，但是主要来自 Chuck 博士对如何使用 Django 会话在整个 Django 应用程序中处理用户数据的解释。用户数据稍后用于拥有的行，这利用了数据模型和登录用户之间的一对多关系。

本课程以多对多关系的材料以及何时使用它的例子结束。总的来说，本课程涵盖了如何使用通用视图和表单重构 Django 中的重复代码。我最喜欢的是这些作业在强化课堂材料方面做得有多好。项目包括构建一个汽车应用程序、一个 Cats 应用程序和启动 4 部分广告应用程序(全部从零开始)。

![](img/3ea74028fd45a9f5d69aa17309310b50.png)

照片由[克里斯托弗·罗宾·艾宾浩斯](https://unsplash.com/@cebbbinghaus?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 4.在 Django 中使用 Javascript、JQuery 和 JSON

读者可以从课程标题中推断出，Django for Everybody 专门化的最后一部分涵盖了如何在前端网站中实现 Javascript。

本课程涵盖了文档对象模型(DOM)，如何使用 Javascript 来操作它，如何利用 JQuery 库来简化常见的 Javascript 代码，前端如何使用 AJAX 发出请求，以及 JSON 如何成为服务器之间通信的标准数据格式。

在专门化的这一点上，大部分的内容已经被覆盖了，剩下的就是给纸杯蛋糕加点糖了。本课程中的项目使用 Javascript 修改浏览器显示，将 github 社交登录、用户收藏夹和搜索功能添加到现有的 ads 应用程序中。

# 结论

我学到了很多新的信息，这些信息给了我信心和知识，让我知道底层的 web 技术是如何独立工作的，以及如何在 Django 中将它们组合在一起。大部分材料可以通过互联网自学，但真正突出这个专业的是对各种编程语言(包括 Python、Javascript、JQuery、JSON)的创造者的采访材料。这些访谈帮助学习者了解创建这些工具的原因。

对于技术行业来说，如何对服务器进行编程是一项关键技能。如果你想开始 web 开发，并且对 Python 有一个基本的了解(主要是阅读代码)，我肯定会向你推荐这个课程。