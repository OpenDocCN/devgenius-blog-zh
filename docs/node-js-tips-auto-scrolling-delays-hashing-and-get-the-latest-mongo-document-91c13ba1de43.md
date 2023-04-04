# Node.js 技巧——自动滚动、延迟、散列和更新 Mongo 文档

> 原文：<https://blog.devgenius.io/node-js-tips-auto-scrolling-delays-hashing-and-get-the-latest-mongo-document-91c13ba1de43?source=collection_archive---------18----------------------->

![](img/533b14fb3561ca4fb2967d2336ebb26b.png)

照片由[弗洛菲](https://unsplash.com/@theflouffy?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 在 Node.js 中使用 Q 延迟

Q promise 库有一个`delay`方法，可以异步延迟代码的执行。

例如，我们可以通过连线来使用它:

```
const Q = require('q');const asyncDelay = (ms) => {
  return Q.delay(ms);
}asyncDelay(1000)
.then(() => {
  //...
});
```

我们有返回`Q.delay(ms)`承诺的`asyncDelay`函数。

它返回一个承诺，将某事的执行延迟给定的毫秒数。

我们可以调用它，然后在`then`回调中的`asyncDelay`完成后，调用`then`并运行我们想要的代码。

此外，我们可以传入第二个参数，它可以被称为解析值。

例如，我们可以写:

```
const Q = require('q');Q.delay(1000, "Success")
  .then((value) => {
      console.log(value);
  });
```

那么`value`就是`'Success'`，因为那是我们作为`delay`的第二个参数传入的。

# 将图像转换为 base64 编码的数据 URL

要将图像转换成 base64 编码的数据 URL，我们可以使用`readFileSync`方法来读取文件的数据。

然后我们将内容传递给`Buffer`构造函数。

然后我们可以调用`toString`将其转换为 base64 字符串。

例如，我们可以写:

```
const fs = require('fs');const file = 'img.jpg';
const bitmap = fs.readFileSync(file);
const dataUrl = new Buffer(bitmap).toString('base64');
```

我们将文件路径传递给`readFileSync`。

然后我们将返回的内容传递给`Buffer`构造函数。

然后我们用`'base64'`调用`toString`将其转换为 base64 字符串。

# 用 MongoDB findOneAndUpdate 返回更新的文档

我们应该在 options 对象中将`returnOriginal`属性设置为`false`,以便在回调中返回更新后的文档。

例如，我们可以写:

```
db.collection('user_setting').findOneAndUpdate({
  user_id: data.userId
}, {
  $set: data
}, {
  returnOriginal: false
}, (err, res) => {
  if (err) {
    return console.log(err);
  } else {
    console.log(res);
  }
});
```

第一个参数是查找要更新的对象的查询。

第二个参数是用来更新对象的数据。

第三是更新选项。

我们在第三个参数中将`returnOriginal`选项设置为`false`。

最后一个参数是回调。

`res`应该有更新的结果。

# 在快速应用程序中使用与 NPM 一起安装的 jQuery

要在 Express 应用程序中使用与 NPM 一起安装的 jQuery，我们可以用`express.static`服务 jQuery 的`dist`文件夹。

例如，我们可以写:

```
const path = require('path');app.use('/jquery', express.static(path.join(__dirname,  '/node_modules/jquery/dist/')));
```

然后，我们可以通过编写以下代码在 HTML 文件中使用它:

```
<script src="/jquery/jquery.js"></script>
```

# 向下滚动，直到我们不能用木偶师滚动

我们可以检查我们是否在页面的末尾，并使用`scrollBy`方法来滚动。

例如，我们可以写:

```
const distance = 100;
const delay = 100;
while (document.scrollingElement.scrollTop + window.innerHeight < document.scrollingElement.scrollHeight) {
  document.scrollingElement.scrollBy(0, distance);
  await new Promise(resolve => { setTimeout(resolve, delay); });
}
```

我们检查是否可以滚动:

```
document.scrollingElement.scrollTop + window.innerHeight < document.scrollingElement.scrollHeight
```

当`scrollTop`和`innerHeight`小于`scrollHeight`时，我们可以滚动。

# 在 Node.js 中查找操作系统用户名

我们可以用`os`模块得到当前用户的用户名。

我们写道:

```
require("os").userInfo().username
```

要获取用户名

在 Windows 10 中，它返回已使用的所有者帐户的名字。

还有`username`模块。

我们可以通过运行以下命令来安装它:

```
npm install username
```

然后我们可以通过写来使用它:

```
const username = require('username');(async () => {
  console.log(await username());
})();
```

# 散列 JavaScript 对象

为了散列 JavaScript 对象，我们可以使用`object-hash`模块。

例如，我们可以写:

```
const hash = require('object-hash');const obj = {a: 1, b: 2};
const hashed = hash(obj);
```

我们只是从包中调用`hash`函数来返回一个散列字符串。

![](img/dc472aa652d4936de984abf5a50cce54.png)

照片由[查理·格林](https://unsplash.com/@charliegreen998?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用`object-hash`模块散列 JavaScript 对象。

此外，我们可以通过`os`或第三方模块获得用户名。

Q promise 库有一个`delay`方法来延迟函数的执行。

我们可以用`findOneAndUpdate`得到更新后的对象。

我们可以通过检查高度来滚动，直到我们不能用木偶师滚动。