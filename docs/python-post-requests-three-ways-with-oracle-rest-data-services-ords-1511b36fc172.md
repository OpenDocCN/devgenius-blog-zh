# Python POST 通过三种方式请求 Oracle REST 数据服务(ORDS)

> 原文：<https://blog.devgenius.io/python-post-requests-three-ways-with-oracle-rest-data-services-ords-1511b36fc172?source=collection_archive---------20----------------------->

![](img/834bda79a51ddd579f02a0511e3b677c.png)

觉得可爱，以后可能会删。

# 简单的 POST 请求

让我烦恼的是，我不能对一个 [ORDS](https://www.oracle.com/cis/database/technologies/appdev/rest.html) 支持 REST 的表执行一个*简单的* Python POST 请求。

![](img/53e6aa0d4610230cd3b1bbe746744e16.png)

这个实际上…

我并不是说这是不可能的。直到最近，我还不能做到这一点。幸运的是，我有几个小时的空闲时间，所以我去看了看医生。你难道不知道吗，就像科技领域的大多数事情一样，阅读文档是对我时间的一种实用而有价值的利用。

## 边注

让我在这里暂停一两分钟。我应该加上这个免责声明，我在这里分享的内容都没有使用 OAuth 2.0 认证。当然，我也希望是这样，但我只是还没到那一步(技术熟练)。将来，我会在考虑安全性的情况下更新此内容*:

*   [角色](https://docs.oracle.com/en/database/oracle/oracle-rest-data-services/22.2/orddg/developing-REST-applications.html#GUID-A54203FA-814A-40EA-B8A0-87D5EADFE3C1)
*   [特权](https://docs.oracle.com/en/database/oracle/oracle-rest-data-services/22.2/orddg/developing-REST-applications.html#GUID-BEDADAAE-9737-4608-9ED6-801EF5C06B3D)
*   [OAuth 2.0 授权](https://docs.oracle.com/en/database/oracle/oracle-rest-data-services/22.2/orddg/developing-REST-applications.html#GUID-569CB974-0067-4C91-A8DB-DA5AD391CDF6)

> *在我们的 [ORDS 最佳实践指南](https://www.oracle.com/database/technologies/appdev/rest/best-practices/)中有一个 API 安全部分。

我要提到的另一件事是，我将在我的 GitHub repo 中包含用于创建该表的 SQL 以及所有 Python 代码(我还将在我的 repo 中添加任何代码更新！).

*此外，您知道吗，用数据库操作保存 DDL 只需点击鼠标即可。*

![](img/33e1825fb23fdce054ff54951fad570e.png)

在数据库操作>将 DDL 保存到 SQL 工作表或文件中右键单击对象。

![](img/fcb9ebebb855ff64c0d44d3550b9dabf.png)

创建这个示例表的 DDL。

## **新用户*速度运行***

这一节有点题外话，但是我也在我的一个自治数据库中创建了一个新的 Python 开发人员用户。作为管理员，这很简单。这里是一个“速度跑”:

# 邮政急件

![](img/df2c2eb679592e752220750d7f6602e1.png)

该死的……他真的那么做了。

创建新用户后，我创建了一个“Python_Post”表。超级有想象力吧？我保持事情简单；命名四列(等等…):“列 1”、“列 2”、“列 3”和“列 4”。

# 作弊代码

我启用了我的表的 auto-REST，并查看了 Bash cURL 命令，这样我就可以提醒自己预期的数据(也就是有效负载)。

![](img/193c333257a636f5691a2411654b707c.png)

用于 POST 请求的 Bash cURL 命令；可以在您的编辑器中用作以后的参考。

> 我注意到，如果我不指定“rowid ”,自治数据库会自动这样做。SQL 新手(像我一样)可能会欣赏这一点，因为我们仍然不知道数据库设计(或 DDL，就此而言)的所有最佳实践！

我的过程可能与您的不同，但是我在数据库操作中使用了 cURL 命令作为作弊手段。我已经将这个 cURL 命令复制/粘贴到我的工作 Python 文件中，所以我不必切换屏幕。此外，它有助于提醒我什么是{'key':'value'}对(尽管我*刚刚*创建了这一切，我仍然不记得我做了什么)。

在本例中，我引用了一个 POST 请求，但是您也可以对其他 HTTPS 方法这样做:

*   获取全部
*   得到
*   邮政
*   批量装载
*   放
*   删除

# 继续前进

在发出这些 POST 请求时，我可以省略“rowid”。我不知道这在处理数据库时是否典型，但这似乎在 Python 和 Requests 库上一直有效(至少在我做的测试中是这样)。

如果您没有被我富有想象力的表名和列名吓到，那么请做好准备，因为我接下来的内容会让您大吃一惊。我为每个 POST 请求变体创建了一个有效负载:

![](img/637517c17a14822326367173d5748237.png)

为了避免混淆，我将尽可能地从字面上理解……这很蹩脚吗？是的。有用吗？犹豫不决，你告诉我。

没抓到的话分别是:***payload 1******payload 2******payload 3***。

## 关于反馈

![](img/78bce21c207ec687fa78f6dd12f1ca85.png)

我也给自己写了一些反馈。我觉得这是一个很好的实践，因为如果*你*成功发布了某个东西，你怎么会知道呢？反过来，如果你发布的东西不成功，情况也是如此。如果我聪明的话，我会在数据库动作中设计一个 REST API，自动包含一些反馈。但我不是。

> 如果您想了解这个隐式参数和其他参数，[点击这里](https://docs.oracle.com/en/database/oracle/oracle-rest-data-services/22.2/orddg/implicit-parameters.html#GUID-060752E2-184F-470B-9310-602F8CD5AEA9)。

幸运的是， [Python 请求](https://requests.readthedocs.io/en/latest/)库包含了各种反馈方法。我在 Python 文件中包含了以下内容:

*   [状态代码](https://requests.readthedocs.io/en/latest/user/quickstart/?highlight=status_code#response-status-codes) —返回响应状态代码
*   raise_for_status() —这将显示错误消息(如果适用)；如果请求成功，显示“无”
*   [头](https://requests.readthedocs.io/en/latest/user/quickstart/?highlight=status_code#response-headers) —以 Python 字典的形式返回服务器的响应头

# 不同的有效载荷

我的 Python 代码足够简单。它看起来比实际情况要多，因为*我包含了三个* POST 请求变体。我还测试了有效载荷，看起来我们可以发送一个“有效载荷”,如下所示:

*   **params** —查询参数(*如*，[https://httpbin.org/get?key2=value2&key 1 = value 1)](https://httpbin.org/get?key2=value2&key1=value1))
*   **json** — JSON 数据
*   **数据** —一个 Python 字典(在这个用例中)

> Oracle REST 数据服务有大量关于使用查询参数进行过滤的文档……我还在学习，但是它们非常强大。[点击这里查看](https://docs.oracle.com/en/database/oracle/oracle-rest-data-services/21.1/aelig/developing-REST-applications.html#GUID-091748F8-3D14-402B-9310-25E6A9116B47)。

你甚至可以发送包含嵌套项目/对象的有效载荷(例如，*，一个数组或列表作为你的一个值)。我确信这违反了规范化规则，但这是可能的。就我而言，我没有这样做；我只是坚持我的四个专栏。*

# 执行代码

在我执行代码之后，我收到了对这些 POST 请求的以下反馈:

![](img/44a167a20a0401288aa8070ca31b27ae.png)

如果你眯着眼睛，你可以看到三种不同的反应。我只打了一个，但你应该能指出另外两个。

您应该看到:

*   一个 **201** 状态码，表示“请求已经完成，并导致一个或多个新资源被创建*
*   **无**——如果有 400 错误，就会显示错误信息
*   **{key: value}** 显示所有标题详细信息的对

*关于[超文本传输协议(HTTP/1.1)中的 201 状态码:语义和内容](https://www.rfc-editor.org/rfc/rfc7231#section-6.3.2)

# 双重检查

在 VS 代码中，看起来我的有效载荷是 **POST** ed。但是因为我有访问权，所以我登录数据库操作并手动检查我的表。成功！

![](img/80ef8e4d66a47a618c68e441fcd1e230.png)

查看数据库操作中的新更新。

就这样…零到**发布**请求在我的 Oracle 自主数据库+ ORDS 中很快实现！

# 这有帮助吗？

我在我的 [GitHub 库](https://github.com/chrishoina/oracle-projects/tree/main/python-post-examples)中有这段代码，所以可以随意分叉、下载或添加评论。但是我在这里也包括:

```
import requesturl = "[Your ORDS URL goes here]"payload1 = {'col1': 'test1', 'col2':'test1', 'col3':'test1', 'col4': 'test1'}r1 = requests.post(url, json=payload1)print(r1.status_code)
print(r1.raise_for_status())
print(r1.headers)payload2 = {'col1': 'test2', 'col2':'test2', 'col3':'test2', 'col4': 'test2'}r2 = requests.post(url, data=payload2)print(r2.status_code)
print(r2.raise_for_status())
print(r2.headers)payload3 = {'col1': 'test3', 'col2':'test3', 'col3':'test3', 'col4': 'test3'}r3 = requests.post(url, params=payload3)print(r3.status_code)
print(r3.raise_for_status())
print(r3.headers)
```

如果你还在这里，这有帮助吗？想看更多或其他与 Python + [ORDS](https://www.oracle.com/cis/database/technologies/appdev/rest.html) 相关的东西吗？请在评论中、在[推特](https://twitter.com/chrishoina)上或者通过[电子邮件](http://mailto:chris.hoina@oracle.com)让我知道！

# 别忘了跟着

如果您还没有关注或分享，请关注或分享:

*   [推特](https://twitter.com/chrishoina)
*   [乳齿象](https://mastodon.technology/web/@chrishoina)
*   [YouTube](https://www.youtube.com/user/chrishoina/)
*   [GitHub](https://github.com/chrishoina)
*   [不和谐](https://discord.gg/seYeAPgq9E)
*   [博客](https://www.followthecoffee.com)
*   [堆栈溢出](https://stackoverflow.com/users/9552864/chris-hoina)
*   [领英](https://www.linkedin.com/in/chrishoina/)
*   [Tealfeed](https://tealfeed.com/chrishoina)