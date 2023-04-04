# Node.js 提示—计划任务、Jest 测试和测试标题

> 原文：<https://blog.devgenius.io/node-js-tips-scheduled-tasks-jest-tests-and-headers-for-tests-4bad59e07f03?source=collection_archive---------5----------------------->

![](img/22f3d6bc93404d7f6deeacc88e7012c5.png)

照片由[Á·阿尔瓦罗·尼诺](https://unsplash.com/@alvaronino?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 用 Jest 循环动态测试用例

我们可以使用`tests.each`方法遍历每个 tets。

例如，我们可以写:

```
test.each([[4, 5, 9], [1, 2, 3], [2, 3, 5]])(
  'add(%i, %i)',
  (a, b, expected) => {
    expect(a + b).toBe(expected);
  },
);
```

第一个参数是我们希望传递到每个测试中的项目。

嵌套数组条目是回调的参数。

第二个参数是每个测试的名称。

第三次是回调，让争论和运行我们的期望。

# 使用 Supertest 设置默认标题

我们可以创建一个可以在测试中使用的头。

然后我们可以把它传递给`set`方法。

例如，我们可以写:

```
const request = require("supertest");const baseUrl = 'http://localhost';
request = request(baseUrl);
const commonHeaders = { authorization: "abc" };describe("testing", () => {
  it.should('present authorization header to server', (done) => {
    request.get('/foo')
      .set(commonHeaders)
      .set({ foo: "bar" })
      .expect(200, done)
  })})
```

`commonHeaders`在多个测试之间共享标题。

我们只是将它传递给`set`方法。

然后我们可以将其他特定于测试的头传递给`set`方法。

# 在 Node.js 中同步读取文件

为了与 Node 同步读取文件，我们可以使用`readFileSync`方法。

例如，我们可以写:

```
const fs = require('fs');
const content = fs.readFileSync('file');
console.log(content);
```

我们只需传入文件的路径，内容就会被返回。

# Base64 编码一个 JavaScript 对象

我们可以通过用`JSON.stringigy`将 JavaScript 对象转换成字符串来对其进行 base64 编码。

然后我们可以调用`Buffer.from`将其转换为缓冲区，然后调用`toString`将其转换为 base64 字符串。

例如，我们可以写:

```
const str = JSON.stringify(obj);
const objJsonB64 = Buffer.from(str).toString("base64");
```

将字符串化的 JSON 对象转换成字节流。

然后我们用`'base64'`参数调用`toString`将其转换成 base64 字符串。

# 猫鼬填充嵌入式

如果我们用相关的模式定义一个模式，我们可以用 Mongoose 在嵌入式文档上调用`populate`。

例如，如果我们有:

```
const UserSchema = new Schema({
  name: String,
  friends: [{ type: ObjectId, ref: 'User' }]
});
```

然后我们有一个自引用模式，因为`friends`引用了另一个`User`文档。

现在我们可以用`friends`来调用`populate`了:

```
User.
  findOne({ name: 'james' }).
  populate({
    path: 'friends',
    populate: { path: 'friends' }
  });
```

我们称`populate`为`friends`来获得朋友。

我们可以使用`populate`属性来获取朋友的朋友。

# 每天运行一项功能

我们可以通过使用`node-schedule`包以预定的方式运行一个函数。

例如，我们可以写:

```
import schedule from 'node-schedule'schedule.scheduleJob('0 0 0 * * *', () => { 
  //...
})
```

我们运行`schedule.scheduleJob`来按照字符串指定的时间表运行回调。

它接受一个字符串，该字符串以与 cron 作业相同的方式指定计划。

第一个数字是第二个。

第二个是分钟。

3 号是整点。

4 号是这个月的第一天。

5 号是月份。

6 号是一周中的第一天。

我们指定小时、分钟和秒来指定它在一天中的运行时间。

其余的都是星号，表示它将每天运行。

我们也可以指定日期来代替一个`cron` 格式的字符串。

例如，我们可以写:

```
const schedule = require('node-schedule');
const date = new Date(2020, 0, 0, 0, 0, 0);const j = schedule.scheduleJob(date, () => {
  console.log('hello');
});
```

那么回调只在给定的日期运行。

我们还可以设置重复规则。

例如，我们可以写:

```
const schedule = require('node-schedule');const rule = new schedule.RecurrenceRule();
rule.minute = 30;const j = schedule.scheduleJob(rule, () => {
  console.log('half hour job');
});
```

我们使用`schedule.RecurrenceRule`构造函数来创建一个规则。

我们在上面的例子中设置了`minute`。

但是我们也可以设置`second`、`hour`、`date`、`month`、`year`、`dayOfWeek`。

![](img/698d36877f3c8a96aa1387ae93016edd.png)

由[瓦尔德马尔·布兰特](https://unsplash.com/@waldemarbrandt67w?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

`node-schedule`是一个有用的调度程序包。

我们可以用 supertest 为测试设置标题。

Jest 可以动态定义和运行测试。

MongoDB 可以参考相关文档。

我们可以将 JavaScript 对象转换成 base64 字符串。