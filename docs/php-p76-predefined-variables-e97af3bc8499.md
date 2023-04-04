# PHP — P76:预定义变量

> 原文：<https://blog.devgenius.io/php-p76-predefined-variables-e97af3bc8499?source=collection_archive---------16----------------------->

![](img/0921b942bbb639faf80c0ad53a0edf82.png)

如果你一直跟着走，你已经看到他们了。PHP 中有一些，我们将介绍一些最常见的。在前一篇文章中，我们查看了`$_GET`和`$_POST`数组变量。PHP 自动访问表单提交变量，因为它们被添加到了`$_POST`变量中。让我们深入了解最常见的变量。

[](/php-p75-get-and-post-request-methods-dafdb0a86562) [## PHP — P75: GET 和 POST 请求方法

### GET 和 POST 请求并不是 PHP 独有的，但它仍然是您需要理解的东西。HTTP 请求允许…

blog.devgenius.io](/php-p75-get-and-post-request-methods-dafdb0a86562) 

# $ _ 服务器

`$_SERVER`变量包含一些在整个应用程序中可能需要的公共信息。该数组包含头、路径和脚本位置等信息。您可以调用预定义的键，它包含有关您的应用程序环境的信息。

`PHP_SELF`:该键包含当前脚本的文件名。例如，如果你要访问 https://dinocajic.com/pages/dino-page.php,，调用`$_SERVER['PHP_SELF']`会产生`pages/dino-page.php`。

`SERVER_ADDR`:返回服务器的 IP 地址。例如，172.15.0.2。

`REQUEST_METHOD`:显示访问页面的方法。即`get`、`post`、`put`、`head`。

`DOCUMENT_ROOT`:当前脚本正在其下执行的文档根目录。比如`/var/www/html`。

`HTTP_HOST`:显示主机。即`127.0.0.1`

`HTTP_REFERER`:显示哪个页面将用户引至此页面。例如，如果用户在`dinocajic.com/about`上，他们点击了一个链接，将他们引向`dinocajic.com/contact`，那么 referrer 将是`about`页面。

`REMOTE_ADDR`:显示查看页面的用户的 IP 地址。

有相当多的选择。完整的列表，请访问 PHP 文档。您也可以通过 var_dumping 变量进行探索。

```
var_dump($_SERVER);**array** *(size=32)*
  'HTTP_HOST' => string '0.0.0.0' *(length=7)*
  'HTTP_CONNECTION' => string 'keep-alive' *(length=10)*
  'HTTP_CACHE_CONTROL' => string 'max-age=0' *(length=9)*
  'HTTP_DNT' => string '1' *(length=1)*
  'HTTP_UPGRADE_INSECURE_REQUESTS' => string '1' *(length=1)*
  'HTTP_USER_AGENT' => string 'Mozilla/5.0...' *(length=117)*
  'HTTP_ACCEPT' => string 'text/html,application/...' *(length=135)*
  'HTTP_ACCEPT_ENCODING' => string 'gzip, deflate' *(length=13)*
  'HTTP_ACCEPT_LANGUAGE' => string 'en-US,en;q=0.9' *(length=14)*
  'PATH' => string '/usr/local/sbin:/usr/local/bin...' *(length=60)*
  'SERVER_SIGNATURE' => string '<address>Apache/2.4...' *(length=68)*
  'SERVER_SOFTWARE' => string 'Apache/2.4.41 (Ubuntu)' *(length=22)*
  'SERVER_NAME' => string '0.0.0.0' *(length=7)*
  'SERVER_ADDR' => string '175.16.0.2' *(length=10)*
  'SERVER_PORT' => string '80' *(length=2)*
  'REMOTE_ADDR' => string '175.14.0.1' *(length=10)*
  'DOCUMENT_ROOT' => string '/var/www/html' *(length=13)*
  'REQUEST_SCHEME' => string 'http' *(length=4)*
  'CONTEXT_PREFIX' => string '' *(length=0)*
  'CONTEXT_DOCUMENT_ROOT' => string '/var/www/html' *(length=13)*
  'SERVER_ADMIN' => string 'webmaster@localhost' *(length=19)*
  'SCRIPT_FILENAME' => string '/var/www/html/75...' *(length=43)*
  'REMOTE_PORT' => string '62844' *(length=5)*
  'GATEWAY_INTERFACE' => string 'CGI/1.1' *(length=7)*
  'SERVER_PROTOCOL' => string 'HTTP/1.1' *(length=8)*
  'REQUEST_METHOD' => string 'GET' *(length=3)*
  'QUERY_STRING' => string '' *(length=0)*
  'REQUEST_URI' => string '/75%20Global%20Variables/' *(length=25)*
  'SCRIPT_NAME' => string '/75 Global Variables/index..' *(length=30)*
  'PHP_SELF' => string '/75 Global Variables/index.php' *(length=30)*
  'REQUEST_TIME_FLOAT' => float 1666177056.2562
  'REQUEST_TIME' => int 1666177056
```

# $ _ 获取

从传递给脚本的 URL 参数中获取数据。例如，在这个实例中的 dinocajic.com/?page=contact.，`page`将被添加为键，`contact`将是当您调用`$_GET['page']`时显示的值。

# $ _ 邮政

类似于`$_GET`关联数组，`$_POST`关联数组包含来自提交的表单数据的键/值对。例如，当用户在输入字段`<input type="text" name="first_name">`中输入数据时，数据被存储在`$_POST`数组中。调用`$_POST[‘first_name’]`产生你输入的任何内容，即`Dino`。

# $ _ 文件

我们还没有看到文件提交，但是当文件通过表单提交时，您可以通过`$_FILES`关联数组访问文件数据。我们将在下一篇文章中讨论上传文件。

# $ _ 请求

包含`$_GET`、`$_POST`和`$_COOKIE`关联数组的内容。例如，这意味着您可以使用`$_REQUEST`和`$_POST`来访问相同的名称。

# $ _ 会话

存储会话数据。我们很快就会看到在服务器上存储数据，这些数据可以跨特定用户会话的不同脚本进行访问。

# $_COOKIE

会话在服务器端存储数据，cookie 变量允许从客户端检索数据。我们将在以后的文章中讨论会话数据和 cookie 数据之间的区别。

# $_ENV

存储环境变量。当我们查看 Laravel 时，环境变量用于设置应用程序的配置，非常有用。

# 摘要

这些是一些最常见的 PHP 预定义变量。要获得完整列表，请阅读 [PHP 文档](https://www.php.net/manual/en/reserved.variables.php)。我想在这里列出这些作为未来文章的介绍，因为我们在上一篇文章中已经讨论了表单提交。

[](https://github.com/dinocajic/php-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### PHP YouTube 教程的代码确保你已经安装了 Docker。克隆回购。运行以下命令…

github.com](https://github.com/dinocajic/php-youtube-tutorials) ![](img/2d7312049454d560a7c4dc5dfb2998cb.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)