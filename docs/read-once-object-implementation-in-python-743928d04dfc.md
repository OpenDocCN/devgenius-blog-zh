# Python 中一次性读取对象的实现

> 原文：<https://blog.devgenius.io/read-once-object-implementation-in-python-743928d04dfc?source=collection_archive---------20----------------------->

# 什么是只读一次的对象？

这一概念在[设计安全](https://www.manning.com/books/secure-by-design)一书中有定义和解释。

在这个环节 [LiveBook](https://livebook.manning.com/concept/security/read-once-object) 也有曝光。

一次性读取对象的总体特征，摘自[书评:安全设计](https://adriancitu.com/tag/read-once-object-pattern/)

```
Read-once objects

A read-once object is an object designed to be read once (or a limited number of times). This object usually represents a value or concept in your domain that’s considered to be sensitive (for example, passport numbers, credit card numbers, or passwords). The main purpose of the read-once object is to facilitate detection of unintentional use of the data it encapsulates.

Here’s a list of the key aspects of a read-once object:

    Its main purpose is to facilitate detection of unintentional use.
    It represents a sensitive value or concept.
    It’s often a domain primitive.
    Its value can be read once, and once only.
    It prevents serialization of sensitive data.
    It prevents sub-classing and extension.
```

# 关于用法

假设您需要向某个服务传递密码，该服务将登录到您的用户。登录服务只需要这个密码一次，那么为什么不限制它只能被读取和使用一次呢？

# 使用 pip 安装:

`pip install readonce`

GitHub 回购->[https://github.com/ShahriyarR/py-read-once](https://github.com/ShahriyarR/py-read-once)

然后就从`ReadOnce`继承:

这里，密码字符串作为一个秘密添加。从我们的定义来看，它只能被读取一次，并且只能使用`get_secret()`，不能直接获取秘密。

*   您也不能公开对象属性:

*   尝试读取密码两次:

*   如果有人试图将自己的秘密添加到已经实例化的对象中，然后得到已经定义的秘密数据(原始秘密)，那么他将只得到一个新的秘密。

*   您不能从敏感类创建子类，这是一种暴露父类数据的方法，但没有成功:

*   如果有人试图直接获取机密:

*   你不能腌制它:

*   你不能 JSON 序列化它:

使用默认编码器:

带定制编码器:

*   在某些情况下，类本身可以被自动转储到日志中，但在这里不行:

# Python [Dataclasses](https://docs.python.org/3.10/library/dataclasses.html) 怎么样？

关于数据类，禁止直接定义一个字段，然后将其添加到机密:

结果将是:

更好的方法是使用字段作为“描述符”。假设您有一个想法，要以整块的方式共享您的数据库凭证。我们可以为每条信息创建单独的敏感数据持有者或机密:

然后我们可以将它们合并到一个数据类中:

通过这种方式，我们可以再次使用`get_secret()`找回我们的秘密，而且只有一次:

打印或转储凭据对象也不会提供任何有价值的信息:

好吧，就 Python 而言，这不是一个完整的“描述符”(没有`__get__`和`__set__`)，但我不是故意打开这扇门的。

*   使用数据类的另一种方法是声明字段:

然后在将来初始化这些字段。这种方法类似于 dto(数据传输对象)。

*   JSON 有没有可能连载`DBCredentials`？如果您决定转储敏感字段，则不可能:尝试使用自定义编码器:

这同样适用于酸洗:

# 与 [Pydantic](https://pydantic-docs.helpmanual.io/) 的关系

正如我们所知，Pydantic 模型是基于类型注释的数据验证的事实上的标准，我们可以很容易地将 ReadOnce 对象与 Pydantic 一起使用。在这一部分，我将分享一些测试。

用 ReadOnce 对象声明 Pydantic 模型的最简单方法是允许任意类型:

创建凭据:

同样，敏感数据不会暴露:

它不能以默认方式序列化:

不幸的是，ReadOnce 对象的性质阻止了在模型类中使用强大的验证机制。在其核心，敏感对象不能被使用两次，如果它已经被消费:

*   如果之前没有调用过`get_secret()`，你可以调用任意时间`add_secret()`。
*   每当您调用`get_secret()`时，敏感对象被视为已耗尽。

假设我们想要验证密码长度，并尝试在 Pydantic 模型中添加一个自定义验证器:

如您所料，我们首先需要获取机密数据，然后验证它，如果验证通过，我们需要将机密数据放回敏感对象中，这是不可能的。

因此，最好将验证逻辑推向`Password`敏感类。我们将在未来深入探讨验证。

如果我们测试这个`InvalidDBCredentialsModel`，它应该会失败:`readonce.UnsupportedOperationException: ('Not allowed on sensitive value', 'Sensitive object exhausted; you can not use it twice')`

> *如果你有任何进一步的想法，请打开一个问题，我们可以探索并找出最佳的用法*

# 应用合同设计中的最佳实践

为了进一步确保数据(秘密)的完整性和安全性，我们可以使用`DbC`的想法，因为它给了我们一个定义可重用约束的更干净的方法。

我喜欢 [icontract](https://github.com/Parquery/icontract) 包，这是一个相当方便的工具。我也试图解释这个 YouTube 教程[用 Python 进行契约式设计编程](https://www.youtube.com/watch?v=yi-GInnc768)。

让我们将我们的敏感类重新定义为:

目前的密码验证相当幼稚，它只是检查字符串的长度:这是我们的`pre-condition`，它被标记为`@icontract.require`。

但是什么是`@icontract.ensure`?这就是我们所谓的`post-condition`:在添加一个秘密之后，秘密存储的长度必须等于 1。

我们可以使用 regex 添加更复杂的密码验证，这取决于您的业务需求。

这里应该问这个问题:*“我们的应用程序的密码是什么？”*

写下密码要求后，作为 DbC 方法的一部分，您可以将它们转换为`pre-conditions`。

*   我在`ReadOnce`实现中也使用了这些想法，比如:

在这里，我让自己确信一切都被正确重置。

另一个重要的话题是不变量。

考虑一下`ReadOnce`对象，在其生命周期中可以没有秘密，也可以只有一个秘密:

如果有人试图将多条数据注入秘密存储器，将会失败，因为这是明显的不变违例。