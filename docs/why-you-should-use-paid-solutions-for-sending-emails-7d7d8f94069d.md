# 为什么你应该使用付费解决方案发送电子邮件。

> 原文：<https://blog.devgenius.io/why-you-should-use-paid-solutions-for-sending-emails-7d7d8f94069d?source=collection_archive---------8----------------------->

![](img/3ac2dcdc1e00527e259fbb7ba6bfa2db.png)

米歇尔·多尔尼克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

与任何免费解决方案相比，使用 SendGrid 这样的服务的好处。

发送电子邮件是一项功能，现在几乎所有的网络应用程序、移动应用程序以及任何一种拥有活跃用户群的应用程序，从登录平台到发送促销信息。

**直入问题**

使用一个免费的库，获取你的用户名和密码，并开始发送电子邮件，这似乎很好，直到你的应用程序进入生产阶段。

现在的情况是，大多数时候不同的组织并不直接与 Gmail、Hotmail 等流行的电子邮件服务提供商合作。相反，他们使用自己的专有解决方案，有时还与这些大型电子邮件服务提供商签订不同的合同。

## 这有什么问题？

当最终用户试图使用一个他认为完全正常的电子邮件，但他们系统的防火墙或其他一些配置出错，中断了连接，而你的邮件突然开始下降，你不知道为什么时，问题就来了。

**send grid 和其他付费解决方案如何帮助您？**

Send Grid 或任何其他付费解决方案为您提供了调试这些问题所需的所有统计数据，它们为您提供了正确的 **SMTP 消息。**有时令人印象深刻的细节错误描述。帮助您和您的客户尽快解决问题。

另一个好处是良好的文档，付费解决方案的问题是他们不能承受糟糕的文档，否则他们的商业模式将不会像预期的那样运行。

SendGrid 可以与几乎所有不同的编程语言一起工作，我非常怀疑它是否不支持任何主流编程语言。

有时，我们陷入了削减成本/性能和健壮性的两难境地，作为软件工程师，我们应该考虑这种权衡，不要在对应用程序的功能至关重要的事情上偷工减料，而是尝试优化应用程序，使其成为整体产品，不烧钱，为公司带来资金。

在结束文章用 nodejs 实现 sendgrid 之前。

```
const sgMail = require('[@sendgrid/mail](http://twitter.com/sendgrid/mail)');
sgMail.setApiKey(process.env.SENDGRID_API_KEY);
const msg = {
  to: '[test@example.com](mailto:test@example.com)',
  from: '[test@example.com](mailto:test@example.com)', // Use the email address or domain you verified above
  subject: 'Sending with Twilio SendGrid is Fun',
  text: 'and easy to do anywhere, even with Node.js',
  html: '<strong>and easy to do anywhere, even with Node.js</strong>',
};

(async () => {
  try {
    await sgMail.send(msg);
  } catch (error) {
    console.error(error);if (error.response) {
      console.error(error.response.body)
    }
  }
})();
```

相信我，没有比这更容易的了。

快乐编码，敬请期待更多；)