# Node.js 最佳实践—数据验证

> 原文：<https://blog.devgenius.io/node-js-best-practices-data-validation-f7d2a608e599?source=collection_archive---------1----------------------->

![](img/7279157aa7e895af4b715aed2a39aecc.png)

照片由 [Leighann Blackwood](https://unsplash.com/@ohleighann?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 具有已知安全漏洞的组件

我们应该用 AWS CloudTrail 记录和审计每个对云管理服务的 API 调用。

应该运行我们的云提供商提供的安全检查器。

# 记录和监控

记录和监控应该足够了。

我们应该寻找任何可疑的审计事件，如用户登录、用户创建、权限更改等。

如果有登录失败，我们应该得到警告。

应该记录每个数据库记录中启动更新的时间和用户名。

# 跨站点脚本

为了避免跨站点脚本，我们应该使用模板引擎和框架，它们通过设计自动转义脚本。

应该大部分都有这个特点。

应根据 HTML 输出对不受信任的 HTTP 请求数据进行转义。

在客户端修改浏览器文档时应用上下文相关编码可以防止在 DOM 上跨站点编写脚本。

此外，我们应该启用内容安全策略来防御跨站点脚本。

# 保护个人身份信息

个人认为，可识别的信息应该受到保护。

任何可用于识别个人身份的数据都应该加密。

不同国家制定了隐私法，所以我们应该遵守。

# 在生产中有一个 security.txt 文件

名为`security.txt`的文本文件应该在`./well-known`目录或根目录下。

它应该给出哪些安全研究人员可以报告漏洞的细节以及负责人的联系方式。

通过这种方式，我们可以在发现任何安全漏洞时得到通知。

# 有一个 SECURITY.md 文件

在一个代码项目中，我们可以有一个`security.md`文件，里面有项目所有者的联系方式。

这样，人们可以报告在项目中发现的漏洞。

# 调整 HTTP 响应头以增强安全性

我们应该调整 HTTP 响应头以增强安全性。

像跨站点脚本、点击劫持和其他恶意攻击这样的攻击利用响应标头中公开的数据来实施攻击。

# 不断自动检查易受攻击的依赖项

我们应该使用`npm audit`或`snyk` 来跟踪、监控和修补易受攻击的依赖项。

这些可以集成到我们的 CI 设置中，以便在生产之前发现易受攻击的依赖项。

# 避免使用 Node.js 加密库来处理密码

Node 加密库不像`bcrypt`那么安全，它允许我们加盐和散列我们的密码。

如果我们不加盐和散列，那么它们可能会被暴力破解或用字典攻击来猜测。

# 转义 HTML、JS 和 CSS 输出

我们应该避开 HTML、JavaScript 和 CSS，以便防止跨站脚本攻击。

我们可以使用专用库，将数据标记为不应运行的纯内容。

# 验证传入的 JSON 模式

传入的 JSON 应该用 JSON 模式进行验证，以确保来自请求的数据是有效的。

如果我们不检查它们，那么恶意的和无效的数据会进入我们的系统并引起问题。

像 [jsonschema](https://www.npmjs.com/package/jsonschema) 或 [joi](https://www.npmjs.com/package/joi) 这样的库让我们很容易地进行检查。

![](img/4e43614828f35a9ee79bc006c5559b0c.png)

[弗洛菲](https://unsplash.com/@theflouffy?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

我们应该检查我们的数据，使它们不是恶意的或无效的。

任何可以在我们的数据中运行的东西都应该被转义。