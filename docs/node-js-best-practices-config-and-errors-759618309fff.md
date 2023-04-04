# Node.js 最佳实践—配置和错误

> 原文：<https://blog.devgenius.io/node-js-best-practices-config-and-errors-759618309fff?source=collection_archive---------7----------------------->

![](img/126007b03423ae64f1b7b6231f124ee3.png)

由[奥莎娜·利亚申科](https://unsplash.com/@sanateddy?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用环境感知、安全和分层的配置

我们应该让我们的应用程序可配置。

为此，我们可以从环境变量中读取设置。

例如，我们可以使用 dotenv 库从一个`.env`文件和其他来源读取环境变量。

这将使我们的应用程序在任何地方都能正常运行。

# 使用 Async-Await 或 Promises 进行异步错误处理

异步代码应该用于任何长时间运行的操作。

链接异步代码的最糟糕的方式是拥有多层嵌套的异步回调。

它们很难阅读和调试。

最好的办法就是利用承诺，并把承诺链起来。

为了使它们更短，我们可以使用 async 和 await。

所以要么，我们写:

```
return functionA()
  .then(functionB)
  .then(functionC)
  .then(functionD)
  .catch((err) => console.error(err))
  .then(alwaysRunThisFunction)
```

或者:

```
async function executeAsyncTask () {
  try {
    const valueA = await functionA();
    const valueB = await functionB(valueA);
    const valueC = await functionC(valueB);
    return await functionD(valueC);
  }
  catch (err) {
    console.error(err);
  } 
  finally {
    await alwaysRunThisFunction();
  }
}
```

他们做同样的事情。

回调都返回承诺，因此我们可以将它们链接起来。

# 仅使用内置的错误对象

当我们抛出错误时，我们应该使用内置的错误类型。

例如，我们应该写:

```
new Error('whoops!')
```

而不是:

```
throw ('whoops!');
```

`Error`实例拥有堆栈跟踪等其他地方没有的信息。

# 区分操作错误和程序员错误

我们应该区分像 API 错误这样的操作错误，它们可以被正确地处理。

程序员错误是我们没有考虑到的错误，比如未定义的错误。

程序员的错误应该得到修复，以便我们的应用程序将运行更顺畅。

操作错误应该由我们的代码来处理。

# 集中处理错误，而不是在 Express 中间件中

错误应该集中处理，而不是使用快速中间件。

否则，我们将在多个地方得到重复的代码来处理相同的错误。

例如，不要写:

```
app.use((err, req, res, next) => {
  logger.logError(err);
  if (err.severity == errors.high) {
    mailer.sendMail(configuration.adminMail, 'error occured', err);
  }
  next(err);  
});
```

我们写道:

```
module.exports.handler = new ErrorHandler();function ErrorHandler() {
  this.handleError = async (err) => {
    await logger.logError(err);
    await sendEmail;
    await saveInOpsQueueIfCritical;
    await determineIfOperationalError;
  };
}
```

我们有一个中央错误处理器，可以导入到其他模块中。

# 使用 Swagger 或 GraphQL 记录 API 错误

我们应该用 Swagger 或 GraphQL 记录 REST APIs 可能出现的错误。

这样，我们可以使用 API 并处理记录的错误。

如果我们处理遇到的错误，那么它就不会崩溃。

# 优雅地退出流程

如果遇到未知错误，我们应该使用流程管理器优雅地退出流程。

这可以通过永久或 PM2 的流程管理器来完成。

如果出现任何错误，这些工具可以重新启动我们的应用程序。

![](img/d043b7d392d5491b4e2f0eac7485a7b4.png)

Hassan OUAJBIR 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们应该记录 API 可能抛出的任何错误，以便可以处理它们。

异步代码和使我们的应用程序可配置也是好主意。