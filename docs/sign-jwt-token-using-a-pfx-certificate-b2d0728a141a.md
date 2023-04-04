# 使用 PFX 证书签署 JWT 令牌

> 原文：<https://blog.devgenius.io/sign-jwt-token-using-a-pfx-certificate-b2d0728a141a?source=collection_archive---------3----------------------->

![](img/9ba3dc8f5bbef362d977376f69de03ad.png)

照片由[飞:D](https://unsplash.com/es/@flyd2069?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JWT 代表 JSON Web 令牌。这是一个开放的标准( [RFC 7519](https://tools.ietf.org/html/rfc7519) )，用于在各方之间安全地传输信息。JWT 令牌有三个部分:报头、有效载荷和签名。

接收者验证用于认证和授权的 JWT 令牌的签名。虽然我们不需要知道它是如何工作的，但我的书呆子性格迫使我更深入地挖掘，以增加我对 JWT 令牌的舒适区。

## 页眉

报头构成了 JWT 令牌的第一部分。它是 JSON 对象的 base 64 编码版本，JSON 对象由签名算法和令牌类型组成。

如果您解码 base64 编码版本的标头，它看起来会像这样:

```
{
“alg”: “RS256”,
“typ”: “JWT”
}
```

## 有效载荷

有效负载构成了 JST 令牌的第二部分。它包含关于用户身份验证和授权的声明。Base64 编码有效负载的解码版本示例如下:

```
{
 “aud”: “audience”,
 “exp”: 1671716625,
 “iss”: “Issuer”,
 “iat”: 1671713325,
 “nbf”: 1671713325
}
```

要理解 iat & nbf 的意义，请阅读这篇[文章](https://getperfectanswers.com/what-is-iat-and-nbf/#:~:text=The%20iat%20claim%20indicates%20the%20time%20a%20JWT,which%20the%20token%20is%20not%20accepted%20for%20processing.)。

## 签名段

这是令牌的最后一部分。令牌的签名部分确保令牌不被修改。当客户端收到令牌时，它使用密钥验证签名。例如，如果签名算法是 RS256，客户端将使用私钥生成签名，而接收方将使用公钥进行验证。

# PFX 证书

在了解了 JWT 令牌的基础知识之后，让我们花几分钟来看看 PFX 证书。

一个[个人信息交换(。pfx)](https://learn.microsoft.com/en-us/windows-hardware/drivers/install/personal-information-exchange---pfx--files) 是一个 [PKCS#12](https://en.wikipedia.org/wiki/PKCS_12) 格式的证书。它是一个存档，存储部署证书所需的一切。pfx 文件还存储证书颁发机构和证书持有人的信息。

> [**PKCS** # **12** 是微软 **PFX** 的继任者；然而，术语“ **PKCS** # **12** 文件”和“ **PFX** 文件”有时会互换使用。PFX 格式被批评为最复杂的加密协议之一。](https://en.wikipedia.org/wiki/PKCS_12#:~:text=PKCS%20%2312%20is%20the%20successor%20to%20Microsoft%20%27s,being%20one%20of%20the%20most%20complex%20cryptographic%20protocols.)

# 使用 PFX 证书签署 JWT 令牌

我们可以借助存储在 PFX 中的 RSA 私钥，通过对客户机-服务器交互的细节进行签名来创建 JWT 令牌。通过在 X.509 证书的实例中导入 PFX 证书来实现。 [X.509 证书是代表用户、计算机、服务或设备的数字文档。它们由证书颁发机构(CA)、下属 CA 或注册机构颁发，包含证书主题的公钥。它们不包含必须安全存储的主体私钥](https://learn.microsoft.com/en-us/azure/iot-hub/tutorial-x509-certificates)。

不同的语言支持这种机制。英寸 net 中我们有 X509Certificate2，而在 Java 中我们可以使用[密钥库](https://community.oracle.com/tech/developers/discussion/1538506/how-do-i-create-an-x-509-certificate-from-pfx-file)。

一旦我们有了持有公钥的 X.509 证书的实例，我们就可以提取 RSA 私钥。然后，这个私钥将为我们使用的 JWT 令牌签名。在接下来的几篇文章中，我将展示如何在不同编程语言的帮助下，使用来自 PFX 的私钥对 JWT 令牌进行签名。

页（page 的缩写）s-Medium 是一个阅读、写作和向其他作者学习的绝佳平台。如果你想加入我的旅程，今天就加入 [medium](https://tarunbhatt9784.medium.com/membership) 。