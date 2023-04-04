# PHP — P94:会话

> 原文：<https://blog.devgenius.io/php-p94-sessions-23e106ad374f?source=collection_archive---------2----------------------->

![](img/55da8330767a52e26ce3d5d260afa224.png)

什么是会话？这是一种在服务器上存储数据而不使用数据库的方法。当用户从一个页面转到另一个页面时，每个请求都会生成一组新的变量，用来帮助生成用户需要看到的页面。变量是临时的，这意味着一旦用户访问另一个页面，代码中的那些变量就消失了。到目前为止，为了实现数据持久性，我们使用了数据库。

如果您需要永久的数据持久性，您绝对应该继续使用数据库。但是，如果您需要存储一些只在用户会话期间(他们主动访问您的 web 应用程序的时间)使用的数据，该怎么办呢？还能用数据库吗？当然了。但是比较慢，有点没必要。

PHP 有一个叫做会话的东西。会话是一个变量`$_SESSION`，允许存储数据。`$_SESSION`变量是一个超全局数组变量。

会话是默认启用的，不需要任何类型的外部库或配置。

服务器如何不把用户的数据和其他用户的数据混淆？当会话初始化时，它是根据 PHP 为每个用户生成的唯一用户 id 来设置的。

为了让会话跨页面保存数据，使用`$_SESSION`的每个页面都需要用`session_start()`函数启动。这将是你的文件的顶部。

# 例子

解释够了，让我们看看它的行动。我们将创建两个文件:`cart.php`和`add_item.php`。`cart`将显示总数，并且`add_item`页面将每次将`100`加到`total`值上。

先来看看`cart`吧。

我们从调用`session_start()`函数开始。这将允许我们的页面使用`$_SESSION`变量。接下来，我们将检查`$_SESSION['total']`数组键是否存在。记住`$_SESSION`超全局是一个数组。如果不存在，我们就将其初始化为`0`。接下来，我们将向用户显示总数。

在我们访问购物车的第一次迭代中，我们将得到:`The total price for items in your cart is: 0`。

接下来是`add_item`页面。

会话将再次启动。我们需要检查是否设置了总数，因为用户可能会在访问购物车之前访问该页面。如果尚未设置会话，则`$_SESSION['total']`将被初始化为`100`。如果设置了`total`，那么我们只需增加`100`。

访问该页面后，点击`Visit the cart`链接，您会看到您的总数现在是`100`。再次返回`add_item`页面，再次访问`cart`，查看`total`值增加到`200`的情况。这是唯一可能的，因为您正在使用`$_SESSION`超全局。

为了清除`total`，我们可以只设置`$_SESSION['total’] = 0`。如果我们想从$_SESSION 变量中完全删除总数会怎么样？我们使用`unset()`功能。

要完全删除特定用户的会话，使用`session_destroy()`功能。

# 摘要

说到疗程就是这样。这是服务器端的临时数据持久性。接下来我们将看看 cookies，它是客户端的数据持久性。

![](img/3056cc608d3d72c2acf58be7f446d01c.png)

Dino Cajic 目前是 [Absolute Biotech](http://absolutebiotech.com/) 的 IT 主管，该公司是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、 [Absolute 抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的母公司。他还担任我的自动系统的首席执行官。他拥有计算机科学学士学位，辅修生物学，并拥有十多年的软件工程经验。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读迪诺·卡吉克(以及媒体上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。