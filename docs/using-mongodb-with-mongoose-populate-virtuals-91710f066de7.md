# 使用 MongoDB 和 mongose——填充虚拟机

> 原文：<https://blog.devgenius.io/using-mongodb-with-mongoose-populate-virtuals-91710f066de7?source=collection_archive---------2----------------------->

![](img/170019e76b7a7cdb4217ac9405e4084f.png)

由[杜安·斯美塔纳](https://unsplash.com/@veverkolog?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了简化 MongoDB 数据库操作，我们可以使用 mongose NPM 包来简化 MongoDB 数据库的操作。

在本文中，我们将了解如何使用 Mongoose 来操作我们的 MongoDB 数据库。

# 填充虚拟

我们可以控制两个模型如何连接在一起。

例如，我们可以写:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const db = createConnection('mongodb://localhost:27017/test');
  const PersonSchema = new Schema({
    name: String,
    band: String
  }); const BandSchema = new Schema({
    name: String
  }); BandSchema.virtual('members', {
    ref: 'Person',
    localField: 'name',
    foreignField: 'band',
    justOne: false,
    options: { sort: { name: -1 }, limit: 5 }
  }); const Person = db.model('Person', PersonSchema);
  const Band = db.model('Band', BandSchema);
  const person = new Person({ name: 'james', band: 'superband' });
  await person.save();
  const band = new Band({ name: 'superband' });
  await band.save();
  const bands = await Band.find({}).populate('members').exec();
  console.log(bands[0].members);
}
run();
```

我们照常创建`PersonSchema`，但是`BandSchema`是不同的。

我们调用带有连接字段调用`members`的`virtual`方法，将带有`band`名字的人设置为给定的名字。

`ref`属性是我们想要加入的模型的名称。

`localField`是我们想要与`PersonSchema`连接的`BandSchema`的字段。

`foreignField`是`PersonSchema`的字段，我们希望将其与`BandSchema`连接起来。

`justOne`意味着我们只返回连接的第一个条目。

`options`有查询选项。

默认情况下，`toJSON()`输出中不包含虚拟。

如果我们想在使用依赖于`JSON.stringify()`的函数时填充虚拟显示，那么添加`virtuals`选项并将其设置为`true`。

例如，我们可以写:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const db = createConnection('mongodb://localhost:27017/test');
  const PersonSchema = new Schema({
    name: String,
    band: String
  }); const BandSchema = new Schema({
    name: String
  }, { toJSON: { virtuals: true } }); BandSchema.virtual('members', {
    ref: 'Person',
    localField: 'name',
    foreignField: 'band',
    justOne: false,
    options: { sort: { name: -1 }, limit: 5 }
  }); const Person = db.model('Person', PersonSchema);
  const Band = db.model('Band', BandSchema);
  const person = new Person({ name: 'james', band: 'superband' });
  await person.save();
  const band = new Band({ name: 'superband' });
  await band.save();
  const bands = await Band.find({}).populate('members').exec();
  console.log(bands[0].members);
}
run();
```

将`virtuals`选项添加到`BandSchema`中。

如果我们使用填充投影，那么`foreignField`应该包含在投影中:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const db = createConnection('mongodb://localhost:27017/test');
  const PersonSchema = new Schema({
    name: String,
    band: String
  }); const BandSchema = new Schema({
    name: String
  }, { toJSON: { virtuals: true } }); BandSchema.virtual('members', {
    ref: 'Person',
    localField: 'name',
    foreignField: 'band',
    justOne: false,
    options: { sort: { name: -1 }, limit: 5 }
  }); const Person = db.model('Person', PersonSchema);
  const Band = db.model('Band', BandSchema);
  const person = new Person({ name: 'james', band: 'superband' });
  await person.save();
  const band = new Band({ name: 'superband' });
  await band.save();
  const bands = await Band.find({}).populate({ path: 'members', select: 'name band' }).exec();
  console.log(bands[0].members);
}
run();
```

我们用一个带有`path`的对象调用`populate`来获取虚拟字段，而`select`属性有一个字符串，其中包含我们希望用空格分隔的字段名。

# 结论

我们可以使用 Mongoose 填充虚拟特性，通过一个列而不是一个 ID 列来连接两个模型。