# 使用 MongoDB 和 mongose——填充现有文档和跨数据库

> 原文：<https://blog.devgenius.io/using-mongodb-with-mongoose-populating-existing-documents-and-across-databases-99e6a7e2cfb8?source=collection_archive---------1----------------------->

![](img/cc60c9a84029725d07490403a6e084fe.png)

照片由[马特·伊森](https://unsplash.com/@matteason?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了简化 MongoDB 数据库操作，我们可以使用 mongose NPM 包来简化 MongoDB 数据库的操作。

在本文中，我们将了解如何使用 Mongoose 来操作我们的 MongoDB 数据库。

# 填充现有文档

我们可以在现有文档上调用`populate`。

例如，我们可以写:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const connection = createConnection('mongodb://localhost:27017/test');
  const personSchema = Schema({
    _id: Schema.Types.ObjectId,
    name: String,
    age: Number,
    stories: [{ type: Schema.Types.ObjectId, ref: 'Story' }]
  });
  const storySchema = Schema({
    author: { type: Schema.Types.ObjectId, ref: 'Person' },
    title: String,
    fans: [{ type: Schema.Types.ObjectId, ref: 'Person' }]
  });
  const Story = connection.model('Story', storySchema);
  const Person = connection.model('Person', personSchema);
  const author = new Person({
    _id: new Types.ObjectId(),
    name: 'James Smith',
    age: 50
  });
  await author.save();
  const fan = new Person({
    _id: new Types.ObjectId(),
    name: 'Fan Smith',
    age: 50
  });
  await fan.save();
  const story1 = new Story({
    title: 'Mongoose Story',
    author: author._id,
  });
  story1.fans.push(fan);
  await story1.save();
  const story = await Story.findOne({ title: 'Mongoose Story' })
  await story.populate('fans').execPopulate();
  console.log(story.populated('fans'));
  console.log(story.fans[0].name);
}
run();
```

我们创建了填充了`fans`和`author`字段的故事。

然后我们用`Story.findOne`方法得到条目。

然后我们调用解析后的`story`对象中的`populate`，然后调用`execPopulate`进行连接。

现在控制台日志应该看到显示的`fans`条目。

# 填充多个现有文档

我们可以跨越多个层次进行填充。

例如，我们可以写:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const connection = createConnection('mongodb://localhost:27017/test');
  const userSchema = new Schema({
    name: String,
    friends: [{ type: Schema.Types.ObjectId, ref: 'User' }]
  });
  const User = connection.model('User', userSchema);
  const user1 = new User({
    _id: new Types.ObjectId(),
    name: 'Friend',
  });
  await user1.save();
  const user = new User({
    _id: new Types.ObjectId(),
    name: 'Val',
  });
  user.friends.push(user1);
  await user.save();
  const userResult = await User.
    findOne({ name: 'Val' }).
    populate({
      path: 'friends',
      populate: { path: 'friends' }
    });
  console.log(userResult);
}
run();
```

我们有一个自引用的`User`模式。

属性引用了模式本身。

然后当我们查询`User`时，我们用`path`调用`populate`进行查询。

我们可以使用`populate`属性跨多个级别进行查询。

# 跨数据库填充

我们可以在数据库中填充。

例如，我们可以写:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const db1 = createConnection('mongodb://localhost:27017/db1');
  const db2 = createConnection('mongodb://localhost:27017/db2'); const conversationSchema = new Schema({ numMessages: Number });
  const Conversation = db2.model('Conversation', conversationSchema); const eventSchema = new Schema({
    name: String,
    conversation: {
      type: Schema.Types.ObjectId,
      ref: Conversation
    }
  });
  const Event = db1.model('Event', eventSchema);
  const conversation = new Conversation({ numMessages: 2 });
  conversation.save();
  const event = new Event({
    name: 'event',
    conversation
  })
  event.save();
  const events = await Event
    .findOne({ name: 'event' })
    .populate('conversation');
  console.log(events);
}
run();
```

我们用链接到不同数据库连接的`Coversation`和`Event`模型创建了两个模型。

我们可以创建`Conversation`和`Event`条目并将它们链接在一起。

然后我们可以调用`Event`上的`findOne`来获取链接的数据。

# 结论

我们可以用 Mongoose 填充现有的文档和数据库。