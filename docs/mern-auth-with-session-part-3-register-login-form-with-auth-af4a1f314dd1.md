# MERN 会话认证第 3 部分:注册/登录表单认证

> 原文：<https://blog.devgenius.io/mern-auth-with-session-part-3-register-login-form-with-auth-af4a1f314dd1?source=collection_archive---------0----------------------->

**TL；在本教程中，我们将使用 Expressjs、Session Auth、Reactjs 和 Material UI 构建一个登录/注册表单。**

这是三部分中的第三部分，在这一部分中，我们将构建一个注册和登录表单，向 API 发送数据，并使用 session 对象从后端获取用户数据。

下面是最终代码:[https://github.com/gsambrotta/auth-session-mern-boilerplate](https://github.com/gsambrotta/auth-session-mern-boilerplate)

下面是第 1 部分:[https://medium . com/@ design bygio/mern-auth-with-session-part-1-express-log in-API-922 CD 29336 A8](https://medium.com/@designbygio/mern-auth-with-session-part-1-express-login-api-922cd29336a8)

下面是第二部分:[https://medium . com/@ design bygio/mern-auth-with-session-part-2-session-with-MongoDB-and-express-b 185 c 17 ad 6 f 0](https://medium.com/@designbygio/mern-auth-with-session-part-2-session-with-mongodb-and-express-b185c17ad6f0)

# 会话登录是如何工作的？

我们将构建两个表单:一个注册表单和一个登录表单。
我们将使用注册表单创建一个新用户。
当用户登录时，我们将调用`/api/login`端点，这将创建一个会话对象。然后，我们将创建另一个端点`/api/isAuth`来检查会话对象是否存在。
当 React 应用程序渲染时，我们将调用这个端点。如果会话存在，我们将为用户显示一个带有欢迎消息的页面。如果没有，将显示登录和注册表单。

# 设置客户端

我们开始建立前端项目。首先，我们转到客户端文件夹，创建一个 react 应用程序。我不是`[create-react-app](https://create-react-app.dev/)`的忠实粉丝，但对于演示和小型项目来说，我认为它真的很棒，所以我要继续使用它。

## 代理人

我们需要对我们的`package.json`
做一个小的修改。让我们添加一个`proxy`配置，它将告诉前端后端的地址。

```
"proxy": "http://localhost:5000"
```

这样，当我们获取 API 端点时，我们可以简单地执行`/api/login`或`api/signup`而不包括域。这有助于避免 CORS 警告，但我们更需要它来查看前端的 cookie。如果我们不这样做，来自后端的 cookie 将为`localhost:5000`而不是`localhost:3000`建立。

## 材料用户界面设置

我安装了[材质 UI](https://mui.com/) 来更快地设计表单样式。
我也安装了 MUI(材质 UI)图标，因为我想使用一些时尚的图标。如果你不在乎让你的表单漂亮或者添加图标，可以跳过这一步。

App.js

从 MUI 开始，不需要实际的设置。
您可以简单地按照文档开始安装软件包，然后需要您想要使用的任何 MUI 组件。就是这样。尽管如此，还有一系列其他基本的东西可以让你的生活更轻松。

*   添加 **CssBaseline** 。这就像一个重置的 css，它将帮助你以一个干净的风格开始
*   加一个**主题**。虽然不强制使用主题，但如果您计划更改一些样式，从颜色到小间距，强烈建议使用主题。这只会让你的生活更轻松。
*   添加一个**字体**样式。

在`public/index.html`中，我将 **Roboto** 字体样式包含在文档头中:

```
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap" />
```

如果您决定添加自己的主题，您将需要在一个对象中编写主题配置，然后在`ThemeProvider`中使用该对象。然后你将整个应用程序包装在`ThemeProvider`中，这样你就可以在每个子组件中使用这个主题。

我在`styles/theme.js`的一个单独的文件中添加了我的主题对象，然后我在`app.js`中将它导入为`theme`。
这是我的`theme.js`:

主题. js

我们现在可以开始在组件中导入 MUI 组件，并查看应用的样式。

## 路线设置

您可能已经注意到，在`app.js`中，我们导入了`Routers`。让我们看看它们是什么。

路由器. js

简单地说，我们为显示登录组件的索引页面创建一个路由，然后为显示注册组件的注册页面创建另一个路由器。记得确保在`App.js`中包含并使用 Routers.js 文件。

好了，我们都设置好了，现在我们可以继续编写两个组件了。

# 登记表

我们前往`components`并创建一个`Signup.js`文件。这里是我们将要编写注册组件的地方。
最终代码相当冗长，因为我添加了几个 MUI 组件来对其进行样式化，我为密码字段添加了显示/隐藏功能，并且添加了验证来对错误进行样式化。

因此，我想先展示一个更精简的代码版本，只显示重要的部分，这样你就可以更容易地集成到你自己的组件中或者改变风格。该组件的完整版本将在稍后推出。

signup.js —更精简的版本

重要零件:

*   **堆栈组件**:堆栈组件是一个 MUI 组件，其作用类似于一个表单。它有一个`onSubmit`函数，指向自定义编写的`handleSubmit`函数。每次提交表单时，它都会调用`handleSubmit`函数。
    确保您的表单也是一个带有`type="submit"`的提交按钮，否则表单中的数据将不会被发送。
*   **handleSubmit 函数**:这是这个组件的核心。在这里，我们从表单中收集值，这些值之前保存在 React 状态中，我们向我们的 Express 后端`/api/registration`发出一个 fetch `POST`请求。然后，Express 会将新用户保存在我们的 MongoDB 上。(本教程的第 1 部分)
*   **handleChange 函数:**这是更新输入字段所必需的。如果没有这个，我们在输入字段中写的时候将看不到变化。

这是带有验证、错误消息、更多 MUI 组件和更好风格的完整代码。

sign up . js-完整版

为了测试它，我们需要运行应用程序，服务器和客户端。
因此，转到您的终端，在一个终端窗口中键入:

```
$ cd client 
$ npm start
```

而在另一个终端窗口类型中:

```
$ cd server
$ npm start
```

您现在应该能够在浏览器中转到`localhost:3000/signup`并看到一个表单。如果是，请填写表单并测试它是否成功地将数据发送到后端。如果一切正常，您应该会在 MongoDb Atlas 文档中看到一个新用户。

# 登录表单

像注册组件一样，我们转到`components`并创建一个新文件:`Login.js`。
这个文件与前一个非常相似。主要区别在于我们向用户请求的信息(这里只有电子邮件和密码)以及我们将这些信息发送到的端点。
同样，我将首先发布组件的精简版本，只包含重要的代码片段(表单、字段、提交按钮、获取函数和更改函数)，然后是完整的代码。

Login.js —更精简的代码

与注册组件完全一样，这里的主要部分是:

*   堆栈或表单组件+提交类型的按钮。当点击提交按钮时，我们在这里调用`hanldeSubmit`函数
*   HandleSubmit 函数:这里完成了对`/api/login`的`POST`请求。在 Express 中，我们在数据库中搜索用户，创建用户会话并将这些信息返回给客户端。
*   HandleChange 函数:保持输入值随着用户输入的内容而更新。

这里是登录组件的完整代码:

login.js —完整版

为了测试它，请记住运行两个应用程序，然后打开一个浏览器窗口到`localhost:3000`，您应该会看到登录表单。
使用您之前通过注册表单创建用户时使用的相同凭证登录(确保凭证在 MongoDB 数据库中)。
为了确保用户成功登录并创建会话，您应该在 web 控制台的 storage > cookie 部分看到包含用户信息的`session-id`。

# 具有反应上下文的验证端点

这个 cookie 是 **httpOnly** ，也就是说前端是看不到的。
所以我们不能用它来检查用户是否登录。
我们需要创建另一个端点，将会话对象返回给客户端(如果有的话)。

让我们回到我们的服务器文件夹，在登录路由中，创建另一个端点来检查用户是否经过身份验证。

```
$ cd server
```

然后在`loginRoutes.js`中，我们添加以下端点。
这里我们检查`req.session.user`是否存在。如果是，我们将它返回给客户端，否则我们返回一个错误。

log in routers . js-is auth 端点

太好了。现在我们需要在 React 应用程序中使用这条路线。
我们返回`client/src/App.js`，我们补充:

App.js —完整文件

这里我们添加了两个主要部分:

*   创建一个用户状态，然后获取`/api/isAuth`并将结果保存在用户状态中。
    结果可以是用户的会话对象，也可以是`undefined`
*   我们添加反应上下文。我们创建一个上下文，用上下文提供者包装我们的应用程序，然后将用户会话对象作为值赋予上下文。
    通过这种方式我们可以很容易地从每个子组件中访问用户信息。

现在，我们可以使用`routers`中的上下文来有条件地显示登录表单或经过验证的路由(即用户仪表板)。
为了使用上下文，我们使用了`useContext`钩子，并传入了在`App.js`中创建的`UserContext`。
然后我们简单检查`userContext`是否未定义。如果是，我们将显示登录/注册表单，否则我们将向用户显示欢迎消息。

Routers.js —完整版

# 包裹

现在我们终于可以使用带有持久身份验证的登录表单了。正如我们所见，认证需要几个步骤才能正常工作和有效实施。
显然，对于生产产品，我们需要考虑更多细节，但我认为这是一个使用 MERN 堆栈的典型会话认证应用的良好起点。

如果你觉得要做的事情太多，记得把大任务分开。确切地说，我是如何将本教程分为三个部分的，如果有必要，还可以进一步划分。慢慢来，摆弄一下代码，所有的过程很快就会清楚了。

![](img/e9d3774c03fc8200a0fb4ca3afda21ea.png)

你的学习之路需要支持吗？[我们来聊聊导师](https://www.codementor.io/@giorgiasambrotta?refer=badge)