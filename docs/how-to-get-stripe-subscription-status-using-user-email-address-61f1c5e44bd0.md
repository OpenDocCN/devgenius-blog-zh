# 如何使用用户电子邮件地址获取条带订阅状态

> 原文：<https://blog.devgenius.io/how-to-get-stripe-subscription-status-using-user-email-address-61f1c5e44bd0?source=collection_archive---------9----------------------->

## Stripe 没有任何直接的方法可以做到这一点，但是有一个方法。

![](img/6e2df2029e198b6fdc53fdadefad3f6f.png)

孟山都的照片:[https://www . pexels . com/photo/surface-of-rugged-brown-thick-paper-7794422/](https://www.pexels.com/photo/surface-of-crumpled-brown-thick-paper-7794422/)

在上一篇文章中，我们讨论了在 ReactJS 应用程序中创建条带订阅。

[](https://www.mohammadfaisal.dev/blog/how-to-create-a-stripe-subscription-with-reactjs-and-nodejs) [## 如何使用 ReactJS 和 NodeJS 创建条带订阅| Mohammad Faisal

### Stripe 是收集客户付款的最受欢迎的方法之一。使用起来很简单，而且…

www.mohammadfaisal.dev](https://www.mohammadfaisal.dev/blog/how-to-create-a-stripe-subscription-with-reactjs-and-nodejs) 

使用 stripe 创建订阅后，您会希望使用用户的电子邮件地址来获取他们的订阅状态，对吗？

> 不幸的是，Stripe 无法通过电子邮件直接获取用户的订阅状态

原因是有可能使用同一个电子邮件地址创建多个客户，这使得使用该电子邮件地址检查订阅变得很棘手。

# 旧解决方案

这个问题有一些老派的解决方法。我找到最接近的是这里的。

它的作用是。

1.  列出所有`customers`。
2.  使用`email`地址过滤客户
3.  使用`customer`参数获取所有订阅。
4.  过滤`subscriptions`。

没那么简单，哈？

让我告诉你一个更好的方法。

# 可能的解决方案

解决这个问题的一个有趣的方法是利用 stripe 提供的`metadata`字段。

您可以在`metadata`字段中存储任何内容，并在以后进行查询。

我们将利用这一点。

# 怎么办？

为了解决这个问题，我们可以做两件事。

1.  确保每个电子邮件地址只创建一个用户。
2.  存储一些元数据

让我们开始编码吧。

# 确保每个电子邮件只创建一个客户。

在创建客户之前，我们可以做以下检查。

```
 let customer = await this.stripe.customers.search({
   query: `email:'${createSubscriptionDto.email}'`,
  });

  if (customer.data.length == 0) {
      console.log(' No Custoemr is found. Let us create one!');
      customer = await this.stripe.customers.create({
        name: createSubscriptionDto.name,
        email: createSubscriptionDto.email,
        payment_method: createSubscriptionDto.paymentMethod,
        invoice_settings: {
          default_payment_method: createSubscriptionDto.paymentMethod,
        },
      });
  }
```

# 创建订阅并存储元数据

下一步，我们需要将客户的电子邮件地址存储在订阅的元数据中。

```
const subscription = await this.stripe.subscriptions.create({
      customer: customer.id,
      items: [{ price: priceId }],
      payment_settings: {
        payment_method_options: {
          card: {
            request_three_d_secure: 'any',
          },
        },
        payment_method_types: ['card'],
        save_default_payment_method: 'on_subscription',
      },
      expand: ['latest_invoice.payment_intent'],
      metadata: { customerEmail: createSubscriptionDto.email }, // NOTICE HERE!
});
```

注意`metadata`字段。我们将客户的电子邮件存储为`customerEmail`字段。

# 最后，获取订阅

现在有趣的部分来了。让我们使用元数据字段搜索订阅。

```
const subscription = await this.stripe.subscriptions.search({
      query: `status:'active' AND metadata['customerEmail']:'${email}'`,
});
```

这个查询应该会为您返回该特定电子邮件地址的有效订阅列表。

# 最后的话

这不是最干净的解决方案，但它是最好的解决方案之一，因为它很好地解决了这个问题。

除非 stripe 给我们提供了更简单的做事方式，否则这就是我们得到的。

希望你今天学到了新东西。祝你有美好的一天！

**可以通过** [**LinkedIn**](https://www.linkedin.com/in/56faisal/) **或者我的** [**网站**](https://www.mohammadfaisal.dev/) 联系我。