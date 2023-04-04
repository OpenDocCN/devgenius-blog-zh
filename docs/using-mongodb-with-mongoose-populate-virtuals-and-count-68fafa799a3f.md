# 使用 MongoDB 和 mongose——填充虚拟体并计数

> 原文：<https://blog.devgenius.io/using-mongodb-with-mongoose-populate-virtuals-and-count-68fafa799a3f?source=collection_archive---------3----------------------->

![](img/0806646915697abc493c6490f392e496.png)

安迪·霍姆斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了简化 MongoDB 数据库操作，我们可以使用 mongose NPM 包来简化 MongoDB 数据库的操作。

在本文中，我们将了解如何使用 Mongoose 来操作我们的 MongoDB 数据库。

# 填充虚拟:计数选项

当我们创建一个 populate virtual 时，我们可以添加`count`选项来获得我们检索到的孩子的数量。

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
  }, { toJSON: { virtuals: true } }); BandSchema.virtual('numMembers', {
    ref: 'Person',
    localField: 'name',
    foreignField: 'band',
    count: true
  }); const Person = db.model('Person', PersonSchema);
  const Band = db.model('Band', BandSchema);
  const person = new Person({ name: 'james', band: 'superband' });
  await person.save();
  const band = new Band({ name: 'superband' });
  await band.save();
  const doc = await Band.findOne({ name: 'superband' })
    .populate('numMembers');
  console.log(doc.numMembers);
}
run();
```

我们创建了`PersonSchema`和`BandSchema`模式对象。

我们用几个选项在`BandSchema`模式上调用`virtual`方法。

`ref`属性是`Band`模型引用的模型。

`localField`是`BandSchema`中我们希望与`PeronSchema`连接的字段。

`foreignField`是`ForeignSchema`中我们要与`PersonSchema`连接的字段。

`count`设置为`true`意味着我们得到了计数。`numMembers`是我们从中获取计数的字段名称。

然后我们保存一个同名的`Person`和`Band`文档。

然后我们用`numMembers`字段调用`populate`来获得成员的数量。

最后，我们从检索到的结果中得到`numMembers`字段，从而得到`Band`中有多少个`Person`子条目。

# 在中间件中填充

我们可以添加前置或后置挂钩来填充操作。

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
  }, { toJSON: { virtuals: true } }); BandSchema.virtual('numMembers', {
    ref: 'Person',
    localField: 'name',
    foreignField: 'band',
    count: true
  }); BandSchema.pre('find', function () {
   this.populate('person');
  }); BandSchema.post('find', async (docs) => {
    for (const doc of docs) {
      await doc.populate('person').execPopulate();
    }
  }); const Person = db.model('Person', PersonSchema);
  const Band = db.model('Band', BandSchema);
  const person = new Person({ name: 'james', band: 'superband' });
  await person.save();
  const band = new Band({ name: 'superband' });
  await band.save();
  const doc = await Band.findOne({ name: 'superband' })
    .populate('numMembers');
  console.log(doc.numMembers);
}
run();
```

增加`pre`和`post`挂钩以监听`find`操作。

我们应该总是用给定的字段调用`populate`来做填充。

# 结论

我们可以添加`count`属性来增加计数，还可以在填充时为`find`操作添加前置和后置挂钩。