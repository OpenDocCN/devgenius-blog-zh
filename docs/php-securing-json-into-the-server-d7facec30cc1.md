# PHP:保护服务器中的 JSON

> 原文：<https://blog.devgenius.io/php-securing-json-into-the-server-d7facec30cc1?source=collection_archive---------11----------------------->

## 让我在这上面签名，以免被篡改

![](img/fcee7a28dab33e669bb27c062d6f4d84.png)

照片由[约格什·佩达姆卡](https://unsplash.com/@yogesh_7?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

不久前，我想知道是否可以将一个对象序列化为 JSON。虽然这在技术上是可能的，但是当服务器从某个地方接收到相同的 JSON 字符串时，不能保证有人修改过它:

```
{
    "name": "My name",
    "email": "attacker.mail@email.com"
}
```

幸运的是，有一种方法可以确保这些数据没有被以任何方式更改，那就是给它添加一个“哈希”，也称为“签名”。有两种方法可以签署一份数据，至少不需要深入细节，也不需要太过专业以至于这篇文章成为一张壁纸:

1.  对称:使用一个公共密钥或秘密密钥。
2.  不对称:使用公钥/私钥。

我要使用的是**公共或秘密密钥**。这是因为只有一方处理密钥，因此保密。如果我需要另一方以安全的方式验证数据，就需要一个公钥/私钥。

主要思想是将签名附加到 JSON 有效负载上，发送它，然后检索它，而不用担心未经同意就被修改。此外，我们应该意识到**接收者可以窥视数据本身**。

```
{
    "name": "John Doe",
    "email": "john.doe@mail.com",
    "signature": "b3eeb0da04ebbc73ee17549d4354ce5de7e7445b3c82eeac"
}
```

# 签署数据

假设我们有一个简单的对象，在一个数组中保存用户名和电子邮件。我们可以简单地获取数组，将其转换为 JSON，并使用该数据串来计算签名。

> 坦率地说，我们添加的签名是使用 SHA-256 算法编码的数据的简单散列。你可以使用 SHA-512 或 SHA-3，但对于这个例子和普通用户来说，由于其速度和常见用法，SHA-256 已经足够了。

为此，我们可以使用`[hash_hmac()](https://www.php.net/manual/en/function.hash-hmac.php)`，这是一个简单的函数，它用一个秘密密钥散列一个给定的字符串，并返回散列本身。

在做任何事情之前，我们必须确保有一个用于签名的密钥。对于这个例子，我们可以在数据库中保存一个假设的随机密钥。这个密钥应该是 256 位的二进制字符串。我们可以用 32 字节的`[random_bytes()](https://www.php.net/manual/en/function.random-bytes.php)`快速创建一个。

```
$object->key = random_bytes(32);
```

我将把它的持久性留给你。`[bin2hex()](https://www.php.net/manual/en/function.bin2hex.php)`函数可以很好的用十六进制字符串来存储它。

一旦一切准备就绪，我们将使用一种方法来接收数据，并用一个新的签名返回它:

我们将把数组复制到一个“原始”数组中。我们稍后将使用这个数组将签名作为最后一个参数。

对于复制的数组，我们将使用`ksort()`对它们进行排序。然后我们将把它转换成 JSON，并使用结果字符串计算散列。使用“签名”密钥将散列附加到“原始”数组。

# 验证签名

我假设您想方便地获取一个 JSON 字符串并从中创建一个新的对象实例。没问题，静态方法可以帮助我们做到这一点。

```
public static function fromJson(string $data, string $key)
{
    $data = json_decode($data, true); $signature = $data['signature']; unset($data['signature']); ksort($data); if (hash_equals(hash_hmac('sha256', json_encode($data), $key)), $signature) {
        return new static($data);
    } throw new \Exception('Invalid signature');
}
```

在我们使用`[json_decode()](https://www.php.net/manual/en/function.json-decode.php)`将数据解码成一个数组后，我们可以简单地从数组中取出签名，像以前一样对其进行排序，并重新创建签名。

现在的区别是，我们将使用`[hash_equals()](https://www.php.net/manual/en/function.hash-equals.php)`来检查我们从数据中创建的散列是否等于包含的散列，而不是返回带有签名的数据。而且，这避免了[定时攻击](https://en.wikipedia.org/wiki/Timing_attack)。

如果一切正常，我们可以使用经过验证的数据数组返回一个新的实例。如果失败了，我们将返回一个异常。

差不多就是这样。我们可以安全地共享这些信息，而不必担心有人会修改它。

你可以在我的 [Laratraits 包](https://github.com/DarkGhostHunter/Laratraits)中找到这个特性和其他助手。如果你发现在你的项目中有用的东西，给它一个机会。