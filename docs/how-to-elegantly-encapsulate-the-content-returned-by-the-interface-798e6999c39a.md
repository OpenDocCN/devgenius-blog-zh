# 如何优雅地封装接口返回的内容？

> 原文：<https://blog.devgenius.io/how-to-elegantly-encapsulate-the-content-returned-by-the-interface-798e6999c39a?source=collection_archive---------19----------------------->

SpringBoot 接口统一返回

![](img/588e114a8b20e3986363e10a20e6a5cc.png)

照片由 [Slava Keyzman](https://unsplash.com/@slavasfotos?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

用 SpringBoot 开发 Restful 接口时，统一返回便于前端开发和封装，出现时给出响应代码和信息。

**RESTful API。**

什么休息？代表性状态转移。

用一句话概括:REST 是所有 web 应用程序都应该遵守的架构设计准则。

面向资源是 REST 最明显的特征，是对同一资源的一组不同操作。资源是服务器上可命名的抽象概念。资源是围绕名词组织的。首先要重点关注的是名词。

REST 要求对资源的各种操作必须通过一个统一的接口来执行，每个资源只能执行有限的一组操作。

什么是 RESTful API？

符合 REST 设计标准的 API 是 RESTful API。REST 架构设计、标准和遵循的准则是 HTTP 协议的性能。

换句话说，HTTP 协议是一种属于 REST 架构的设计模式。比如无状态，请求-响应。

Restful 相关文档可以参考[https://restfulapi.net/](https://restfulapi.net/)。

**为什么要统一封装接口？**

现在大部分项目都是前后分离的模式开发，统一返回方便前端开发和封装，出现时给出响应代码和信息。

就查询用户界面而言，如果没有封装，返回的结果如下。

```
{
  "userId": 1,
  "userName": "foo"
}
```

如果是封装的，返回的正常结果如下:

```
{
  "timestamp": 11111111111,
  "status": 200,
  "message": "success",
  "data": {
    "userId": 1,
    "userName": "foo"
  }
}
```

异常返回结果如下:

```
{
  "timestamp": 11111111111,
  "status": 10001,
  "message": "User not exist",
  "data": **null**
}
```

**实施案例。**

# 1.状态代码封装。

下面是一个常见状态代码的例子，它包含两个属性，`responseCode`和`description`。

如果有其他业务状态代码，也可以放在这个类中。

# 2.返回内容封装。

包含公共接口返回时间、`status`、`message`、`data`。

考虑到数据的序列化(如通过网络传输)，这里的数据有时`extends Serializable`。

# 3.当接口返回时调用。

接口返回时调用，以用户界面为例。

感谢您阅读这篇文章。

敬请关注更多内容。