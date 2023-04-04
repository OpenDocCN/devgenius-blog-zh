# Node.js 提示——mongose 更新、HTML 电子邮件和存根依赖

> 原文：<https://blog.devgenius.io/node-js-tips-mongoose-updates-html-emails-and-stubbing-dependencies-1f15b509aa81?source=collection_archive---------19----------------------->

![](img/08f581a0140061a3caceab51d1d25db0.png)

丹·缪尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 通过 async/await 使用 Mongoose 承诺

猫鼬方法返回承诺。

所以我们可以用它们搭配`async`和`await`。

例如，我们可以写:

```
const orderEmployees = async(companyID) => {
  try {
    const employees = await User.find({ company: companyID }).exec();
    console.log(employees);
    return employees;
  } catch (err) {
    return 'error occured';
  }
}
```

我们可以通过查询`User.find`。

它返回一个带有解析结果的承诺。

我们也可以用`catch`捕捉错误。

# 将变量传递给 nodemailer 中的 HTML 模板

Nodemailer 接受 HTML 作为内容。

为了使创建 HTML 电子邮件更容易，我们可以使用像 Handlebars 这样的模板引擎。

例如，我们可以写:

```
const nodemailer = require('nodemailer');
const smtpTransport = require('nodemailer-smtp-transport');
const handlebars = require('handlebars');
const path= require('path');
const fs = require('fs');const readHTMLFile = (path, callback) => {
  fs.readFile(path, {
    encoding: 'utf-8'
  }, (err, html) => {
    if (err) {
      throw err;
      callback(err);
    } else {
      callback(null, html);
    }
  });
};smtpTransport = nodemailer.createTransport(smtpTransport({
  host: mailConfig.host,
  secure: mailConfig.secure,
  port: mailConfig.port,
  auth: {
    user: mailConfig.auth.user,
    pass: mailConfig.auth.pass
  }
}));readHTMLFile(path.join(__dirname, '/template.html'), (err, html) => {
  const template = handlebars.compile(html);
  const replacements = {
    username: "joe"
  };
  const htmlToSend = template(replacements);
  const mailOptions = {
    from: 'from@email.com',
    to: 'to@email.com',
    subject: 'test',
    html: htmlToSend
  };
  smtpTransport.sendMail(mailOptions, (error, response) => {
    if (error) {
      console.log(error);
      callback(error);
    }
  });
});
```

我们导入车把库，用模板调用`compiler`。

模板从使用`fs`读取文件的`readHTMLFile`函数中读取。

然后传递给回调。

我们得到带有`html`参数的模板。

一旦我们编译了模板，我们就用带有属性的对象调用`template`,将属性插入到模板中，得到最终的电子邮件。

然后我们调用`smptTransport.sendMail`发送邮件。

我们用 HTML 内容设置了`html`属性。

`htmlToSend`是一个 HTML 字符串。

# 从 MongoDB 格式化 ISODate

我们可以用`toTimeString`方法格式化 MongoDB 中的 date 对象。

我们只是写信给 ut:

```
const date = new Date("2019-07-14T01:00:00+01:00")
consr str = date.toTimeString();
```

MongoDB 也有`ISODate`函数来简化 ISO 日期字符串的处理。

例如，我们可以写:

```
ISODate("2019-07-14T01:00:00+01:00").toLocaleTimeString()
```

解析 ISO 日期字符串并将其转换为区分区域设置的时间字符串。

我们还可以得到小时和分钟:

```
ISODate("2019-07-14T01:00:00+01:00").getHours()
ISODate("2019-07-14T01:00:00+01:00").getMinutes()
```

# 测试期间 Stub Node.js 内置 fs

我们想要模仿`fs`模块，这样测试就不会进行真正的文件操作。

为此，我们使用`rewire`来剔除所需的模块。

例如，如果我们真正的代码是:

`reader.js`

```
const fs = require('fs');
const findFile = (path, callback) => {
  fs.readdir(path, (err, files) => {
     //...
  })
}
```

我们可以使用`rewire`来删除测试代码中的`readdir`,方法是:

```
const rewire = require('rewire')
const reader = rewire('./reader')

const fsStub = {
  readdir(path, callback){
    callback(null, [])
  }
}reader.__set__('fs', fsStub)

reader()
```

现在我们可以使用`reader`而不用真正调用`readdir`，因为我们调用了`__set__`来设置`fs`模块到我们的存根，而不是读取`fs`模块。

# 用猫鼬返回更新的集合

我们可以在 Mongoose 中返回一个更新的集合，然后我们可以通过将`new`选项设置为`true`从`findOneAndUpdate`中获取它。

例如，我们可以写:

```
Model
  .findOneAndUpdate({
    user_id: data.userId
  }, {
    $set: {
      session_id: id
    }
  }, {
    "new": true
  })
  .exec()
  .then(data => {
    return {
      sid: data.session_id
    }
  })
```

我们调用`findOneAndUpdate`，然后使用`$set`来更新一个字段，

我们传入一个属性设置为`true`的对象，在`then`回调中返回最新的数据。

此外，我们可以写:

```
Lists.findByIdAndUpdate(listId, {
  $push: {
    items: taskId
  }
}, (err, list) => {
  ...
});
```

我们用`list`得到最新的名单。

`$push`在文档的`items`列表中追加一个项目。

![](img/a1e4956e99b250f338f9816c469321ac.png)

照片由 [Ruby Schmank](https://unsplash.com/@rubyschmank?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以在更新后得到最新的列表。

我们可以用`rewire`来存根依赖关系。

我们可以在 Nodemailer 中使用 HTML 模板。

猫鼬方法返回承诺。