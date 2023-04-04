# 用 express-validator 验证 Express.js 应用程序上的表单输入

> 原文：<https://blog.devgenius.io/validating-user-inputs-on-your-express-js-application-with-express-validator-4d82b995f524?source=collection_archive---------5----------------------->

![](img/4bbd9321f0c369346f6b7e8d7d96a9a9.png)

照片由[戈兰·艾沃斯](https://unsplash.com/@goran_ivos?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

表格无处不在。无论是简单的博客还是最复杂的应用程序，接收用户输入都是当今每个现代 web 应用程序最常见的功能之一。因此，在前端和后端验证来自用户的信息变得非常重要。在没有后端验证的情况下构建 API 就像在新房子上安装门而让窗框开着一样。这些门把一些人挡在外面，但任何知道环顾四周的人都可以穿过窗框。客户端验证很棒，但它仍然无法阻止来自技术用户的恶意数据，这些技术用户知道如何绕过浏览器对 HTTP 请求的默认行为。技术用户也知道 HTTP 请求可以通过多种方式发送。大多数时候甚至不需要浏览器。

幸运的是，作为开发人员，构建后端验证并不复杂。此外，还有一些由其他开发者构建的库可供我们免费使用。一些 ORM，比如 Mongoose for MongoDB，也内置了验证器。对于简单的应用程序，您可以很容易地使用简单的 if else 语句和正则表达式。然而，随着你的应用程序变得越来越大，需要进行内在的验证。

在本教程中，您将学习如何使用一个免费且易于安装的模块 Express-Validator 来验证基本和复杂的应用程序。

![](img/35a862fc029b3bae43d382e23815978b.png)

Mongoose 和 Vanilla Javascript 中验证的基本示例

# **项目设置**

需要注意的是，本教程需要预先了解 Nodejs 和 Express.js。我假设你已经知道如何设置你的 Nodejs 应用程序，并直接进入。

我们可以用`npm install express-validator --save`安装 express-validator，你可以像这样把它包含到你的应用入口点(我用的是 index.js)。

![](img/1ff312ebebe5f0995da39c2798c49050.png)

现在我们已经设置了我们的 express.js 应用程序，并要求我们的应用程序在端口`8080`上运行。此外，我们已经设置了一个接收`POST`请求的登录路径，现在只是向前端发回一条消息。

# 快速验证

我们新路线的问题就像上面解释的那样。任何人都可以向它发送任何类型的数据，并篡改我们的应用程序。我们的路线目前不与任何数据库相关，但如果我们正在构建一个现实生活中的应用程序，我们将最有可能用数据库检查我们的数据。发送任何恶意数据的黑客都可以通过这种途径为所欲为。

![](img/bd1015427846e35f6de77dfdb45e7dcf.png)

我们可以毫不费力地快速设置登录路径的验证:

![](img/f7d0fbfacd76f8aaf77875bb6fd0b383.png)

瞧啊。现在，我们正在检查我们的登录路径，以确保没有错误的数据，并且用户不只是发送一个空框。这里有几个命令，我将试着解释一下。

首先是 body 方法，它最初是在上面的代码中从 express-validator 导入的。接下来是`trim()` Javascript 方法，它删除了表单值周围的空白。然后是`isEmail()`方法，检查用户输入的电子邮件格式是否正确。电子邮件检查的最后一项是`notEmpty()`，它检查用户是否确实在传递一个值。还有一种 `withMessage`方法，在检查失败的情况下，将错误消息发送到前端。然而，它是可选的，并且当它没有被添加时，默认消息将被发送到前端。我意识到我错过了一个我很想参加的考试，叫做`normalizeEmail()`。`The normalizeEmail()`方法帮助检查用户输入的电子邮件并将其转换为标准格式。这意味着像`testdata@googlemail.com`这样的邮件会被转换成`testdata@gmail.com`。

对于密码，我们将`trim()`设置为空格，将`notEmpty()`设置为必填字段，将`isString()`设置为强制用户输入数据类型，将`isLength({min: 6})`设置为检查用户输入的密码是否不少于 6 个字符。我们也可以将 max 设置为同一个`isLength`对象的一部分。即使没有`withMessage`方法，对密码字段的`isLength`检查仍然有效，因为它是一个可选字段，如前所述。

在此之后，我们创建一个新变量，并将其设置为等于来自 express-validator 的`validationResult`函数。然后，我们检查它的长度，看它是否包含错误，并将响应发送回前端供用户查看。

如果我们的代码运行良好，如果用户没有以正确的格式输入电子邮件地址，我们会在控制台中得到这种错误消息。

![](img/1b0fa1b6f75be9e0851710daf4a4d554.png)

显示错误消息的代码块

这种测试有一个问题。现在我们只检查电子邮件和密码，这看起来可能不错。但是想象一下，当我们需要查看更多信息的时候，当用户注册时会发生什么。我们的代码很快变得臃肿和不可读。它甚至变得难以追踪。这是引入中间件的理想情况。

# 分离验证器和主代码

根据 [Express.js 网站](https://expressjs.com/en/guide/writing-middleware.html)的说法，中间件函数是在应用的请求-响应周期中可以访问请求(`req`)、响应(`res`)和 T2 函数的函数。这意味着他们可以访问用户发送的请求体，对其进行处理，并将操作传递给我们的主代码。这对我们很有用。

我们将在 app 目录中创建一个名为`validator.js`的新文件，并在其中添加以下验证代码。

![](img/8c0db4d47488bf175c2cfe8dd7b7083a.png)

中间件中的新验证代码

这里有一些我之前没有解释的新方法。有一种`isInt()`方法，它的工作方式和`isString()`一样，只是针对数字。`isArray()`检查以确认阵列。有一个`isBoolean`检查用户发送的是真/假值。我们还有与 Javascript 中的`!`工作方式相同的`not()`。它否定了即将到来的方法，所以当我们使用`not().isEmpty()`时，我们实际上是在检查那个字段是否为空。这只是编写`notEmpty`方法的不同方式。假设您正在构建一个移动应用程序，它接收来自用户的输入，并且必须检查该输入是否是一周中的某一天。在这种情况下，`isIn`方法会有所帮助。我们可以有`isIn['monday','tuesday','wednesday','thursday','friday','saturday',sunday']`。例如，当用户输入`mondayful`时，会返回一条错误消息。

我们可以在 index.js 入口点中使用这种检查，如下所示:

![](img/9f36d33298e172c9474b6989aaec6095.png)

使用中间件验证的代码块

这与之前的运行方式相同，并验证用户输入。

# 自定义验证程序

尽管 express-validator 模块有很多验证器可以在构建最大的应用程序时提供帮助。您自己的应用程序可能需要模块中尚未介绍的内容。Express-Validator 模块为自定义函数提供了空间，以提高应用程序的质量。我们将构建一个自定义函数来比较两个密码输入——即`confirm password and password fields`。我们可以使用已经构建在模块上的`custom()`特性来实现这一点。

![](img/7b489cb14eb2462d2a3ffabd0894b5fe.png)

显示自定义验证的代码块

express-validator 模块帮助我们处理大部分验证工作，并为我们提供了探索的空间。我已经尽了最大努力为你详细解释和解释，这样你就可以尽快开始了。

如果您遇到任何问题或觉得我应该做得更好，请在下面留下评论或随时在 [*Twitter*](https://twitter.com/eadelekeife) 上给我发消息，我会尽快回复。

您也可以关注我，以便在我发布新帖子时获得通知。这是我的第一个技术岗位，我一直在努力做得更好。同样，如果你认为这是值得一读的，请不要犹豫鼓掌——这对我真的意义重大。谢谢大家！:)