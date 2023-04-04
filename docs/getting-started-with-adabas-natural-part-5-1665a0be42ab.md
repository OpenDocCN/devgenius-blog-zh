# Adabas & Natural 入门第 5 部分

> 原文：<https://blog.devgenius.io/getting-started-with-adabas-natural-part-5-1665a0be42ab?source=collection_archive---------7----------------------->

## 使用 NaturalONE IDE 进行大型机编程的介绍

![](img/0ffa9b724eefc8fa1836a4e2bcbfdc39.png)

随着主机行业婴儿潮一代程序员退休人数的增加，同时大学教授的主机课程越来越少，千禧一代程序员面临着获取该领域知识和技能的挑战( [InformationAge 2017](https://www.information-age.com/baby-boomers-millennials-securing-future-mainframe-123469346/) )。为了解决这个问题，这篇文章试图提供一个使用 NaturalONE IDE 的自然编程语言的介绍。

```
PRE-REQUISITE:
(1) ADABAS and NATURAL Server Community Edition (Docker Version). 
(2) NaturalONE IDE.
(refer [here](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-1-6597688406ad) for the [setup guide](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-1-6597688406ad)).
```

运行 Natural ONE IDE ( [版本一. 9.1.4.CE](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-1-6597688406ad) )。

![](img/437b66da8aa3e4cce00a6f0f5d2fb160.png)

[版本一. 9.1.4.CE](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-1-6597688406ad)

输入工作空间名称，例如“工作空间 1011nat”。点击“启动”。

![](img/8e989f92499d2fc74f214d59347be33d.png)

欢迎页面应该出现在主窗口中。否则，进入菜单“帮助/欢迎”进入欢迎页面。选择“打开 NaturalONE 透视图”。

![](img/a62917df4b75a9d0befb4ed7b9353615.png)

自然导航器面板应出现在自然 ide 窗口的左侧。

![](img/fec1b094e1b63a51fa5847a772f04faa.png)

在开始编码工作之前，建议在源代码编辑器中设置可见的行号。选择菜单“窗口/首选项…”然后在首选项窗口中选择“通用/编辑器/文本编辑器”，最后勾选“显示行号”复选框。

![](img/c66437e5768a7361db1bc03b81e5c9d2.png)

# (1)创建一个新项目。

进入菜单，选择“新建/自然项目”。

![](img/df2082cbef44e0379ab86b25f8c41ae0.png)

在项目名称框中键入“PRJ1 ”,然后单击“下一步”。

![](img/09ae897504d07b05532f345f9e5a9f86.png)

输入自然服务器连接详细信息，即“主机名:本地主机”和“端口号:2700”。单击“完成”。

![](img/a1d554a5879e174fa688da57a39016c0.png)

# (2)创建库。

右键单击自然项目，然后选择“新建/自然库”。

![](img/465a8825f152f3c0e3739c8ade8c99f9.png)

在“库”框中键入“LIB1”。

![](img/c1d858aec46aa7ae4e43dba940d263f9.png)

LIB1 库由三个启动文件夹创建。

![](img/1ae72ebf860c1e2abe5c30c034f9c1bd.png)

一个自然库可能包含**特殊文件夹** : **(1) SRC** ， **(2) ERR** 和 **(3) RES.** SRC 包含自然源文件，ERR 包含特定于应用程序的消息，RES 包含非自然文件，如图像或 HTML 文件([单击此处获得进一步解释](https://documentation.softwareag.com/naturalONE/natONE914/core/using/use-local-libs.htm))。

# (3)创建程序。

右键单击 SRC，选择“新建/程序”。

![](img/d5052c55b1375c9abc187d134e925bdb.png)

在对象/文件名中，键入“PRG1”。

![](img/6f2381d56f0f9c8b948cc24eee879807.png)

PRG1 现在在 SRC 下创建。

![](img/41b16954dbd1ef36c6139b701444537b.png)

向文件中添加代码。

```
WRITE "Hello world!"
END
```

WRITE 语句用于在屏幕上写入输出。

# (4)保存并运行。

按[CTRL+S]保存。

要构建代码，右键单击 PRJ1 并选择“构建自然项目”(或按[ALT+B])。

![](img/93b06c41607046194e9218f0cdca47f7.png)

要运行代码，右键单击 PRG1，选择“作为/自然应用程序运行”(或按[SHIFT+ALT+X]然后按[N])。

![](img/9386f4a29fda065090103ff5432f4155.png)

应出现一个自然的网络 I/O 输出窗口，并显示“Hello World！”文字。

![](img/73d7a42da5b539fef9bbccd0fd442e60.png)

假设程序需要得到一个用户名。如下编辑 PRG1。

```
DEFINE DATA LOCAL
  1 #USERNAME     (A20)
END-DEFINE
INPUT
  "Enter your name:" #USERNAME 
WRITE "Hello " #USERNAME
END
```

定义数据本地语句用于声明要在程序中使用的数据元素，例如#USERNAME。INPUT 语句从用户处获得一个输入值，并将其存储在#USERNAME 中。WRITE 语句写入屏幕上的内容，在本例中是字符串“Hello”和数据元素#USERNAME。

按[CTRL+S]保存。

右键点击 PRJ1，选择“NaturalONE/Update”(或按[ALT+T])。

![](img/f6fef5ab4a9c42876cda48922a079cc8.png)

右击 PRG1，选择“运行方式/自然应用”(或者按[SHIFT+ALT+X]后按[N])。

![](img/de6411ffbeb06c60edfca39c0fb2d7a7.png)

Natural Web I/O 输出窗口显示一个带有标签的输入框，标签上写着“输入您的姓名:”。在输入框中输入名称，例如“Razzi”。

![](img/309828972e7982602256d4b8168fef8b.png)

然后，自然 Web I/O 输出窗口显示文本“Hello Razzi”。

![](img/2b24f2a90df4f125efeb1f4fb0852543.png)

有了这些基本的程序代码，就可以进行进一步的处理，如数据库操作。

未完待续在下[部分](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-6-48b4b2fd3e6d)。

本帖是“Adabas & Natural 入门”系列的一部分，该系列包括:
(1) [设置 Adabas & Natural 社区版(Docker 版)](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-1-6597688406ad)。
(2) [通过 Adabas REST Web app 访问 Adabas 数据库](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-2-34621e576fa4)。
(3) [Adabas“周期组”和 JSON 数据格式的“多值”表示](/getting-started-with-adabas-natural-part-3-a334822db12)。
(4) [使用 Adabas TCP-IP 节点包访问 Adabas 数据库](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-4-728e6977ad4f)。
(5) [使用 NaturalONE IDE 的大型机编程(Natural)介绍](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-5-1665a0be42ab)。
(6) [使用自然编程和自然 IDE 访问 Adabas 数据库](https://medium.com/@mohamad.razzi.my/getting-started-with-adabas-natural-part-6-48b4b2fd3e6d)。
(7) [大型机数据库编程基础](https://medium.com/@mohamad.razzi.my/mainframe-database-programming-fundamentals-b34fd88acf6e)。
(8) [大型机数据库编程中级](https://medium.com/@mohamad.razzi.my/mainframe-database-programming-27803b92a3a3)。
(9) [使用自然 AJAX 框架开发 AJAX 网页](https://medium.com/@mohamad.razzi.my/developing-ajax-web-pages-e270eb59fc92)。
(10)[AJAX 实际上是如何工作的](https://medium.com/@mohamad.razzi.my/how-does-ajax-actually-work-2f57cf4ddc55)？
(11) [向 ADABAS REST Web app 发送带有 JWT 令牌的 AJAX 请求](https://medium.com/dev-genius/how-does-ajax-actually-work-2f57cf4ddc55)。
(12)通过 Java Servlets 和 Java Web Services (SOAP)向 ADABAS REST 服务发送 HTTP 请求。