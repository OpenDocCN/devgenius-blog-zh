# PHP — P74:表单简介

> 原文：<https://blog.devgenius.io/php-p74-forms-introduction-6f8838e0d16?source=collection_archive---------10----------------------->

![](img/cd211117484166a1a4c0a9beebbcd8f7.png)

我们成功了。我们已经学习了足够多的语法，可以开始看一些实际的例子了，比如表单处理。我们将在后面的文章中介绍数据库和数据持久性。我们想知道的是如何将数据从客户端的表单发送到服务器，不管服务器在哪里。

一旦用户提交了表单，表单数据就会使用`post`方法发送到目标页面。我们也可以使用`get`方法，但是这超出了本文的范围。当我们查看 URL 时，我们将讨论`get`方法。

[](/php-p73-errors-7c8abc9c6a13) [## PHP — P73:错误

### 在过去的 72 篇文章中，我们生活在乐观的一面。我们从来没有停下来想过错误可能是…

blog.devgenius.io](/php-p73-errors-7c8abc9c6a13) 

# 表单(客户端)

我们将生成一个显示在前端的表单。表单周围有一个`form`标签。我们将添加一个`input`字段并将其命名为`full_name`。这将是发送到我们服务器的名称。我们在`input`中期望的数据类型是`text`。该表单还将有一个`submit`按钮，它将触发提交的发生。数据去哪里了？这将用我们的表单`action`来定义。我们将使用什么方法发送这些数据？我们将指定`method`应该是一个`post`请求。

如果您点击`Submit`按钮，您将看到一个新页面加载。检查你的网址，你会注意到`form_processor.php`。我们可以创建我们的`form_processor.php`页面来接受我们的`post`请求。让我们接下来做那件事。

# 表单处理器(服务器端)

一旦用户点击`Submit`按钮，数据就被打包到一个`$_POST`数组中。每个字段名，比如`full_name`，都可以作为`$_POST`数组的键来访问。我们只想`var_dump`数据，看看是什么样子。

现在让我们在表单中输入一个名字，然后点击`Submit`。我们得到以下响应:

```
/app/73 Forms/form_processor.php:3:
**array** *(size=1)*
  'full_name' => string 'Dino Cajic' *(length=10)*
```

要获得全名并显示在屏幕上，我们可以使用数组键`full_name`。

```
<?php

echo $_POST['full_name'] . " submitted this form";
```

如果我们再次提交表单，我们会得到以下响应:

```
Dino Cajic submitted this form
```

# 注意

如果你直接在地址栏输入`0.0.0.0/form_processor.php`，你将一无所获。这是因为我们通过一个`get`请求，而不是一个`post`请求来访问那个页面。通过`post`请求访问该页面的唯一方法是点击我们表单上的`Submit`按钮。

```
/app/73 Forms/form_processor.php:3:
**array** *(size=0)*
  *empty*submitted this form
```

我们将在下一篇文章中进一步探讨表单以及`get`和`post`请求之间的差异。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### PHP YouTube 教程的代码确保你已经安装了 Docker。克隆回购。运行以下命令…

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/3a9cb83f89ec2c0a48c67308353e9612.png)

Dino Cajic 目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 负责人。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。