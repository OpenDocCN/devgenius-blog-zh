# 什么是硬件安全模块？简短的解释

> 原文：<https://blog.devgenius.io/what-is-hardware-security-module-a-brief-explanation-6ac448f2cfa9?source=collection_archive---------13----------------------->

![](img/37a55297f681399c5ed8eec6a8cc41cb.png)

资料来源:FreePik

我想讨论一下硬件安全模块(HSM)及其相关技术。这将包含关于 HSM 的简要说明，在哪里可以了解它，以及如何使用它的代码示例。

**定义**

HSM 是保护和管理数字密钥并提供加密处理功能物理设备。简而言之，HSM 将从生成密钥、存储密钥、使用密钥进行解密/加密操作以及丢弃密钥开始提供您的秘密数字密钥。基本上，秘密密钥*永远不会以未加密的格式*离开 HSM。

**我们为什么需要它？**

我们主要需要它来保证我们的加密密钥的安全。像 PCI-DSS 这样最严格的标准要求使用安全设备来保存加密密钥。这就是 HSM 适合的地方。

**HSM 的类型**

HSM 有许多不同的形式，各有利弊。以下是市场上可以买到的几种 HSM:

1.  在 PCIe，HSM 以 PCIe 的形式嵌入到服务器中。例子:[泰雷兹卢娜 PCIe HSM](https://cpl.thalesgroup.com/encryption/hardware-security-modules/pcie-hsms)
2.  独立设备，其中 HSM 以独立设备的形式出现。示例: [Utimaco Cryptoserver CP5](https://utimaco.com/products/categories/general-purpose-solutions/cryptoserver-cp5)
3.  HSM 是以 u 盘的形式出现的。示例: [YubiHSM 2](https://www.yubico.com/id/product/yubihsm-2/)

**选哪个？**

HSM 有多种形式，这就产生了一个问题:选择哪一种？这取决于很多事情，比如你的预算和目的。通常在银行等严格合规的业务中，他们会规定必须达到的 HSM 标准水平。本标准通常指 [FIPS 140](https://en.wikipedia.org/wiki/FIPS_140) 。FIPS 140 有 4 个等级，其中 4 级是最高的。市场上的每一个 HSM 通常都符合 FIPS 140 的等级之一。

**用例**

HSM 可以在许多用例中使用。通常，用例与拥有安全加密存储设备的法规要求密切相关。以下是 HSM 的几个使用案例:

*   证书颁发机构(CA)私钥的生成、存储和操作
*   用于 web 服务器的 https 操作的私钥的生成、存储和操作
*   对 PDF 文件进行数字签名，
*   …以及许多其他涉及加密密钥存储和操作的操作

**使用的协议**

为了与 HSM 互动，我们需要某种协议。使用的通用协议称为 PKCS#11。PKCS#11 本身指定了加密 API(Cryptoki)。HSM 厂商将通过这个 Cryptoki 暴露它的功能。

通常，HSM 供应商也有自己的专有协议和 SDK 供开发人员使用。但是就可移植性而言，如果我们使用 PKCS#11 会更好，因为它是在各种 HSM 中使用的更常见的协议(一种类似于 web 服务器的 http/https 协议)。

**去哪里学 PKCS 11 号？**

这是一个有趣的问题。我学习这个协议的经验是通过使用模拟器，有时由 HSM 供应商提供。通常你必须先买 HSM，然后才能得到模拟器。所以，试用 PKCS 11 号最便宜的方法是使用 SoftHSM。

什么是 **SoftHSM** ？你可以把它看作是 HSM 基于软件的实现。是的，它们还附带了 PKCS 11 号协议。你可以在[这里](https://www.opendnssec.org/softhsm/)找到一切。

**使用 SoftHSM 的示例**

这是一个相当冗长的解释。这就是为什么我将在单独文章中更彻底地谈论它。现在，你可以查看这个 [github](https://github.com/rsatrio/SoftHSM2-REST) 的例子，看看如何通过 PKCS#11 协议与 SoftHSM 交互。

**参考文献**

*   [https://github.com/rsatrio/SoftHSM2-REST](https://github.com/rsatrio/SoftHSM2-REST)
*   [https://en.wikipedia.org/wiki/FIPS_140](https://en.wikipedia.org/wiki/FIPS_140)
*   [https://www.opendnssec.org/softhsm/](https://www.opendnssec.org/softhsm/)