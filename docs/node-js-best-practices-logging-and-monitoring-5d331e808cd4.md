# Node.js 最佳实践—日志记录和监控

> 原文：<https://blog.devgenius.io/node-js-best-practices-logging-and-monitoring-5d331e808cd4?source=collection_archive---------6----------------------->

![](img/3a02f99cdae3ca6e3ba7f14bc790671f.png)

安东尼奥·格罗斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 集中处理错误

我们应该在节点应用程序中集中处理错误。

例如，我们可以在一个中间件中调用`next`来调用在它之后添加的错误处理中间件。

我们可以写:

```
try {
  User.addNew(req.body).then((result) => {
    res.status(200).json(result);
  }).catch((error) => {
    next(error)
  });
} catch (error) {
  next(error);
}
```

用`catch`方法捕捉承诺链中的错误。

通过 try-catch 块，我们可以捕获同步代码中的错误。

然后，我们可以通过编写以下代码在该代码之后添加一个错误处理程序中间件:

```
app.use((err, req, res, next) => {
  errorHandler.handleError(err).then((isOperationalError) => {
    if (!isOperationalError)
      next(err);
  });
});
```

我们调用另一个承诺链来处理任何操作错误。

这些是我们可以预见并妥善处理的错误。

# 使用 Swagger 记录 API 错误

API 错误应该以某种方式记录下来，以便处理。

我们不希望我们的应用程序崩溃。

Swagger 让我们以自动化的方式记录我们的 API。

如果我们不处理错误，应用程序就会崩溃。

# 遇到未知错误时，正常关闭进程

如果我们的应用程序遇到一些没有处理的错误，那么我们应该优雅地关闭应用程序，然后重新启动它。

一种常见的方法是使用像 PM2 这样的流程管理器来完成这项工作。

我们不希望任何错误使我们的应用程序崩溃并停机。

如果我们没有进程管理器，那么我们每次都必须自己重启它。

这肯定是不可接受的，因为它可能在一天中的任何时候发生多次。

我们可以通过编写以下代码来创建自己的错误处理程序:

```
function errorHandler() {
  this.handleError = function(error) {
    return logger.logError(err));
  } this.isTrustedError = function(error) {
    return error.isOperational;
  }
}
```

我们有一个错误处理构造函数，它有一些处理错误的方法。

# 使用成熟的记录器

如果一个节点应用程序在生产中，那么除非我们在某个地方记录错误，否则错误是不可见的。

有许多针对节点应用的日志解决方案，包括 Winston、Bunyan 和 Log4js。

它们将帮助我们更快地发现错误。

我们不应该使用像`console.log`这样粗糙的解决方案来记录生产中的错误。

这些错误应该存在于我们可以搜索的一些云服务或文件中。

例如，我们可以用 Winston 来写:

```
const logger = new winston.Logger({
  level: 'info',
  transports: [
    new (winston.transports.Console)(),
    new (winston.transports.File)({ filename: 'somefile.log' })
  ]
});logger.log('info', 'log: %s', 'something', { anything: 'This is metadata' });
```

我们使用`winston.Logger`构造函数来创建记录器。

然后我们调用`log`以我们想要的格式记录我们想要的东西。

# 使用 APM 产品发现错误和停机时间

APM 产品应记录错误和停机时间。

这样，我们可以很容易地在一个地方找到它们。

它们提供网站或 API 监控等功能。

每个请求都记录了它们的性能指标。

如果有慢代码，它会记录慢发生在哪里。

此外，他们将汇总数据，然后将它们一起显示在一个仪表板上。

![](img/4d26dccabc63424b5766692b4ba72dde.png)

照片由 [Prasunjit Dey](https://unsplash.com/@prasunjit?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

错误应该集中处理。

此外，日志记录和监控应该使用各种库和服务来完成。