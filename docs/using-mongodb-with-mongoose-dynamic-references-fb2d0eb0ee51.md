# 使用 MongoDB 和 mongose—动态引用

> 原文：<https://blog.devgenius.io/using-mongodb-with-mongoose-dynamic-references-fb2d0eb0ee51?source=collection_archive---------3----------------------->

![](img/e398516a0a355d103afb49ac99d0768d.png)

由[杜安·斯美塔纳](https://unsplash.com/@veverkolog?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了简化 MongoDB 数据库操作，我们可以使用 mongose NPM 包来简化 MongoDB 数据库的操作。

在本文中，我们将了解如何使用 Mongoose 来操作我们的 MongoDB 数据库。

# 通过“refPath”进行动态引用

我们可以用动态引用和`refPath`属性连接多个模型。

例如，我们可以写:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const db = createConnection('mongodb://localhost:27017/test');
  const commentSchema = new Schema({
    body: { type: String, required: true },
    subject: {
      type: Schema.Types.ObjectId,
      required: true,
      refPath: 'subjectModel'
    },
    subjectModel: {
      type: String,
      required: true,
      enum: ['BlogPost', 'Product']
    }
  });
  const Product = db.model('Product', new Schema({ name: String }));
  const BlogPost = db.model('BlogPost', new Schema({ title: String }));
  const Comment = db.model('Comment', commentSchema);
  const book = await Product.create({ name: 'Mongoose for Dummies' });
  const post = await BlogPost.create({ title: 'MongoDB for Dummies' });
  const commentOnBook = await Comment.create({
    body: 'Great read',
    subject: book._id,
    subjectModel: 'Product'
  });
  await commentOnBook.save();
  const commentOnPost = await Comment.create({
    body: 'Very informative',
    subject: post._id,
    subjectModel: 'BlogPost'
  });
  await commentOnPost.save();
  const comments = await Comment.find().populate('subject').sort({ body: 1 });
  console.log(comments)
}
run();
```

我们有具有`subject`和`subjectModel`属性的`commentSchema`。

`subject`被设置为一个对象 ID。我们有`refPath`属性，它引用了它可以引用的模型。

`refPath`设置为`subjectModel`，`subjectModel`参照`BlogPost`和`Product`型号。

因此我们可以将评论链接到一个`Product`条目或一个`Post`条目。

为了链接到我们想要的模型，我们在用`create`方法创建条目时设置了`subject`和`subjectModel`。

然后我们用`subject`调用`populate`来获取`subject`字段的数据。

同样，我们可以将相关的项目放到根模式中。

例如，我们可以写:

```
async function run() {
  const { createConnection, Types, Schema } = require('mongoose');
  const db = createConnection('mongodb://localhost:27017/test');
  const commentSchema = new Schema({
    body: { type: String, required: true },
    product: {
      type: Schema.Types.ObjectId,
      required: true,
      ref: 'Product'
    },
    blogPost: {
      type: Schema.Types.ObjectId,
      required: true,
      ref: 'BlogPost'
    }
  });
  const Product = db.model('Product', new Schema({ name: String }));
  const BlogPost = db.model('BlogPost', new Schema({ title: String }));
  const Comment = db.model('Comment', commentSchema);
  const book = await Product.create({ name: 'Mongoose for Dummies' });
  const post = await BlogPost.create({ title: 'MongoDB for Dummies' });
  const commentOnBook = await Comment.create({
    body: 'Great read',
    product: book._id,
    blogPost: post._id,
  });
  await commentOnBook.save();
  const comments = await Comment.find()
    .populate('product')
    .populate('blogPost')
    .sort({ body: 1 });
  console.log(comments)
}
run();
```

然后我们将`product`和`blogPost`设置在同一个对象中。

我们重新安排了`commentSchema`以包含`products`和`blogPost`引用。

然后我们在两个字段上调用`populate`,这样我们就可以得到注释。

# 结论

我们可以在一个模式中引用多个模式。

然后我们可以调用`populate`来获取所有字段。