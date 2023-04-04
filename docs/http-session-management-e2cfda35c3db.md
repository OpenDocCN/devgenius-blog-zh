# HTTP 会话管理

> 原文：<https://blog.devgenius.io/http-session-management-e2cfda35c3db?source=collection_archive---------15----------------------->

## 文章

## *出自* [*全栈 Python 安全*](https://www.manning.com/books/full-stack-python-security?utm_source=medium&utm_medium=organic&utm_campaign=book_byrne_full_10_29_20) *出自丹尼斯·伯恩*

除了最普通的 web 应用程序之外，HTTP 会话是所有应用程序的必需品。Web 应用程序使用 HTTP 会话来隔离每个用户的流量、上下文和状态。这是所有在线交易的基础。如果你在亚马逊上买东西，在脸书上给某人发信息，或者从银行转账，服务器必须能够在多个请求中识别你。这用 Django 说明了这些概念。

在[manning.com](https://www.manning.com/?utm_source=medium&utm_medium=organic&utm_campaign=book_byrne_full_10_29_20)的结账处，将 **fccbyrne** 输入折扣代码框，即可享受 40%的折扣 [*全栈 Python Security*](https://www.manning.com/books/full-stack-python-security?utm_source=medium&utm_medium=organic&utm_campaign=book_byrne_full_10_29_20) 。

假设爱丽丝第一次访问维基百科。维基百科无法识别爱丽丝的浏览器，所以它创建了一个会话。在这个过程中，Wikipedia 为这个会话生成并存储一个 ID。这个 ID 在 HTTP 响应中被发送到 Alice 的浏览器。Alice 的浏览器持有会话 ID，并在所有后续请求中将其发送回 Wikipedia。当 Wikipedia 收到每个请求时，它使用入站会话 ID 来标识与请求相关联的会话。

现在假设 Wikipedia 为另一个新访问者 Bob 创建了一个会话。像 Alice 一样，Bob 被分配了一个唯一的会话 ID。他的浏览器存储了他的会话 ID，并在每个后续请求中发回。Wikipedia 现在可以使用会话 id 来区分 Alice 的流量和 Bob 的流量。下图说明了该协议。

![](img/eec452ed407bb78e2ce48940619e19bb.png)

图一。说明维基百科如何管理两个用户 Alice 和 Bob 的会话的序列图

Alice 和 Bob 的会话 id 保持私密是很重要的。如果窃听者 Eve 窃取了会话 ID，她可以用它来冒充 Alice 或 Bob。来自 Eve 的请求包含 Bob 被劫持的会话 ID，看起来与来自 Bob 的合法请求没有什么不同。许多攻击依赖于窃取或未授权控制会话 id。这就是为什么会话 id 应该通过 HTTPS 而不是 HTTP 保密地发送和接收。

你可能已经注意到一些网站使用 HTTP 与匿名用户交流，使用 HTTPS 与认证用户交流。恶意的网络窃听者通过试图窃取 HTTP 上的会话 ID，等待用户登录，并通过 HTTPS 劫持用户的帐户来锁定这些站点。这被称为*会话嗅探*。

Django 和许多 web 应用程序框架一样，通过在用户登录时更改会话标识符来防止会话嗅探。为了安全起见，无论协议是否从 HTTP 升级到 HTTPS，Django 都会这样做。我推荐一个额外的防御层:在你的整个网站上使用 HTTPS。

管理 HTTP 会话可能是一项挑战；本文涵盖了许多解决方案。每个解决方案都有不同的安全权衡，但是它们都有一个共同点:HTTP cookies。

## **HTTP cookie**

一个浏览器存储和管理被称为 *cookies* 的少量文本。cookie 可以由浏览器创建，但通常是由服务器创建的。服务器通过响应将 cookie 发送到您的浏览器。浏览器在对服务器的后续请求中回显 cookie。

网站和浏览器通过 cookies 来传递会话 id。当创建新的用户会话时，服务器将会话 ID 作为 cookie 发送给浏览器。服务器向浏览器发送带有 Set-Cookie 响应头的 Cookie。Set-Cookie 响应头包含一个表示 Cookie 名称和值的键-值对。默认情况下，Django 会话 ID 与名为 session ID 的 cookie 进行通信，这里用粗体显示:

```
Set-Cookie: **sessionid**=<cookie-value>
```

Cookie 在后续请求中通过 Cookie 请求头回显到服务器。Cookie 头是一个分号分隔的键值对列表。每一对代表一个 cookie。以下示例说明了发往 alice.com 的请求的几个报头。以粗体显示的 Cookie 头包含两个 Cookie。

```
...
 **Cookie: sessionid=cgqbyjpxaoc5x5mmm9ymcqtsbp7w7cn1; key=value;**    #A
 Host: alice.com
 Referer: [https://alice.com/admin/login/?next=/admin/](https://alice.com/admin/login/?next=/admin/)
 ...
```

**#A 把两块饼干送回 alice.com**

Set-Cookie 响应头包含多个指令。当 cookie 是会话 ID 时，这些指令与安全性高度相关。在本文中，我将介绍以下三个指令:

*   安全的
*   领域
*   最大年龄

## 安全指令

服务器通过发送带有安全指令的会话 ID cookie 来抵御中间人攻击。此处显示了一个示例响应头，其中一个安全指令以粗体显示。

```
Set-Cookie: sessionid=<session-id-value>; **Secure**
```

安全指令禁止浏览器通过 HTTP 将 cookie 发送回服务器。这确保了 cookie 只通过 HTTPS 传输，防止网络窃听者截取会话 ID。由于这个原因，安全指令经常与 HttpOnly 指令混淆，我在[全栈 Python 安全性](https://www.manning.com/books/full-stack-python-security)的第 14 章中介绍了 http only 指令。

在 Django 中，SESSION_COOKIE_SECURE 设置是一个布尔值，它向会话 ID Set-Cookie 头添加或删除安全指令。你可能会惊讶地发现这个设置默认为假。这允许新的 Django 应用程序立即支持用户会话；这也意味着会话 ID 可以被中间人攻击截获。

**警告:对于系统的所有生产部署，您必须确保 SESSION_COOKIE_SECURE 设置为 True。姜戈不会为你这样做。**

**注意:更改设置模块您必须重启 Django，对设置模块的更改才会生效。要重启 Django，在 shell 中按 CTRL + C 停止服务器，然后再次启动它。**

## 领域指令

服务器使用 Domain 指令来控制浏览器应该将会话 ID 发送给哪些主机。这里显示了一个示例响应头，用粗体显示了一个域指令。

```
Set-Cookie: sessionid=<session-id-value>; **Domain=alice.com**
```

假设 alice.com 向一个没有域指令的浏览器发送了一个 Set-Cookie 报头。如果没有域指令，浏览器会将 cookie 回显到 alice.com，而不是 sub.alice.com 这样的子域。

现在假设 alice.com 发送了一个 Set-Cookie 报头，其中的域指令设置为“Alice . com”。然后浏览器将 cookie 回传给 alice.com 和 sub.alice.com。这允许 Alice 支持跨两个系统的 HTTP 会话，但是不太安全。例如，如果马洛里黑了 sub.alice.com，她就处于更有利的位置来妥协 alice.com，因为来自 alice.com 的会话 id 正被交给她。

SESSION_COOKIE_DOMAIN 设置为会话 ID Set-Cookie 头配置域指令。该设置接受两个值:None 和一个表示域名的字符串，如“alice.com”。该设置默认为 None，从响应头中省略了 Domain 指令。此处显示了一个配置设置示例:

```
SESSION_COOKIE_DOMAIN = “alice.com”    #A
```

**#A 从 settings.py 配置域指令**

**注意 SameSite 指令 Domain 指令有时会与 SameSite 指令混淆(我会在第十六章的** [**全栈 Python 安全**](https://www.manning.com/books/full-stack-python-security) **中介绍)。).为了避免这种混淆，请记住以下对比:域指令与 cookie 的位置有关；SameSite 指令与 cookie 的来源有关。**

## 最大年龄指令

服务器发送 Max-Age 指令来声明 cookie 的到期时间。这里显示了一个示例响应头，其中 Max-Age 指令以粗体显示。

```
Set-Cookie: sessionid=<session-id-value>; **Max-Age=1209600**
```

一旦 cookie 过期，浏览器就不再将它回显到它来自的站点。这种行为对你来说可能听起来很熟悉。你可能已经注意到，像 Gmail 这样的网站不会强迫你每次返回都登录，但是如果你很长时间没有回来，你会被强迫再次登录。您的 cookie 和 HTTP 会话可能已经过期。

为您的站点选择最佳会话长度归结为安全性与功能性。当浏览器无人值守时，极长的会话很容易成为攻击者的攻击目标。另一方面，极短的会话会迫使合法用户一次又一次地重新登录。

在 Django 中，SESSION_COOKIE_AGE 设置为会话 ID Set-Cookie 头配置 Max-Age 指令。该设置默认为 1209600 秒(两周)。这个值对于大多数系统来说是合理的，但是适当的值是特定于站点的。

## 浏览器长度的会话

如果 cookie 设置时没有 Max-Age 指令，只要选项卡保持打开，浏览器就会使它保持活动状态。这就是所谓的*浏览器长度会话*。用户关闭浏览器标签后，浏览器长度的会话不会被攻击者劫持。这看起来更安全，但是你怎么能强迫每个用户在使用完一个站点后关闭每个标签页呢？此外，当用户不关闭浏览器标签时，会话实际上没有到期。浏览器长度的会话会增加总体风险，通常应该避免这一特性。

浏览器长度会话由 SESSION _ EXPIRE _ AT _ BROWSER _ CLOSE 设置配置。将此项设置为 True 会从会话 ID Set-Cookie 标头中删除 Max-Age 指令。Django 默认禁用浏览器长度的会话。

## 以编程方式设置 cookies

我介绍的响应头指令适用于任何 cookie，而不仅仅是会话 ID。如果您以编程方式设置 cookies，您应该考虑这些指令来限制风险。下面的代码演示了在 Django 中设置自定义 cookie 时如何使用这些指令。

**清单 1 以编程方式在 Django 中设置一个 cookie**

```
from django.http import HttpResponse

 response = HttpResponse()
 response.set_cookie(
     'cookie-name',
     'cookie-value',
     secure=True,    #A
     domain='alice.com',    #B
     max_age=42, )    #C
```

**#A 浏览器只通过 HTTPS 发送这个 cookie**

**# B alice.com 和所有子域都会收到这个 cookie**

**# C 42 秒后，此 cookie 过期**

到目前为止，您已经了解了很多关于服务器和 HTTP 客户端如何使用 cookies 来管理用户会话的知识。至少，会话可以区分用户之间的流量。除此之外，会话还是管理每个用户状态的一种方式。用户名、地区和时区是会话状态的常见示例。下一节将介绍如何访问和保持会话状态。

## **会话状态持久性**

像大多数 web 框架一样，Django 用 API 为用户会话建模。这个 API 是通过会话对象(请求的一个属性)来访问的。session 对象的行为类似于 Python 字典，通过键存储值。通过这个 API 创建、读取、更新和删除会话状态；这些操作将在下一个清单中演示。

**清单 2 Django 会话状态访问**

```
request.session['name'] = 'Alice'    #A
 name = request.session.get('name', 'Bob')    #B
 request.session['name'] = 'Charlie'    #C
 del request.session['name']    #D
```

**#A 创建会话状态条目**

**#B 读取会话状态条目**

**#C 更新会话状态条目**

**#D 删除会话状态条目**

Django 自动管理会话状态持久性。收到请求后，从可配置的数据源加载和反序列化会话状态。如果会话状态在请求生命周期中被修改，Django 会在发送响应时序列化并持久化修改。序列化和反序列化的抽象层称为会话序列化程序。

## **会话串行化器**

Django 将会话状态的序列化和反序列化委托给一个可配置的组件。此组件由 SESSION_SERIALIZER 设置配置。Django 本身支持两个会话序列化组件:

*   JSONSerializer，默认会话序列化程序
*   分拣器

JSONSerializer 将会话状态与 JSON 相互转换。这种方法允许您用基本的 Python 数据类型(如整数、字符串、字典和列表)组成会话状态。下面的代码使用 JSONSerializer 来序列化和反序列化 dict，以粗体显示。

```
>>> from django.contrib.sessions.serializers import JSONSerializer
 >>>
 >>> json_serializer = JSONSerializer()
 >>> serialized = json_serializer.dumps(**{'name': 'Bob'}**)    #A
 >>> serialized
 b'{"name":"Bob"}'    #B
 >>> json_serializer.loads(serialized)    #C
 {'name': 'Bob'}    #D
```

**#A 序列化一个 Python 字典**

**#B 序列化 JSON**

**#C 反序列化 JSON**

**#D 反序列化的 Python 字典**

PickleSerializer 将会话状态与字节流相互转换。顾名思义，PickleSerializer 是 Python pickle 模块的包装器。除了基本的 Python 数据类型之外，这种方法还允许您存储任意的 Python 对象。应用程序定义的 Python 对象(以粗体定义和创建)由以下代码进行序列化和反序列化。

```
>>> from django.contrib.sessions.serializers import PickleSerializer
 >>>
 **>>> class Profile:**
 **...     def __init__(self, name):**
 **...         self.name = name**
 ...
 >>> pickle_serializer = PickleSerializer()
 >>> serialized = pickle_serializer.dumps(**Profile('Bob')**)    #A
 >>> serialized
 b'\x80\x05\x95)\x00\x00\x00\x00\x00\x00\x00\x8c\x08__main__...'    #B
 >>> deserialized = pickle_serializer.loads(serialized)    #C
 >>> deserialized.name    #D
 'Bob'
```

**#A 序列化应用程序定义的对象**

**#B 序列化字节流**

**#C 反序列化字节流**

**#D 反序列化对象**

JSONSerializer 和 PickleSerializer 之间的权衡是安全性和功能性。JSONSerializer 是安全的，但是它不能序列化任意的 Python 对象。PickleSerializer 执行此功能，但它伴随着严重的风险。pickle 模块文档给出了以下警告(https://docs . python . org/3/library/pickle . html):

```
“The pickle module is not secure. Only unpickle data you trust. It is possible to construct malicious pickle data which will execute arbitrary code during unpickling. Never unpickle data that could have come from an untrusted source, or that could have been tampered with.”
```

如果攻击者能够修改会话状态，PickleSerializer 可能会被滥用。我将在本文后面介绍这种形式的攻击；敬请关注。

Django 通过会话引擎自动持久化序列化的会话状态。会话引擎是底层数据源的可配置抽象层。Django 提供了五种不同的选项，每种选项都有自己的优缺点，如下所示:

*   简单的基于缓存的会话
*   基于直写缓存的会话
*   基于数据库的会话，默认选项
*   基于文件的会话
*   签名 cookie 会话

## **简单的基于缓存的会话**

简单的基于缓存的会话允许您将会话状态存储在缓存服务中，如 Memcached 或 Redis。缓存服务将数据存储在内存中，而不是磁盘上。这意味着您可以快速存储和加载来自这些服务的数据，但有时数据可能会丢失。例如，如果缓存服务用完了可用空间，它会在最近最少访问的旧数据上写入新数据。如果缓存服务重新启动，所有数据都会丢失。

缓存服务的最大优势是速度，它补充了会话状态的典型访问模式。频繁读取会话状态(针对每个请求)。通过将会话状态存储在内存中，整个站点可以减少延迟，增加吞吐量，并提供更好的用户体验。

缓存服务的最大弱点是数据丢失，它对会话状态的影响程度不同于其他用户数据。在最坏的情况下，用户必须重新登录到站点，重新创建会话。这是不可取的，但称之为“数据丢失”是言过其实的。因此，会话状态是可牺牲的，其负面影响是有限的。

存储 Django 会话状态的最流行和最快的方法是将简单的基于缓存的会话引擎与 Memcached 之类的缓存服务结合起来。在设置模块中，将 SESSION_ENGINE 分配给“django . contrib . sessions . backends . cache”将 Django 配置为简单的基于缓存的会话。Django 本身支持两种不同的 Memcached 缓存后端类型。

## Memcached 后端

MemcachedCache 和 PyLibMCCache 是最快和最常用的缓存后端。缓存设置配置缓存服务集成。这个设置是一个 dict，代表一个单独的缓存后端的集合。下一个清单展示了为 Memcached 集成配置 Django 的两种方法。MemcachedCache 选项被配置为使用本地环回地址；PyLibMCCache 选项被配置为使用 unix 套接字。

**清单 3 使用 Memcached 进行缓存**

```
CACHES = {
     'default': {
         'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
         'LOCATION': '127.0.0.1:11211',    #A
     },
     'cache': {
         'BACKEND': 'django.core.cache.backends.memcached.PyLibMCCache',
         'LOCATION': '/tmp/memcached.sock',    #B
     }
 }
```

**#本地环回地址**

**#B Unix 套接字地址**

本地环回地址和 unix 套接字是安全的，因为到这些地址的流量不会离开机器。不幸的是，在撰写本文时，TLS 功能在 Memcached wiki 上被描述为“实验性的”。

Django 支持四个额外的缓存后端。这些选项要么不太受欢迎，要么不安全，或者两者兼而有之，我在这里简单介绍一下。

*   数据库后端
*   本地内存后端，默认选项
*   虚拟后端
*   文件系统后端

## 数据库后端

DatabaseCache 选项配置 Django 使用您的数据库作为缓存后端。使用该选项为您提供了一个通过 TLS 发送数据库流量的理由。如果没有 TLS 连接，您缓存的所有内容，包括会话 id，都可以被网络窃听者访问。下一个清单展示了如何配置 Django 来缓存数据库后端。

**清单 4 数据库缓存**

```
CACHES = {
     'default': {
         'BACKEND': 'django.core.cache.backends.db.DatabaseCache',
         'LOCATION': 'database_table_name',
     }
 }
```

缓存服务和数据库之间的主要权衡是性能和存储容量。您的数据库的性能不如缓存服务。数据库将数据保存到磁盘上；缓存服务将数据保存到内存中。另一方面，您的缓存服务永远无法存储像数据库一样多的数据。在会话状态不可消耗的罕见情况下，此选项很有价值。

## 本地内存、虚拟内存和文件系统后端

LocMemCache 将数据缓存在本地内存中，只有位置非常好的攻击者才能访问它。DummyCache 是唯一比 LocMemCache 更安全的东西，因为它不存储任何东西。下面的清单展示了这些选项，它们是安全的，但是除了开发或测试环境之外，它们都没有用。Django 默认使用 LocMemCache。

**清单 5 用本地内存缓存，或者什么都不用**

```
CACHES = {
     'default': {
         'BACKEND': 'django.core.cache.backends.locmem.LocMemCache',
     },
     'dummy': {
         'BACKEND': 'django.core.cache.backends.dummy.DummyCache',
     }
 }
```

你可能已经猜到了，FileBasedCache 不受欢迎，也不安全。FileBasedCache 用户不必担心他们的未加密数据是否通过网络发送，而是写入文件系统。

**清单 6 文件系统缓存**

```
CACHES = {
     'default': {
         'BACKEND': 'django.core.cache.backends.filebased.FileBasedCache',
         'LOCATION': '/var/tmp/file_based_cache',
     }
 }
```

## **基于直写缓存的会话**

基于直写缓存的会话允许您将缓存服务和数据库结合起来管理会话状态。在这种方法下，当 Django 将会话状态写入缓存服务时，操作也会“直写”到数据库。这意味着会话状态是持久的，以写性能为代价。当 Django 需要读取会话状态时，它首先从缓存服务中读取，最后才使用数据库。这意味着读取操作偶尔也会影响性能。

将 SESSION_ENGINE 设置设置为“django . contrib . sessions . backends . cache _ db”将启用基于直写缓存的会话。

## **基于数据库的会话引擎**

基于数据库的会话完全绕过 Django 的缓存集成。如果您已经选择放弃将应用程序与缓存服务集成的开销，此选项非常有用。基于数据库的会话通过将 SESSION_ENGINE 设置为“django . contrib . sessions . backends . db”来配置。这是默认行为。

Django 不会自动清理被放弃的会话状态。使用持久会话的系统需要确保定期调用 clearsessions 子命令。这有助于您降低存储成本，但更重要的是，如果您在会话中存储敏感数据，这有助于您减少攻击面。从项目根目录执行的以下命令演示了如何调用 clearsessions 子命令。

```
$ python manage.py clearsessions
```

## **基于文件的会话引擎**

正如你可能已经猜到的那样，这个选项非常不安全。每个文件备份会话都序列化为一个文件。会话 ID 在文件名中，会话状态以未加密方式存储。任何对文件系统具有读取权限的人都可以劫持会话或查看会话状态。将 SESSION_ENGINE 设置为“django . contrib . sessions . backends . file”会将 Django 配置为在文件系统中存储会话状态。

## **基于 Cookie 的会话引擎**

基于 cookie 的会话引擎将会话状态存储在会话 ID cookie 本身中。有了这个选项，会话 ID cookie 不仅*识别*会话，它还*识别*会话。Django 没有在本地存储会话，而是将整个会话序列化并发送给浏览器。然后，当浏览器在后续请求中回显有效负载时，Django 会对其进行反序列化。

在将会话状态发送到浏览器之前，基于 cookie 的会话引擎使用 HMAC 函数对会话状态进行哈希处理。从 HMAC 函数获得的哈希值与会话状态配对；Django 将它们作为会话 ID cookie 一起发送给浏览器。

当浏览器回显会话 ID cookie 时，Django 提取哈希值并验证会话状态。Django 通过散列入站会话状态并将新散列值与旧散列值进行比较来做到这一点。如果哈希值不匹配，Django 知道会话状态已经被篡改，请求被拒绝。如果哈希值匹配，Django 信任会话状态。下图说明了往返过程。

![](img/43e73a1d75b5852b7ce04a72ca03c937.png)

图二。Django 对发送的内容进行哈希处理，对接收的内容进行认证

每个 HMAC 函数都需要一个键。姜戈从哪里得到秘密钥匙的？从设置模块。

## 秘密密钥设置

每个生成的 Django 应用程序都包含设置模块中的 SECRET_KEY 设置。这个设置很重要。与普遍的看法相反，Django 不使用 SECRET_KEY 来加密数据。相反，Django 使用这个参数来执行键控散列。该设置的值默认为唯一的随机字符串。在您的开发或测试环境中使用这个值是没问题的，但是在您的生产环境中，从比您的代码库更安全的位置检索不同的值是很重要的。

**警告:SECRET_KEY 的生产值应该保持三个属性。该值应该是唯一的、随机的并且足够长。生成的默认值的长度为 50 个字符，这已经足够长了。不要将 SECRET_KEY 设置为密码或通行短语；没人需要记住它。如果有人能记住这个值，系统就不太安全。**

乍一看，基于 cookie 的会话引擎似乎是一个不错的选择。Django 使用 HMAC 函数对每个请求的会话状态的完整性进行认证和验证。不幸的是，这种选择有很多缺点，其中一些非常危险:

*   Cookie 大小限制
*   对会话状态的未授权访问
*   重放攻击
*   远程代码执行攻击

## Cookie 大小限制

文件系统和数据库意味着存储大量数据；饼干不是。RFC 6265 要求 HTTP 客户端支持“每个 cookie 至少 4096 字节”(【https://tools.ietf.org/html/rfc6265#section-5.3】T2)。HTTP 客户端可以自由支持比这个更大的 cookies，但是他们没有义务这样做。因此，序列化的基于 cookie 的 Django 会话的大小应该保持在 4 千字节以下。

## 对会话状态的未授权访问

基于 cookie 的会话引擎对出站会话状态进行哈希处理；它不加密会话状态。这保证了完整性，但不能保证机密性。因此，恶意用户很容易通过浏览器获得会话状态。如果会话包含用户不应访问的信息，这会使系统易受攻击。

假设爱丽丝和伊芙都是社交媒体网站 social.bob.com 的用户。爱丽丝对伊芙实施中间人攻击感到愤怒，她阻止了伊芙。像其他社交媒体网站一样，social.bob.com 没有通知伊芙她已经被屏蔽了。与其他社交媒体网站不同，social.bob.com 将这些信息存储在基于 cookie 的会话状态中。

Eve 使用下面的代码来查看谁阻止了她。首先，她用请求包以编程方式进行身份验证。接下来，她从会话 ID cookie 中提取、解码并反序列化自己的会话状态。反序列化的会话状态显示 Alice 已经阻止了 Eve(粗体)。

```
>>> import base64
 >>> import json
 >>> import requests
 >>>
 >>> credentials = {
 ...     'username': 'eve',
 ...     'password': 'evil', }
 >>> response = requests.post(    #A
 ...     'https://social.bob.com/login/',    #A
 ...     data=credentials, )    #A
 >>> sessionid = response.cookies['sessionid']    #B
 >>> decoded = base64.b64decode(sessionid.split(':')[0])    #B
 >>> json.loads(decoded)    #B
 {'name': 'Eve', 'username': 'eve', **'blocked_by': ['alice']**}    #C
```

**#一个夏娃登录鲍勃的社交媒体网站**

**#B Eve 提取、解码并反序列化会话状态**

**#C 夏娃看到爱丽丝挡住了她的**

## 重放攻击

基于 cookie 的会话引擎使用 HMAC 函数来验证入站会话状态。这告诉服务器谁是有效载荷的原始作者。这不能告诉服务器它接收的有效载荷是否是有效载荷的最新版本。浏览器不能不修改会话 ID cookie，但是浏览器可以重放它的旧版本。攻击者可以利用这个限制进行*重放攻击*。

假设 ecommerce.alice.com 配置了一个基于 cookie 的会话引擎。网站给每个新用户一次性折扣。会话状态中的布尔值表示用户的折扣资格。恶意用户 Mallory 第一次访问该站点。作为新用户，她有资格享受折扣，她的会话状态反映了这一点。她保存了会话状态的本地副本。然后，她进行了第一次购买，获得了折扣，当付款被捕获时，站点更新她的会话状态。她不再有资格享受折扣。后来，Mallory 在后续购买请求中重放她的会话状态副本，以获得额外的未授权折扣。马洛里成功实施了重放攻击。

重放攻击是通过在无效上下文中重复有效输入来破坏系统的任何利用方式。如果不能区分重放输入和普通输入，任何系统都容易受到重放攻击。区分重放输入和普通输入是困难的，因为在某个时间点，重放输入*是*普通输入。

这些攻击并不局限于电子商务系统。重放攻击已被用于伪造自动柜员机(ATM)交易、解锁车辆、打开车库门以及绕过语音识别认证。

## 远程代码执行攻击

将基于 cookie 的会话与 PickleSerializer 结合起来是一条不归路。如果攻击者能够访问 SECRET_KEY 设置，他们就可以严重利用这种配置设置组合。

**警告:远程代码执行攻击非常残忍。不要将基于 cookie 的会话与 PickleSerializer 结合使用；风险太大了。这种组合不受欢迎是有充分理由的。**

假设 vulnerable.alice.com 用 PickleSerializer 序列化基于 cookie 的会话。马洛里是 vulnerable.alice.com 的一名心怀不满的前雇员，他记得《秘密钥匙》。她对 vulnerable.alice.com 进行了一次攻击，计划如下:

1.  编写恶意代码
2.  用 HMAC 函数和 SECRET_KEY 散列恶意代码
3.  将恶意代码和哈希值作为会话 cookie 发送给 vulnerable.alice.com
4.  坐下来看 vulnerable.alice.com 执行马洛里的恶意代码

首先，Mallory 编写了恶意的 Python 代码。她的目标是骗 vulnerable.alice.com 执行这段代码。她安装 Django，创建 PickleSerializer，并将恶意代码序列化为二进制格式。

接下来，Mallory 散列序列化的恶意代码。她使用 HMAC 函数和 SECRET_KEY，以与服务器散列会话状态相同的方式来实现这一点。Mallory 现在有了恶意代码的有效哈希值。

最后，Mallory 将序列化的恶意代码与哈希值配对，将它们伪装成基于 cookie 的会话状态。她将有效载荷作为请求报头中的会话 cookie 发送给 vulnerable.alice.com。不幸的是，服务器成功地验证了 cookie 毕竟，恶意代码是用服务器使用的同一个 SECRET_KEY 散列的。对 cookie 进行身份验证后，服务器用 PickleSerializer 反序列化会话状态，无意中执行了恶意脚本。马洛里已经成功实施了一次*远程代码执行攻击*。图 3 说明了马洛里的攻击。

![](img/07653de439bc7c67c413099250523acb.png)

图 3。Mallory 使用泄露的 SECRET_KEY 执行远程代码执行攻击

下面的例子演示了 Mallory 如何从一个交互式 Django shell 执行她的远程代码执行攻击。在这次攻击中，马洛里通过调用 sys.exit 函数诱使 vulnerable.alice.com 自杀。Mallory 在 PickleSerializer 反序列化其代码时调用的方法中调用 sys.exit。Mallory 使用 Django 的签名模块来序列化和哈希恶意代码，就像基于 cookie 的会话引擎一样。最后，她使用请求包发送请求。对请求没有响应，接收者死亡(粗体)。

```
$ python manage.py shell
 >>> import sys
 >>> from django.contrib.sessions.serializers import PickleSerializer
 >>> from django.core import signing
 >>> import requests
 >>>
 >>> class MaliciousCode:
 ...     def __reduce__(self):    #A
 ...         return sys.exit, ()    #B
 ...
 >>> session_state = {'malicious_code': MaliciousCode(), }
 >>> sessionid = signing.dumps(    #C
 ...     session_state,    #C
 ...     salt='django.contrib.sessions.backends.signed_cookies',    #C
 ...     serializer=PickleSerializer)    #C
 >>>
 >>> session = requests.Session()
 >>> session.cookies['sessionid'] = sessionid
 **>>> session.get('https://vulnerable.alice.com/')**    #D
 **Starting new HTTPS connection (1): vulnerable.com**

 http.client.RemoteDisconnected: Remote end closed connection without response    #E
```

**# Pickle 在反序列化时调用这个方法**

**#B Django 用这行代码自杀**

**#C Django 的签名模块序列化并哈希 Mallory 的恶意代码**

**#D 发送请求**

**#E 未收到响应**

将 SESSION_ENGINE 设置为“django . contrib . sessions . backends . signed _ cookies”会将 Django 配置为使用基于 cookie 的会话引擎。

## **总结**

*   服务器使用 Set-Cookie 响应头在浏览器上设置会话 id
*   浏览器向服务器发送带有 Cookie 请求头的会话 id
*   使用 Secure、Domain 和 Max-Age 指令来抵御在线攻击
*   Django 本身支持五种不同的方式来存储会话状态
*   Django 本身支持六种不同的数据缓存方式
*   重放攻击可以滥用基于 cookie 的会话
*   远程代码执行攻击可以滥用 pickle 序列化
*   Django 使用 SECRET_KEY 设置进行加密哈希，而不是加密

如果你想看更多，请在曼宁的 liveBook 平台上查看这本书[这里](https://livebook.manning.com/book/full-stack-python-security?origin=product-look-inside&utm_source=medium&utm_medium=organic&utm_campaign=book_byrne_full_10_29_20)。