# Python 使用 Gmail 发送运行时出错的电子邮件

> 原文：<https://blog.devgenius.io/python-send-email-with-runtime-error-using-gmail-3fdb9991bfb3?source=collection_archive---------1----------------------->

![](img/e7f11f33bdc4c904daf3febadacac49c.png)

照片由[西格蒙德](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

Python 日志记录非常有用。

使用 python 日志记录，我们可以将错误写入文件。但是这样做需要我们不时回来检查文件，看看是否有错误发生。

但这种解决方案并不是最好的。

下面是我们如何通过电子邮件发送日志。首先，如果你想从 Gmail 发送邮件，你必须生成一个 [pp 密码](https://support.google.com/accounts/answer/185833?hl=en)

```
import logging
import logging.handlers# Enable logging
logging.basicConfig(
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    level=logging.INFO, filename='log_application.log')logger = logging.getLogger(__name__)smtp_handler = logging.handlers.SMTPHandler(mailhost=('smtp.gmail.com', 587), fromaddr='email', toaddrs=['email'], subject='Error', credentials=('email','password_App'), secure=())logger.addHandler(smtp_handler)
```

如果您愿意，您可以发送到多个地址:

```
toaddrs=[‘email_1’, ‘email_2’],
```

这样，您将错误保存在 log_application.log 文件中，每次生成错误时，都会通过电子邮件发送。

这是一个如何使用的示例:

感谢阅读！

为了获得无限的故事，你也可以考虑只花 5 美元注册成为一名媒体会员。如果你用我的 [*链接*](https://pietrocolombo.medium.com/membership) *注册，我会收到一小笔佣金。*