# 使用 MongoDB 和 mongose——鉴别器和钩子

> 原文：<https://blog.devgenius.io/using-mongodb-with-mongoose-discriminators-and-hooks-482de1614a26?source=collection_archive---------1----------------------->

![](img/4cf30aede0819cd83649d3000c97c427.png)

由[杜安·斯美塔纳](https://unsplash.com/@veverkolog?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了简化 MongoDB 数据库操作，我们可以使用 mongose NPM 包来简化 MongoDB 数据库的操作。

在本文中，我们将了解如何使用 Mongoose 来操作我们的 MongoDB 数据库。

# 鉴别器和查询

查询足够聪明，可以考虑鉴别符。

例如，如果我们有:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const db = createConnection('mongodb://localhost:27017/test');
  const options = { discriminatorKey: 'kind' };
  const eventSchema = new Schema({ time: Date }, options);
  const Event = db.model('Event', eventSchema); const ClickedLinkEvent = Event.discriminator('ClickedLink',
    new Schema({ url: String }, options)); const SignedUpEvent = Event.discriminator('SignedUp',
    new Schema({ user: String }, options)); const event1 = new Event({ time: Date.now() });
  const event2 = new ClickedLinkEvent({ time: Date.now(), url: 'mongodb.com' });
  const event3 = new SignedUpEvent({ time: Date.now(), user: 'mongodbuser' });
  await Promise.all([event1.save(), event2.save(), event3.save()]);
  const clickedLinkEvent =  await ClickedLinkEvent.find({});
  console.log(clickedLinkEvent);
}
run();
```

我们称之为`ClickedLinkEvent.find`方法。

因此，我们将获得所有的`ClickedLinkEvent`实例。

# 识别器前后挂钩

我们可以给用鉴别器创建的模式添加前置和后置挂钩。

例如，我们可以写:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const db = createConnection('mongodb://localhost:27017/test');
  const options = { discriminatorKey: 'kind' };
  const eventSchema = new Schema({ time: Date }, options);
  const Event = db.model('Event', eventSchema); const clickedLinkSchema = new Schema({ url: String }, options)
  clickedLinkSchema.pre('validate', (next) => {
    console.log('validate click link');
    next();
  });
  const ClickedLinkEvent = Event.discriminator('ClickedLink',
    clickedLinkSchema); const event1 = new Event({ time: Date.now() });
  const event2 = new ClickedLinkEvent({ time: Date.now(), url: 'mongodb.com' });
  await event2.validate();
}
run();
```

在`clickedLinkSchema`上增加一个用于`validate`操作的预钩。

# 处理 Custom _id 字段

如果在基础模式上设置了一个`_id`字段，那么它将总是覆盖鉴别器的`_id`字段。

例如，我们可以写:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const db = createConnection('mongodb://localhost:27017/test');
  const options = { discriminatorKey: 'kind' }; const eventSchema = new Schema({ _id: String, time: Date },
    options);
  const Event = db.model('BaseEvent', eventSchema); const clickedLinkSchema = new Schema({
    url: String,
    time: String
  }, options); const ClickedLinkEvent = Event.discriminator('ChildEventBad',
    clickedLinkSchema); const event1 = new ClickedLinkEvent({ _id: 'custom id', time: '12am' });
  console.log(typeof event1._id);
  console.log(typeof event1.time);
}
run();
```

从控制台日志中，我们可以看到`event1`的`_id`和`time`字段都是字符串。

因此`_id`字段与`eventSchema`字段相同，但是`ClickedLinkEvent`字段与`clickedLinkSchema`字段的类型相同。

# 对 Model.create()使用鉴别器

我们可以通过`Model.create`方法使用鉴别器。

例如，我们可以写:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const db = createConnection('mongodb://localhost:27017/test');
  const shapeSchema = new Schema({
    name: String
  }, { discriminatorKey: 'kind' }); const Shape = db.model('Shape', shapeSchema); const Circle = Shape.discriminator('Circle',
    new Schema({ radius: Number }));
  const Square = Shape.discriminator('Square',
    new Schema({ side: Number })); const shapes = [
    { name: 'Test' },
    { kind: 'Circle', radius: 5 },
    { kind: 'Square', side: 10 }
  ];
  const [shape1, shape2, shape3] = await Shape.create(shapes);
  console.log(shape1 instanceof Shape);
  console.log(shape2 instanceof Circle);
  console.log(shape3 instanceof Square);
}
run();
```

我们用`discriminator`方法为形状创建了 3 个模式。

然后我们用一组不同形状的对象调用`Shape.create`。

我们用`kind`属性指定了类型，因为我们将它设置为鉴别器键。

然后在控制台日志中，它们应该都记录了`true`，因为我们指定了每个条目的类型。

如果没有指定，那么它具有基本类型。

# 结论

我们可以将钩子添加到由鉴别器创建的模式中。

`_id`字段的处理方式不同于其他鉴别器字段。

我们可以使用带有鉴别器的`create`方法。