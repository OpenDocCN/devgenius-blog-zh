# PHP — P75: GET 和 POST 请求方法

> 原文：<https://blog.devgenius.io/php-p75-get-and-post-request-methods-dafdb0a86562?source=collection_archive---------8----------------------->

![](img/bb6a1ab649e47bf2f8429dff870b8d87.png)

GET 和 POST 请求并不是 PHP 独有的，但它仍然是您需要理解的东西。HTTP 请求允许客户端和服务器之间的通信。在前一篇文章中，我们在提交表单时看到了这种交流。表单数据在客户端(用户的计算机)输入，当按下提交按钮时，数据被发送到服务器。

由于我们的服务器运行在我们的计算机上，这可能还不完全清楚，但是想象一下，您将代码部署到 AWS EC2 实例。那个服务器已经不在我们的机器上了，所以这个过程更加清晰。我们将很快浏览一个例子，但让我们看看各种通信方法。

[](/php-p74-forms-introduction-6f8838e0d16) [## PHP — P74:表单简介

### 我们成功了。我们已经学习了足够多的语法，可以开始看一些实际的例子了，比如…

blog.devgenius.io](/php-p74-forms-introduction-6f8838e0d16) 

# 常见请求方法

有几个请求方法，但是我想介绍几个可以作为开发人员应用到您日常生活中的方法:

`GET`:用于向服务器请求数据。数据可以通过地址栏作为键/值对发送到服务器。你可能见过这样一个地址:[https://www.dinocajic.com/?p=123](https://www.dinocajic.com/?p=123)。在这种情况下，`p`是键，`123`是值。它只是返回一个图像。

`POST`:用于向服务器发送数据。无需过多讨论细节，数据将被发送到服务器，并通过`$_POST`数组提供给服务器。大多数时候，通过`post`发送到服务器的数据是通过一个表单完成的。表单字段将有一个`name`属性，它将是键，用户输入的数据将是值。例如，`<input type="text" name="full_name">`将有一个键`full_name`，用户输入的数据，即`Dino Cajic`，将是值。如果你在服务器端`echo $_POST['full_name']`，你会收到`Dino Cajic`。与`get`方法不同，通过`post`请求提交的数据在 URL 中对用户是不可见的。

`PUT/PATCH`:向服务器发送数据，通知它资源将被更新。在什么时候使用一个而不是另一个方面有一些不同，但是它们都更新资源。大多数时候，使用哪一个只是基于开发人员的偏好。像 Laravel 这样的框架允许通过简单地传递这样的请求来进行资源管理。可以使用`post`请求来更新数据库中的内容吗？当然了。你还在向服务器发送数据。这只是更加明确。

`DELETE`:类似于`put/patch`请求，这是另一个显式请求，允许将数据发送到服务器。在本例中，我们指定要删除一个资源。

# 浏览表单示例

在前一篇文章中，我们创建了一个允许用户提交全名的表单。

让我们看看数据是如何移动的。

*   用户在浏览器中输入一个 url。因为我们在自己的机器上托管服务器，所以 url 是`https://0.0.0.0/form_example.php`或`https://127.0.0.1/form_example.php`。
*   当用户按 Enter 键时，一个`get`请求被发送到服务器以返回`form_example.php`的内容。服务器可以访问`$_GET`变量，但是没有数据通过 url 字符串传递。我们也没有对来自`$_GET`变量的内容做任何事情。
*   服务器收到请求，进入`form_example.php`，看到要返回 html 数据，然后返回 html 数据。
*   浏览器接收 html 数据，并将其转换为用户可以看到的内容。在这种情况下，用户会看到一个表单，允许他们输入全名并按 Submit 按钮。
*   用户输入他们的全名，然后点击提交按钮。
*   表单`action`指定数据应该发送到`https://127.0.0.1/form_processor.php`并且应该发送到服务器的方式是用等于`post`的表单方法定义的。
*   一个`post`请求被发送到服务器。服务器可以访问`$_POST`变量。我们知道`$_POST`变量将有一个`full_name`键。
*   服务器处理我们的代码，也就是到`echo $_POST['full_name']`。数据一旦生成，就会被发送回客户端。
*   用户会看到他们刚刚输入的内容。

# 将表单的方法从 POST 更改为 GET

如果我们把表单方法从`post`改成`get`会怎么样。表单数据可以发送到服务器吗？让我们看看。

因为数据是通过`get`请求发送的，所以我们的`$_POST`数组中不会有任何数据。它应该在我们的`$_GET`阵中。在`form_processor.php`文件中，让我们`var_dump`我们的`$_GET`变量。

如果我们提交表单，我们会看到它是有效的。我们得到以下数据。

```
/app/74 Form Requests/form_processor.php:3:
**array** *(size=1)*
  'full_name' => string 'Dino Cajic' *(length=10)*
```

有趣的是，当我们点击提交时，数据被转移到了 URL。看看你的网址。您应该会看到类似这样的内容:

```
https://0.0.0.0/form_processor.php?full_name=Dino+Cajic
```

变量被附加到 url 字符串中。这有时是需要的，例如当您希望能够为表单结果添加书签时，但也可能是不需要的，例如当您提交用户名和密码时。

需要注意的一点是，当您提交带有`post`请求的表单时，浏览器中的后退按钮会询问您是否要重新提交表单。使用`get`请求，您只需获得正常的后退按钮功能。

# 传递没有表单的数据

我相信你已经把这些线索联系起来了。你需要表格吗？你能不能不通过 URL 字符串发送变量？是的，你可以！让我们创建一个新文件。我们将声明我们接受 URL 字符串中存在的`first_name`和`last_name`键。

现在，键入带有这些键的 URL，并传递一个您想要的值。

```
https://0.0.0.0/get_request_example.php?first_name=Dino&last_name=Cajic
```

在`?`符号后开始第一个变量。`?first_name=Dino`。您的第二个变量及以后的变量将附加在`&`符号之后:`&last_name=Cajic`。

一旦我们点击了 Enter，get 请求就和我们刚刚在 url 字符串中输入的数据一起被发送，服务器通过回显`first_name`和`last_name`中的数据来处理它。

如果我们没有传递这些值，您会得到警告消息:

```
Warning: Undefined array key "first_name" in /home/user/scripts/code.php on line 2Warning: Undefined array key "last_name" in /home/user/scripts/code.php on line 2
```

在回显数据之前，您总是可以检查数据是否存在。您将使用`isset()`函数来完成这项工作，该函数检查一个数组键是否存在，而不是`null`。

在这种情况下，只有当`first_name`和`last_name`参数都被定义并且数据被传递时，数据才会被显示，否则用户会看到`You must have both the first_name and last_name defined`消息。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### PHP YouTube 教程的代码确保你已经安装了 Docker。克隆回购。运行以下命令…

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/65e5a0a75bd84fe497904b644917c604.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)