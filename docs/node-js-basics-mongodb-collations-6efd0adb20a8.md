# Node.js 基础— MongoDB 排序规则

> 原文：<https://blog.devgenius.io/node-js-basics-mongodb-collations-6efd0adb20a8?source=collection_archive---------9----------------------->

![](img/df965752f08039873573ee76d4c436f0.png)

何塞·穆里略在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Node.js 是一个流行的运行时平台，用于创建在其上运行的程序。

它让我们可以在浏览器之外运行 JavaScript。

在本文中，我们将了解如何开始使用 Node.js 创建程序。

# 校对

排序规则是一组排序规则，在我们对特定语言和地区进行字符串排序时使用。

我们可以在创建集合时指定`collation`属性。

例如，我们可以写:

```
const { MongoClient } = require('mongodb');
const connection = "mongodb://localhost:27017";
const client = new MongoClient(connection);async function run() {
  try {
    await client.connect();
    const db = client.db("test");
    await db.dropCollection('test');
    await db.createCollection("test", {
      collation: { locale: "en" },
    });
    const testCollection = await db.collection('test');
    await testCollection.dropIndexes();
    await testCollection.deleteMany({})
    const result = await testCollection.insertMany([
      { "_id": 1, "name": "apples", "qty": 5, "rating": 3 },
      { "_id": 2, "name": "bananas", "qty": 7, "rating": 1 },
      { "_id": 3, "name": "oranges", "qty": 6, "rating": 2 },
      { "_id": 4, "name": "avocados", "qty": 3, "rating": 5 },
    ]);
    console.log(result)
    const query = {};
    const projection = { name: 1 };
    const cursor = testCollection
      .find(query)
      .project(projection);
    cursor.forEach(console.dir);
  } finally {
    await client.close();
  }
}
run().catch(console.dir);
```

我们用集合名作为第一个参数调用`createCollection`。

第二个参数是一个对象，我们使用了`collation`属性来设置排序规则。

然后我们调用`find`查询集合。

# 为索引分配排序规则

我们还可以将集合分配给索引。

例如，我们可以写:

```
const { MongoClient } = require('mongodb');
const connection = "mongodb://localhost:27017";
const client = new MongoClient(connection);async function run() {
  try {
    await client.connect();
    const db = client.db("test");
    await db.dropCollection('test');
    await db.createCollection("test");
    const testCollection = await db.collection('test');
    await testCollection.createIndex(
      { 'name': 1 },
      { 'collation': { 'locale': 'en' } });
    await testCollection.dropIndexes();
    await testCollection.deleteMany({})
    const result = await testCollection.insertMany([
      { "_id": 1, "name": "apples", "qty": 5, "rating": 3 },
      { "_id": 2, "name": "bananas", "qty": 7, "rating": 1 },
      { "_id": 3, "name": "oranges", "qty": 6, "rating": 2 },
      { "_id": 4, "name": "avocados", "qty": 3, "rating": 5 },
    ]);
    console.log(result)
    const query = { name: 'apple' };
    const options = { "collation": { "locale": "en_US" } };
    const cursor = testCollection
      .find(query, options)
    cursor.forEach(console.dir);
  } finally {
    await client.close();
  }
}
run().catch(console.dir);
```

我们有:

```
await testCollection.createIndex(
      { 'name': 1 },
      { 'collation': { 'locale': 'en' } });
```

我们用第一个参数中要索引的字段调用了`createIndex`。

第二个参数具有排序规则选项。

然后我们可以通过写来使用它:

```
const { MongoClient } = require('mongodb');
const connection = "mongodb://localhost:27017";
const client = new MongoClient(connection);async function run() {
  try {
    await client.connect();
    const db = client.db("test");
    await db.dropCollection('test');
    await db.createCollection("test");
    const testCollection = await db.collection('test');
    await testCollection.createIndex(
      { 'name': 1 },
      { 'collation': { 'locale': 'en' } });
    await testCollection.dropIndexes();
    await testCollection.deleteMany({})
    const result = await testCollection.insertMany([
      { "_id": 1, "name": "apples", "qty": 5, "rating": 3 },
      { "_id": 2, "name": "bananas", "qty": 7, "rating": 1 },
      { "_id": 3, "name": "oranges", "qty": 6, "rating": 2 },
      { "_id": 4, "name": "avocados", "qty": 3, "rating": 5 },
    ]);
    console.log(result)
    const cursor = testCollection
      .find()
      .sort({ "name": -1 });
    cursor.forEach(console.dir);
  } finally {
    await client.close();
  }
}
run().catch(console.dir);
```

我们用`sort`方法对`name`字段进行降序排序。

# 结论

我们可以使用 MongoDB 为特定语言添加排序规则。