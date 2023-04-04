# Node.js 提示—测试事件、环境变量和编辑 Mongoose 对象

> 原文：<https://blog.devgenius.io/node-js-tips-testing-events-env-variables-and-editing-mongoose-objects-c8aff25aebaf?source=collection_archive---------26----------------------->

![](img/a0bd33c949cf90d42c315fe40ae6451e.png)

Andriyko Podilnyk 在 Unsplash 上的照片

像任何类型的应用程序一样，当我们编写节点应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些编写节点应用程序时常见问题的解决方案。

# 如何将图像从 URL 加载到 Node.js 中的缓冲区

我们可以请求图像。

然后我们可以使用`Buffer.from`方法将其加载到一个缓冲区中。

例如，我们可以写:

```
const response = await axios.get(url,  { responseType: 'arraybuffer' })
const buffer = Buffer.from(response.data, "utf-8")
```

我们使用 Axios HTTP 客户端发出一个 GET 请求。

为此，我们传入图像的 URL，并将响应的`responseType`设置为`'arraybuffer'`。

然后我们用`'utf-8'`编码将`response.data`传递给`Buffer.from`方法。

这将数据保存到缓冲区。

# 用 dotenv 加载环境变量

我们可以用 dotenv 库加载环境变量。

为此，我们写道:

```
const path = require('path')
require('dotenv').config({ path: path.resolve(__dirname, '../.env') })
```

我们需要`dotenv`库。

然后我们调用`config`方法来设置`.env`文件的路径。

我们指定从当前目录向上移动一级来找到`.env`文件。

# 从 Node.js 中的文本文件同步读取行

我们可以使用`readFileSync`方法同步读取一个文本文件。

例如，m 我们可以写成:

```
const fs = require('fs');
const lines = fs.readFileSync(filename, 'utf-8')
  .split('\n')
  .filter(Boolean);
```

我们调用`readFileSync`来读取文本文件的内容。

然后我们调用`split`通过换行符拆分字符串。

然后我们用`Boolean`调用`filter`来删除任何包含空字符串的行。

这是因为空字符串是假的。

所以`Boolean`会把它们转换成 false。

# 用 Node.js 从 URL 解析文件名

要从 URL 获取文件名，我们可以使用`url`和`path`模块。

例如，我们可以写:

```
const url = require("url");
const path = require("path");
const parsed = url.parse("http://example.com:8080/test/foo.txt/?q=100");
console.log(path.basename(parsed.pathname));
```

我们调用`url.parse`将 URL 解析成各个部分。

然后我们可以使用`basename`来获取查询字符串之前的 URL 的最后一部分，也就是`'foo.txt'`。

# 通过节点提取发送 Cookies

我们可以通过设置`cookie`头来发送带有节点获取的 cookies。

例如，我们可以写:

```
fetch('/some/url', {
  headers: {
    accept: '*/*',
    cookie: 'accessToken=1234; userId=1234',
  },
  method: 'GET',
});
```

我们使用库中的`fetch`函数。

cookie 设置在第二个参数的`headers`属性中。

`cookie`地产有饼干。

# 向从 Mongoose 返回的对象添加属性

如果我们想给从 Mongoose 中检索到的对象添加属性，我们可以使用`lean`方法，也可以对返回的文档调用`toObject`。

例如，我们可以写:

```
Item.findById(id).lean().exec((err, doc) => {
  //...
});
```

然后我们可以给`doc`添加属性，因为它是一个普通的对象。

或者我们可以写:

```
Item.findById(id).exec((err, doc) => {
  const obj = doc.toObject();
  //...
});
```

在回调中，我们调用`doc.toObject()`从文档中返回一个普通对象。

然后我们就可以随心所欲地操纵`obj`了。

# 如何用 Sinon 测试 Node.js 事件发射器

我们可以利用西农的间谍来测试事件发射器。

例如，我们可以写:

```
const sinon = require('sinon');
const EventEmitter = require('events').EventEmitter;describe('EventEmitter', function() {
  describe('#emit()', function() {
    it('should invoke the callback', () => {
      const spy = sinon.spy();
      const emitter = new EventEmitter();
      emitter.on('foo', spy);
      emitter.emit('foo');
      spy.called.should.equal.true;
    }) it('should pass arguments to the callbacks', () => {
      const spy = sinon.spy();
      const emitter = new EventEmitter();
      emitter.on('foo', spy);
      emitter.emit('foo', 'bar', 'baz');
      sinon.assert.calledOnce(spy);
      sinon.assert.calledWith(spy, 'bar', 'baz');
    })
  })
})
```

我们将`spy`作为事件处理函数传入。

然后我们可以检查`spy`是否被调用。

如果是，则发出事件。

否则就不是了。

我们可以通过使用`calledWith`来检查参数是否被`emit`调用。

![](img/02123763334c3c1296c0296254d9e0f0.png)

照片由 [Ariel Acosta](https://unsplash.com/@arielacosta?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以将图像作为数组缓冲区加载，然后保存到节点缓冲区。

dotenv 库可用于在节点应用程序中加载环境变量。

如果我们想向从 Mongoose 检索的文档添加属性，我们可以使用`lean`或调用`toObject`方法。

我们甚至可以用间谍测试发射器事件。