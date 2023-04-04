# 使用 Python-Python 编程获取域名信息

> 原文：<https://blog.devgenius.io/get-domain-name-information-using-python-python-programming-c4f16543b0ef?source=collection_archive---------3----------------------->

## 在本文中，我们将讨论如何使用 Python 获取域名信息。

![](img/ebaa2b2108ebab54af2851d76d768cb4.png)

伊戈尔·米斯克在 [Unsplash](https://unsplash.com/s/photos/website-design?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

**目录**

*   介绍
*   检查域名注册
*   获取域名信息
*   结论

# 介绍

域名是资源的 IP 地址的表示。当你决定访问[https://pyshark.com/](https://pyshark.com/)时，你将进入该网站的一个 IP 地址，这里的域名只是它的识别字符串。

要获得任何域名，需要从域名注册公司购买。在域名注册过程中，注册人会提供许多信息，如名称、地址、国家等。

所有这些信息都存储在 WHOIS 中，并可通过 WHOIS 检索。这是一种广泛用于从存储域名信息的数据库中获取数据的协议。

让我们看看如何使用 Python 获取域名信息。

为了继续学习本教程，我们需要以下 Python 库:python-whois。

如果您没有安装，请打开“命令提示符”(在 Windows 上)并使用以下代码安装它们:

```
pip install python-whois
```

# 使用 Python 检查域名注册

首先，我们将导入所需的库，创建一个**域**变量，并传递我们想要获取信息的 URL:

python-whois 库功能的使用非常简单。现在，我们知道[www.pyshark.com](http://www.pyshark.com)的存在，因为你在这个网站上并且正在阅读这篇文章。

要获取包含该域名的 WHOIS 信息的对象，我们需要使用以下代码:

请注意，只有在注册了域名的情况下，此代码才会成功执行。如果不是，它会给你一个错误。

我们可以使用此信息构建一个函数，该函数将简单地返回该域名是注册的还是未注册的真/假:

此函数将尝试检索包含域名信息的 WHOIS 对象，如果成功，将返回 True。如果不为假，这意味着该域名没有注册。

让我们试一试:

您应该得到:

```
True
```

这个结果告诉我们的是，这是一个注册域名。对我们来说，这意味着我们可以检索它的一些信息。

现在，如果您尝试对某个不存在的随机域运行该函数，该函数将返回“False ”,这意味着任何进一步的信息检索都是不可能的，因为该域没有注册。

# 使用 Python 获取域名信息

现在让我们看看如何从有效的域名中检索注册商的信息。

从上一节我们已经了解了如何获取包含 WHOIS 信息的对象:

我们得到的回报是一个 WHOIS 对象，我们将像使用字典一样使用它。

因为我们可以将它作为字典来使用，所以我们可以获取它的关键字来确定我们在其中包含了什么信息:

我们得到了:

```
domain_name
registrar
whois_server
referral_url
updated_date
creation_date
expiration_date
name_servers
status
emails
dnssec
name
org
address
city
state
zipcode
country
```

这是相当多的可用信息，根据您想要检索的信息，您可以选择需要的信息。

最后一步是打印出键值对，以获得关于我们的域的实际信息:

我们得到了:

```
domain_name : PYSHARK.COM
registrar : FastDomain Inc.
whois_server : whois.bluehost.com
referral_url : None
updated_date : [datetime.datetime(2020, 2, 4, 0, 39, 22), datetime.datetime(2020, 2, 4, 0, 39, 23)]
creation_date : 2020-02-04 00:39:22
expiration_date : 2021-02-04 00:39:22
name_servers : ['NS1.BLUEHOST.COM', 'NS2.BLUEHOST.COM']
status : clientTransferProhibited https://icann.org/epp#clientTransferProhibited
emails : ['support@bluehost.com', 'WHOIS@BLUEHOST.COM']
dnssec : unsigned
name : DOMAIN PRIVACY SERVICE FBO REGISTRANT
org : THE ENDURANCE INTERNATIONAL GROUP, INC.
address : 10 CORPORATE DR, STE 300
city : BURLINGTON
state : MASSACHUSETTS
zipcode : 01803
country : US
```

# 结论

在本文中，我们探讨了如何使用 WHOIS 检索域名信息。

这些信息是公开的，当您购买域名时，您在注册时提供这些信息，然后这些信息就可供使用和检索。

我也鼓励你看看我在 [Python 编程](https://pyshark.com/category/python-programming/)上的其他帖子。

如果你有任何问题或者对编辑有任何建议，请在下面留下你的评论。

*原载于 2020 年 10 月 11 日*[*【https://pyshark.com】*](https://pyshark.com/get-domain-name-information-using-python/)*。*