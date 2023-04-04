# Node.js 最佳实践—日志、测试和 Lint

> 原文：<https://blog.devgenius.io/node-js-best-practices-log-test-and-lint-279536228c15?source=collection_archive---------3----------------------->

![](img/b9069d92646911efe7b54ff7fba4d57a.png)

由 [Mysaell Armendariz](https://unsplash.com/@mysa21?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用记录器来增加错误可见性

我们需要让错误可见，以便我们可以跟踪和修复它们。

我们的应用程序中有许多记录工具，如 Pino 或 Winston 来记录错误。

例如，我们可以写:

```
const logger = new winston.Logger({
  level: 'info',
  transports: [
    new (winston.transports.Console)()
  ]
});logger.log('info', 'log %s', 'foo', { anything: 'some metadata' });
```

`logger.log`记录发生的错误。

# 使用测试框架测试错误流

测试错误案例和测试非错误案例一样重要。

如果我们测试了错误，那么我们就可以知道我们是否返回了正确的错误。

例如，我们可以写:

```
it('creates an item', () => {
  const invalidGroupInfo = {};
  return httpRequest({
    method: 'POST',
    uri: '/item',
    resolveWithFullResponse: true,
    body: invalidGroupInfo,
    json: true
  }).then((response) => {
    expect.fail('error')
  }).catch((response) => {
    expect(400).to.equal(response.statusCode);
  });
});
```

我们测试了一个 API，它在一些`expect`函数调用中抛出了一个错误。

# 使用 APM 产品发现错误和停机时间

我们可以检查 APM 产品的性能问题。

他们检查缓慢的代码并列出缓慢的代码。

此外，我们可以观察是否有停机时间。

一些工具包括 [Pingdom](https://www.pingdom.com/) 、 [Uptime Robot](https://uptimerobot.com/) 和 [New Relic](https://newrelic.com/application-monitoring) 。

他们让我们通过易于阅读的仪表板来检查问题。

# 捕捉未处理的承诺拒绝

如果我们有一个未处理的承诺拒绝，那么我们应该通过捕捉它们来处理它们。

例如，我们可以写:

```
Promise.reject('error').catch(() => {
  throw new Error('error');
});
```

我们在`Promise.reject`之后调用`catch`来捕捉被拒绝的承诺。

我们还可以捕捉`ubhandledRejection`或`uncaughtException`事件来捕捉错误。

为此，我们可以写:

```
process.on('unhandledRejection', (reason, p) => {
  throw reason;
});

process.on('uncaughtException', (error) => {
  handleError(error);
  if (!isTrustedError(error))
    process.exit(1);
});
```

我们可以用`process`模块监听这些事件，然后用我们的方式处理它们。

`reason`是拒绝承诺的原因。

第二次回调检查错误，然后针对某些错误重新启动。

# 使用专用库快速失败并验证参数

我们应该用一个专用的库来检查数据。

例如，我们可以写:

```
const memberSchema = Joi.object().keys({
  age: Joi.number().integer().min(0).max(200),
  email: Joi.string().email()
});function addNewMember(newMember) {
  Joi.assert(newMember, memberSchema); 
}
```

`Joi`库让我们根据模式验证我们的对象。

我们验证`age`是一个介于 0 和 200 之间的整数。

让我们检查一下电子邮件。

然后我们调用`Joi.assert`来做检查。

# 使用 ESLint

ESLint 让我们能够识别各种问题，包括格式、安全性和错误的构造。

因此，我们应该使用它来自动修复其中的许多问题。

还有其他工具可以统一格式化代码，比如[更漂亮的](https://www.npmjs.com/package/prettier)和[美化的](https://www.npmjs.com/package/js-beautify)，它们有更强大的格式化选项。

# Node.js 特定插件

ESLint 附带了各种插件，我们可以添加这些插件来对节点应用程序进行更多的检查。

它们包括像 [eslint-plugin-node](https://www.npmjs.com/package/eslint-plugin-node) 、 [eslint-plugin-mocha](https://www.npmjs.com/package/eslint-plugin-mocha) 和[eslint-plugin-node-security](https://www.npmjs.com/package/eslint-plugin-security)这样的东西。

[eslint-plugin-node](https://www.npmjs.com/package/eslint-plugin-node) 检查基本节点应用程序问题。

eslint-plugin-mocha 有基本的林挺摩卡代码。

[eslint-plugin-node-security](https://www.npmjs.com/package/eslint-plugin-security)检查安全问题。

![](img/adb3291530df59a05eae21dab0a8e881.png)

劳拉·德维尔德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以通过日志、测试和 lint 来改进我们的节点应用。