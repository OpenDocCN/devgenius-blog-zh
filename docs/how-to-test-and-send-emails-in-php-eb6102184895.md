# 如何用 PHP 测试和发送邮件？

> 原文：<https://blog.devgenius.io/how-to-test-and-send-emails-in-php-eb6102184895?source=collection_archive---------7----------------------->

![](img/7d0ab9e4642c0ec4e56238180e77f5b4.png)

测试和发送电子邮件是电子邮件传递的非常重要的方面。如果你使用基于 PHP 的在线服务来测试和发送电子邮件，有各种各样的标准和认证，如 SMTP 和 MIME，Base64 和 DKIM。

这些年来，我们可以用 PHP 测试和发送电子邮件的替代方法越来越多，这将在本帖中讨论。我们开始吧！

# 目录

**用 PHP 测试邮件**

*   电子邮件测试清单
*   PHP 中测试电子邮件的各种方法

**用 PHP 发送电子邮件**

*   内置 PHP 邮件功能
*   PHP 库
*   PHP 框架工具
*   CMS 工具
*   电子邮件服务

**最后的话**

# **在 PHP 中测试电子邮件**

在向你的订户发送邮件之前，邮件测试是一个非常重要的步骤。我们应该[关心在发送前测试电子邮件](https://www.benchmarkemail.com/blog/email-testing/)的原因有很多，其中之一是确保你的电子邮件内容按照预期呈现。

## 电子邮件测试清单

在发送电子邮件之前，有一些事情需要考虑，还有一份清单需要遵循。以下是我们想要测试和验证的一些最重要的事情:

1.  确保正确创建电子邮件标题。
2.  电子邮件的文本显示正确。
3.  图像、链接和字体都是准确的。
4.  HTML 标记正确，包含相关附件。
5.  HTML 和 CSS 没有引起问题
6.  你的垃圾邮件分数没有被超越，你的域名也没有被封禁。

## 用 PHP 测试电子邮件的各种方法

开发人员创建大量脚本和包来在开发环境中执行基本测试是常见的做法。这个过程的结果是，PHP 中的本地电子邮件测试机制受到了严重的限制。

您可能需要执行以下任一操作来测试您的电子邮件:

*   使用各种第三方电子邮件测试应用程序执行所有必要的测试，例如 [Mailtrap](https://mailtrap.io/) 。
*   创建虚构的电子邮件帐户，以模拟向多个收件人发送电子邮件。
*   使用各种电子邮件应用程序向您自己的电子邮件帐户发送和转发测试电子邮件，并在所有可访问的设备上检查它们。

使用第三方邮件测试 app 效率很高；例如，您可以将 Mailtrap 与 PHP 包(如 PHPMailer、Swift Mailer、Pear Mail 等)集成在一起。更有效地测试你的电子邮件。这里有一个全面的[免费电子邮件测试](https://blog.mailtrap.io/email-testing-checklist/)工具列表，你可以根据你想要测试的内容来测试你的电子邮件。

# 用 PHP 发送电子邮件

测试完你的邮件后，是时候把它们发送给你的订户了。下面是使用 PHP 发送电子邮件的一些方法，这取决于您的具体需要和要求。

*   内置 PHP mail()函数。
*   PHP 库
*   PHP 框架
*   内容管理工具(CMS)
*   电子邮件服务

我们将讨论下面列出的每种方法的优缺点。

## 内置 PHP mail()函数

内置的 PHP mail()方法随时可用，绝对免费，通常不需要任何进一步的配置。当在本地设置中用于简单的基于文本的警报时，它表现良好。如果您只需要以简单的消息格式发送几封电子邮件，并且不关心消息的品牌或可送达性，那么使用 PHP mail()函数是最合适的。

使用 PHP 的邮件函数()，您可以执行以下操作:

*   发送基本短信
*   使用本地主机和设置发送电子邮件

然而，这种方法也有一些缺点:

*   您可能会发送一封不正确的电子邮件，无法到达预期的收件人，并最终成为垃圾邮件。
*   许多托管公司在设置 PHP mail()时不允许使用第三方 SMTP 服务器。
*   在处理大量数据或频繁提交方面，它的效率并不是很高。
*   您无权访问送达率和打开率的统计数据。

下面的代码演示了如何使用内置的 PHP mail()函数发送电子邮件。

*T6？php
$message = wordwrap('嗨，这是我的第一封邮件！'，70，' \r\n '，true)；
mail('my@example.com '，' Hi Friend '，$ message)；*

## PHP 库

PHP Mailer、Swift Mailer 和 Pear Mail 是一些可以使发送电子邮件更容易的库。PHPMailer 是其中最受欢迎的一个。然而，对于发送多封电子邮件，Pear Mailer 派上了用场。这些库有许多好处:

*   确保地址和文本正确编码。
*   一封邮件中可以包含多种格式和附件。
*   确保按照标准准备电子邮件标题。
*   以更及时和有效的方式处理错误。

尽管库有其优点，但也有其缺点，例如:

*   只有通过商业 SMTP 提供商，您才能访问传递数据。
*   取消订阅邮件列表和跟踪那些取消订阅的邮件列表并不是这些库的标准功能。

下面的代码演示了如何使用 PHPMailer 发送电子邮件:

*<？php
使用 PHPMailer \ PHPMailer \ PHPMailer
要求' vendor/autoload . PHP '；
$ mail = new PHP mailer(true)；
试试{
$ mail->set from(' hello @ example . net '，'测试邮件')；
$ mail->addAddress(' my @ example . com '，' Jane ')；
$ mail->Subject = ' Hi Friend '；
$mail- > Body = '嗨，这是我的第一封邮件'；
$ mail->send()；
} catch(PHP mailer \ PHP mailer \ Exception $ e){
echo“邮件发送 Errpr:{ $ mail->error info }”；
}*

关于不同库的更深入的代码示例，在这篇博文[使用 PHP 发送电子邮件](https://blog.mailtrap.io/php-email-sending/)中有解释。

## 框架工具

建议使用框架的内置库，而不是外部库。在 PHP 中发送电子邮件最流行的方式是通过 Symfony 的 Mailer 和 Laravel 的 Mailable 类。它们具有与 PHPMailer 相似的特性，但是由于它们与 PHP 的紧密联系，所以更加实用。

Symfony Mailer 支持 Sendmail 和 SMTP。该框架为 SendGrid 和 Mailgun 等商业电子邮件提供商预装了插件。

要发送电子邮件，以下是 Symfony Mailer 代码的示例:

*<？PHP
$ Email =(new Symfony \ Component \ Mime \ Email())
from(*[*hello@example.net*](mailto:hello@example.net)*')
to(*[*my@example.com*](mailto:my@example.com)*')
subject(' Hi Friend ')
text(' Hi，这是我的第一封邮件')；
/* ** [*@ var*](http://twitter.com/var)*Symfony \ Component \ Mailer \ Mailer interface $ Mailer
*/
$ Mailer->send($ email)；*

## CMS 工具:WordPress，Joomla 等

很多知名的 CMS 工具如 WordPress、Joomla 等都利用 PHPMailer 来发送邮件，这是使用 CMS 工具的一个关键优势。如果您需要添加自己的电子邮件发送代码，您必须遵循 CMS 工具的说明。

然而，当涉及到发送电子邮件时，CMS 工具也有和 PHP 库一样的缺点。

以下是从 WordPress 发送电子邮件的代码示例:

*<？PHP
WP _ mail(' my @ example . com '，'你好朋友'，'你好，这是我的第一封邮件')；*

## 电子邮件服务

诸如 Sendgrid、Mailgun、Mailjet 等外部服务为使用 PHP 发送电子邮件提供了更高级的解决方案。如果您需要更多的 SMTP 认证支持和各种 HTML 特性，可以考虑这种方法。

# 最后的话

我们提供了用 PHP 发送和测试电子邮件的各种方法。在发送电子邮件之前对其进行测试是非常重要的。在发送电子邮件之前，使用正确的工具根据[电子邮件测试清单](https://smallbiz.tools/test-email-message-sample/)进行测试是非常必要的，因为这可以加快测试和发送电子邮件的过程。

![](img/317786f46cfb1021efc4d97c33133bff.png)

Dino Cajic 目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。