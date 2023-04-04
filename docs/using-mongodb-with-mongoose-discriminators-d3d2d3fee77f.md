# 使用 MongoDB 和 mongose-discriminator

> 原文：<https://blog.devgenius.io/using-mongodb-with-mongoose-discriminators-d3d2d3fee77f?source=collection_archive---------6----------------------->

![](img/6ca4c979332ef4e99b84abf2730456e6.png)

照片由[马特·伊森](https://unsplash.com/@matteason?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了简化 MongoDB 数据库操作，我们可以使用 mongose NPM 包来简化 MongoDB 数据库的操作。

在本文中，我们将了解如何使用 Mongoose 来操作我们的 MongoDB 数据库。

# 鉴别器

鉴别器是一种模式继承机制。

它们让我们能够让多个模型在同一个 MongoDB 集合上拥有重叠的模式。

例如，我们可以如下使用它们:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const db = createConnection('mongodb://localhost:27017/test');
  const options = { discriminatorKey: 'kind' }; const eventSchema = new Schema({ time: Date }, options);
  const Event = db.model('Event', eventSchema);
  const ClickedLinkEvent = Event.discriminator('ClickedLink',
    new Schema({ url: String }, options));
  const genericEvent = new Event({ time: Date.now(), url: 'mongodb.com' });
  console.log(genericEvent) const clickedEvent =
    new ClickedLinkEvent({ time: Date.now(), url: 'mongodb.com' });
  console.log(clickedEvent)
}
run();
```

我们从`eventSchema`创建了一个`Event`模型。

它有`discriminatorKey`,这样我们就可以区分后来创建的两个文档。

为了创建`ClickedLinkEvent`模型，我们调用`Event.discriminator`通过继承`Event`模式来创建模型。

我们将`url`字段添加到`ClickedLinkEvent`模型中。

然后当我们将`url`添加到`Event`文档和`ClickedLinkEvent`文档时，只有`clickedEvent`对象具有`url`属性。

我们得到:

```
{ _id: 5f6f78f17f83ca22408eb627, time: 2020-09-26T17:22:57.690Z }
```

作为`genericEvent`的值，并且:

```
{
  _id: 5f6f78f17f83ca22408eb628,
  kind: 'ClickedLink',
  time: 2020-09-26T17:22:57.697Z,
  url: 'mongodb.com'
}
```

作为`clickedEvent`的值。

# 鉴别器保存到模型的集合中

我们可以一次保存不同种类的事件。

例如，我们可以写:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const db = createConnection('mongodb://localhost:27017/test');
  const options = { discriminatorKey: 'kind' }; const eventSchema = new Schema({ time: Date }, options);
  const Event = db.model('Event', eventSchema); const ClickedLinkEvent = Event.discriminator('ClickedLink',
    new Schema({ url: String }, options)); const SignedUpEvent = Event.discriminator('SignedUp',
    new Schema({ user: String }, options)); const event1 = new Event({ time: Date.now() });
  const event2 = new ClickedLinkEvent({ time: Date.now(), url: 'mongodb.com' });
  const event3 = new SignedUpEvent({ time: Date.now(), user: 'mongodbuser' });
  await Promise.all([event1.save(), event2.save(), event3.save()]);
  const count = await Event.countDocuments();
  console.log(count);
}
run();
```

我们创建了一个普通的模式`eventSchema`。

其余的模型由`Event.discriminator`方法创建。

然后我们创建模型并保存它们。

最后，我们调用`Event.countDocuments`来获取在`Event`模型下保存的项目数量。

那么`count`应该是 3，因为`ClickedLinkEvent`和`SignedUpEvent`都继承自`Event`本身。

# 鉴别键

默认情况下，我们可以用`__t`属性来区分每种类型的模型。

例如，我们可以写:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const db = createConnection('mongodb://localhost:27017/test'); const eventSchema = new Schema({ time: Date });
  const Event = db.model('Event', eventSchema); const ClickedLinkEvent = Event.discriminator('ClickedLink',
    new Schema({ url: String })); const SignedUpEvent = Event.discriminator('SignedUp',
    new Schema({ user: String })); const event1 = new Event({ time: Date.now() });
  const event2 = new ClickedLinkEvent({ time: Date.now(), url: 'mongodb.com' });
  const event3 = new SignedUpEvent({ time: Date.now(), user: 'mongodbuser' });
  await Promise.all([event1.save(), event2.save(), event3.save()]);
  console.log(event1.__t);
  console.log(event2.__t);
  console.log(event3.__t);
}
run();
```

获取控制台日志中保存的数据类型。我们应该得到:

```
undefined
ClickedLink
SignedUp
```

记录在案。

我们可以在选项中添加`discriminatorKey`来改变鉴别器密钥。

所以我们可以写:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const db = createConnection('mongodb://localhost:27017/test');
  const options = { discriminatorKey: 'kind' };
  const eventSchema = new Schema({ time: Date }, options);
  const Event = db.model('Event', eventSchema);const ClickedLinkEvent = Event.discriminator('ClickedLink',
    new Schema({ url: String }, options));const SignedUpEvent = Event.discriminator('SignedUp',
    new Schema({ user: String }, options));const event1 = new Event({ time: Date.now() });
  const event2 = new ClickedLinkEvent({ time: Date.now(), url: 'mongodb.com' });
  const event3 = new SignedUpEvent({ time: Date.now(), user: 'mongodbuser' });
  await Promise.all([event1.save(), event2.save(), event3.save()]);
  console.log(event1.kind);
  console.log(event2.kind);
  console.log(event3.kind);
}
run();
```

为每个模型设置`options`,然后访问`kind`属性而不是`__t`,得到与之前相同的结果。

# 结论

我们可以使用鉴别器通过从一个模型继承另一个模型来创建新的模型。