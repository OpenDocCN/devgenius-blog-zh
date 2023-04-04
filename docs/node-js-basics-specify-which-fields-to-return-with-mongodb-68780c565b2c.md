# Node.js 基础——指定使用 MongoDB 返回哪些字段

> 原文：<https://blog.devgenius.io/node-js-basics-specify-which-fields-to-return-with-mongodb-68780c565b2c?source=collection_archive---------9----------------------->

![](img/8a0d2786affa10fdc93ef53bf238c0fe.png)

[Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Wonderlane](https://unsplash.com/@wonderlane?utm_source=medium&utm_medium=referral) 的照片

Node.js 是一个流行的运行时平台，用于创建在其上运行的程序。

它让我们可以在浏览器之外运行 JavaScript。

在本文中，我们将了解如何开始使用 Node.js 创建程序。

# 指定用 MongoDB 返回哪些字段

我们可以为每个条目/指定要返回的字段。

例如，我们可以写道:

```
const { MongoClient } = require('mongodb');
const connection = "mongodb://localhost:27017";
const client = new MongoClient(connection);async function run() {
  try {
    await client.connect();
    const testCollection = await client.db("test").collection('test');
    await testCollection.deleteMany({})
    const result = await testCollection.insertMany([
      { "_id": 1, "name": "apples", "qty": 5, "rating": 3 },
      { "_id": 2, "name": "bananas", "qty": 7, "rating": 1 },
      { "_id": 3, "name": "oranges", "qty": 6, "rating": 2 },
      { "_id": 4, "name": "avocados", "qty": 3, "rating": 5 },
    ]);
    console.log(result)
    const projection = { name: 1 };
    const cursor = testCollection.find().project(projection);
    await cursor.forEach(console.dir);
  } finally {
    await client.close();
  }
}
run().catch(console.dir);
```

我们用一个对象调用`project`方法，这个对象有我们想要包含在键中的键。

值为 1 意味着我们在结果中包含了给定的属性。

默认情况下自动返回`_id`字段。

所以我们得到:

```
{ _id: 1, name: 'apples' }
{ _id: 2, name: 'bananas' }
{ _id: 3, name: 'oranges' }
{ _id: 4, name: 'avocados' }
```

已退回。

如果我们想禁用它，我们可以写:

```
const { MongoClient } = require('mongodb');
const connection = "mongodb://localhost:27017";
const client = new MongoClient(connection);async function run() {
  try {
    await client.connect();
    const testCollection = await client.db("test").collection('test');
    await testCollection.deleteMany({})
    const result = await testCollection.insertMany([
      { "_id": 1, "name": "apples", "qty": 5, "rating": 3 },
      { "_id": 2, "name": "bananas", "qty": 7, "rating": 1 },
      { "_id": 3, "name": "oranges", "qty": 6, "rating": 2 },
      { "_id": 4, "name": "avocados", "qty": 3, "rating": 5 },
    ]);
    console.log(result)
    const projection = { _id: 0, name: 1 };
    const cursor = testCollection.find().project(projection);
    await cursor.forEach(console.dir);
  } finally {
    await client.close();
  }
}
run().catch(console.dir);
```

然后我们得到:

```
{ name: 'apples' }
{ name: 'bananas' }
{ name: 'oranges' }
{ name: 'avocados' }
```

我们可以在查询中指定多个字段。

例如，我们可以写:

```
const { MongoClient } = require('mongodb');
const connection = "mongodb://localhost:27017";
const client = new MongoClient(connection);async function run() {
  try {
    await client.connect();
    const testCollection = await client.db("test").collection('test');
    await testCollection.deleteMany({})
    const result = await testCollection.insertMany([
      { "_id": 1, "name": "apples", "qty": 5, "rating": 3 },
      { "_id": 2, "name": "bananas", "qty": 7, "rating": 1 },
      { "_id": 3, "name": "oranges", "qty": 6, "rating": 2 },
      { "_id": 4, "name": "avocados", "qty": 3, "rating": 5 },
    ]);
    console.log(result)
    const projection = { _id: 0, rating: 1, name: 1 };
    const cursor = testCollection.find().project(projection);
    await cursor.forEach(console.dir);
  } finally {
    await client.close();
  }
}
run().catch(console.dir);
```

然后，我们得到:

```
{ name: 'apples', rating: 3 }
{ name: 'bananas', rating: 1 }
{ name: 'oranges', rating: 2 }
{ name: 'avocados', rating: 5 }
```

已退回。

# 结论

我们可以选择使用 MongoDB 查询返回的字段。