# Node.js 基础— MongoDB 文本和唯一索引

> 原文：<https://blog.devgenius.io/node-js-basics-mongodb-text-and-unique-indexes-f232ad8c9057?source=collection_archive---------5----------------------->

![](img/8e5a03e416fdc33a156fbda7ed8ea667.png)

照片由 [Jason Leung](https://unsplash.com/@ninjason?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Node.js 是一个流行的运行时平台，用于创建在其上运行的程序。

它让我们可以在浏览器之外运行 JavaScript。

在本文中，我们将了解如何开始使用 Node.js 创建程序。

# 文本索引

文本索引让我们可以对包含字符串内容的查询进行文本搜索。

它可以包含任何值为字符串或字符串数组的字段。

例如，我们可以创建索引并通过编写“

```
const { MongoClient } = require('mongodb');
const connection = "mongodb://localhost:27017";
const client = new MongoClient(connection);async function run() {
  try {
    await client.connect();
    const db = client.db("test");
    const testCollection = await db.collection('test');
    await testCollection.dropIndexes();
    const indexResult = await testCollection.createIndex({ name: "text" }, { default_language: "english" });
    console.log(indexResult)
    await testCollection.deleteMany({})
    const result = await testCollection.insertMany([
      { "_id": 1, "name": "apples", "qty": 5, "rating": 3 },
      { "_id": 2, "name": "bananas", "qty": 7, "rating": 1 },
      { "_id": 3, "name": "oranges", "qty": 6, "rating": 2 },
      { "_id": 4, "name": "avocados", "qty": 3, "rating": 5 },
    ]);
    console.log(result)
    const query = { $text: { $search: "apple" } };
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

我们通过编写以下内容来创建索引:

```
const indexResult = await testCollection.createIndex({ name: "text" }, { default_language: "english" });
```

`createIndex`方法将索引添加到`name`字段。

第二个参数包含创建索引的选项。

`default_language`设置索引语言。

# 唯一索引

我们可以添加一个唯一的索引来索引不存储重复值的字段。

创建集合时，`_id`字段会添加一个唯一的索引。

例如，我们可以写:

```
const { MongoClient } = require('mongodb');
const connection = "mongodb://localhost:27017";
const client = new MongoClient(connection);async function run() {
  try {
    await client.connect();
    const db = client.db("test");
    const testCollection = await db.collection('test');
    await testCollection.dropIndexes();
    const indexResult = await testCollection.createIndex({ name: "text" }, { unique: true });
    console.log(indexResult)
    await testCollection.deleteMany({})
    const result = await testCollection.insertMany([
      { "_id": 1, "name": "apples", "qty": 5, "rating": 3 },
      { "_id": 2, "name": "bananas", "qty": 7, "rating": 1 },
      { "_id": 3, "name": "oranges", "qty": 6, "rating": 2 },
      { "_id": 4, "name": "avocados", "qty": 3, "rating": 5 },
    ]);
    console.log(result)
    const query = {};
    const projection { name: 1 };
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

调用`createIndex`，第二个参数是一个`unique`属性设置为`true`的对象。

如果集合中的字段中有任何重复值，那么当我们尝试创建索引时，就会得到“重复键错误索引”错误。

# 结论

我们可以为文本搜索添加索引，为 MongoDB 字段添加唯一值索引。