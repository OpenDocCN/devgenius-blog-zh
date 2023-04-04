# JavaScript 问题——计时器、案例、请求体等等

> 原文：<https://blog.devgenius.io/javascript-problems-timers-cases-request-bodies-and-more-2b9588864051?source=collection_archive---------29----------------------->

![](img/2c41a7202eedc953efb67684b60414b3.png)

迈克尔·波德格在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 在 Express 应用程序中检索 POST 参数

我们可以使用 body-parser 包在 Express 应用程序中获取 POST 参数。

要安装它，我们可以运行:

```
npm install --save body-parser
```

然后我们可以通过写来使用它:

```
const bodyParser = require('body-parser');
app.use(bodyParser.json());      
app.use(bodyParser.urlencoded({    
  extended: true
}));
```

我们在包中有`bodyParser`，它包含了`json`中间件，我们可以用它来解析来自 POST 请求的 JSON 数据。

`urlencoded`让我们解析 URL 编码的请求负载。

Express 4 没有 body-parser，所以我们必须按照上面的说明自己安装它。

Express 3 有这个包所以我们不用安装。

我们只是写:

```
app.use(express.json());      
app.use(express.urlencoded());
```

分别解析 JSON 和 URL 编码的有效负载。

在有效载荷被解析之后，我们可以写:

```
app.post('/test', (req, res) => {
  const name = req.body.name;
  // ...
});
```

`req.body`拥有我们在请求有效负载中包含的所有参数。

# 在浏览器控制台中包含 jQuery

如果我们在脚本标签中包含 jQuery，那么我们可以在控制台中使用它。

如果不是，那么我们可以通过编写以下代码用 JavaScript 创建脚本标记:

```
const jq = document.createElement('script');
jq.src = "https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js";
document.getElementsByTagName('head')[0].appendChild(jq);
jQuery.noConflict();
```

我们用 jQuery 的 URL 创建脚本标签，并将其附加到页面的 head 标签上。

# 使用 jQuery 打开一个引导模式窗口

使用 jQuery 很容易打开一个引导模型。

我们可以只获取模态的 DOM 元素，然后使用`modal`方法。

要打开或关闭它，我们可以写:

```
$('#modal').modal('toggle');
```

`#modal`是模态的选择器。

为了打开它，我们写道:

```
$('#modal').modal('show');
```

为了结束它，我们写下:

```
$('#modal').modal('show');
```

我们还可以添加一个按钮来打开模态，方法是编写“

```
<button type="button" data-toggle="modal" data-target="#modal">open modal</button>
```

`data-target`将模态的选择器作为值。

`data-toggle`设置为`modal`让我们打开模态。

# setTimeout 或 setInterval

`setTimeout`或`setInterval`虽然听起来很像，但却是不同的。

`setTimeout`用于延时后运行代码。

`setInterval`用于定期运行代码。

`setInterval`也可能因为 JavaScript 的单线程性质而延迟。

例如，我们可以写:

```
setTimeout(() => console.log('hello'), 1000);
```

然后`'hello'`在大约一分钟后被记录到控制台，因为它是 1000 毫秒。

此外，我们可以写:

```
setInterval(() => console.log('hello'), 1000);
```

每秒将`'hello'`记录到控制台。

我们可以用两个函数返回的定时器对象来清除两个定时器。

例如，我们可以写:

```
const timer = setTimeout(() => console.log('hello'), 1000);
```

并且:

```
const timer = setInterval(() => console.log('hello'), 1000);
```

然后我们可以写:

```
clearTimeout(timer);
```

并且:

```
clearInteval(timer);
```

从内存中删除计时器。

如果我们不需要它们，我们应该这样做来将它们从内存中清除。

# JavaScript 中具有多种情况的 Switch 语句

我们可以在一个案例中有多个`case`子句。

例如，我们可以写:

```
switch (val){
  case "foo":
  case "bar":
  case "baz": 
    alert('hello');
    break;
  default: 
    alert('default');
}
```

我们有一个包含 3 个`case`语句的子句，所以当`val`为`'foo'`、`'bar;`或`'baz'`时。

`'hello'`将显示在警报中。

否则，将显示`'default'`。

# 将带有 jQuery 的 div 放在屏幕中央

我们可以使用 jQuery 将一个 div 居中显示在屏幕上。

例如，我们可以写:

```
const center = (div) => {
  div.css("position", "absolute");
  div.css("transform", translate(-50%, -50%)");
}
```

是带有我们想要居中的 div 的 DOM 元素。

然后我们将`position`设置为`absolute`，这样我们就可以移动 div 了。

然后，我们使用`transform`属性根据宽度和高度将 div 平移屏幕的 50%。

![](img/ef7d1baa75a90ce610b9d9d96457d1aa.png)

照片由[卢克·麦克](https://unsplash.com/@lukemichael?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以通过用`translate`属性平移 div 并将位置设置为`abosolute`来使它居中。

`switch`可以有一个匹配多个值的事例。

我们可以通过 body-parser 包在 Express 应用程序中获得 POST 参数。

`setTimeout`和`setInterval`是延时或周期性运行代码的定时器。