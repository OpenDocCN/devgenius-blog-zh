# Node.js 基础— MongoDB 游标

> 原文：<https://blog.devgenius.io/node-js-basics-mongodb-cursor-6fe8aa8b8c03?source=collection_archive---------3----------------------->

![](img/9d97b152f08d099fee613e47b4616fee.png)

Jan Antonin Kolar 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Node.js 是一个流行的运行时平台，用于创建在其上运行的程序。

它让我们可以在浏览器之外运行 JavaScript。

在本文中，我们将了解如何开始使用 Node.js 创建程序。

# 事件 API

我们可以使用 Node.js 的事件 API 从我们的集合中获得查询结果。

例如，我们可以写:

```
const { MongoClient } = require('mongodb');
const connection = "mongodb://localhost:27017";
const client = new MongoClient(connection);async function run() {
  try {
    await client.connect();
    const testCollection = await client.db("test").collection('test');
    const result = await testCollection.insertMany([
      {
        name: "Popeye",
        rating: 5,
        qty: 100
      },
      {
        name: "KFC",
        rating: 4,
        qty: 121
      },
    ]);
    console.log(result)
    const query = {
      name: "Popeye",
    };
    const cursor = testCollection.find(query);
    cursor.on("data", data => console.log(data));
  } finally {
    await client.close();
  }
}
run().catch(console.dir);
```

观察`'data'`事件，这样我们就可以从结果中获取数据。

# 以数组形式获取结果

我们可以把结果作为一个数组。

例如，我们可以写:

```
const { MongoClient } = require('mongodb');
const connection = "mongodb://localhost:27017";
const client = new MongoClient(connection);async function run() {
  try {
    await client.connect();
    const testCollection = await client.db("test").collection('test');
    const result = await testCollection.insertMany([
      {
        name: "Popeye",
        rating: 5,
        qty: 100
      },
      {
        name: "KFC",
        rating: 4,
        qty: 121
      },
    ]);
    console.log(result)
    const query = {
      name: "Popeye",
    };
    const cursor = testCollection.find(query);
    const allValues = await cursor.toArray();
    console.log(allValues);
  } finally {
    await client.close();
  }
}
run().catch(console.dir);
```

我们调用`cursor`上的`toArray`方法，该方法返回解析为来自`test`集合的结果的承诺。

# 游标实用程序方法

cursor 对象有几个实用方法。

`count`方法返回从集合中检索到的结果数。

例如，我们可以写:

```
const { MongoClient } = require('mongodb');
const connection = "mongodb://localhost:27017";
const client = new MongoClient(connection);async function run() {
  try {
    await client.connect();
    const testCollection = await client.db("test").collection('test');
    const result = await testCollection.insertMany([
      {
        name: "Popeye",
        rating: 5,
        qty: 100
      },
      {
        name: "KFC",
        rating: 4,
        qty: 121
      },
    ]);
    console.log(result)
    const query = {
      name: "Popeye",
    };
    const cursor = testCollection.find(query);
    const count = await cursor.count();
    console.log(count);
  } finally {
    await client.close();
  }
}
run().catch(console.dir);
```

获取查询返回的项目。

`rewind`方法让我们将光标重置到它在返回文档集中的初始位置。

例如，我们可以写:

```
const { MongoClient } = require('mongodb');
const connection = "mongodb://localhost:27017";
const client = new MongoClient(connection);async function run() {
  try {
    await client.connect();
    const testCollection = await client.db("test").collection('test');
    const result = await testCollection.insertMany([
      {
        name: "Popeye",
        rating: 5,
        qty: 100
      },
      {
        name: "KFC",
        rating: 4,
        qty: 121
      },
    ]);
    console.log(result)
    const query = {
      name: "Popeye",
    };
    const cursor = testCollection.find(query);
    const firstResult = await cursor.toArray();
    console.log("First count: ", firstResult.length);
    await cursor.rewind();
    const secondResult = await cursor.toArray();
    console.log("Second count: ", secondResult.length);
  } finally {
    await client.close();
  }
}
run().catch(console.dir);
```

我们调用了`rewind`，在调用`rewind`前后从光标处得到结果。

我们在每个控制台日志中得到相同的结果，因为我们调用了`rewind`来重置光标。

# 结论

我们可以监听`data`事件，从那里获取数据。

此外，我们可以使用光标做各种检索操作。