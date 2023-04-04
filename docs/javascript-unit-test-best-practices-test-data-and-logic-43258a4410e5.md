# JavaScript 单元测试最佳实践—测试数据和逻辑

> 原文：<https://blog.devgenius.io/javascript-unit-test-best-practices-test-data-and-logic-43258a4410e5?source=collection_archive---------5----------------------->

![](img/f253d160c0611958711faeb0781203c0.png)

兰迪·拜恩在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

单元测试对于检查我们的应用程序是如何工作的非常有用。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究在编写 JavaScript 单元测试时应该遵循的一些最佳实践。

# 孤立地测试中间件

我们应该孤立地测试中间件，以便我们可以测试应用程序的关键部分。

各种各样的东西都在使用中间件，所以我们应该对它们进行测试。

因为它们是函数，所以我们可以单独测试它们。

我们可以直接调用它。

此外，我们还可以使用 node-mock-http 来监视中间件的行为。

我们可以这样写:

```
const unitUnderTest = require("./middleware");
const httpMocks = require("node-mocks-http");

test("should return http status 403 when request header is empty", () => {
  const request = httpMocks.createRequest({
    method: "GET",
    url: "/user/1",
    headers: {
      authentication: ""
    }
  });
  const response = httpMocks.createResponse();
  unitUnderTest(request, response);
  expect(response.statusCode).toBe(403);
});
```

我们使用节点模型 http 来提出我们的请求。

然后，我们可以将请求和响应对象传递到中间件中。

然后得到回应。

# 使用静态分析工具进行度量和重构

我们应该使用静态分析工具来提高我们的代码质量并保持我们的代码可维护性。

它对于检测重复、代码复杂性和其他问题非常有用。

我们可以使用 [Sonarqube](https://www.sonarqube.org/) 和 [Code Climate](https://codeclimate.com/) 等工具来检查静态分析我们的应用。

# 检查我们对节点相关混乱的准备情况

我们还应该检查服务器或进程何时死亡，或者内存何时过载等问题。

速度问题也是重要的检查。

为了定期释放资源，我们可以使用工具定期扼杀我们的应用实例。

我们可以通过性能测试来发现这些问题。

APM 工具也将帮助我们找到它们。

# 避免全局测试装置和种子

全局测试设备不是好的，因为它们在多个测试之间共享。

我们不想在多个测试之间共享它们，因为我们想要独立的测试。

测试不应该从被其他代码片段操纵的数据开始。

因此，我们应该在每次测试之后重置数据，以便我们始终能够使用干净的数据进行测试。

我们的测试应该能够以任何顺序运行，并且仍然给我们相同的结果。

它们还使我们的测试保持简单，因为它们更容易跟踪。

例如，我们写的内容如下:

```
it("should return true if we can log in with valid username and password", async () => {
  const username = 'james';
  const password = 'password';
  await addUser({ username, password });
  const result = login(username, password);
  expect(result).to.be(true);
});
```

我们创建一个新用户，然后使用该用户登录，以便我们知道我们正在测试的用户是否存在。

# 前端测试

前端测试也很重要。

# 将用户界面与功能分开

我们应该将 UI 与功能分开进行测试。

当我们测试组件逻辑时，UI 就变成了应该被提取的噪音。

这样，我们的测试可以只关注于操纵数据。

UI 可以单独进行测试。

我们可以禁用动画，只检查测试中的数据。

![](img/470aec4a17a68d353e12fd23efc9fc42.png)

照片由[杰玛·埃文斯](https://unsplash.com/@stayandroam?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以将逻辑测试与 UI 测试分开。

此外，我们单独测试中间件。

对于每个测试，还应该从头开始创建数据。